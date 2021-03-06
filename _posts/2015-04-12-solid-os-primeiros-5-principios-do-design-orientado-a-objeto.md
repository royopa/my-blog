<p>{
"title" : "S.O.L.I.D - Os 5 princípios do design orientado a objeto",
"author":"Royopa",
"date":"12-04-2015",
"tag":"solid",
"slug" : "solid-os-primeiros-5-principios-do-design-orientado-a-objeto",
"category":"Solid"
}</p>

<p>Tradução do artigo <a href="https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design">S.O.L.I.D: The First 5 Principles of Object Oriented Design, de Samuel Oloruntoba</a></p>

<p>S.O.L.I.D é um acrônimo para os primeiros cinco princípios de design orientado a objetos (OOD) criado por Robert C. Martin, popularmente conhecido como <a href="http://en.wikipedia.org/wiki/Robert_Cecil_Martin">Uncle Bob</a>.
Estes princípios quando combinados, facilita para um programador desenvolver um software que seja fácil de se manter e estender. Eles também facilitam que desenvolvedores evitem códigos malcheirosos, facilita na refatoração do código e são também uma parte do desenvolvimento ágil ou desenvolvimento adaptativo de software.</p>

<p>Nota: Esse é um simples artigo que somente diz o que é S.O.L.I.D.</p>

<p>S.O.L.I.D significa:</p>

<p>Quando o acrônimo é expandido as siglas podem parecer complicadas, mas elas são bem simples de entender.</p>

<p>S – Single-responsiblity principle
O – Open-closed principle
L – Liskov substitution principle
I – Interface segregation principle
D – Dependency Inversion Principle
Vamos olhar cada princípio individualmente para entender porque S.O.L.I.D pode ajudar a tornar os desenvolvedores melhores.</p>

<h2 id="single-responsibility-principle-ou-princ%C3%ADpio-da-responsabilidade-%C3%9Anica">Single-responsibility Principle ou Princípio da Responsabilidade Única</h2>

<p>Abreviado como S.R.P – esse princípio estabelece que:</p>

<p>Uma classe deve ter uma e apenas uma razão para mudar, o que significa que uma classe deve ter uma única responsabilidade.
Por exemplo, digamos que temos algumas formas e gostaríamos de somar todas as áreas dessas formas. Bem, isso é bastante simples, certo?</p>

<pre><code class="php">class Circle {
    public $radius;

    public function __construct($radius) {
        $this-&gt;radius = $radius;
    }
}

class Square {
    public $length;

    public function __construct($length) {
        $this-&gt;length = $length;
    }
}
</code></pre>

<p>Primeiro, nós criamos nossas classes de formas e definimos os construtores com os parâmetros obrigatórios. Em seguida, criaremos a classe AreaCalculator e escreveremos nossa lógica para somar as áreas de todas as formas previstas.</p>

<pre><code class="php">class AreaCalculator {

    protected $shapes;

    public function __construct($shapes = array()) {
        $this-&gt;shapes = $shapes;
    }

    public function sum() {
        // logic to sum the areas
    }

    public function output() {
        return implode('', array(
            "&lt;h1&gt;",
                "Sum of the areas of provided shapes: ",
                $this-&gt;sum(),
            "&lt;/h1&gt;"
        ));
    }
}
</code></pre>

<p>Para usar a classe AreaCalculator, nós simplesmente instanciamos a classe e passamos um array de formas, e exibimos a saída no fim da página.</p>

<pre><code class="php">$shapes = array(
    new Circle(2),
    new Square(5),
    new Square(6)
);

$areas = new AreaCalculator($shapes);

echo $areas-&gt;output();
</code></pre>

<p>O problema com o método de saída é que a classe AreaCalculator lida com lógica da saída de dados. Então, o que fazer se o usuário quiser que a saída de dados seja um json ou outro tipo?</p>

<p>Toda a lógica precisaria ser tratada pela classe AreaCalculator e é isso que vai contra o princípio SRP; a classe AreaCalculator só deve somar as áreas de formas previstas, não deve se importar se o usuário deseja json ou HTML.</p>

<p>Assim, para corrigir isso, você pode criar uma classe SumCalculatorOutputter e usá-la para manipular qualquer lógica que você precisa para lidar com a exibição da soma das formas previstas.</p>

<p>A classe SumCalculatorOutputter funcionaria assim:</p>

<pre><code class="php">$shapes = array(
    new Circle(2),
    new Square(5),
    new Square(6)
);

$areas = new AreaCalculator($shapes);
$output = new SumCalculatorOutputter($areas);

echo $output-&gt;JSON();
echo $output-&gt;HAML();
echo $output-&gt;HTML();
echo $output-&gt;JADE();
</code></pre>

