<p>{
"title" : "Recompilando o DSpace",
"author":"Royopa",
"date":"25-03-2015",
"tag":"dspace",
"slug" : "recompilando-o-dspace",
"category":"DSpace"
}</p>

<p>https://wiki.duraspace.org/display/DSPACE/Rebuild+DSpace</p>

<p>Diretórios utilizados nesse artigo:</p>

<p>[dspace] - O diretório de instalação do DSpace
[dspace-source]/dspace/ - O projeto compilado dentro do código fonte do DSpace
[dspace-source]/dspace/target/dspace-[version]-build.dir/ - O diretório onde o DSpace compilado constrói um um novo pacote de instalação para o DSpace.
[dspace-src]/dspace/target/dspace-installer/ - O mesmo descrito no item acima, porém para DSpace a partir da versão 5.
[Tomcat]/webapps/ - O diretório com as aplicações web do Tomcat</p>

<h3 id="reinicializa%C3%A7%C3%A3o-r%C3%A1pida-apenas-reinicia-o-servidor-web-depois-de-mudan%C3%A7as-na-configura%C3%A7%C3%A3o">Reinicialização rápida (apenas reinicia o servidor web depois de mudanças na configuração)</h3>

<p>(*Exceção – Mudanças no arquivo Messages.properties sempre precisam de um rebuild!)</p>

<p>Parar o Tomcat
(Linux / OS X / Solaris) 
    $ [Tomcat]/bin/shutdown.sh</p>

<p>Iniciar o Tomcat
(Linux / OS X / Solaris) 
    $ [Tomcat]/bin/startup.sh</p>

<h2 id="build-r%C3%A1pido%3A-build-menor%2C-geralmente-quando-s%C3%A3o-feitas-mudan%C3%A7as-em-temas-jsp-ou-xmlui">Build rápido: (Build menor, geralmente quando são feitas mudanças em temas JSP ou XMLUI)</h2>

<p>Entre no servidor onde o DSpace está rodando (exemplo, via SSH). Certifique-se de fazer o login com o usuário utilizado na instalação do DSpace!</p>

<p>Abra o prompt de comando e execute os comandos:</p>

<pre><code>$ [dspace-source]/dspace/ (entra na pasta onde está o código fonte do DSpace)
$ mvn package (recompila o código DSpace e reconstrói o pacote de instalação)
$ cd [dspace-src]/dspace/target/dspace-installer/ (entra no diretório onde o DSpace foi reconstruído)
$ ant update (atualiza a instalação DSpace baseada no novo conteúdo no diretório target)
</code></pre>

<p>Alternativamente, se você não precisa instalar os arquivos JAR, você pode rodar o comando <strong>ant update_webapps</strong> (que somente copia as mudanças para o diretório [dspace]).
Se você não tem um Tomcat apontando para o diretório [dspace]/webapps/, você vai precisar copiar o aplicativo web final no diretório webapps, conforme abaixo:
    $ cp -R [dspace]/webapps/ [Tomcat]/webapps/</p>

<p>Teste suas mudanças no DSpace pelo navegador</p>

<h2 id="atualiza%C3%A7%C3%A3o%2Frebuild-completo%3A-atualiza-o-dspace-por-completo">Atualização/Rebuild completo: (Atualiza o DSpace por completo)</h2>

<p>Entre no servidor onde o DSpace está rodando (exemplo, via SSH). Certifique-se de fazer o login com o usuário utilizado na instalação do DSpace!</p>

<p>Abra o prompt de comando e execute os comandos:</p>

<pre><code>$ cd [dspace-source]/
$ mvn clean package (recompila todo o código DSpace e reconstrói o pacote de instalação)
$ cd [dspace-src]/dspace/target/dspace-installer/ (entra no diretório onde o DSpace foi reconstruído)
$ ant update (atualiza a instalação DSpace baseada no novo conteúdo no diretório target)
</code></pre>

<p>Alternativamente, se você não precisa instalar os arquivos JAR, você pode rodar o comando <strong>ant update_webapps</strong> (que somente copia as mudanças para o diretório [dspace]).
Se você não tem um Tomcat apontando para o diretório [dspace]/webapps/, você vai precisar copiar o aplicativo web final no diretório webapps, conforme abaixo:
    $ cp -R [dspace]/webapps/ [Tomcat]/webapps/</p>

<p>Para forçar o Tomcat a recompilar tudo, você pode remover qualquer diretório de aplicações web DSpace criados em [Tomcat]/work/Catalina/localhost.</p>

<p>Iniciar o Tomcat
(Linux / OS X / Solaris) 
    $ [Tomcat]/bin/startup.sh</p>

<p>Teste suas mudanças no DSpace pelo navegador</p>
