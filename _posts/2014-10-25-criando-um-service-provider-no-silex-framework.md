{
"title" : "Criando um Service Provider no Silex Framework",
"author":"Royopa",
"date":"25-10-2014",
"tag":"php, silex, validator, respect, service, provider",
"slug" : "criando-um-service-provider-no-silex-framework",
"category":"PHP"
}

Comecei a utilizar há pouco tempo o microframework Silex para desenvolver
algumas aplicações mais simples.
Lendo o artigo [Silex + Respect\Relational – Uma boa dupla][1], do [Willian
Carminato][9], resolvi fazer um parecido usando o Respect Validation.

Respect Validation
------------------

O [Respect Validation][2] é um componente criado pelo [Alexandre Gaigalas (alganet)][8]
para validar vários tipos de informação. Veja nesse [artigo][3] do Gaigalas em português
sobre o Respect Validation.

Iniciando a criação do Provider
-------------------------------

O primeiro passo é criar uma classe que implementa a interface
Silex\ServiceProviderInterface com seus métodos boot() e register().

```php
use Silex\Application;
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
```

Incluindo as dependências necessárias
-------------------------------------

Para que a aplicação rode precisaremos incluir a dependência para o Respect/Validation.
Para isso criamos um arquivo composer.json na pasta raiz do nosso Service Provider
contendo essa dependência:

```json
    "require": {
        "respect/validation": "~0.6"
    },
```

Implementação do método register()
----------------------------------

Agora vamos implementar o serviço 'respect.validator' no método register() da
classe.

Em nosso caso não é necessário nenhum argumento na Closure, basta a
criação e retorno do objeto Validator do Respect.

```php
    public function register(Application $app)
    {
        $app['respect.validator'] = $app->share(function ($app) {
            return new Validator();
        });
    }
```

Registrando e utilizando o Provider
-----------------------------------

Após a implementação do serviço, basta registrar o Provider criado para utilizá-lo
em sua aplicação.

```php
    $app->register(new RespectValidationServiceProvider());
```

Após o registro você poderá utilizá-lo em qualquer parte de sua página através da
variável $app['respect.validator']. Vejamos alguns exemplos de utilização:

```php
    $app['respect.validator']::countryCode()->validate('BR'); //true

    $app['respect.validator']::numeric()->validate(123); //true

    $app['respect.validator']::not(
        $app['respect.validator']::int()
    )->validate(10); //false, input must not be integer
```

Conclusão
---------

Como podemos ver, o processo de criação de Providers é bastante simples.

Existem alguns outros Providers já incluídos no Silex por padrão, que podem ser
encontrados [aqui][5] e alguns outros de terceiros podem ser localizados [nessa página][6].

Criei um [repositório no GitHub][7] para os arquivos desse projeto, caso você queira testá-lo
ou utilizá-lo.

Mais informações podem ser obtidas na [documentação do Silex][4].

[1]: http://phpsp.org.br/silex-respectrelational-uma-boa-dupla/
[2]: http://documentup.com/Respect/Validation/
[3]: http://imasters.com.br/linguagens/php/respect-um-microframework-de-respeito/
[4]: http://silex.sensiolabs.org/doc/providers.html
[5]: http://silex.sensiolabs.org/doc/providers.html#included-providers
[6]: https://github.com/silexphp/Silex/wiki/Third-Party-ServiceProviders
[7]: https://github.com/royopa/respect-validation-service-provider
[8]: http://gaigalas.net/#home
[9]: https://github.com/williancarminato/
