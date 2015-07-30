<p>{
"title" : "Event Listeners e Subscribers no Symfony 2",
"author":"Royopa",
"date":"25-09-2014",
"tag":"doctrine, php, symfony, listeners",
"slug" : "events-listeners-e-subscribers-no-symfony-2",
"category":"PHP"
}</p>

<p>Doctrine packages a rich event system that fires events when almost anything
happens inside the system. For you, this means that you can create arbitrary
:doc:<code>services &lt;/book/service_container&gt;</code> and tell Doctrine to notify those
objects whenever a certain action (e.g. <code>prePersist</code>) happens within Doctrine.
This could be useful, for example, to create an independent search index
whenever an object in your database is saved.</p>

<p>Doctrine defines two types of objects that can listen to Doctrine events:
listeners and subscribers. Both are very similar, but listeners are a bit
more straightforward. For more, see <code>The Event System</code>_ on Doctrine's website.</p>

<p>The Doctrine website also explains all existing events that can be listened to.</p>

<h2 id="configuring-the-listener%2Fsubscriber">Configuring the Listener/Subscriber</h2>

<p>To register a service to act as an event listener or subscriber you just have
to :ref:<code>tag &lt;book-service-container-tags&gt;</code> it with the appropriate name. Depending
on your use-case, you can hook a listener into every DBAL connection and ORM
entity manager or just into one specific DBAL connection and all the entity
managers that use this connection.</p>

<p>.. configuration-block::</p>

<pre><code>.. code-block:: yaml

    doctrine:
        dbal:
            default_connection: default
            connections:
                default:
                    driver: pdo_sqlite
                    memory: true

    services:
        my.listener:
            class: Acme\SearchBundle\EventListener\SearchIndexer
            tags:
                - { name: doctrine.event_listener, event: postPersist }
        my.listener2:
            class: Acme\SearchBundle\EventListener\SearchIndexer2
            tags:
                - { name: doctrine.event_listener, event: postPersist, connection: default }
        my.subscriber:
            class: Acme\SearchBundle\EventListener\SearchIndexerSubscriber
            tags:
                - { name: doctrine.event_subscriber, connection: default }

.. code-block:: xml

    &lt;?xml version="1.0" ?&gt;
    &lt;container xmlns="http://symfony.com/schema/dic/services"
        xmlns:doctrine="http://symfony.com/schema/dic/doctrine"&gt;

        &lt;doctrine:config&gt;
            &lt;doctrine:dbal default-connection="default"&gt;
                &lt;doctrine:connection driver="pdo_sqlite" memory="true" /&gt;
            &lt;/doctrine:dbal&gt;
        &lt;/doctrine:config&gt;

        &lt;services&gt;
            &lt;service id="my.listener" class="Acme\SearchBundle\EventListener\SearchIndexer"&gt;
                &lt;tag name="doctrine.event_listener" event="postPersist" /&gt;
            &lt;/service&gt;
            &lt;service id="my.listener2" class="Acme\SearchBundle\EventListener\SearchIndexer2"&gt;
                &lt;tag name="doctrine.event_listener" event="postPersist" connection="default" /&gt;
            &lt;/service&gt;
            &lt;service id="my.subscriber" class="Acme\SearchBundle\EventListener\SearchIndexerSubscriber"&gt;
                &lt;tag name="doctrine.event_subscriber" connection="default" /&gt;
            &lt;/service&gt;
        &lt;/services&gt;
    &lt;/container&gt;

.. code-block:: php

    use Symfony\Component\DependencyInjection\Definition;

    $container-&gt;loadFromExtension('doctrine', array(
        'dbal' =&gt; array(
            'default_connection' =&gt; 'default',
            'connections' =&gt; array(
                'default' =&gt; array(
                    'driver' =&gt; 'pdo_sqlite',
                    'memory' =&gt; true,
                ),
            ),
        ),
    ));

    $container
        -&gt;setDefinition(
            'my.listener',
            new Definition('Acme\SearchBundle\EventListener\SearchIndexer')
        )
        -&gt;addTag('doctrine.event_listener', array('event' =&gt; 'postPersist'))
    ;
    $container
        -&gt;setDefinition(
            'my.listener2',
            new Definition('Acme\SearchBundle\EventListener\SearchIndexer2')
        )
        -&gt;addTag('doctrine.event_listener', array('event' =&gt; 'postPersist', 'connection' =&gt; 'default'))
    ;
    $container
        -&gt;setDefinition(
            'my.subscriber',
            new Definition('Acme\SearchBundle\EventListener\SearchIndexerSubscriber')
        )
        -&gt;addTag('doctrine.event_subscriber', array('connection' =&gt; 'default'))
    ;
