{
"title" : "S.O.L.I.D - Os 5 princípios do design orientado a objeto",
"author":"Royopa",
"date":"12-04-2015",
"tag":"solid",
"slug" : "solid-os-primeiros-5-principios-do-design-orientado-a-objeto",
"category":"Solid"
}

Tradução do artigo [S.O.L.I.D: The First 5 Principles of Object Oriented Design, de Samuel Oloruntoba](https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)


S.O.L.I.D é um acrônimo para os primeiros cinco princípios de design orientado a objetos (OOD) criado por Robert C. Martin, popularmente conhecido como [Uncle Bob](http://en.wikipedia.org/wiki/Robert_Cecil_Martin).
Estes princípios quando combinados, facilita para um programador desenvolver um software que seja fácil de se manter e estender. Eles também facilitam que desenvolvedores evitem códigos malcheirosos, facilita na refatoração do código e são também uma parte do desenvolvimento ágil ou desenvolvimento adaptativo de software.

Nota: Esse é um simples artigo que somente diz o que é S.O.L.I.D.

S.O.L.I.D significa:

Quando o acrônimo é expandido as siglas podem parecer complicadas, mas elas são bem simples de entender.

S – Single-responsiblity principle
O – Open-closed principle
L – Liskov substitution principle
I – Interface segregation principle
D – Dependency Inversion Principle
Vamos olhar cada princípio individualmente para entender porque S.O.L.I.D pode ajudar a tornar os desenvolvedores melhores.

Single-responsibility Principle ou Princípio da Responsabilidade Única
----------------------------------------------------------------------

Abreviado como S.R.P – esse princípio estabelece que:

Uma classe deve ter uma e apenas uma razão para mudar, o que significa que uma classe deve ter uma única responsabilidade.
Por exemplo, digamos que temos algumas formas e gostaríamos de somar todas as áreas dessas formas. Bem, isso é bastante simples, certo?

```php
class Circle {
    public $radius;

    public function __construct($radius) {
        $this->radius = $radius;
    }
}

class Square {
    public $length;

    public function __construct($length) {
        $this->length = $length;
    }
}
```
Primeiro, nós criamos nossas classes de formas e definimos os construtores com os parâmetros obrigatórios. Em seguida, criaremos a classe AreaCalculator e escreveremos nossa lógica para somar as áreas de todas as formas previstas.

```php
class AreaCalculator {

    protected $shapes;

    public function __construct($shapes = array()) {
        $this->shapes = $shapes;
    }

    public function sum() {
        // logic to sum the areas
    }

    public function output() {
        return implode('', array(
            "<h1>",
                "Sum of the areas of provided shapes: ",
                $this->sum(),
            "</h1>"
        ));
    }
}
```
Para usar a classe AreaCalculator, nós simplesmente instanciamos a classe e passamos um array de formas, e exibimos a saída no fim da página.

```php
$shapes = array(
    new Circle(2),
    new Square(5),
    new Square(6)
);

$areas = new AreaCalculator($shapes);

echo $areas->output();
```
O problema com o método de saída é que a classe AreaCalculator lida com lógica da saída de dados. Então, o que fazer se o usuário quiser que a saída de dados seja um json ou outro tipo?

Toda a lógica precisaria ser tratada pela classe AreaCalculator e é isso que vai contra o princípio SRP; a classe AreaCalculator só deve somar as áreas de formas previstas, não deve se importar se o usuário deseja json ou HTML.

Assim, para corrigir isso, você pode criar uma classe SumCalculatorOutputter e usá-la para manipular qualquer lógica que você precisa para lidar com a exibição da soma das formas previstas.

A classe SumCalculatorOutputter funcionaria assim:

```php
$shapes = array(
    new Circle(2),
    new Square(5),
    new Square(6)
);

$areas = new AreaCalculator($shapes);
$output = new SumCalculatorOutputter($areas);

echo $output->JSON();
echo $output->HAML();
echo $output->HTML();
echo $output->JADE();
```
Agora, qualquer lógica que você precisa para a saída dos dados para o usuário é tratada pela classe SumCalculatorOutputter.

Open-closed Principle ou Princípio Aberto-fechado
-------------------------------------------------

Objetos ou entidades devem ser abertos para extensão, mas fechados para modificação.
Isto significa resumidamente que uma classe deve ser facilmente extensível sem modificar a própria classe. Vamos dar uma olhada na classe AreaCalculator, especialmente o método soma.
```php
public function sum() {
    foreach($this->shapes as $shape) {
        if(is_a($shape, 'Square')) {
            $area[] = pow($shape->length, 2);
        } else if(is_a($shape, 'Circle')) {
            $area[] = pi() * pow($shape->radius, 2);
        }
    }

    return array_sum($area);
}
```
Se nós quiséssemos que o método sum seja capaz de somar as áreas de mais formas, nós teríamos que adicionar mais blocos if/else, o que vai contra o princípio Open-closed.

Uma maneira de melhorar o método soma é remover a lógica para calcular a área de cada forma fora do método sum e anexá-la para a sua classe forma.
```php
class Square {
    public $length;

    public function __construct($length) {
        $this->length = $length;
    }

    public function area() {
        return pow($this->length, 2);
    }
}
```
A mesma coisa deve ser feita para a classe Circle, um método área deve ser adicionado. Agora, para calcular a soma de qualquer forma deve ser tão simples como:

```php
public function sum() {
    foreach($this->shapes as $shape) {
        $area[] = $shape->area;
    }

    return array_sum($area);
}
```
Agora nós podemos criar outra classe forma e passar a maneira de calcular a soma sem quebrar nosso código. NO entanto, agora surge um novo problema, como sabemos que o objeto passado para a classe AreaCalculator é uma forma ou se a forma tem um método com o nome area?

Programar para uma interface é uma parte integrante do S.O.L.I.D., um exemplo rápido é que criamos uma interface que implementa todas as formas:

```php
interface ShapeInterface {
    public function area();
}

class Circle implements ShapeInterface {
    public $radius;

    public function __construct($radius) {
        $this->radius = $radius;
    }

    public function area() {
        return pi() * pow($this->radius, 2);
    }
}
```
Em nosso método sum da classe AreaCalculator nós podemos checar se as formas previstas são realmente instâncias da interface Shape, caso contrário será lançada uma exceção:

```php
public function sum() {
    foreach($this->shapes as $shape) {
        if(is_a($shape, 'ShapeInterface')) {
            $area[] = $shape->area();
            continue;
        }

        throw new AreaCalculatorInvalidShapeException;
    }

    return array_sum($area);
}
```

Liskov substitution principle ou Princípio da substituição de Liskov
--------------------------------------------------------------------

Considere que q(x) seja uma propriedade demonstrável dos objetos de x de tipo T. Então q(y) deve ser verdadeiro para objetos y do tipo S onde S é um subtipo de T.
Tudo isso está afirmando que cada suclasse/classe derivada devem ser substituíveis por sua classe base;
Ainda fazendo o uso da classe AreaCalculator, digamos que temos uma classe VolumeCalculator que estende a classe AreaCalculator:
```php
class VolumeCalculator extends AreaCalulator
{
    public function __construct($shapes = array())
    {
        parent::__construct($shapes);
    }

    public function sum()
    {
        // logic to calculate the volumes and then return and array of output
        return array($summedData);
    }
}
```
Na classe SumCalculatorOutputter:

```php
class SumCalculatorOutputter
{
    protected $calculator;

    public function __constructor(AreaCalculator $calculator)
    {
        $this->calculator = $calculator;
    }

    public function JSON()
    {
        $data = array(
            'sum' => $this->calculator->sum();
        );

        return json_encode($data);
    }

    public function HTML()
    {
        return implode('', array(
            '<h1>',
                'Sum of the areas of provided shapes: ',
                $this->calculator->sum(),
            '</h1>'
        ));
    }
}
```
Se tentássemos rodar um exemplo como este:

```php
$areas = new AreaCalculator($shapes);
$volumes = new AreaCalculator($solidShapes);

$output = new SumCalculatorOutputter($areas);
$output2 = new SumCalculatorOutputter($volumes);
```
O programa não dará erro, mas quando chamarmos o método HTML no objeto $output2 nós teremos um erro E_NOTICE informando da conversão de um array em string.

Para corrigir isso, ao invés de retornar um array do método sum da classe VolumeCalculator, você deve simplesmente:

```php
public function sum()
{
    // lógica para calcular os volumes e retornar um array
    return $summedData;
}
```
Os dados são somados como um float, double ou integer.

Interface segregation principle ou Princípio da Segregação de Interface
-----------------------------------------------------------------------

Um cliente nunca deve ser forçado a implementar uma interface que ele não usa ou clientes não devem ser forçados a depender de métodos que eles não utilizam.
Ainda usando nosso exemplo de formas, nós sabemos que temos formas sólidas, uma vez que também gostaríamos de calcular o volume de uma forma, nós podemos adicionar um outro contrato na interface ShapeInterface:

```php
interface ShapeInterface
{
    public function area();
    public function volume();
}
```
Qualquer forma que criamos implementa o método voluma, mas nós sabemos que quadrados são formas planas e que eles não tem volumes, então esta interface forçaria a classe Square a implementar um método que ele não utiliza.

Esse princípio diz não para isso, em vez disso você poderia criar outra interface chamada SolidShapeInterface que tem o contrato de volume e formas sólidas como cubos que podem implementar essa interface:

```php
interface ShapeInterface
{
    public function area();
}

interface SolidShapeInterface
{
    public function volume();
}

class Cuboid implements ShapeInterface, SolidShapeInterface
{
    public function area()
    {
        // calcula a área de um cubo
    }

    public function volume()
    {
        // calcula o volume de um cubo
    }
}
```

Essa é uma abordagem muito melhor, mas uma armadilha para quem vê de fora, quando fizer o type-hint dessas interfaces, em vez de usar uma ShapeInterface ou uma SolidShapeInterface.

Você pode ciar uma outra interface, talvez ManageShapeInterface e implementá-la em ambas as formas planas e sólidas, desta forma você pode facilmente ver que ele tem uma única API para gerenciar as formas. Por exemplo:

```php
interface ManageShapeInterface
{
    public function calculate();
}

class Square implements ShapeInterface, ManageShapeInterface {
    public function area()
    {
        /*Do stuff here*/
    }

    public function calculate()
    {
        return $this->area();
    }
}

class Cuboid implements ShapeInterface, SolidShapeInterface, ManageShapeInterface
{
    public function area()
    {
        /*Do stuff here*/
    }

    public function volume()
    {
        /*Do stuff here*/
    }

    public function calculate()
    {
        return $this->area() + $this->volume();
    }
}
```
Now in AreaCalculator class, we can easily replace the call to the area method with calculate and also check if the object is an instance of the ManageShapeInterface and not the ShapeInterface.

Dependency Inversion principle ou Princípio da Inversão de Dependência
----------------------------------------------------------------------

O último, mas definitivamente não o menos importante afirma que:

Entidades devem depender de abstrações e não de classes concretas. Ele afirma que o módulo de alto nível não deve depender do módulo de baixo nível, mas eles devem depender de abstrações.
Isso pode parecer "inchado", mas é muito fácil de entender. Este princípio permite o desacoplamento, um exemplo parece ser a melhor maneira de explicar esse princípio:

```php
class PasswordReminder
{
    private $dbConnection;

    public function __construct(MySQLConnection $dbConnection)
    {
        $this->dbConnection = $dbConnection;
    }
}
```
Primeiro o MySQLConnection é o módulo de baixo nível, enquanto o PasswordRemnder é o de alto nível, mas de acordo com a definição D no S.O.L.I.D. este trecho acima viola este princípio pois a classe PasswordReminder está sendo forçada a depender da classe MySQLConnection.

Mais tarde, se você tivesse que mudar o banco de dados, você também teria que editar a classe PasswordReminder e, portanto viola o princípio Open-close.

A classe PasswordReminder não deve se preocupar com o banco de dados usado pela aplicação, para corrigir isso mais uma vez nós "programamos para uma interface", já que os módulos de alto e baixo nível devem depender de uma abstraão, podemos criar uma interface:
```php
interface DBConnectionInterface
{
    public function connect();
}
```php
A interface tem um método connect e a classe MySQLConnection implementa essa interface, também ao invés de fazer um type-hinting da classe MySQLConnection no construtor do PassWordReminder nós fizemos um type-hint da interface e não importa o tipo do banco de dados que o seu aplicativo usa, a classe PasswordReminder pode facilmente conectar ao banco de dados sem quaisquer problemas e o princícpio Open-Closed não é violado.
```php
class MySQLConnection implements DBConnectionInterface
{
    public function connect()
    {
        return "Database connection";
    }
}

class PasswordReminder
{
    private $dbConnection;

    public function __construct(DBConnectionInterface $dbConnection)
    {
        $this->dbConnection = $dbConnection;
    }
}
```
De acordo com o o pequeno trecho acima, agora você pode ver que tanto módulos de alto e baixo níveis dependem de abstração.

Conclusão
---------

Honestamente, S.O.L.I.D. pode parecer "irritante" no início, mas com o uso contínuo e adesão de suas diretrizes, torna-se parte de você e de seu código, que poderá ser facilmente estendido, modificado, testado e refatorado sem quaisquer problemas.
