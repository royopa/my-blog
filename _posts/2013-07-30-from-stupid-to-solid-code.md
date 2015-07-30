<p>{
"title" : "From STUPID to SOLID Code!",
"author":"Royopa",
"date":"30-07-2013",
"tag":"orientação a objetos, qualidade de código, solid",
"slug" : "from-stupid-to-solid-code",
"category":"SOLID"
}</p>

<p>Last week I gave a talk about <a href="https://en.wikipedia.org/wiki/Object-oriented&#95;programming">Object-Oriented
Programming</a> at
Michelin, the company I am working for. I talked about writing better code,
<a href="http://williamdurand.fr/from-stupid-to-solid-code-slides/">from STUPID to SOLID
code!</a>
<strong>STUPID</strong> as well as
<a href="http://en.wikipedia.org/wiki/SOLID&#95;&#40;object-oriented&#95;design&#41;"><strong>SOLID</strong></a> are
two acronyms, and have been
<a href="http://blog.ircmaxell.com/2012/05/dont-be-stupid-grasp-solid-slides.html">covered</a>
<a href="http://nikic.github.io/2011/12/27/Dont-be-STUPID-GRASP-SOLID.html">quite</a> a
<a href="http://lostechies.com/chadmyers/2008/03/08/pablo-s-topic-of-the-month-march-solid-principles/">lot</a>
for a long time. However, these mnemonics are not always well-known, so it is
worth spreading the word.</p>

<p>In the following, I will introduce both <strong>STUPID</strong> and <strong>SOLID</strong> principles. Keep
in mind that these are <strong>principles</strong>, <strong>not laws</strong>. However, considering them as
laws would be good for those who want to improve themselves.</p>

<h2 id="stupid-code%2C-seriously%3F">STUPID code, seriously?</h2>

<p>This may hurt your feelings, but you have probably written STUPID code already. I have too. But, what does that mean?</p>

<ul>
<li><a href="#singleton"><strong>S</strong>ingleton</a></li>
<li><a href="#tight-coupling"><strong>T</strong>ight Coupling</a></li>
<li><a href="#untestability"><strong>U</strong>ntestability</a></li>
<li><a href="#premature-optimization"><strong>P</strong>remature Optimization</a></li>
<li><a href="#indescriptive-naming"><strong>I</strong>ndescriptive Naming</a></li>
<li><a href="#duplication"><strong>D</strong>uplication</a></li>
</ul>

<p>In the following, I will explain the individual points with more details. This
is more or less the transcript of my talk.</p>

<h3 id="singleton">Singleton</h3>

<p>The <a href="http://en.wikipedia.org/wiki/Singleton_pattern">Singleton pattern</a> is
probably the most well-known design pattern, but also the most misunderstood
one. Are you aware of the <em>Singleton syndrome</em>? It is when you think the
Singleton pattern is the most appropriate pattern for the current use case you
have. In other words, you use it everywhere. That is definitely <strong>not</strong> cool.</p>

<p><a href="https://code.google.com/p/google-singleton-detector/wiki/WhySingletonsAreControversial">Singletons are
controversial</a>,
and they are often <a href="http://stackoverflow.com/questions/11292109/why-is-singleton-considered-an-anti-pattern-in-java-world-sometimes">considered
anti-patterns</a>.
You should <a href="http://programmers.stackexchange.com/questions/40373/so-singletons-are-bad-then-what">avoid
them</a>.
Actually, the use of a <a href="http://programmers.stackexchange.com/questions/40373/so-singletons-are-bad-then-what/40376#40376">singleton is not the problem, but the symptom of
a problem</a>.
Here are two reasons why:</p>

<ul>
<li>Programs using global state are very difficult to test;</li>
<li>Programs that rely on global state hide their dependencies.</li>
</ul>

<p>But should you really avoid them all the time? I would say <em>yes</em> because you can
often replace the use of a singleton by something better. Avoiding static things
is important to avoid something called <strong>tight coupling</strong>.</p>

<h3 id="tight-coupling">Tight Coupling</h3>

