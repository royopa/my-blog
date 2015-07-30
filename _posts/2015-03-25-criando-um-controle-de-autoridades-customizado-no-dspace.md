<p>{
"title" : "Criando um controle de autoridades customizado no DSpace",
"author":"Royopa",
"date":"25-03-2015",
"tag":"dspace",
"slug" : "criando-um-controle-de-autoridades-customizado-no-dspace",
"category":"DSpace"
}</p>

<p>http://libraryblogs.is.ed.ac.uk/redic/2013/02/12/authority-control/</p>

<h1 id="mudan%C3%A7as-no-dspace">Mudanças no DSpace</h1>

<p>Baixe as classes Java do DSpace, no repositório do <a href="https://github.com/royopa/dspace-tematres">GitHub</a></p>

<p>Copie as classes TematresProtocol.java e TematresSponsorship.java para o diretório 
[dspace-src]/dspace-api/src/main/java/org/dspace/content/authority/.</p>

<p>Recompile o DSpace com o comandos abaixo:</p>

<pre><code>$ cd [dspace-src]
$ mvn package
$ cd [dspace-src]/dspace/target/dspace-installer/
$ ant update
</code></pre>

<h2 id="inclua-as-informa%C3%A7%C3%B5es-abaixo-no-arquivo-dspace.cfg-para-usar-o-controle-de-autoridades-do-tematres%3A">Inclua as informações abaixo no arquivo dspace.cfg para usar o controle de autoridades do Tematres:</h2>

<pre><code class="cfg">    #####  Authority Control Settings  #####
    plugin.named.org.dspace.content.authority.ChoiceAuthority = \
     org.dspace.content.authority.TematresSponsorship = TematresSponsorship

    ## URL para acesso ao web service do Tematres
    tematres.url = http://bdpife2.sibi.usp.br/a/tematres/vocab/services.php

    ## Configurado plugin para Sponsorship para acesso aos dados do Tematres
    choices.plugin.dc.description.sponsorship = TematresSponsorship
    choices.presentation.dc.description.sponsorship = lookup
    authority.controlled.dc.description.sponsorship = true
</code></pre>

<h2 id="reinicie-o-servidor-tomcat%3A">Reinicie o servidor Tomcat:</h2>

<pre><code>$ sudo service tomcat7 restart
</code></pre>
