{
"title" : "Mudando a origin do seu repositório GIT",
"author":"Royopa",
"date":"09-01-2014",
"tag":"git, vcs",
"slug" : "mudando-a-origin-do-seu-repositorio-git",
"category":"GIT"
}

Tradução do artigo [Change "origin" of your GIT repository][1] do blog do Dariusz Cieslak.

Na [documentação oficial do GitHub][2] também tem essa informação de forma mais detalhada.

Mudando a "origin" do seu repositório GIT
-----------------------------------------

O GIT é um sistema de controle de versão distribuído - o que significa que ele não precisa ter um repositório central. É possível construir um sistema através da troca de commits entre nós iguais.
É conveniente, no entanto, marcar um repositório como o central. É claro que você pode mudar a sua decisão a qualquer momento. Eu vou te mostrar como fazer isso.

Se você criou um sua cópia do repo com a operação "clone" você terá o branch "origin" definido. Esse remote pode ser usado para fazer mudanças pull/push.

    $ git remote -v

    origin  https://github.com/royopa/api-rosetta-code (fetch)
    origin  https://github.com/royopa/api-rosetta-code (push)

Se você decidir alterar esta definição mais tarde você pode rodar os seguintes comandos:

    $ git remote rm origin
    $ git remote add origin https://github.com/twitterdev/api-rosetta-code
    $ git config master.remote origin
    $ git config master.merge refs/heads/master

Depois de rodar os comandos acima, verifique se a origin foi alterada, com o comando abaixo:
    
    $ git remote -v

    origin  https://github.com/twitterdev/api-rosetta-code (fetch)
    origin  https://github.com/twitterdev/api-rosetta-code (push)


Depois dessa mudança você poderá fazer push de seus commits para o novo local do repositório (o origin é selecionado como o branch remote padrão para o master e é configurado em .git/config):

    $ git push

Isso é tudo. Muito mais simples do que mover um repositório do Subversion.

[1]: http://blog.aplikacja.info/2010/08/switch-origin-of-your-git-repository/
[2]: https://help.github.com/articles/changing-a-remote-s-url/