<p>{
"title" : "Iniciando com o Apache Solr - Parte 1",
"author":"Royopa",
"date":"09-07-2015",
"tag":"Apache Solr",
"slug" : "iniciando-com-o-apache-solr-parte-1",
"category":"Apache SOLR"
}</p>

<p>O Apache Solr é um projeto Open Source de um servidor de buscas de alta
performance do projeto Apache Lucene. É desenvolvido em Java e utiliza o Lucene
Core como base para indexação e busca, além de fornecer APIs baseadas em REST,
o que lhe permite ser integrado a praticamente qualquer linguagem de programação.</p>

<h2 id="principais-caracter%C3%ADsticas">Principais características</h2>

<ul>
<li>Altamente escalável através da replicação de dados para outros servidores;</li>
<li>Fácil configuração através de configuração XML, sem necessidade de codificação;</li>
<li>Fácil extensão através de plugins;</li>
<li>Otimizado para alto volume de tráfego;</li>
<li>Interfaces padrão de integração: XML, JSON e HTTP;</li>
<li>Ferramenta de administração de fácil compreensão;</li>
<li>Estatísticas providas via JMX para monitoramento;</li>
</ul>

<h2 id="principais-recursos">Principais recursos</h2>

<ul>
<li>Busca Textual;</li>
<li>Busca Geoespacial;</li>
<li>Destaque de Termos;</li>
<li>Busca facetada;</li>
<li>Integração com Banco de Dados;</li>
<li>Manipulação de rich documents como PDFs ou DOCs;</li>
</ul>

<p>A idéia é fazer uma série de artigos curtos explicando sobre os principais
recursos, então aguarde próximos posts.</p>

<h2 id="instala%C3%A7%C3%A3o">Instalação</h2>

<p>Antes de iniciar a utilização do Apache Solr, verifique se você tem o Java 7 ou
superior instalado, você pode conferir isso com o comando:</p>

<pre><code class="cmd">java -version
Java(TM) SE Runtime Environment (build 1.7.0_1.71-b14)
Java HotSpot (TM) Client VM (build 24.71-b01, mixed mode, sharing)
</code></pre>

<p>A última versão do Solr pode ser baixada <a href="http://lucene.apache.org/solr/mirrors-solr-latest-redir.html">nesse link</a>.</p>

<p>Após ter baixado o arquivo, basta extraí-lo, usando o comando abaixo (caso
tenha optado pelo formato zip):</p>

<pre><code class="cmd">ls solr*
solr-5.2.1.zip
unzip -q solr-5.2.1.zip
cd solr-5.2.1/
</code></pre>

<p>E então para iniciar o serviço, execute o comando abaixo:</p>

<pre><code class="cmd">./bin/solr start -e cloud -noprompt

Welcome to the SolrCloud example!


Starting up 2 Solr nodes for your example SolrCloud cluster.
...

Started Solr server on port 8983 (pid=8404). Happy searching!
...

Started Solr server on port 7574 (pid=8549). Happy searching!
...

SolrCloud example running, please visit http://localhost:8983/solr
</code></pre>

<p>Veremos nos próximos artigos como fazer a indexação de dados. Caso você não
queira esperar, veja a documentação no link <a href="http://lucene.apache.org/solr/quickstart.html">http://lucene.apache.org/solr/quickstart.html</a>.</p>
