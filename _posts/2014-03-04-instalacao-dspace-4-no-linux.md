<p>{
"title" : "Instalação DSpace 4 no Linux",
"author":"Royopa",
"date":"04-03-2014",
"tag":"DSpace, DSpace install",
"slug" : "instalacao-dspace-4-no-linux",
"category":"DSpace"
}</p>

<p>Este é a replicação com algumas atualizações de um artigo que publiquei no meu blog em <a href="http://royopa.wordpress.com/2014/03/04/instalacao-do-dspace-4-1-em-sistemas-ubuntu-like/">Wordpress</a>.</p>

<p>Após o lançamento da versão 4.1 do DSpace em 03/03/2014, resolvi criar uma máquina virtual para testar a estabilidade e novas funcionalidades da versão.</p>

<p>Devido ao processo de instalação ser um pouco <del>difícil</del> chato, com diversas configurações para se fazer e tudo mais (como já havia publicado no <a href="http://www.slideshare.net/royopa1/instalao-dspace-3x-windows-e-linux">Slideshare</a> o guia para a instalação da versão 3, resolvi criar um script para instalação de forma quase que automática do DSpace.</p>

<p>Para esse processo utilizei como base o artigo DSpace LiveCD da <a href="https://wiki.duraspace.org/display/DSPACE/LiveCD">Wiki do DSpace</a>.</p>

<p>Meu projeto está no <a href="https://github.com/royopa/dspace-auto-install/">GitHub</a>, então quem tiver interesse de melhorá-lo, adequá-lo conforme as suas necessidades fiquem à vontade!.</p>

<h2 id="1%C2%BA-passo---baixar-uma-imagem-do-ubuntu">1º Passo - Baixar uma imagem do Ubuntu</h2>

<p>Para evitar o processo de instalação do Linux foi baixada uma imagem do <a href="http://downloads.sourceforge.net/virtualboximage/xubuntu_1204.7z">XUbuntu</a>, localizada no site <a href="http://virtualboxes.org/images/">Virtual Box Images</a>, conforme abaixo:</p>

<pre><code>Xubuntu 12.04 codename Precise Pangolin
Size (compressed/uncompressed): 502.2 MBytes / 2.6 GBytes
Active user account(s) (username/password): xubuntu/reverse
</code></pre>

<p>Outra alternativa é usar uma versão de Linux bem pequena, chamada Puppy Linux, através da <a href="http://sourceforge.net/projects/virtualboximage/files/Puppy%20Linux/5.2.0/LucidPuppy-520.7z/download">imagem</a> baixada no mesmo site.</p>

<pre><code>Puppy Linux 5.2.0 Lucid
Size (compressed/uncompressed): 167.0 MBytes/ 655 MBytes
Link:LucidPuppy-520.7z
</code></pre>

<p>Após ter a máquina virtual baixada e funcionando siga para os próximos passos. Esse artigo não tem a intenção de ensinar como criar uma máquina virtual, mas procurando no Google você verá que o processo é bem fácil.</p>

<h2 id="2%C2%BA-passo---baixar-os-arquivos%2Fscripts-para-instala%C3%A7%C3%A3o-do-dspace">2º Passo - Baixar os arquivos/scripts para instalação do DSpace</h2>

<p>Baixe os <a href="https://github.com/royopa/dspace-auto-install/archive/master.zip">arquivos do GitHub</a> para a instalação do DSpace e extraia-os numa pasta no diretório home da máquina virtual.</p>

<h2 id="3%C2%BA-passo---alterar-os-par%C3%A2metros-de-instala%C3%A7%C3%A3o">3º Passo - Alterar os parâmetros de instalação</h2>

<p>Altere e salve os arquivos abaixo, localizados na pasta que você extraiu no passo acima, com os parâmetros que você deseja.</p>

<p>No meu caso eu utilizei como servidor de e-mail o Gmail, portanto caso você utilize uma conta do Gmail também basta alterar o endereço de e-mail e a senha com os dados da sua conta.</p>

<h3 id="build-dspace-altere-para-a-vers%C3%A3o-do-dspace-que-se-deseja-instalar">build-dspace (altere para a versão do DSpace que se deseja instalar)</h3>

<pre><code>VERSION_DSPACE="4.1"
</code></pre>

<h3 id="build.properties">build.properties</h3>

<pre><code>dspace.name = DSpace
default.language = pt_BR
</code></pre>

<h3 id="dspace.cfg">dspace.cfg</h3>

<pre><code>dspace.name = DSpace
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
</code></pre>

<h2 id="4%C2%BA-passo---iniciar-o-processo-de-instala%C3%A7%C3%A3o">4º Passo - Iniciar o processo de instalação</h2>

<p>Abra o terminal, vá até a pasta onde os arquivos estão localizados e execute o script de instalação:</p>

<pre><code>$ ./build-dspace
</code></pre>

<p>Algumas vezes o script solicitará a senha do root ou alguma confirmação, basta responder a solicitação que o script continua sem problemas.</p>

<p>Esse processo baixa todas as depêndencias necessárias, instala e compila o DSpace, portanto é um processo um pouco demorado, dependendo da velocidade da sua conexão de internet.</p>

<p>Caso tenha ocorrido tudo bem, será exibida a mensagem <strong><em>Build completed</em></strong>.</p>

<p>Pronto, o DSpace está instalado, basta acessar através do endereço: <a href="http://localhost:8080/xmlui">http://localhost:8080/xmlui</a> para a interface XMLUI e <a href="http://localhost:8080/jspui">http://localhost:8080/jspui</a> para a interface JSPUI.</p>

<p>O usuário administrador criado foi o dspace com a senha dspace.</p>

<h3 id="senhas-da-aplica%C3%A7%C3%A3o">Senhas da aplicação</h3>

<p><strong>DSpace admin</strong>
Usuário: dspace
Senha: dspace</p>

<p><strong>PostgreSQL</strong>
Usuário: postgres
Senha: dspace</p>

<p>Mais informações sobre a instalação podem ser encontradas na <a href="https://wiki.duraspace.org/display/DSDOC4x/Installing+DSpace">documentação oficial do DSpace</a>.</p>
