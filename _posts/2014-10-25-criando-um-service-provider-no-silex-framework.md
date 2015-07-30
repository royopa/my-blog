<p>{
"title" : "Criando um Service Provider no Silex Framework",
"author":"Royopa",
"date":"25-10-2014",
"tag":"php, silex, validator, respect, service, provider",
"slug" : "criando-um-service-provider-no-silex-framework",
"category":"PHP"
}</p>

<p>Comecei a utilizar há pouco tempo o microframework Silex para desenvolver
algumas aplicações mais simples.
Lendo o artigo <a href="http://phpsp.org.br/silex-respectrelational-uma-boa-dupla/">Silex + Respect\Relational – Uma boa dupla</a>, do <a href="https://github.com/williancarminato/">Willian
Carminato</a>, resolvi fazer um parecido usando o Respect Validation.</p>

<h2 id="respect-validation">Respect Validation</h2>

<p>O <a href="http://documentup.com/Respect/Validation/">Respect Validation</a> é um componente criado pelo <a href="http://gaigalas.net/#home">Alexandre Gaigalas (alganet)</a>
para validar vários tipos de informação. Veja nesse <a href="http://imasters.com.br/linguagens/php/respect-um-microframework-de-respeito/">artigo</a> do Gaigalas em português
sobre o Respect Validation.</p>

<h2 id="iniciando-a-cria%C3%A7%C3%A3o-do-provider">Iniciando a criação do Provider</h2>

<p>O primeiro passo é criar uma classe que implementa a interface
Silex\ServiceProviderInterface com seus métodos boot() e register().</p>

<pre><code class="php">use Silex\Application;
use Silex\ServiceProviderInterface;
use Respect\Validation\Validator;

class RespectValidationServiceProvider implements ServiceProviderInterface
{
    public function register(Application $app)
    {
    }

    public function boot(Application $app)
    {
    }
}
</code></pre>

<h2 id="incluindo-as-depend%C3%AAncias-necess%C3%A1rias">Incluindo as dependências necessárias</h2>

<p>Para que a aplicação rode precisaremos incluir a dependência para o Respect/Validation.
Para isso criamos um arquivo composer.json na pasta raiz do nosso Service Provider
contendo essa dependência:</p>

<pre><code class="json">    "require": {
        "respect/validation": "~0.6"
    },
</code></pre>

<h2 id="implementa%C3%A7%C3%A3o-do-m%C3%A9todo-register">Implementação do método register()</h2>

<p>Agora vamos implementar o serviço 'respect.validator' no método register() da
classe.</p>

<p>Em nosso caso não é necessário nenhum argumento na Closure, basta a
criação e retorno do objeto Validator do Respect.</p>

<pre><code class="php">    public function register(Application $app)
    {
        $app['respect.validator'] = $app-&gt;share(function ($app) {
            return new Validator();
        });
    }
</code></pre>

<h2 id="registrando-e-utilizando-o-provider">Registrando e utilizando o Provider</h2>

<p>Após a implementação do serviço, basta registrar o Provider criado para utilizá-lo
em sua aplicação.</p>

<pre><code class="php">    $app-&gt;register(new RespectValidationServiceProvider());
</code></pre>

<p>Após o registro você poderá utilizá-lo em qualquer parte de sua página através da
variável $app['respect.validator']. Vejamos alguns exemplos de utilização:</p>

<pre><code class="php">    $app['respect.validator']::countryCode()-&gt;validate('BR'); //true

    $app['respect.validator']::numeric()-&gt;validate(123); //true

    $app['respect.validator']::not(
        $app['respect.validator']::int()
    )-&gt;validate(10); //false, input must not be integer
</code></pre>

<h2 id="conclus%C3%A3o">Conclusão</h2>

<p>Como podemos ver, o processo de criação de Providers é bastante simples.</p>

<p>Existem alguns outros Providers já incluídos no Silex por padrão, que podem ser
encontrados <a href="http://silex.sensiolabs.org/doc/providers.html#included-providers">aqui</a> e alguns outros de terceiros podem ser localizados <a href="https://github.com/silexphp/Silex/wiki/Third-Party-ServiceProviders">nessa página</a>.</p>

<p>Criei um <a href="https://github.com/royopa/respect-validation-service-provider">repositório no GitHub</a> para os arquivos desse projeto, caso você queira testá-lo
ou utilizá-lo.</p>

<p>Mais informações podem ser obtidas na <a href="http://silex.sensiolabs.org/doc/providers.html">documentação do Silex</a>.</p>
