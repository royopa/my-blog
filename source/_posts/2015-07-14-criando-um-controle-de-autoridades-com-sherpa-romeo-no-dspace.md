{
"title" : "Criando um controle de autoridades com SHERPA RoMEO no DSpace",
"author":"Royopa",
"date":"14-07-2015",
"tag":"DSpace, SherpaRomeo",
"slug" : "criando-um-controle-de-autoridades-com-sherpa-romeo-no-dspace",
"category":"DSpace"
}

O DSpace permite que você utilize um controle de autoridades para os campos
da editora e título do jornal usando a API do SHERPA/RoMEO. Veremos abaixo como 
habilitar essa funcionalidade.

Antes de iniciar, leia o post [Integrando SHERPA/RoMEO no DSpace](http://www.royopa.url.ph/2015/07/09/integrando-sherpa-romeo-no-dspace) para ter mais informações sobre 
o SHERPA RoMEO e fazer as configurações iniciais no seu repositório DSpace.

Mão na massa!
-------------

Confira se o parâmetro sherpa.romeo.url foi configurado no arquivo 
[dspace]/config/dspace.cfg conforme abaixo: 

```cfg
sherpa.romeo.url = http://www.sherpa.ac.uk/romeo/api29.php
```

[Veja esse trecho do arquivo aqui](https://github.com/DSpace/DSpace/blob/master/dspace/config/dspace.cfg#L1580)

Altere a seção "Authority Control Settings" do arquivo [dspace]/config/dspace.cfg,
conforme abaixo:

```cfg
#####  Authority Control Settings  #####
plugin.named.org.dspace.content.authority.ChoiceAuthority = \
 org.dspace.content.authority.SHERPARoMEOPublisher = SRPublisher, \
 org.dspace.content.authority.SHERPARoMEOJournalTitle = SRJournalTitle
```

[Veja esse trecho do arquivo aqui](https://github.com/DSpace/DSpace/blob/master/dspace/config/dspace.cfg#L1590-1596)

Altere a seção "Demo: publisher name lookup through SHERPA/RoMEO:" do arquivo 
[dspace]/config/dspace.cfg, conforme abaixo:

```cfg
## Demo: publisher name lookup through SHERPA/RoMEO:
choices.plugin.dc.publisher = SRPublisher
choices.presentation.dc.publisher = lookup
```

Altere a seção "demo: journal title lookup, with ISSN as authority" do arquivo 
[dspace]/config/dspace.cfg, conforme abaixo:

```cfg
## demo: journal title lookup, with ISSN as authority
choices.plugin.dc.title.alternative = SRJournalTitle
choices.presentation.dc.title.alternative = lookup
authority.controlled.dc.title.alternative = true
```

E então reinicie o servidor tomcat com o comando abaixo:

```sh
sudo service tomcat7 restart
```

Testando
--------

Para testar a funcionalidade, basta adicionar um item numa coleção e você verá
que do lado direito dos campos "Other Titles" e "Publilsher" foram adicionados 
os botões lookup para buscar e selecionar os valores do SHERPA/RoMEO.

![](http://www.royopa.url.ph/themes/royopa-blog/assets/img/sherpa-romeo-form-lookup.jpg)
