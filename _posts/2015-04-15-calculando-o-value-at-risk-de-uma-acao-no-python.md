<p>{
"title" : "Calculando o Value at Risk de uma ação no Python",
"author":"Royopa",
"date":"15-04-2015",
"tag":"python, finance",
"slug" : "calculando-o-value-at-risk-de-uma-acao-no-python",
"category":"finance"
}</p>

<p>Em uma madrugada que utilizei para juntar o conhecimento que estou adquirindo fazendo um curso preparatório para a <a href="http://certificacao.anbid.com.br/cea.asp">Certificação de Especialista em Investimentos Anbima (CEA)</a> e também aprendendo a <a href="https://www.python.org/">linguagem de programação Python</a> resolvi fazer alguns testes e pesquisas para calcular o <a href="http://pt.wikipedia.org/wiki/Value_at_Risk">Value at Risk (VaR)</a> de uma ação pegando os dados do <a href="http://finance.yahoo.com/">Yahoo Finance</a>.</p>

<p>Uma alternativa para pegar os dados de cotações é usar o <a href="https://www.quandl.com/">Quandl</a>. Existe um <a href="http://wilsonfreitas.github.io/posts/2014-03/quandl-uma-otima-forma-de-obter-dados-estruturados-em-python.html">artigo sobre o Quandl do Wilson Freitas</a>, um cara que é fera em Python e ciência de dados. Tinha visto o blog dele na semana passada e gostei bastante das análises. Por coincidência assisti uma <a href="http://www.slideshare.net/wfreitas/analise-dos-campeoes-da-corrida-de-sao-silvestre-com-python">palestra dele</a> no <a href="http://www.meetup.com/Grupy-SP/events/221188662/">encontro GrupySP do dia 11/04/2015</a>.</p>

