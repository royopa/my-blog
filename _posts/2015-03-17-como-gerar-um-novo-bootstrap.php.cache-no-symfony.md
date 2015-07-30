<p>{
"title":"Como gerar um novo 'bootstrap.php.cache' no Symfony",
"author":"Royopa",
"date":"17-03-2015",
"tag":"php",
"slug":"como-gerar-um-novo-bootstrap-php-cache-no-symfony",
"category":"PHP"
}</p>

<p>É muito comum num projeto ter diversos componentes de terceiros em uma aplicação (vendors). Veja na pasta vendor da sua aplicação a quantidade de arquivos que o seu projeto possui, imagine então ter que carregar um por um para utilizá-los!</p>

<p>O Symfony possui um mecanismo para tentar diminuir a utilização do disco ao ter que carregar cada uma das classes de terceiros em arquivos separados. O framework cria um arquivo de inicialização que contém várias classes juntas em um único arquivo chamado de <strong><em>bootstrap.php.cache</em></strong>.</p>

<p>O arquivo bootstrap.php.cache é recriado toda vez que você utilizar o comando <strong><em>composer install</em></strong>. Poŕem quando você utiliza o comando <strong><em>composer update</em></strong> o bootstrap.php.cache não é regerado. Veja abaixo como gerar um novo arquivo bootstrap.php.cache.</p>

<h2 id="gerando-um-novo-%22bootstrap.php.cache%22-no-symfony-%2B2.1">Gerando um novo "bootstrap.php.cache" no Symfony +2.1</h2>

<p>Para gerar um novo arquivo bootstrap.php.cache no Symfony2 (versão +2.1), basta executar o seguinte comando:</p>

<pre><code>$ cd [your_app_dir]
$ php ./vendor/sensio/distribution-bundle/Sensio/Bundle/DistributionBundle/Resources/bin/build_bootstrap.php
</code></pre>

<p>Mais informação sobre o arquivo de cache pode ser encontrada na <a href="(http://symfony.com/doc/current/book/performance.html#use-bootstrap-files)">Documentação Oficial do Symfony</a></p>