<p><strong>Tight coupling</strong>, also known as <strong>strong coupling</strong>, is a generalization of
the Singleton issue. Basically, you should <a href="http://martinfowler.com/ieeeSoftware/coupling.pdf">reduce
coupling</a> between your
modules. <strong>Coupling</strong> is <a href="http://en.wikipedia.org/wiki/Coupling&#95;&#40;computer&#95;programming&#41;">the degree to which each program module relies on
each one of the other modules</a>.</p>

<p>If making a change in one module in your application requires you to change another module,
then coupling exists. For instance, you instantiate objects in your
constructor's class instead of passing instances as arguments. That is bad because
it <strong>doesn't allow further changes</strong> such as replacing the instance by an instance
of a sub-class, a <em>mock</em> or whatever.</p>

<p>Tightly coupled modules are <strong>difficult to reuse</strong>, and also <strong>hard to test</strong>.</p>

<h3 id="untestability">Untestability</h3>

<p>In my opinion, <strong>testing should not be hard!</strong> No, really. Whenever you don't
write unit tests because you don't have time, the real issue is that your code
is bad, but that is another story.</p>

<p>Most of the time, <strong>untestability</strong> is caused by <strong>tight coupling</strong>.</p>

<h3 id="premature-optimization">Premature Optimization</h3>

<p>Donald Knuth said: "<em>premature optimization is the root of all evil</em>. There is
<strong>only cost</strong>, and <strong>no benefit</strong>". Actually, optimized systems are much more
complex than just rewriting a loop or using <a href="http://stackoverflow.com/questions/24886/is-there-a-performance-difference-between-i-and-i-in-c">pre-increment instead of
post-increment</a>.
You will just end up with unreadable code. That is why <a href="http://www.c2.com/cgi/wiki?PrematureOptimization">Premature Optimization
is often considered an anti-pattern</a>.</p>

<p>A friend of mine often says that there are two rules to optimize an
application:</p>

<ul>
<li>don't do it;</li>
<li>(for experts only!) don't do it yet.</li>
</ul>

<h3 id="indescriptive-naming">Indescriptive Naming</h3>

<p>This should be obvious, but still needs to be said: <strong>name</strong> your classes, methods,
attributes, and variables <strong>properly</strong>. Oh, and <a href="/2013/06/03/object-calisthenics/#6-don-t-abbreviate">don't
abbreviate</a>! You write code
for people, not for computers. They don't understand what you write anyway.
Computers just understand <code>0</code> and <code>1</code>. <strong>Programming languages are for humans</strong>.</p>

<h3 id="duplication">Duplication</h3>

<p><a href="http://c2.com/cgi/wiki?DuplicatedCode">Duplicated code</a> is bad, so please <a href="http://lostechies.com/patricklioi/2013/07/30/no-seriously-dont-repeat-yourself/">Don't
Repeat Yourself</a>
(<a href="http://en.wikipedia.org/wiki/Don't&#95;repeat&#95;yourself">DRY</a>),
and also <a href="http://en.wikipedia.org/wiki/KISS&#95;principle">Keep It Simple, Stupid</a>.
Be lazy the right way - write code only once!</p>

<p>Now that I have explained what STUPID code is, you may think that your code
is STUPID. It does not matter (yet). Don't feel bad, keep calm and be awesome
by writing SOLID code instead!</p>

<h2 id="solid-to-the-rescue%21">SOLID to the rescue!</h2>

<p>SOLID is a term describing a <strong>collection of design principles</strong> for good code
that was invented by Robert C. Martin, also known as <em>Uncle Bob</em>.</p>

<p>SOLID means:</p>

<ul>
<li><a href="#single-responsibility-principle"><strong>S</strong>ingle Responsibility Principle</a></li>
<li><a href="#open/closed-principle"><strong>O</strong>pen/Closed Principle</a></li>
<li><a href="#liskov-substitution-principle"><strong>L</strong>iskov Substitution Principle</a></li>
<li><a href="#interface-segregation-principle"><strong>I</strong>nterface Segregation Principle</a></li>
<li><a href="#dependency-inversion-principle"><strong>D</strong>ependency Inversion Principle</a></li>
</ul>

<h3 id="single-responsibility-principle">Single Responsibility Principle</h3>

<p><a href="http://www.objectmentor.com/resources/articles/srp.pdf">Single Responsibility
Principle</a> or
<a href="http://en.wikipedia.org/wiki/Single&#95;responsibility&#95;principle">SRP</a> states that
<strong>every class should have a single responsibility</strong>. There should <strong>never be
more than one reason for a class to change</strong>.</p>