<p>Fiz algumas pesquisas encontrei um artigo legal sobre como calcular o VaR usando Python, então resolvi traduzi-lo e adaptá-lo, o texto original está no endereço (http://gosmej1977.blogspot.com.br/2013/06/value-at-risk.html):</p>

<h2 id="a-teoria">A teoria</h2>

<p>De acordo com a Wikipédia, Value at Risk (VaR) é um método para avaliar o risco em operações financeiras. O VaR resume, em um número, o risco de um produto financeiro ou o risco de uma carteira de investimentos , de um montante financeiro. Esse número representa a pior perda esperada em um dado horizonte de tempo e é associado a um intervalo de confiança.</p>

<p>Normalmente o VaR é calculado com 95%, 97,5% ou 99% de confiança. Este nível de confiança nos indica que é esperada perda maior que a calculada pelo VaR. Assim, ao utilizar 99% de confiança, espera-se que a cada 100 observações do VaR, em pelo menos 1 vez a perda do investimento financeiro seja superior à perda estimada no cálculo do VaR.</p>

<p>Assim, o VaR diário de 95% é uma quantidade negativa de dinheiro que separa os nossos resultados dos outros 5% piores resultados. Então, sabemos que 95% dos nossos dias será melhor do que esse número e 5% pior.</p>

<p>Existem várias técnicas para o cálculo do VaR. Estas técnicas podem ser divididas em dois grandes grupos: VaR Paramétrico e VaR Não Paramétrico (Simulação). 
O VaR Paramétrico baseia-se no conhecimento prévio de uma distribuição estatística (Ex.: Curva Normal) para fazer o cálculo das perdas financeiras com base em hipótese de comportamento da distribuição de probabilidades dos retornos dos ativos. 
O VaR Não Paramétrico não faz hipótese alguma sobre a distribuição de probabilidade dos retornos dos ativos. Nestas técnicas (Ex.: Simulação Histórica, Simulação de Monte Carlo) são utilizadas a história dos próprios retornos para obtenção de informações sobre as perdas financeiras.</p>

<p>O VaR deve ser sempre associado a:
    Uma moeda (valor monetário)
    Um intervalo de tempo
    Uma probabilidade com que a perda será percebida</p>

<p>Ex.: "O VaR da minha carteira, para 1 dia e com 95% de confiança é de R$ 100.000,00" onde:</p>

<ul>
<li>"...para 1 dia": significa que o cálculo do VaR considerou a hipótese de maior perda para acontecer no próximo dia</li>
<li>"...com 95% de confiança": significa que para cada 100 dias é esperado que em 5 dias a perda realizada seja maior do que a prevista pelo VaR</li>
<li>"...é de R$ 100.000,00": Montante financeiro máximo de perda esperada</li>
</ul>

<p>Tendo em conta que a distribuição dos preços muda com o tempo, não se pode realmente saber o VaR, mas pode-se estimá-lo.</p>

<p>Veja um trecho do regulamento do fundo do BTG Pactual <a href="http://www.guiainvest.com.br/dados/fundo/16522%5CRegulamento_16522_2014_04_28.pdf">Joatina I Fi Multimercado Crédito Privado</a> que fala sobre o risco de mercado do FUndo medido pelo VaR:</p>

<blockquote>
  <p>O limite de risco de mercado diário do fundo, medido pelo
  VaR, será de 50% do CDI projetado para 21 dias, tendo como parâmetros, 95% de nível
  de confiança, modelo não paramétrico e horizonte de 21 dias, com janela de volatilidade
  de 252 dias úteis.</p>
</blockquote>

<h2 id="vamos-a-programa%C3%A7%C3%A3o">Vamos a programação</h2>

<p>O código a seguir usa python3 o <a href="http://pandas.pydata.org/">pandas</a>, uma biblioteca bastante útil para manipular dados financeiros e também o <a href="http://matplotlib.org/">matplotlib</a> para geração dos gráficos.</p>

<p>Eu não tenho muito conhecimento em estatística, então é outro assunto que estou aprendendo. Sobre o que é quantil veja na <a href="http://pt.wikipedia.org/wiki/Quantil">Wikipedia</a> e tem uma vídeo aula bem didática sobre Quartis (os 4-quantis - Q) neste link <a href="https://www.youtube.com/watch?v=szKwOaWY-Nk">Aula de Estatística - Quartis e Percentis</a>.</p>

<pre><code class="python"># Escolha do período de tempo inicial d0 e final d1
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
</code></pre>

<p>Estou usando a ação PETR4.SA mas isso não importa, caso você queira mudar basta alterar o parâmetro stock, de acordo com o código da ação no <a href="http://finance.yahoo.com/">Yahoo Finance</a>. A volatilidade vai ser estimada utilizando uma janela móvel e também será testado uma <a href="http://www.uic.edu/cuppa/pa/academics/Duplicate/Lectures,%20Outlines%20and%20Handouts/Public%20Finance/Asset%20Pricing-%20Dennis%20Pelletier%20of%20North%20Carolina%20State%20University/Estimating%20Volatilities,%20correlations%20and%20VaR.pdf">janela móvel (rolling window)</a> com estimativas quantis.
Note que a janela para os quantis precisa ser grande enquanto testamos estimar o quantil de 5% (ou 1%) e para isso precisamos garantir que temos observações suficientes. A janela para volatilidade não deve ser muito grande pois a volatilidade muda bem rápido.</p>

<p>A linha <strong>price = price.asfreq('B').fillna(method='pad')"</strong> tem a amostra de dados de todos os dias úteis, se o preço tiver faltando então preço anterior será copiado.</p>

<p>Depois os próprios cálculos</p>

<pre><code class="python">#VaR simples usando todos os dados
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
</code></pre>

<p>Usando funções de rolling do Panda torna o processo simples. A função <em>rolling_quantile</em> calcula o quantil em uma janela móvel e a função <em>expanding_quantile</em> estima os quantis usando todos os dados disponíveis até a data considerada.</p>

<p>Lembre-se que temos uma série de preços do Yahoo, em seguida, cada data na séria tem uma estimativa de VaR baseado nos dados passados e não sobre todos os dados, o que seria uma enganação.</p>

<p>Nós podemos então criar um gráfico:</p>

<pre><code class="python">ret2 = ret.shift(-1)

courbe = pd.DataFrame({'returns':ret2,
              'quantiles':unnormedquantile,
              'Rolling quantiles':unnormedquantileR,
              'Normed quantiles':normedquantile,
              'Rolling Normed quantiles':normedquantileR,
              })
courbe.plot()
plt.show()
</code></pre>

<p>Ele mostra os retornos da PETRO4 e os diferentes VAR computados.
É um gráfico bastante denso então não foi reproduzido nesse artigo, mas você não terá problemas para reproduzí-lo.</p>

<p>Nós vemos que o VaR normalizado é muito mais variável pois segue a volatilidade. Finalmente nós podemos julgar a eficiência dos nossos cálculos. o VaR 95% deve ser quebrado em 5% do tempo, o código seguinte faz esse teste:</p>

<pre><code class="python">courbe['nqBreak']   = np.sign(ret2-normedquantile)/(-2) +0.5
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
</code></pre>

<p>O rolling VaR usa uma abordagem de janela móvel para a estimativa dos quantis, mas precisa de algum tempo antes da janela ser preenchida de modo que o número de dias para testá-la seja diferente. A partir desses números por si só o VaR simples é melhor, mais perto dos 5% de quebras de VaR. Porém, a partir do gráfico o VaR simples parece muito estático e independente do mercado e como uma medida de risco não é algo desejado.</p>

<p>Isso pode ser melhorado mudando a forma de calcular a volatilidade. Eu uso um estimador simples, mas pode se usar um estimador baseado em OHLC <a href="http://blog.bussoladoinvestidor.com.br/grafico-ohlc/">(open/high/low/close)</a> ou um <a href="http://www.maxwell.vrac.puc-rio.br/14872/14872_3.PDF">modelo GARCH</a>.</p>
