{
"title" : "Otimização de portfólio de Markovitz no Python",
"author":"Royopa",
"date":"21-04-2015",
"tag":"python, finance",
"slug" : "otimizacao-de-portfolio-de-markovitz-no-python",
"category":"finance"
}

Tradução e adaptação dos artigos abaixo:

(http://blog.quantopian.com/markowitz-portfolio-optimization-2/)
(http://work.ange.le.free.fr/works/MarkowitzPortfolio/MarkowitzPortfolio.pdf)
(http://www.rodrigofernandez.com.br/ecomp/ref/excel_markowitz.pdf)
(http://hcinvestimentos.com/2009/08/14/harry-markowitz-fronteira-eficiente/)

Introdução
----------

De acordo com a Wikipédia, o Modelo de Markowitz permite que se calcule o risco de uma carteira de investimentos, não importando se é composta por ações, opções, renda fixa ou qualquer outro ativo. Um ponto interessante é que usando o Modelo de Markowitz é possível construir carteiras de investimento em que o risco é inferior ao ativo de menor risco da carteira. Isto é, imagine uma carteira com PETR4 (suponha risco de 3% ao dia) e TAMM4 (risco de 4% ao dia) em que o risco da carteira é inferior ao ativo de menor risco - o que significaria dizer que posso investir em Petrobrás e Tam e ainda assim obter um risco menor que 3% ao dia.

Neste post você vai aprender sobre a idéia básica por trás de otimização de carteiras de Markowitz, bem como a forma de calcular em Python. Veremos também como criar um backtest simples que reequilibra seu portfólio da melhor forma.

Vamos começar usando dados aleatórios e só mais tarde usar dados de estoque reais. Esperamos que possa ajudá-lo a ter uma noção de como usar modelagem e simulação para melhorar a sua compreensão dos conceitos teóricos. Não se esqueça que a habilidade de um [algotrader](https://fernandonogueiracosta.wordpress.com/2012/09/06/seguidor-automatico-de-estrategias-financeiras-algotrader-real-people-real-money/) é colocar modelos matemáticos em código e este exemplo é ótimo para praticar.

Vamos começar com a importação de alguns módulos, que precisaremos mais tarde para produzir uma série de retornos normalmente distribuídos. O [***cvxopt***](http://cvxopt.org/) é um pacote para otimização convexa que pode ser facilmente fazer a instalação com comando ***sudo pip instalar cvxopt*** ou num sistema Ubuntu like ***sudo apt-get install python-cvxopt***.

Simulações
----------

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import numpy as np
import matplotlib.pyplot as plt
import cvxopt as opt
from cvxopt import blas, solvers
import pandas as pd

np.random.seed(123)

# Turn off progress printing
solvers.options['show_progress'] = False

## NUMBER OF ASSETS
n_assets = 4

## NUMBER OF OBSERVATIONS
n_obs = 1000

return_vec = np.random.randn(n_assets, n_obs)

plt.plot(return_vec.T, alpha=.4)
plt.xlabel('time')
plt.ylabel('returns')

plt.show()

#######################################

def rand_weights(n):
    ''' Produces n random weights that sum to 1 '''
    k = np.random.rand(n)
    return k / sum(k)

print(rand_weights(n_assets))
print(rand_weights(n_assets))

#######################################

def random_portfolio(returns):
    '''
    Returns the mean and standard deviation of returns for a random portfolio
    '''

    p = np.asmatrix(np.mean(returns, axis=1))
    w = np.asmatrix(rand_weights(returns.shape[0]))
    C = np.asmatrix(np.cov(returns))

    mu = w * p.T
    sigma = np.sqrt(w * C * w.T)

    # This recursion reduces outliers to keep plots pretty
    if sigma > 2:
        return random_portfolio(returns)
    return mu, sigma


n_portfolios = 500
means, stds = np.column_stack([
    random_portfolio(return_vec)
    for _ in range(n_portfolios)
])

plt.plot(stds, means, 'o', markersize=5)
plt.xlabel('std')
plt.ylabel('mean')
plt.title(
    'Mean and standard deviation of returns of randomly generated portfolios'
)

plt.show()

#######################################


def optimal_portfolio(returns):
    n = len(returns)
    returns = np.asmatrix(returns)

    N = 100
    mus = [10**(5.0 * t/N - 1.0) for t in range(N)]

    # Convert to cvxopt matrices
    S = opt.matrix(np.cov(returns))
    pbar = opt.matrix(np.mean(returns, axis=1))
    # Create constraint matrices
    G = -opt.matrix(np.eye(n))  # negative n x n identity matrix
    h = opt.matrix(0.0, (n, 1))
    A = opt.matrix(1.0, (1, n))
    b = opt.matrix(1.0)

    # Calculate efficient frontier weights using quadratic programming
    portfolios = [solvers.qp(mu*S, -pbar, G, h, A, b)['x'] for mu in mus]
    ## CALCULATE RISKS AND RETURNS FOR FRONTIER
    returns = [blas.dot(pbar, x) for x in portfolios]
    risks = [np.sqrt(blas.dot(x, S*x)) for x in portfolios]
    ## CALCULATE THE 2ND DEGREE POLYNOMIAL OF THE FRONTIER CURVE
    m1 = np.polyfit(returns, risks, 2)
    x1 = np.sqrt(m1[2] / m1[0])
    # CALCULATE THE OPTIMAL PORTFOLIO
    wt = solvers.qp(opt.matrix(x1 * S), -pbar, G, h, A, b)['x']
    return np.asarray(wt), returns, risks

weights, returns, risks = optimal_portfolio(return_vec)

plt.plot(stds, means, 'o')
plt.ylabel('mean')
plt.xlabel('std')
plt.plot(risks, returns, 'y-o')

plt.show()

################################################################################

print(weights)

################################################################################

from zipline.utils.factory import load_bars_from_yahoo
end = pd.Timestamp.utcnow()
start = end - 2500 * pd.tseries.offsets.BDay()

data = load_bars_from_yahoo(
    stocks=['IBM', 'GLD', 'XOM', 'AAPL', 'MSFT', 'TLT', 'SHY'],
    start=start,
    end=end)


data.loc[:, :, 'price'].plot(figsize=(8, 5))
plt.ylabel('price in $')

plt.show()

################################################################################

from zipline.api import add_history
from zipline.api import history
from zipline.api import set_slippage
from zipline.api import slippage
from zipline.api import set_commission
from zipline.api import commission
from zipline.api import order_target_percent
from zipline import TradingAlgorithm


def initialize(context):
    '''
    Called once at the very beginning of a backtest (and live trading).
    Use this method to set up any bookkeeping variables.

    The context object is passed to all the other methods in your algorithm.

    Parameters

    context: An initialized and empty Python dictionary that has been
             augmented so that properties can be accessed using dot
             notation as well as the traditional bracket notation.

    Returns None
    '''
    # Register history container to keep a window of the last 100 prices.
    add_history(100, '1d', 'price')
    # Turn off the slippage model
    set_slippage(slippage.FixedSlippage(spread=0.0))
    # Set the commission model (Interactive Brokers Commission)
    set_commission(commission.PerShare(cost=0.01, min_trade_cost=1.0))
    context.tick = 0


def handle_data(context, data):
    '''
    Called when a market event occurs for any of the algorithm's
    securities.

    Parameters

    data: A dictionary keyed by security id containing the current
          state of the securities in the algo's universe.

    context: The same context object from the initialize function.
             Stores the up to date portfolio as well as any state
             variables defined.

    Returns None
    '''
    # Allow history to accumulate 100 days of prices before trading
    # and rebalance every day thereafter.
    context.tick += 1
    if context.tick < 100:
        return
    # Get rolling window of past prices and compute returns
    prices = history(100, '1d', 'price').dropna()
    returns = prices.pct_change().dropna()
    try:
        # Perform Markowitz-style portfolio optimization
        weights, _, _ = optimal_portfolio(returns.T)
        # Rebalance portfolio accordingly
        for stock, weight in zip(prices.columns, weights):
            order_target_percent(stock, weight)
    except ValueError as e:
        # Sometimes this error is thrown
        # ValueError: Rank(A) < p or Rank([P; A; G]) < n
        print(e)
        pass

# Instantinate algorithm
algo = TradingAlgorithm(initialize=initialize, handle_data=handle_data)
# Run algorithm
results = algo.run(data)
results.portfolio_value.plot()

plt.show()
```