<p>Just because you can add everything you want into your class doesn't mean that you
should. Thinking in term of responsibilities will help you design your
application better. Ask yourself whether the logic you are introducing should live in
this class or not. Using <strong>layers</strong> in your application helps a lot. Split big
classes in smaller ones, and avoid <a href="http://c2.com/cgi/wiki?GodClass">god
classes</a>.
Last but not least, <strong>write straightforward comments</strong>. If you start writing
comments such as <code>in this case</code>, <code>but if</code>, <code>except when</code>, <code>or</code>, then you are
doing it wrong.</p>

<h3 id="open%2Fclosed-principle">Open/Closed Principle</h3>

<p><a href="http://www.objectmentor.com/resources/articles/ocp.pdf">Open/Closed Principle</a>
or <a href="http://en.wikipedia.org/wiki/Open/closed&#95;principle">OCP</a> states that
software entities should be <strong>open for extension</strong>, but <strong>closed for
modification</strong>.</p>

<p>You should make all member variables <strong>private</strong> by default. Write getters and setters only
when you need them. I've already covered this point in a previous
article, as <a href="/2013/06/03/object-calisthenics/#9-no-getters/setters/properties">the ninth rule of the Object
Calisthenics</a>
is related to this principle.</p>

<h3 id="liskov-substitution-principle">Liskov Substitution Principle</h3>

<p><a href="http://www.objectmentor.com/resources/articles/lsp.pdf">Liskov Substitution
Principle</a> or
<a href="http://c2.com/cgi/wiki?LiskovSubstitutionPrinciple">LSP</a> states that objects
in a program should be <strong>replaceable with instances of their subtypes without
altering the correctness</strong> of the program.</p>

<p>Let's take an example. A rectangle is a plane figure with four right angles. It
has a <code>width</code>, and a <code>height</code>. Now, take a look at the following pseudo-code:</p>

<pre><code>rect = new Rectangle();

rect.width  = 10;
rect.height = 20;

assert 10 == rect.width
assert 20 == rect.height
</code></pre>

<p>We simply set a <code>width</code> and a <code>height</code> on a <code>Rectangle</code> instance, and then
we assert that both properties are correct. So far, so good.</p>

<p>Now we can improve our definition by saying that a rectangle with four sides of
equal length is called a square. A square <strong>is a</strong> rectangle so we can create a
<code>Square</code> class that extends the <code>Rectangle</code> one, and replace the first line above
by the one below:</p>

<pre><code>rect = new Square();
</code></pre>

<p>According to the definition of a square, its width is equal to its height. Can
you spot the problem? The first assertion will fail because we had to change the
behavior of the setters in the <code>Square</code> class to fit the definition. This is a
violation of the Liskov Substitution Principle.</p>

<h3 id="interface-segregation-principle">Interface Segregation Principle</h3>

<p><a href="http://www.objectmentor.com/resources/articles/isp.pdf">Interface Segregation
Principle</a> or
<a href="http://en.wikipedia.org/wiki/Interface_segregation_principle">ISP</a> states that
<strong>many</strong> client-specific <strong>interfaces are better than one</strong> general-purpose
interface. In other words, you should not have to implement methods that you
don't use. Enforcing ISP gives you <strong>low coupling</strong>, and <strong>high cohesion</strong>.</p>

<p>When talking about <strong>coupling</strong>,
<a href="http://en.wikipedia.org/wiki/Cohesion&#95;&#40;computer&#95;science&#41;">cohesion</a> is often
mentioned as well. <strong>High cohesion</strong> means to keep similar and related things
together. The <strong>union</strong> of cohesion and coupling is <a href="http://www.jasoncoffin.com/cohesion-and-coupling-principles-of-orthogonal-object-oriented-programming/">orthogonal
design</a>.
The idea is to <strong>keep your components focused</strong> and try to <strong>minimize the
dependencies between them</strong>.</p>

<p>Note that this is similar to the <a href="#single-responsibility-principle">Single Responsibility
Principle</a>. An interface is a contract that
meets a need. It is ok to have a class that implements different interfaces, but be careful, don't violate SRP.</p>

