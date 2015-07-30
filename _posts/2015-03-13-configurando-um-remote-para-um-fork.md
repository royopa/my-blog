<p>{
"title" : "Configurando um remote para um fork",
"author":"Royopa",
"date":"13-03-2015",
"tag":"git",
"slug" : "configurando-um-remote-para-um-fork",
"category":"GIT"
}</p>

<p>Para <a href="http://royopa.url.ph/2015/03/13/sincronizando-um-fork-no-git">sincronizar alterações que você faz em um fork</a> com o repositório original, você deve configurar um remote que aponte para o repositório upstream no Git.</p>

<p>O texto foi uma tradução do artigo <a href="https://help.github.com/articles/configuring-a-remote-for-a-fork/">Configuring a remote for a fork</a> do <a href="https://github.com">GitHub</a>.</p>

<p>1 - Abra o terminal.</p>

<p>2 - Mude para o diretório do seu projeto local.</p>

<p>3 - Liste o remote atual configurado para seu fork.</p>

<pre><code>$ git remote -v
# origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
# origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
</code></pre>

<p>4- Especifique um novo remote para o repositório upstream  que será sincronizado com o fork.</p>

<pre><code>$ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
</code></pre>

<p>5 - Verifique o novo repositório upstream que você especificou para seu fork.</p>

<pre><code>$ git remote -v
# origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
# origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
# upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
# upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
</code></pre>
