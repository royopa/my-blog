<p>{
"title" : "Calculando o Beta de uma ação com Python",
"author":"Royopa",
"date":"24-04-2015",
"tag":"python, finance",
"slug" : "calculando-o-beta-de-uma-acao-com-python",
"category":"finance"
}</p>

<p>Tradução e adaptação do artigo <a href="https://composmachina.com/blog/2013/03/calculating-portfolio-beta-with-python/">Calculating Portfolio Beta with Python</a> do <a href="https://composmachina.com/blog/">Dave Tucker</a>.</p>

<h2 id="a-teoria">A teoria</h2>

<p>O Índice Beta é um indicador que mede a sensibilidade de um ativo em relação ao comportamento de uma carteira que represente o mercado.</p>

<p>É a relação entre a variação do retorno de uma ação (ativo) e o Ibovespa (mercado), por exemplo.</p>

<p>Portanto, o Índice Beta é uma medida do risco que um investidor está exposto ao investir em um ativo em particular em comparação com o mercado como um todo.</p>

<p>A fórmula do Índice Beta é bem simples:</p>

<p>Beta = Covariância entre o Retorno do Ativo e do Mercado / Variância do Retorno do Mercado</p>

<p>Ou desta maneira:</p>

<p><img src="http://hcinvestimentos.com/wp-content/uploads/2011/12/formula-beta.png" alt="Fórmula do Índice Beta" title="Fórmula do Índice Beta" /></p>

<p>onde:</p>

<p>βa = Beta</p>

<p>ra = Retorno do Ativo</p>

<p>rp = Retorno do Portfólio (Também pode ser usado como rm = Retorno do Mercado)</p>

<p>O coeficiente β é usado para medir o risco não-diversificável, isto é, fatores de mercado que afetam todas as empresas, como guerra, inflação, crises internacionais, etc. Ou seja, quanto o ativo está sujeito às variações não controláveis do mercado e do ambiente. É um índice que mede a relação entre o retorno de um ativo e o retorno do mercado. Desta forma, o prêmio por risco será sempre multiplicado por este coeficiente, exigindo um prêmio maior por risco quanto maior a variação do ativo em relação à carteira de mercado.</p>

<p>O beta da carteira de ações padrão, IBOVESPA, é sempre igual a 1, uma vez que ela é a base para o cálculo comparativo. O beta desta carteira é o beta médio de todos os títulos disponíveis. Desta forma, concluímos:</p>

<p>β = 1 Ativo médio.</p>

<p>Sua variação tende a acompanhar perfeitamente o mercado. Quando o IBOVESPA valoriza 5%, o ativo valoriza na mesma proporção.</p>

<p>β &lt; 1 Ativo defensivo.</p>

<p>Possui oscilações inferiores ao mercado e no mesmo sentido. Quando o IBOVESPA valoriza 5%, o ativo tende a valorizar menos do que 5%.</p>

<p>β > 1 Ativo agressivo.</p>

<p>Possui oscilações maiores do que o mercado e no mesmo sentido. Por exemplo, uma ação com β=2,0 tende a valorizar/desvalorizar o dobro do mercado (IBOVESPA).</p>

<p>Suponha-se que seja necessário fazer a escolha entre três ações que farão parte de um portfólio agressivo de médio prazo (até 1 ano). As ações são: VALE5, TNLP4 e ELPL5. Iremos determinar o risco do ativo a partir do índice beta.</p>

<p>Considere-se, para este exemplo, que o mercado encontra-se em alta, com boas perspectivas para o próximo ano.</p>

<p>Analisando a explicação sobre o índice beta fornecida acima, concluímos que devemos escolher ativos com betas superiores a 1, pois pertencem a ativos que possuem oscilações maiores do que a carteira. Tanto positivamente quanto negativamente.</p>

<p>O próximo passo é observar o período de análise do β. Este deve estar alinhado com a estratégia de investimento, isto é, para um investimento de médio prazo, devemos escolher o β de um período similar.</p>

<p>Fontes:
<a href="http://hcinvestimentos.com/2011/12/20/indice-beta/">O que é Índice Beta?</a>
<a href="http://pt.wikipedia.org/wiki/%C3%8Dndice_beta">Wikipédia</a></p>

<h2 id="vamos-a-programa%C3%A7%C3%A3o">Vamos a programação</h2>

<p>O código a seguir usa Python3 e o <a href="http://numpy.scipy.org/">Numpy</a>, pacote para a linguagem Python que suporta arrays e matrizes multidimensionais, possuindo uma larga coleção de funções matemáticas para trabalhar com estas estruturas.</p>

<pre><code class="python">#!/usr/bin/python3
# -*- coding: utf-8 -*-

import numpy as np
from datetime import date
import Quandl


# Faz a consulta de dados históricos do Quandl e retorna um array Numpy
def get_prices(stock, startDate, endDate):
    # Retorna os Ãºltimos 252 registros de cotações
    data = Quandl.get(
        stock, trim_start=startDate,
        trim_end=endDate,
        returns="numpy"
    )

    return data


