{
"title" : "Iniciando com o Apache Solr - Parte 1",
"author":"Royopa",
"date":"09-07-2015",
"tag":"Apache Solr",
"slug" : "iniciando-com-o-apache-solr-parte-1",
"category":"Apache SOLR"
}

O Apache Solr é um projeto Open Source de um servidor de buscas de alta
performance do projeto Apache Lucene. É desenvolvido em Java e utiliza o Lucene
Core como base para indexação e busca, além de fornecer APIs baseadas em REST,
o que lhe permite ser integrado a praticamente qualquer linguagem de programação.

Principais características
--------------------------

* Altamente escalável através da replicação de dados para outros servidores;
* Fácil configuração através de configuração XML, sem necessidade de codificação;
* Fácil extensão através de plugins;
* Otimizado para alto volume de tráfego;
* Interfaces padrão de integração: XML, JSON e HTTP;
* Ferramenta de administração de fácil compreensão;
* Estatísticas providas via JMX para monitoramento;

Principais recursos
-------------------

* Busca Textual;
* Busca Geoespacial;
* Destaque de Termos;
* Busca facetada;
* Integração com Banco de Dados;
* Manipulação de rich documents como PDFs ou DOCs;

A idéia é fazer uma série de artigos curtos explicando sobre os principais
recursos, então aguarde próximos posts.

Instalação
----------

Antes de iniciar a utilização do Apache Solr, verifique se você tem o Java 7 ou
superior instalado, você pode conferir isso com o comando:

```cmd
java -version
Java(TM) SE Runtime Environment (build 1.7.0_1.71-b14)
Java HotSpot (TM) Client VM (build 24.71-b01, mixed mode, sharing)
```

A última versão do Solr pode ser baixada [nesse link][1].

Após ter baixado o arquivo, basta extraí-lo, usando o comando abaixo (caso
tenha optado pelo formato zip):

```cmd
ls solr*
solr-5.2.1.zip
unzip -q solr-5.2.1.zip
cd solr-5.2.1/
```

E então para iniciar o serviço, execute o comando abaixo:

```cmd
./bin/solr start -e cloud -noprompt

Welcome to the SolrCloud example!


Starting up 2 Solr nodes for your example SolrCloud cluster.
...

Started Solr server on port 8983 (pid=8404). Happy searching!
...

Started Solr server on port 7574 (pid=8549). Happy searching!
...

SolrCloud example running, please visit http://localhost:8983/solr
```

Veremos nos próximos artigos como fazer a indexação de dados. Caso você não
queira esperar, veja a documentação no link [http://lucene.apache.org/solr/quickstart.html](http://lucene.apache.org/solr/quickstart.html).

[1]:http://lucene.apache.org/solr/mirrors-solr-latest-redir.html
