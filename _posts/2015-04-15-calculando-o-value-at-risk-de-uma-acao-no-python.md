{
"title" : "Calculando o Value at Risk de uma ação no Python",
"author":"Royopa",
"date":"15-04-2015",
"tag":"python, finance",
"slug" : "calculando-o-value-at-risk-de-uma-acao-no-python",
"category":"finance"
}

Em uma madrugada que utilizei para juntar o conhecimento que estou adquirindo fazendo um curso preparatório para a [Certificação de Especialista em Investimentos Anbima (CEA)](http://certificacao.anbid.com.br/cea.asp) e também aprendendo a [linguagem de programação Python](https://www.python.org/) resolvi fazer alguns testes e pesquisas para calcular o [Value at Risk (VaR)](http://pt.wikipedia.org/wiki/Value_at_Risk) de uma ação pegando os dados do [Yahoo Finance](http://finance.yahoo.com/).

Uma alternativa para pegar os dados de cotações é usar o [Quandl](https://www.quandl.com/). Existe um [artigo sobre o Quandl do Wilson Freitas](http://wilsonfreitas.github.io/posts/2014-03/quandl-uma-otima-forma-de-obter-dados-estruturados-em-python.html), um cara que é fera em Python e ciência de dados. Tinha visto o blog dele na semana passada e gostei bastante das análises. Por coincidência assisti uma [palestra dele](http://www.slideshare.net/wfreitas/analise-dos-campeoes-da-corrida-de-sao-silvestre-com-python) no [encontro GrupySP do dia 11/04/2015](http://www.meetup.com/Grupy-SP/events/221188662/).

Fiz algumas pesquisas encontrei um artigo legal sobre como calcular o VaR usando Python, então resolvi traduzi-lo e adaptá-lo, o texto original está no endereço (http://gosmej1977.blogspot.com.br/2013/06/value-at-risk.html):

A teoria
--------

De acordo com a Wikipédia, Value at Risk (VaR) é um método para avaliar o risco em operações financeiras. O VaR resume, em um número, o risco de um produto financeiro ou o risco de uma carteira de investimentos , de um montante financeiro. Esse número representa a pior perda esperada em um dado horizonte de tempo e é associado a um intervalo de confiança. 

Normalmente o VaR é calculado com 95%, 97,5% ou 99% de confiança. Este nível de confiança nos indica que é esperada perda maior que a calculada pelo VaR. Assim, ao utilizar 99% de confiança, espera-se que a cada 100 observações do VaR, em pelo menos 1 vez a perda do investimento financeiro seja superior à perda estimada no cálculo do VaR.

Assim, o VaR diário de 95% é uma quantidade negativa de dinheiro que separa os nossos resultados dos outros 5% piores resultados. Então, sabemos que 95% dos nossos dias será melhor do que esse número e 5% pior.

Existem várias técnicas para o cálculo do VaR. Estas técnicas podem ser divididas em dois grandes grupos: VaR Paramétrico e VaR Não Paramétrico (Simulação). 
O VaR Paramétrico baseia-se no conhecimento prévio de uma distribuição estatística (Ex.: Curva Normal) para fazer o cálculo das perdas financeiras com base em hipótese de comportamento da distribuição de probabilidades dos retornos dos ativos. 
O VaR Não Paramétrico não faz hipótese alguma sobre a distribuição de probabilidade dos retornos dos ativos. Nestas técnicas (Ex.: Simulação Histórica, Simulação de Monte Carlo) são utilizadas a história dos próprios retornos para obtenção de informações sobre as perdas financeiras.

O VaR deve ser sempre associado a:
    Uma moeda (valor monetário)
    Um intervalo de tempo
    Uma probabilidade com que a perda será percebida

Ex.: "O VaR da minha carteira, para 1 dia e com 95% de confiança é de R$ 100.000,00" onde:

- "...para 1 dia": significa que o cálculo do VaR considerou a hipótese de maior perda para acontecer no próximo dia
- "...com 95% de confiança": significa que para cada 100 dias é esperado que em 5 dias a perda realizada seja maior do que a prevista pelo VaR
- "...é de R$ 100.000,00": Montante financeiro máximo de perda esperada

Tendo em conta que a distribuição dos preços muda com o tempo, não se pode realmente saber o VaR, mas pode-se estimá-lo.

Veja um trecho do regulamento do fundo do BTG Pactual [Joatina I Fi Multimercado Crédito Privado](http://www.guiainvest.com.br/dados/fundo/16522%5CRegulamento_16522_2014_04_28.pdf) que fala sobre o risco de mercado do FUndo medido pelo VaR:

> O limite de risco de mercado diário do fundo, medido pelo
> VaR, será de 50% do CDI projetado para 21 dias, tendo como parâmetros, 95% de nível
> de confiança, modelo não paramétrico e horizonte de 21 dias, com janela de volatilidade
> de 252 dias úteis.

Vamos a programação
-------------------

O código a seguir usa python3 o [pandas](http://pandas.pydata.org/), uma biblioteca bastante útil para manipular dados financeiros e também o [matplotlib](http://matplotlib.org/) para geração dos gráficos.

Eu não tenho muito conhecimento em estatística, então é outro assunto que estou aprendendo. Sobre o que é quantil veja na [Wikipedia](http://pt.wikipedia.org/wiki/Quantil) e tem uma vídeo aula bem didática sobre Quartis (os 4-quantis - Q) neste link [Aula de Estatística - Quartis e Percentis](https://www.youtube.com/watch?v=szKwOaWY-Nk).

```python
# Escolha do período de tempo inicial d0 e final d1
startDate = datetime.datetime(2001, 1, 1)
endDate   = datetime.datetime(2012, 1, 1)

#get the tickers
stock = 'PETR4.SA'
price = DataReader(stock, 'yahoo',startDate,endDate)['Adj Close']
price = price.asfreq('B').fillna(method='pad')

ret = price.pct_change()

#escolhe o quantil
quantile = 0.05
#a janela de volatilidade
volwindow = 50
#e a janela móvel do VaR
varwindow = 250
```

Estou usando a ação PETR4.SA mas isso não importa, caso você queira mudar basta alterar o parâmetro stock, de acordo com o código da ação no [Yahoo Finance](http://finance.yahoo.com/). A volatilidade vai ser estimada utilizando uma janela móvel e também será testado uma [janela móvel (rolling window)](http://www.uic.edu/cuppa/pa/academics/Duplicate/Lectures,%20Outlines%20and%20Handouts/Public%20Finance/Asset%20Pricing-%20Dennis%20Pelletier%20of%20North%20Carolina%20State%20University/Estimating%20Volatilities,%20correlations%20and%20VaR.pdf) com estimativas quantis.
Note que a janela para os quantis precisa ser grande enquanto testamos estimar o quantil de 5% (ou 1%) e para isso precisamos garantir que temos observações suficientes. A janela para volatilidade não deve ser muito grande pois a volatilidade muda bem rápido.

A linha **price = price.asfreq('B').fillna(method='pad')"** tem a amostra de dados de todos os dias úteis, se o preço tiver faltando então preço anterior será copiado.

Depois os próprios cálculos

```python
#VaR simples usando todos os dados
unnormedquantile = pd.expanding_quantile(ret,quantile)

#similar usando uma janela móvel
unnormedquantileR = pd.rolling_quantile(ret,varwindow,quantile)

#nós também podemos normalizar o retorno por parte da vol
vol     = pd.rolling_std(ret,volwindow)*np.sqrt(256)
unitvol = ret/vol

#e também obter o quantil expandido ou o rolling dos quantis
Var  = pd.expanding_quantile(unitvol,quantile)
VarR = pd.rolling_quantile(unitvol,varwindow,quantile)

normedquantile  = Var*vol
normedquantileR = VarR*vol
```

Usando funções de rolling do Panda torna o processo simples. A função *rolling_quantile* calcula o quantil em uma janela móvel e a função *expanding_quantile* estima os quantis usando todos os dados disponíveis até a data considerada.

Lembre-se que temos uma série de preços do Yahoo, em seguida, cada data na séria tem uma estimativa de VaR baseado nos dados passados e não sobre todos os dados, o que seria uma enganação.

Nós podemos então criar um gráfico:

```python
ret2 = ret.shift(-1)

courbe = pd.DataFrame({'returns':ret2,
              'quantiles':unnormedquantile,
              'Rolling quantiles':unnormedquantileR,
              'Normed quantiles':normedquantile,
              'Rolling Normed quantiles':normedquantileR,
              })
courbe.plot()
plt.show()
```

Ele mostra os retornos da PETRO4 e os diferentes VAR computados.
É um gráfico bastante denso então não foi reproduzido nesse artigo, mas você não terá problemas para reproduzí-lo.

Nós vemos que o VaR normalizado é muito mais variável pois segue a volatilidade. Finalmente nós podemos julgar a eficiência dos nossos cálculos. o VaR 95% deve ser quebrado em 5% do tempo, o código seguinte faz esse teste:

```python
courbe['nqBreak']   = np.sign(ret2-normedquantile)/(-2) +0.5
courbe['nqBreakR']  = np.sign(ret2-normedquantileR)/(-2) +0.5
courbe['UnqBreak']  = np.sign(ret2-unnormedquantile)/(-2) +0.5
courbe['UnqBreakR'] = np.sign(ret2-unnormedquantileR)/(-2) +0.5


nbdays = price.count()

print('Número de retornos piores que o VaR')
print('Ideal VaR                : ', (quantile)*nbdays)
print('Simple VaR               : ', np.sum(courbe['UnqBreak']))
print('Normalized VaR           : ', np.sum(courbe['nqBreak']))
print('---------------------------')
print('Ideal Rolling Var        : ', (quantile)*(nbdays-varwindow))
print('Rolling VaR              : ', np.sum(courbe['UnqBreakR']))
print('Rolling Normalized VaR   : ', np.sum(courbe['nqBreakR']))
```
O rolling VaR usa uma abordagem de janela móvel para a estimativa dos quantis, mas precisa de algum tempo antes da janela ser preenchida de modo que o número de dias para testá-la seja diferente. A partir desses números por si só o VaR simples é melhor, mais perto dos 5% de quebras de VaR. Porém, a partir do gráfico o VaR simples parece muito estático e independente do mercado e como uma medida de risco não é algo desejado.

Isso pode ser melhorado mudando a forma de calcular a volatilidade. Eu uso um estimador simples, mas pode se usar um estimador baseado em OHLC [(open/high/low/close)](http://blog.bussoladoinvestidor.com.br/grafico-ohlc/) ou um [modelo GARCH](http://www.maxwell.vrac.puc-rio.br/14872/14872_3.PDF).