# Verifica se existe datas faltando entre dados históricos da ação e de mercado
def check_missing_date_prices(stock_prices, market_prices):
    # transforma os dados históricos num array com de datas de preços
    stock_dates = get_datas_by_prices(stock_prices)
    # transforma os dados históricos num array com de datas de mercdo
    market_dates = get_datas_by_prices(market_prices)

    for stock_date in stock_dates:
        if stock_date not in market_dates:
            print('Data não existe nos dados de mercado: ', stock_date)

    for market_date in market_dates:
        if market_date not in stock_dates:
            print('Data não existe nos dados da ação: ', market_date)

    return False


# Transforma o array de preços retornados pelo Quantl num array de datas
def get_datas_by_prices(prices):

    # array com as datas que serão armazenadas
    datas = []

    # Percorre cada linha (ou seja, dia a dia)
    for price in prices:
        # Adiciona o elemento data no array
        datas.append(price[0])

    # Retorna os resultados
    return datas


# Transforma o array de preços retornados pelo Quantl
# num array Numpy somente com os preços de fechamento (index)
def get_closing_prices(prices, index):

    # preços de fechamento que serão os preços de fechamento diÃ¡rios
    closing_prices = []

    # Percorre cada linha (ou seja, dia a dia)
    for price in prices:
        # Adiciona o elemento preço de fechamento no array, depois
        # de convertÃª-lo para float
        closing_prices.append(float(price[index]))

    # Retorna os resultados como um array Numpy
    return np.array(closing_prices)


# Transforma o array de preços de fechamento num array de retornos
# (preço atual / preço dia anterior - 1)
def get_returns_prices(closing_prices, lenght):

    # Cria arrays zerados que serão preenchidos com os retornos dos preços.
    # Os dados de retorno sempre serão uma unidade menor do que os arrays
    # originais
    returns = np.zeros(lenght - 1)

    # Percorre cada preço dado
    for i in range(lenght - 1):
        # O retorno é igual ao preço atual dividido pelo preço anterior menos 1.
        # Devido a isso, vamos sempre começar com o segundo preço
        returns[i] = (closing_prices[i + 1] / closing_prices[i]) - 1

    # Retorna os resultados como um array Numpy
    return np.array(returns)


# Calcula o beta de uma ação dados os preços bem como os preços para o mercado
# como um todo (como um índice, Bovespa por exemplo).
# Os dois arrays devem ter o mesmo tamanho.
# O resultado é o beta arredondado com duas casas decimais
def calc_beta(prices_stock, prices_market):

    # Calcula e armazena o tamanho dos arrays porque eles serão usados várias
    # vezes
    stock_len = len(prices_stock)
    market_len = len(prices_market)

    # Decide qual o conjunto de dados tem menos itens, pois para calcular o
    # beta é necessário que os dados da ação e de mercado tenha a mesma
    # quantidade de registros
    smallest = market_len
    if stock_len &lt; market_len:
        smallest = stock_len

    # Cria os arrays de retornos dos preços
    # Os dados de retorno sempre serão uma unidade menor do que os arrays
    # originais
    stock_ret = get_returns_prices(prices_stock, smallest)
    market_ret = get_returns_prices(prices_market, smallest)

    # Calcula a covariância entre a ação e o mercado
    # http://docs.scipy.org/doc/numpy/reference/generated/numpy.cov.html
    covar_stock_market = np.cov(stock_ret, market_ret, ddof=0)[0, 1]
    print('Covariância ação/mercado: ', covar_stock_market)

    # Calcula a variância dos retornos do mercado
    # http://docs.scipy.org/doc/numpy/reference/generated/numpy.var.html
    var_market = np.var(market_ret)
    print('Variância mercado: ', var_market)

    # Beta é igual a covariância entre os retornos do ativo e retornos
    # do mercado dividido pela variância dos retornos do mercado.
    # Além disso, estamos arredondado o beta para duas casas decimais.
    return np.around(covar_stock_market / var_market, decimals=2)

# data atual
today = date.today()
# data de início para pegar os dados de mercado - 5 anos
startDate = date(today.year - 3, today.month, today.day)
# a data final é a data de hoje
endDate = today

# pega os dados do índice Bovespa
stock = 'YAHOO/INDEX_BVSP'
market_prices = get_prices(stock, startDate, endDate)
print('Mercado:', stock[stock.find('/')+1:])

# para os dados do YAHOO o índice de preço de fechamento ajustado é o 6
# ["Date","Open","High","Low","Close","Volume","Adjusted Close"]
closing_prices_market = get_closing_prices(market_prices, 6)

# pega os dados da ação PETR4
stock = 'GOOG/BVMF_PETR4'
stock_prices = get_prices(stock, startDate, endDate)
print('Ação:', stock[stock.find('/')+1:])

# para os dados do GOOGLE o índice de preço de fechamento é o 4
# ["Date","Open","High","Low","Close","Volume"]
closing_prices_stock = get_closing_prices(stock_prices, 4)

beta = calc_beta(closing_prices_stock, closing_prices_market)
print('O beta é = ', beta)
</code></pre>

<p>O código em Pyhton acima pode ser baixado no Gist, no endereço: <a href="https://gist.github.com/royopa/a8af15e8cead604ff6c2">https://gist.github.com/royopa/a8af15e8cead604ff6c2</a></p>
