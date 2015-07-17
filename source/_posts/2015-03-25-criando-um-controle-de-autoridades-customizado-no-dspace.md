{
"title" : "Criando um controle de autoridades customizado no DSpace",
"author":"Royopa",
"date":"25-03-2015",
"tag":"dspace",
"slug" : "criando-um-controle-de-autoridades-customizado-no-dspace",
"category":"DSpace"
}

http://libraryblogs.is.ed.ac.uk/redic/2013/02/12/authority-control/

#Mudanças no DSpace

Baixe as classes Java do DSpace, no repositório do [GitHub](https://github.com/royopa/dspace-tematres)

Copie as classes TematresProtocol.java e TematresSponsorship.java para o diretório 
[dspace-src]/dspace-api/src/main/java/org/dspace/content/authority/.

Recompile o DSpace com o comandos abaixo:

    $ cd [dspace-src]
    $ mvn package
    $ cd [dspace-src]/dspace/target/dspace-installer/
    $ ant update

## Inclua as informações abaixo no arquivo dspace.cfg para usar o controle de autoridades do Tematres:

```cfg
    #####  Authority Control Settings  #####
    plugin.named.org.dspace.content.authority.ChoiceAuthority = \
     org.dspace.content.authority.TematresSponsorship = TematresSponsorship

    ## URL para acesso ao web service do Tematres
    tematres.url = http://bdpife2.sibi.usp.br/a/tematres/vocab/services.php

    ## Configurado plugin para Sponsorship para acesso aos dados do Tematres
    choices.plugin.dc.description.sponsorship = TematresSponsorship
    choices.presentation.dc.description.sponsorship = lookup
    authority.controlled.dc.description.sponsorship = true
```

## Reinicie o servidor Tomcat:

    $ sudo service tomcat7 restart