<p>Agora, qualquer lógica que você precisa para a saída dos dados para o usuário é tratada pela classe SumCalculatorOutputter.</p>

<h2 id="open-closed-principle-ou-princ%C3%ADpio-aberto-fechado">Open-closed Principle ou Princípio Aberto-fechado</h2>

<p>Objetos ou entidades devem ser abertos para extensão, mas fechados para modificação.
Isto significa resumidamente que uma classe deve ser facilmente extensível sem modificar a própria classe. Vamos dar uma olhada na classe AreaCalculator, especialmente o método soma.</p>

<pre><code class="php">public function sum() {
    foreach($this-&gt;shapes as $shape) {
        if(is_a($shape, 'Square')) {
            $area[] = pow($shape-&gt;length, 2);
        } else if(is_a($shape, 'Circle')) {
            $area[] = pi() * pow($shape-&gt;radius, 2);
        }
    }

    return array_sum($area);
}
</code></pre>

<p>Se nós quiséssemos que o método sum seja capaz de somar as áreas de mais formas, nós teríamos que adicionar mais blocos if/else, o que vai contra o princípio Open-closed.</p>

<p>Uma maneira de melhorar o método soma é remover a lógica para calcular a área de cada forma fora do método sum e anexá-la para a sua classe forma.</p>

<pre><code class="php">class Square {
    public $length;

    public function __construct($length) {
        $this-&gt;length = $length;
    }

    public function area() {
        return pow($this-&gt;length, 2);
    }
}
</code></pre>

<p>A mesma coisa deve ser feita para a classe Circle, um método área deve ser adicionado. Agora, para calcular a soma de qualquer forma deve ser tão simples como:</p>

<pre><code class="php">public function sum() {
    foreach($this-&gt;shapes as $shape) {
        $area[] = $shape-&gt;area;
    }

    return array_sum($area);
}
</code></pre>

<p>Agora nós podemos criar outra classe forma e passar a maneira de calcular a soma sem quebrar nosso código. NO entanto, agora surge um novo problema, como sabemos que o objeto passado para a classe AreaCalculator é uma forma ou se a forma tem um método com o nome area?</p>

<p>Programar para uma interface é uma parte integrante do S.O.L.I.D., um exemplo rápido é que criamos uma interface que implementa todas as formas:</p>

<pre><code class="php">interface ShapeInterface {
    public function area();
}

class Circle implements ShapeInterface {
    public $radius;

    public function __construct($radius) {
        $this-&gt;radius = $radius;
    }

    public function area() {
        return pi() * pow($this-&gt;radius, 2);
    }
}
</code></pre>

<p>Em nosso método sum da classe AreaCalculator nós podemos checar se as formas previstas são realmente instâncias da interface Shape, caso contrário será lançada uma exceção:</p>

<pre><code class="php">public function sum() {
    foreach($this-&gt;shapes as $shape) {
        if(is_a($shape, 'ShapeInterface')) {
            $area[] = $shape-&gt;area();
            continue;
        }

        throw new AreaCalculatorInvalidShapeException;
    }

    return array_sum($area);
}
</code></pre>

<h2 id="liskov-substitution-principle-ou-princ%C3%ADpio-da-substitui%C3%A7%C3%A3o-de-liskov">Liskov substitution principle ou Princípio da substituição de Liskov</h2>

<p>Considere que q(x) seja uma propriedade demonstrável dos objetos de x de tipo T. Então q(y) deve ser verdadeiro para objetos y do tipo S onde S é um subtipo de T.
Tudo isso está afirmando que cada suclasse/classe derivada devem ser substituíveis por sua classe base;
Ainda fazendo o uso da classe AreaCalculator, digamos que temos uma classe VolumeCalculator que estende a classe AreaCalculator:</p>

<pre><code class="php">class VolumeCalculator extends AreaCalulator
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
</code></pre>

<p>Na classe SumCalculatorOutputter:</p>

<pre><code class="php">class SumCalculatorOutputter
{
    protected $calculator;

    public function __constructor(AreaCalculator $calculator)
    {
        $this-&gt;calculator = $calculator;
    }

    public function JSON()
    {
        $data = array(
            'sum' =&gt; $this-&gt;calculator-&gt;sum();
        );

        return json_encode($data);
    }

    public function HTML()
    {
        return implode('', array(
            '&lt;h1&gt;',
                'Sum of the areas of provided shapes: ',
                $this-&gt;calculator-&gt;sum(),
            '&lt;/h1&gt;'
        ));
    }
}
</code></pre>

<p>Se tentássemos rodar um exemplo como este:</p>

<pre><code class="php">$areas = new AreaCalculator($shapes);
$volumes = new AreaCalculator($solidShapes);

$output = new SumCalculatorOutputter($areas);
$output2 = new SumCalculatorOutputter($volumes);
</code></pre>

