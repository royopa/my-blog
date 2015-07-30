<p>{
"title" : "Mudando a origin do seu repositório GIT",
"author":"Royopa",
"date":"09-01-2014",
"tag":"git, vcs",
"slug" : "mudando-a-origin-do-seu-repositorio-git",
"category":"GIT"
}</p>

<p>Tradução do artigo <a href="http://blog.aplikacja.info/2010/08/switch-origin-of-your-git-repository/">Change "origin" of your GIT repository</a> do blog do Dariusz Cieslak.</p>

<p>Na <a href="https://help.github.com/articles/changing-a-remote-s-url/">documentação oficial do GitHub</a> também tem essa informação de forma mais detalhada.</p>

<h2 id="mudando-a-%22origin%22-do-seu-reposit%C3%B3rio-git">Mudando a "origin" do seu repositório GIT</h2>

<p>O GIT é um sistema de controle de versão distribuído - o que significa que ele não precisa ter um repositório central. É possível construir um sistema através da troca de commits entre nós iguais.
É conveniente, no entanto, marcar um repositório como o central. É claro que você pode mudar a sua decisão a qualquer momento. Eu vou te mostrar como fazer isso.</p>

<p>Se você criou um sua cópia do repo com a operação "clone" você terá o branch "origin" definido. Esse remote pode ser usado para fazer mudanças pull/push.</p>

<pre><code>$ git remote -v

origin  https://github.com/royopa/api-rosetta-code (fetch)
origin  https://github.com/royopa/api-rosetta-code (push)
</code></pre>

<p>Se você decidir alterar esta definição mais tarde você pode rodar os seguintes comandos:</p>

<pre><code>$ git remote rm origin
$ git remote add origin https://github.com/twitterdev/api-rosetta-code
$ git config master.remote origin
$ git config master.merge refs/heads/master
</code></pre>

<p>Depois de rodar os comandos acima, verifique se a origin foi alterada, com o comando abaixo:</p>

<pre><code>$ git remote -v

origin  https://github.com/twitterdev/api-rosetta-code (fetch)
origin  https://github.com/twitterdev/api-rosetta-code (push)
</code></pre>

<p>Depois dessa mudança você poderá fazer push de seus commits para o novo local do repositório (o origin é selecionado como o branch remote padrão para o master e é configurado em .git/config):</p>

<pre><code>$ git push
</code></pre>

<p>Isso é tudo. Muito mais simples do que mover um repositório do Subversion.</p>