<h3 id="dependency-inversion-principle">Dependency Inversion Principle</h3>

<p><a href="http://www.objectmentor.com/resources/articles/dip.pdf">Dependency Inversion
Principle</a>
or <a href="http://www.c2.com/cgi/wiki?DependencyInversionPrinciple">DIP</a> has two key
points:</p>

<ul>
<li><strong>Abstractions should not depend upon details</strong>;</li>
<li><strong>Details should depend upon abstractions</strong>.</li>
</ul>

<p>This principle could be rephrased as <strong>use the same level of abstraction at a
given level</strong>. Interfaces should depend on other interfaces. Don't add concrete
classes to method signatures of an interface. However, use interfaces in your
class methods.</p>

<p>Note that <a href="http://lostechies.com/derickbailey/2011/09/22/dependency-injection-is-not-the-same-as-the-dependency-inversion-principle/">Dependency Inversion Principle is not the same as Dependency
Injection</a>.
<strong>Dependency Injection</strong> is about how one object knows about another dependent
object. In other words, it is about <a href="http://martinfowler.com/articles/dipInTheWild.html">how one object acquires a
dependency</a>. On the other
hand, DIP is about the level of abstraction. Also, a <a href="http://www.martinfowler.com/articles/injection.html">Dependency Injection
Container</a> is a way to
auto-wire classes together. That does not mean you do Dependency Injection
though. Look at the <a href="http://en.wikipedia.org/wiki/Service_locator_pattern">Service
Locator</a> for example.</p>

<p>Also, rather than working with classes that are tight coupled, use interfaces.
This is called <a href="http://www.fatagnus.com/program-to-an-interface-not-an-implementation/">programming to the
interface</a>.
This <strong>reduces dependency</strong> on implementation specifics and makes code <strong>more
reusable</strong>. It also ensures that you can replace the implementation without
violating the expectations of that interface, according to the Liskov
Substitution Principle seen before.</p>

<h2 id="conclusion">Conclusion</h2>

<p>As you probably noticed, <strong>avoiding tight coupling is the key</strong>. It is present
in a lot of code, and if you start by focusing on <em>fixing</em> this alone, you will immediately start writing better code.</p>

<p>If I may leave you with only one piece of advice, that would be to <strong>use your brain</strong>. There
are a lot of principles in software engineering. Even if you don't understand all these
principles, always think before writing code. Take the time to understand those that you don't understand.</p>

<p>Honestly, writing SOLID code is not that hard.</p>

<h2 id="slides">Slides</h2>

<script async class="speakerdeck-embed" data-id="e04c5ae0d74d013022821e9ac6d7834e" data-ratio="1.29456384323641" src="//speakerdeck.com/assets/embed.js"></script>

<h2 id="tl%3Bdr">TL;DR</h2>

<p><strong>STUPID</strong> is an acronym that describes bad practices in Oriented Object
Programming:</p>

<ul>
<li><a href="#singleton"><strong>S</strong>ingleton</a></li>
<li><a href="#tight-coupling"><strong>T</strong>ight Coupling</a></li>
<li><a href="#untestability"><strong>U</strong>ntestability</a></li>
<li><a href="#premature-optimization"><strong>P</strong>remature Optimization</a></li>
<li><a href="#indescriptive-naming"><strong>I</strong>ndescriptive Naming</a></li>
<li><a href="#duplication"><strong>D</strong>uplication</a></li>
</ul>

<p><strong>SOLID</strong> is an acronym that stands for <strong>five basic principles</strong> of
Object-Oriented Programming and design to <em>fix</em> STUPID code:</p>

<ul>
<li><a href="#single-responsibility-principle"><strong>S</strong>ingle Responsibility Principle</a></li>
<li><a href="#open/closed-principle"><strong>O</strong>pen/Closed Principle</a></li>
<li><a href="#liskov-substitution-principle"><strong>L</strong>iskov Substitution Principle</a></li>
<li><a href="#interface-segregation-principle"><strong>I</strong>nterface Segregation Principle</a></li>
<li><a href="#dependency-inversion-principle"><strong>D</strong>ependency Inversion Principle</a></li>
</ul>

<p>Rule of thumb: <strong>use your brain</strong>!</p>