</code></pre>

<h2 id="creating-the-listener-class">Creating the Listener Class</h2>

<p>In the previous example, a service <code>my.listener</code> was configured as a Doctrine
listener on the event <code>postPersist</code>. The class behind that service must have
a <code>postPersist</code> method, which will be called when the event is dispatched::</p>

<pre><code>// src/Acme/SearchBundle/EventListener/SearchIndexer.php
namespace Acme\SearchBundle\EventListener;

use Doctrine\ORM\Event\LifecycleEventArgs;
use Acme\StoreBundle\Entity\Product;

class SearchIndexer
{
    public function postPersist(LifecycleEventArgs $args)
    {
        $entity = $args-&gt;getEntity();
        $entityManager = $args-&gt;getEntityManager();

        // perhaps you only want to act on some "Product" entity
        if ($entity instanceof Product) {
            // ... do something with the Product
        }
    }
}
</code></pre>

<p>In each event, you have access to a <code>LifecycleEventArgs</code> object, which
gives you access to both the entity object of the event and the entity manager
itself.</p>

<p>One important thing to notice is that a listener will be listening for <em>all</em>
entities in your application. So, if you're interested in only handling a
specific type of entity (e.g. a <code>Product</code> entity but not a <code>BlogPost</code>
entity), you should check for the entity's class type in your method
(as shown above).</p>

<p>.. tip::</p>

<pre><code>In Doctrine 2.4, a feature called Entity Listeners was introduced.
It is a lifecycle listener class used for an entity. You can read
about it in `the Doctrine Documentation`_.
</code></pre>

<h2 id="creating-the-subscriber-class">Creating the Subscriber Class</h2>

<p>A Doctrine event subscriber must implement the <code>Doctrine\Common\EventSubscriber</code>
interface and have an event method for each event it subscribes to::</p>

<pre><code>// src/Acme/SearchBundle/EventListener/SearchIndexerSubscriber.php
namespace Acme\SearchBundle\EventListener;

use Doctrine\Common\EventSubscriber;
use Doctrine\ORM\Event\LifecycleEventArgs;
// for Doctrine 2.4: Doctrine\Common\Persistence\Event\LifecycleEventArgs;
use Acme\StoreBundle\Entity\Product;

class SearchIndexerSubscriber implements EventSubscriber
{
    public function getSubscribedEvents()
    {
        return array(
            'postPersist',
            'postUpdate',
        );
    }

    public function postUpdate(LifecycleEventArgs $args)
    {
        $this-&gt;index($args);
    }

    public function postPersist(LifecycleEventArgs $args)
    {
        $this-&gt;index($args);
    }

    public function index(LifecycleEventArgs $args)
    {
        $entity = $args-&gt;getEntity();
        $entityManager = $args-&gt;getEntityManager();

        // perhaps you only want to act on some "Product" entity
        if ($entity instanceof Product) {
            // ... do something with the Product
        }
    }
}
</code></pre>

<p>.. tip::</p>

<pre><code>Doctrine event subscribers can not return a flexible array of methods to
call for the events like the :ref:`Symfony event subscriber &lt;event_dispatcher-using-event-subscribers&gt;`
can. Doctrine event subscribers must return a simple array of the event
names they subscribe to. Doctrine will then expect methods on the subscriber
with the same name as each subscribed event, just as when using an event listener.
</code></pre>

<p>For a full reference, see chapter <code>The Event System</code>_ in the Doctrine documentation.</p>

<p>.. _<code>The Event System</code>: http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/events.html
.. _<code>the Doctrine Documentation</code>: http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/events.html#entity-listeners</p>
