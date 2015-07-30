<p>{
"title" : "Sincronizando um fork no git",
"author":"Royopa",
"date":"13-03-2015",
"tag":"git",
"slug" : "sincronizando-um-fork-no-git",
"category":"GIT"
}</p>

<p>Para quem trabalha com projetos open source é comum acontecer de após fazer um pull request precisarmos ter nosso fork sincronizado com o repositório upstream para permitir fazer novos PRs com código o atualizado.</p>

<p>Veremos abaixo como sincronizar um fork para manter seu código atualizado com o repositório upstream. O texto abaixo foi uma tradução do arquivo <a href="https://help.github.com/articles/syncing-a-fork/">Syncing a fork</a> do <a href="https://github.com">GitHub</a>.</p>

<p><strong><em>Dica:</em></strong> Antes de você sincronizar o seu fork com o repositório upstream, você deve <a href="https://help.github.com/articles/configurando-um-remote-para-um-fork/">configurar o remote para que aponte para o repositório upstream</a> no Git.</p>

<p>1 - Abra o terminal.</p>

<p>2 - Mude para o diretório do seu projeto local.</p>

<p>3 - Busque os branches e seus respectivos commits do repositório upstream. Os commits para o branch master serão armazenados em um branch local chamado upstream/master.</p>

<pre><code>$ git fetch upstream
# remote: Counting objects: 75, done.
# remote: Compressing objects: 100% (53/53), done.
# remote: Total 62 (delta 27), reused 44 (delta 9)
# Unpacking objects: 100% (62/62), done.
# From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
#  * [new branch]      master     -&gt; upstream/master
</code></pre>

<p>4- Faça checkout do branch <strong><em>master</em></strong> do seu fork local</p>

<pre><code>$ git checkout master
# Switched to branch 'master'
</code></pre>

<p>5 - Faça um merge das mudanças do upstream/master no seu branch master local. Isso sincroniza o seu branch master sem perder suas alterações locais.</p>

<pre><code>$ git merge upstream/master
# Updating a422352..5fdff0f
# Fast-forward
#  README                    |    9 -------
#  README.md                 |    7 ++++++
#  2 files changed, 7 insertions(+), 9 deletions(-)
#  delete mode 100644 README
#  create mode 100644 README.md
</code></pre>

<p>Se seu branch local não tiver nenhum commit único, o Git em vez disso fará um "fast-forward":</p>

<pre><code>$ git merge upstream/master
# Updating 34e91da..16c56ad
# Fast-forward
#  README.md                 |    5 +++--
#  1 file changed, 3 insertions(+), 2 deletions(-)
</code></pre>

<p><strong><em>Dica:</em></strong> Sincronizar o seu fork somente atualiza a sua cópia local do repositório. Para atualizar seu fork no GitHub, você precisa fazer um push das suas alterações.</p>

<p>Mais informações podem ser obtidas no <a href="https://help.github.com/articles/syncing-a-fork/">documentação do Silex</a>.</p>
