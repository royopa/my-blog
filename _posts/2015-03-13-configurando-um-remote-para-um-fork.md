{
"title" : "Configurando um remote para um fork",
"author":"Royopa",
"date":"13-03-2015",
"tag":"git",
"slug" : "configurando-um-remote-para-um-fork",
"category":"GIT"
}

Para [sincronizar alterações que você faz em um fork][3] com o repositório original, você deve configurar um remote que aponte para o repositório upstream no Git.

O texto foi uma tradução do artigo [Configuring a remote for a fork][1] do [GitHub][2].

1 - Abra o terminal.

2 - Mude para o diretório do seu projeto local.

3 - Liste o remote atual configurado para seu fork.

    $ git remote -v
    # origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
    # origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)

4- Especifique um novo remote para o repositório upstream  que será sincronizado com o fork.

    $ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
    
5 - Verifique o novo repositório upstream que você especificou para seu fork.

    $ git remote -v
    # origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
    # origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
    # upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
    # upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)

[1]: https://help.github.com/articles/configuring-a-remote-for-a-fork/
[2]: https://github.com
[3]: http://royopa.url.ph/2015/03/13/sincronizando-um-fork-no-git