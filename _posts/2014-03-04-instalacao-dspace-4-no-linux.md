{
"title" : "Instalação DSpace 4 no Linux",
"author":"Royopa",
"date":"04-03-2014",
"tag":"DSpace, DSpace install",
"slug" : "instalacao-dspace-4-no-linux",
"category":"DSpace"
}

Este é a replicação com algumas atualizações de um artigo que publiquei no meu blog em [Wordpress](http://royopa.wordpress.com/2014/03/04/instalacao-do-dspace-4-1-em-sistemas-ubuntu-like/).

Após o lançamento da versão 4.1 do DSpace em 03/03/2014, resolvi criar uma máquina virtual para testar a estabilidade e novas funcionalidades da versão.

Devido ao processo de instalação ser um pouco <del>difícil</del> chato, com diversas configurações para se fazer e tudo mais (como já havia publicado no [Slideshare](http://www.slideshare.net/royopa1/instalao-dspace-3x-windows-e-linux) o guia para a instalação da versão 3, resolvi criar um script para instalação de forma quase que automática do DSpace.

Para esse processo utilizei como base o artigo DSpace LiveCD da [Wiki do DSpace](https://wiki.duraspace.org/display/DSPACE/LiveCD).

Meu projeto está no [GitHub](https://github.com/royopa/dspace-auto-install/), então quem tiver interesse de melhorá-lo, adequá-lo conforme as suas necessidades fiquem à vontade!.

##1º Passo - Baixar uma imagem do Ubuntu

Para evitar o processo de instalação do Linux foi baixada uma imagem do [XUbuntu](http://downloads.sourceforge.net/virtualboximage/xubuntu_1204.7z), localizada no site [Virtual Box Images](http://virtualboxes.org/images/), conforme abaixo:

```
Xubuntu 12.04 codename Precise Pangolin
Size (compressed/uncompressed): 502.2 MBytes / 2.6 GBytes
Active user account(s) (username/password): xubuntu/reverse
```

Outra alternativa é usar uma versão de Linux bem pequena, chamada Puppy Linux, através da [imagem](http://sourceforge.net/projects/virtualboximage/files/Puppy%20Linux/5.2.0/LucidPuppy-520.7z/download) baixada no mesmo site.

```
Puppy Linux 5.2.0 Lucid
Size (compressed/uncompressed): 167.0 MBytes/ 655 MBytes
Link:LucidPuppy-520.7z
```

Após ter a máquina virtual baixada e funcionando siga para os próximos passos. Esse artigo não tem a intenção de ensinar como criar uma máquina virtual, mas procurando no Google você verá que o processo é bem fácil.

##2º Passo - Baixar os arquivos/scripts para instalação do DSpace

Baixe os [arquivos do GitHub](https://github.com/royopa/dspace-auto-install/archive/master.zip) para a instalação do DSpace e extraia-os numa pasta no diretório home da máquina virtual.

##3º Passo - Alterar os parâmetros de instalação

Altere e salve os arquivos abaixo, localizados na pasta que você extraiu no passo acima, com os parâmetros que você deseja.

No meu caso eu utilizei como servidor de e-mail o Gmail, portanto caso você utilize uma conta do Gmail também basta alterar o endereço de e-mail e a senha com os dados da sua conta.

###build-dspace (altere para a versão do DSpace que se deseja instalar)

~~~
VERSION_DSPACE="4.1"
~~~

###build.properties

~~~
dspace.name = DSpace
default.language = pt_BR
~~~

###dspace.cfg

~~~
dspace.name = DSpace
default.language = pt_BR
mail.server=smtp.gmail.com
mail.server.username = treinamento.dspace@gmail.com
mail.server.password = yourPassword
mail.extraproperties = mail.smtp.socketFactory.port=465, \
mail.smtp.socketFactory.class=javax.net.ssl.SSLSocketFactory, \
mail.smtp.socketFactory.fallback=false
mail.from.address = treinamento.dspace@gmail.com
feedback.recipient = treinamento.dspace@gmail.com
mail.admin = treinamento.dspace@gmail.com
alert.recipient = treinamento.dspace@gmail.com
registration.notify = treinamento.dspace@gmail.com
~~~

##4º Passo - Iniciar o processo de instalação

Abra o terminal, vá até a pasta onde os arquivos estão localizados e execute o script de instalação:

~~~
$ ./build-dspace
~~~

Algumas vezes o script solicitará a senha do root ou alguma confirmação, basta responder a solicitação que o script continua sem problemas.

Esse processo baixa todas as depêndencias necessárias, instala e compila o DSpace, portanto é um processo um pouco demorado, dependendo da velocidade da sua conexão de internet.

Caso tenha ocorrido tudo bem, será exibida a mensagem ***Build completed***.

Pronto, o DSpace está instalado, basta acessar através do endereço: <http://localhost:8080/xmlui> para a interface XMLUI e <http://localhost:8080/jspui> para a interface JSPUI.

O usuário administrador criado foi o dspace com a senha dspace.

###Senhas da aplicação

**DSpace admin**
Usuário: dspace
Senha: dspace

**PostgreSQL**
Usuário: postgres
Senha: dspace

Mais informações sobre a instalação podem ser encontradas na [documentação oficial do DSpace](https://wiki.duraspace.org/display/DSDOC4x/Installing+DSpace).
