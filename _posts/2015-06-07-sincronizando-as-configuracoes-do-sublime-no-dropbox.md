<p>{
"title" : "Sincronizando as configurações do Sublime no Dropbox",
"author":"Royopa",
"date":"07-06-2015",
"tag":"coding, dropbox, plugins, preferences, python, settings, sublime text 3, sync",
"slug" : "sincronizando-as-configuracoes-do-sublime-no-dropbox",
"category":"sublime"
}</p>

<p>Hoje eu reinstalei o meu xUbuntu do notebook e sei que dá o maior trabalho para reinstalar o Sublime Text com todas as configurações e pacotes que eu tinha. Diante disso estava pesquisando uma maneira fácil de manter essas configurações sincronizadas para todas as máquinas que utilizo.</p>

<p>Espero que seja útil! Tradução do artigo <a href="http://www.alexconrad.org/2013/07/sync-sublime-text-3-settings-with.html">Sync Sublime Text 3 settings with Dropbox</a> do <a href="https://twitter.com/alexconrad">Alex Conrad</a>.</p>

<p>Recentemente eu atualizei para o Subliem Text 3 (ST3)</p>

<p>I recently upgraded to Sublime Text 3 (ST3) e achei que era uma boa hora para descobrir como sincronizar as minhas preferências do ST3 entre os computadores do meu trabalho e de casa. Especialmente por que o ST3 usa o Python 3: nem todos os plugins disponíveis no Sublime Package Control funcionam no Python 3, então frequentemente eu tenho que fazer a instalação manual, o que é muito chato se você tem que fazer em todos os outros computadores que rodam o Sublime Text 3.</p>

<h2 id="algumas-das-minhas-configura%C3%A7%C3%B5es">Algumas das minhas configurações</h2>

<p>Acontece que, a sincronização do ST3 é muito simples se você usar um qualquer tipo de hospedagem de arquivos e serviços de sincronização como o Dropbox.</p>

<p>Existem discussões de como fazer isso no Sublime Text 2 mas não muito com a versão 3. Dito isto, os passos são quase idênticos e você certamente descobriria sozinho. Mas hey, você não terá que fazer isso!</p>

<h2 id="sincronizando-o-sublime">Sincronizando o Sublime</h2>

<p>Os passos a seguir devem ser feitas no computador que tem as suas configurações ST3 mais atualizadas (aquelas que você deseja propagar para outros computadores).</p>

<p>Antes de fazer qualquer coisa, feche o Sublime Text 3.</p>

<pre><code class="shell">#  Cria a pasta de sincronização no Dropbox
$ mkdir ~/Dropbox/sublime-text-3/

# Move os seus "Packages" e "Installed Packages" do ST3 para o Dropbox
$ cd ~/.config/sublime-text-3/
$ mv Packages/ ~/Dropbox/sublime-text-3/
$ mv Installed\ Packages/ ~/Dropbox/sublime-text-3/

# Então cria um link simbólico dos diretórios anteriores
$ ln -s ~/Dropbox/sublime-text-3/Packages/
$ ln -s ~/Dropbox/sublime-text-3/Installed\ Packages/
</code></pre>

<p>Note que o ST3 tem três outros diretórios dentro da pasta ~/.config.sublime-text-3/: Cache, Index e Local. Não os sincronize, pois cada computador deve gerenciar suas próprias cópias locais.</p>

<h2 id="sincronizando-outros-computadores">Sincronizando outros computadores</h2>

<p>Todos os outros computadores terão agora ~/Dropbox/sublime-text-3/. Siga estes passos em cada um dos computadores que você tem o ST3 instalado para sincronizá-los:</p>

<pre><code class="shell"># Remove os diretórios "desatualizados"
$ cd ~/.config/sublime-text-3/
$ rm -rf Packages/
$ rm -rf Installed\ Packages/

# Então cria links simbólicos dos diretórios anteriores para o Dropbox
$ ln -s ~/Dropbox/sublime-text-3/Packages/
$ ln -s ~/Dropbox/sublime-text-3/Installed\ Packages/
</code></pre>

<p>Agora quando você alterar as configurações do seu Sublime Text 3, elas serão propagadas automaticamente. Mesmo novos pacotes serão instalados em todos os lugares! Você sempre se sentirá em casa, onde quer que esteja!</p>