<p>O programa não dará erro, mas quando chamarmos o método HTML no objeto $output2 nós teremos um erro E_NOTICE informando da conversão de um array em string.</p>

<p>Para corrigir isso, ao invés de retornar um array do método sum da classe VolumeCalculator, você deve simplesmente:</p>

<pre><code class="php">public function sum()
{
    // lógica para calcular os volumes e retornar um array
    return $summedData;
}
</code></pre>

<p>Os dados são somados como um float, double ou integer.</p>

<h2 id="interface-segregation-principle-ou-princ%C3%ADpio-da-segrega%C3%A7%C3%A3o-de-interface">Interface segregation principle ou Princípio da Segregação de Interface</h2>

<p>Um cliente nunca deve ser forçado a implementar uma interface que ele não usa ou clientes não devem ser forçados a depender de métodos que eles não utilizam.
Ainda usando nosso exemplo de formas, nós sabemos que temos formas sólidas, uma vez que também gostaríamos de calcular o volume de uma forma, nós podemos adicionar um outro contrato na interface ShapeInterface:</p>

<pre><code class="php">interface ShapeInterface
{
    public function area();
    public function volume();
}
</code></pre>

<p>Qualquer forma que criamos implementa o método voluma, mas nós sabemos que quadrados são formas planas e que eles não tem volumes, então esta interface forçaria a classe Square a implementar um método que ele não utiliza.</p>

<p>Esse princípio diz não para isso, em vez disso você poderia criar outra interface chamada SolidShapeInterface que tem o contrato de volume e formas sólidas como cubos que podem implementar essa interface:</p>

<pre><code class="php">interface ShapeInterface
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
</code></pre>

<p>Essa é uma abordagem muito melhor, mas uma armadilha para quem vê de fora, quando fizer o type-hint dessas interfaces, em vez de usar uma ShapeInterface ou uma SolidShapeInterface.</p>

<p>Você pode ciar uma outra interface, talvez ManageShapeInterface e implementá-la em ambas as formas planas e sólidas, desta forma você pode facilmente ver que ele tem uma única API para gerenciar as formas. Por exemplo:</p>

<pre><code class="php">interface ManageShapeInterface
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
        return $this-&gt;area();
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
        return $this-&gt;area() + $this-&gt;volume();
    }
}
</code></pre>

<p>Now in AreaCalculator class, we can easily replace the call to the area method with calculate and also check if the object is an instance of the ManageShapeInterface and not the ShapeInterface.</p>

<h2 id="dependency-inversion-principle-ou-princ%C3%ADpio-da-invers%C3%A3o-de-depend%C3%AAncia">Dependency Inversion principle ou Princípio da Inversão de Dependência</h2>

<p>O último, mas definitivamente não o menos importante afirma que:</p>

<p>Entidades devem depender de abstrações e não de classes concretas. Ele afirma que o módulo de alto nível não deve depender do módulo de baixo nível, mas eles devem depender de abstrações.
Isso pode parecer "inchado", mas é muito fácil de entender. Este princípio permite o desacoplamento, um exemplo parece ser a melhor maneira de explicar esse princípio:</p>

<pre><code class="php">class PasswordReminder
{
    private $dbConnection;

    public function __construct(MySQLConnection $dbConnection)
    {
        $this-&gt;dbConnection = $dbConnection;
    }
}
</code></pre>

<p>Primeiro o MySQLConnection é o módulo de baixo nível, enquanto o PasswordRemnder é o de alto nível, mas de acordo com a definição D no S.O.L.I.D. este trecho acima viola este princípio pois a classe PasswordReminder está sendo forçada a depender da classe MySQLConnection.</p>

<p>Mais tarde, se você tivesse que mudar o banco de dados, você também teria que editar a classe PasswordReminder e, portanto viola o princípio Open-close.</p>

<p>A classe PasswordReminder não deve se preocupar com o banco de dados usado pela aplicação, para corrigir isso mais uma vez nós "programamos para uma interface", já que os módulos de alto e baixo nível devem depender de uma abstraão, podemos criar uma interface:</p>

<pre><code class="php">interface DBConnectionInterface
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
        $this-&gt;dbConnection = $dbConnection;
    }
}
</code></pre>

<p>De acordo com o o pequeno trecho acima, agora você pode ver que tanto módulos de alto e baixo níveis dependem de abstração.</p>

<h2 id="conclus%C3%A3o">Conclusão</h2>

<p>Honestamente, S.O.L.I.D. pode parecer "irritante" no início, mas com o uso contínuo e adesão de suas diretrizes, torna-se parte de você e de seu código, que poderá ser facilmente estendido, modificado, testado e refatorado sem quaisquer problemas.</p>
