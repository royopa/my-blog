{
"title" : "Recompilando o DSpace",
"author":"Royopa",
"date":"25-03-2015",
"tag":"dspace",
"slug" : "recompilando-o-dspace",
"category":"DSpace"
}

https://wiki.duraspace.org/display/DSPACE/Rebuild+DSpace

Diretórios utilizados nesse artigo:

[dspace] - O diretório de instalação do DSpace
[dspace-source]/dspace/ - O projeto compilado dentro do código fonte do DSpace
[dspace-source]/dspace/target/dspace-[version]-build.dir/ - O diretório onde o DSpace compilado constrói um um novo pacote de instalação para o DSpace.
[dspace-src]/dspace/target/dspace-installer/ - O mesmo descrito no item acima, porém para DSpace a partir da versão 5.
[Tomcat]/webapps/ - O diretório com as aplicações web do Tomcat

###Reinicialização rápida (apenas reinicia o servidor web depois de mudanças na configuração)
(*Exceção – Mudanças no arquivo Messages.properties sempre precisam de um rebuild!)

Parar o Tomcat
(Linux / OS X / Solaris) 
    $ [Tomcat]/bin/shutdown.sh

Iniciar o Tomcat
(Linux / OS X / Solaris) 
    $ [Tomcat]/bin/startup.sh

##Build rápido: (Build menor, geralmente quando são feitas mudanças em temas JSP ou XMLUI)

Entre no servidor onde o DSpace está rodando (exemplo, via SSH). Certifique-se de fazer o login com o usuário utilizado na instalação do DSpace!

Abra o prompt de comando e execute os comandos:

    $ [dspace-source]/dspace/ (entra na pasta onde está o código fonte do DSpace)
    $ mvn package (recompila o código DSpace e reconstrói o pacote de instalação)
    $ cd [dspace-src]/dspace/target/dspace-installer/ (entra no diretório onde o DSpace foi reconstruído)
    $ ant update (atualiza a instalação DSpace baseada no novo conteúdo no diretório target)

Alternativamente, se você não precisa instalar os arquivos JAR, você pode rodar o comando **ant update_webapps** (que somente copia as mudanças para o diretório [dspace]).
Se você não tem um Tomcat apontando para o diretório [dspace]/webapps/, você vai precisar copiar o aplicativo web final no diretório webapps, conforme abaixo:
    $ cp -R [dspace]/webapps/ [Tomcat]/webapps/

Teste suas mudanças no DSpace pelo navegador

##Atualização/Rebuild completo: (Atualiza o DSpace por completo)

Entre no servidor onde o DSpace está rodando (exemplo, via SSH). Certifique-se de fazer o login com o usuário utilizado na instalação do DSpace!

Abra o prompt de comando e execute os comandos:

    $ cd [dspace-source]/
    $ mvn clean package (recompila todo o código DSpace e reconstrói o pacote de instalação)
    $ cd [dspace-src]/dspace/target/dspace-installer/ (entra no diretório onde o DSpace foi reconstruído)
    $ ant update (atualiza a instalação DSpace baseada no novo conteúdo no diretório target)

Alternativamente, se você não precisa instalar os arquivos JAR, você pode rodar o comando **ant update_webapps** (que somente copia as mudanças para o diretório [dspace]).
Se você não tem um Tomcat apontando para o diretório [dspace]/webapps/, você vai precisar copiar o aplicativo web final no diretório webapps, conforme abaixo:
    $ cp -R [dspace]/webapps/ [Tomcat]/webapps/

Para forçar o Tomcat a recompilar tudo, você pode remover qualquer diretório de aplicações web DSpace criados em [Tomcat]/work/Catalina/localhost.

Iniciar o Tomcat
(Linux / OS X / Solaris) 
    $ [Tomcat]/bin/startup.sh

Teste suas mudanças no DSpace pelo navegador
