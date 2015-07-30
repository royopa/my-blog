<p>{
"title" : "Eventos no Doctrine 2",
"author":"Royopa",
"date":"04-09-2014",
"tag":"Doctrine, PHP, ORM",
"slug" : "eventos-no-doctrine-2",
"category":"PHP"
}</p>

<p>O Doctrine 2 possui um sistema de eventos que faz parte do pacote Common. O
doctrine usa esse sistema para disparar eventos de sistema, principalmente
<code>lifecycle events</code>. Mas podemos também criar nossos próprios eventos personalizados.</p>

<h3 id="o-sistema-de-eventos">O Sistema de Eventos</h3>

<p>O sistema de eventos é controlado pelo <code>EventManager</code>. Ele é o ponto central do
sistema de event listener do Doctrine. Os Listeners são registrados no manager e
os eventos são disparados pelo manager.</p>

<pre><code class="php">    &lt;?php
    $evm = new EventManager();
</code></pre>

<p>Now we can add some event listeners to the <code>$evm</code>. Let's create a
<code>TestEvent</code> class to play around with.</p>

<pre><code class="php"><br />    &lt;?php
    class TestEvent
    {
        const preFoo = 'preFoo';
        const postFoo = 'postFoo';

        private $_evm;

        public $preFooInvoked = false;
        public $postFooInvoked = false;

        public function __construct($evm)
        {
            $evm-&gt;addEventListener(array(self::preFoo, self::postFoo), $this);
        }

        public function preFoo(EventArgs $e)
        {
            $this-&gt;preFooInvoked = true;
        }

        public function postFoo(EventArgs $e)
        {
            $this-&gt;postFooInvoked = true;
        }
    }

    // Create a new instance
    $test = new TestEvent($evm);

Events can be dispatched by using the ``dispatchEvent()`` method.

```php

    &lt;?php
    $evm-&gt;dispatchEvent(TestEvent::preFoo);
    $evm-&gt;dispatchEvent(TestEvent::postFoo);

You can easily remove a listener with the ``removeEventListener()``
method.

```php

    &lt;?php
    $evm-&gt;removeEventListener(array(self::preFoo, self::postFoo), $this);

The Doctrine 2 event system also has a simple concept of event
subscribers. We can define a simple ``TestEventSubscriber`` class
which implements the ``\Doctrine\Common\EventSubscriber`` interface
and implements a ``getSubscribedEvents()`` method which returns an
array of events it should be subscribed to.

```php

    &lt;?php
    class TestEventSubscriber implements \Doctrine\Common\EventSubscriber
    {
        public $preFooInvoked = false;

        public function preFoo()
        {
            $this-&gt;preFooInvoked = true;
        }

        public function getSubscribedEvents()
        {
            return array(TestEvent::preFoo);
        }
    }

    $eventSubscriber = new TestEventSubscriber();
    $evm-&gt;addEventSubscriber($eventSubscriber);

.. note::

    The array to return in the ``getSubscribedEvents`` method is a simple array
    with the values being the event names. The subscriber must have a method
    that is named exactly like the event.

Now when you dispatch an event, any event subscribers will be
notified for that event.

```php

    &lt;?php
    $evm-&gt;dispatchEvent(TestEvent::preFoo);
</code></pre>

<p>Now you can test the <code>$eventSubscriber</code> instance to see if the
<code>preFoo()</code> method was invoked.</p>

<pre><code class="php"><br />    &lt;?php
    if ($eventSubscriber-&gt;preFooInvoked) {
        echo 'pre foo invoked!';
    }
</code></pre>

<p>Naming convention
~~~~~~~~~~~~~~~~~</p>

<p>Events being used with the Doctrine 2 EventManager are best named
with camelcase and the value of the corresponding constant should
be the name of the constant itself, even with spelling. This has
several reasons:</p>

<ul>
<li>It is easy to read.</li>
<li>Simplicity.</li>
<li>Each method within an EventSubscriber is named after the
corresponding constant's value. If the constant's name and value differ
it contradicts the intention of using the constant and makes your code
harder to maintain.</li>
</ul>

<p>An example for a correct notation can be found in the example
<code>TestEvent</code> above.</p>

<p>.. _reference-events-lifecycle-events:</p>

<h2 id="lifecycle-events">Lifecycle Events</h2>

<p>The EntityManager and UnitOfWork trigger a bunch of events during
the life-time of their registered entities.</p>

<ul>
<li>preRemove - The preRemove event occurs for a given entity before
the respective EntityManager remove operation for that entity is
executed.  It is not called for a DQL DELETE statement.</li>
<li>postRemove - The postRemove event occurs for an entity after the
entity has been deleted. It will be invoked after the database
delete operations. It is not called for a DQL DELETE statement.</li>
<li>prePersist - The prePersist event occurs for a given entity
before the respective EntityManager persist operation for that
entity is executed. It should be noted that this event is only triggered on
<em>initial</em> persist of an entity (i.e. it does not trigger on future updates).</li>
<li>postPersist - The postPersist event occurs for an entity after
the entity has been made persistent. It will be invoked after the
database insert operations. Generated primary key values are
available in the postPersist event.</li>
<li>preUpdate - The preUpdate event occurs before the database
update operations to entity data. It is not called for a DQL UPDATE statement.</li>
<li>postUpdate - The postUpdate event occurs after the database
update operations to entity data. It is not called for a DQL UPDATE statement.</li>
<li>postLoad - The postLoad event occurs for an entity after the
entity has been loaded into the current EntityManager from the
database or after the refresh operation has been applied to it.</li>
<li>loadClassMetadata - The loadClassMetadata event occurs after the
mapping metadata for a class has been loaded from a mapping source
(annotations/xml/yaml). This event is not a lifecycle callback.</li>
<li>preFlush - The preFlush event occurs at the very beginning of a flush
operation. This event is not a lifecycle callback.</li>
<li>onFlush - The onFlush event occurs after the change-sets of all
managed entities are computed. This event is not a lifecycle
callback.</li>
<li>postFlush - The postFlush event occurs at the end of a flush operation. This
event is not a lifecycle callback.</li>
<li>onClear - The onClear event occurs when the EntityManager#clear() operation is
invoked, after all references to entities have been removed from the unit of
work. This event is not a lifecycle callback.</li>
</ul>

<p>.. warning::</p>

<pre><code>Note that the postLoad event occurs for an entity
before any associations have been initialized. Therefore it is not
safe to access associations in a postLoad callback or event
handler.
</code></pre>

<p>.. warning::</p>

<pre><code>Note that the postRemove event or any events triggered after an entity removal
can receive an uninitializable proxy in case you have configured an entity to
cascade remove relations. In this case, you should load yourself the proxy in
the associated pre event.
</code></pre>

<p>You can access the Event constants from the <code>Events</code> class in the
ORM package.</p>

<pre><code class="php"><br />    &lt;?php
    use Doctrine\ORM\Events;
    echo Events::preUpdate;

These can be hooked into by two different types of event
listeners:

-  Lifecycle Callbacks are methods on the entity classes that are
   called when the event is triggered. As of v2.4 they receive some kind
   of ``EventArgs`` instance.
-  Lifecycle Event Listeners and Subscribers are classes with specific callback
   methods that receives some kind of ``EventArgs`` instance.

The EventArgs instance received by the listener gives access to the entity,
EntityManager and other relevant data.

.. note::

    All Lifecycle events that happen during the ``flush()`` of
    an EntityManager have very specific constraints on the allowed
    operations that can be executed. Please read the
    :ref:`reference-events-implementing-listeners` section very carefully
    to understand which operations are allowed in which lifecycle event.


Lifecycle Callbacks
-------------------

Lifecycle Callbacks are defined on an entity class. They allow you to
trigger callbacks whenever an instance of that entity class experiences
a relevant lifecycle event. More than one callback can be defined for each
lifecycle event. Lifecycle Callbacks are best used for simple operations
specific to a particular entity class's lifecycle.

```php

    &lt;?php

    /** @Entity @HasLifecycleCallbacks */
    class User
    {
        // ...

        /**
         * @Column(type="string", length=255)
         */
        public $value;

        /** @Column(name="created_at", type="string", length=255) */
        private $createdAt;

        /** @PrePersist */
        public function doStuffOnPrePersist()
        {
            $this-&gt;createdAt = date('Y-m-d H:i:s');
        }

        /** @PrePersist */
        public function doOtherStuffOnPrePersist()
        {
            $this-&gt;value = 'changed from prePersist callback!';
        }

        /** @PostPersist */
        public function doStuffOnPostPersist()
        {
            $this-&gt;value = 'changed from postPersist callback!';
        }

        /** @PostLoad */
        public function doStuffOnPostLoad()
        {
            $this-&gt;value = 'changed from postLoad callback!';
        }

        /** @PreUpdate */
        public function doStuffOnPreUpdate()
        {
            $this-&gt;value = 'changed from preUpdate callback!';
        }
    }
</code></pre>

<p>Note that the methods set as lifecycle callbacks need to be public and,
when using these annotations, you have to apply the
<code>@HasLifecycleCallbacks</code> marker annotation on the entity class.</p>

<p>If you want to register lifecycle callbacks from YAML or XML you
can do it with the following.</p>

<pre><code class="yaml"><br />    User:
      type: entity
      fields:
    # ...
        name:
          type: string(50)
      lifecycleCallbacks:
        prePersist: [ doStuffOnPrePersist, doOtherStuffOnPrePersist ]
        postPersist: [ doStuffOnPostPersist ]

</code></pre>

<p>In YAML the <code>key</code> of the lifecycleCallbacks entry is the event that you
are triggering on and the value is the method (or methods) to call. The allowed
event types are the ones listed in the previous Lifecycle Events section.</p>

<p>XML would look something like this:</p>

<pre><code class="xml"><br />    &lt;?xml version="1.0" encoding="UTF-8"?&gt;

    &lt;doctrine-mapping xmlns="http://doctrine-project.org/schemas/orm/doctrine-mapping"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://doctrine-project.org/schemas/orm/doctrine-mapping
                              /Users/robo/dev/php/Doctrine/doctrine-mapping.xsd"&gt;

        &lt;entity name="User"&gt;

            &lt;lifecycle-callbacks&gt;
                &lt;lifecycle-callback type="prePersist" method="doStuffOnPrePersist"/&gt;
                &lt;lifecycle-callback type="postPersist" method="doStuffOnPostPersist"/&gt;
            &lt;/lifecycle-callbacks&gt;

        &lt;/entity&gt;

    &lt;/doctrine-mapping&gt;

</code></pre>

<p>In XML the <code>type</code> of the lifecycle-callback entry is the event that you
are triggering on and the <code>method</code> is the method to call. The allowed event
types are the ones listed in the previous Lifecycle Events section.</p>

<p>When using YAML or XML you need to remember to create public methods to match the
callback names you defined. E.g. in these examples <code>doStuffOnPrePersist()</code>,
<code>doOtherStuffOnPrePersist()</code> and <code>doStuffOnPostPersist()</code> methods need to be
defined on your <code>User</code> model.</p>

<pre><code class="php"><br />    &lt;?php
    // ...

    class User
    {
        // ...

        public function doStuffOnPrePersist()
        {
            // ...
        }

        public function doOtherStuffOnPrePersist()
        {
            // ...
        }

        public function doStuffOnPostPersist()
        {
            // ...
        }
    }

</code></pre>

<h2 id="lifecycle-callbacks-event-argument">Lifecycle Callbacks Event Argument</h2>

<p>.. versionadded:: 2.4</p>

<p>Since 2.4 the triggered event is given to the lifecycle-callback.</p>

<p>With the additional argument you have access to the
<code>EntityManager</code> and <code>UnitOfWork</code> APIs inside these callback methods.</p>

<pre><code class="php"><br />    &lt;?php
    // ...

    class User
    {
        public function preUpdate(PreUpdateEventArgs $event)
        {
            if ($event-&gt;hasChangedField('username')) {
                // Do something when the username is changed.
            }
        }
    }

</code></pre>

<h2 id="listening-and-subscribing-to-lifecycle-events">Listening and subscribing to Lifecycle Events</h2>

<p>Lifecycle event listeners are much more powerful than the simple
lifecycle callbacks that are defined on the entity classes. They
sit at a level above the entities and allow you to implement re-usable
behaviors across different entity classes.</p>

<p>Note that they require much more detailed knowledge about the inner
workings of the EntityManager and UnitOfWork. Please read the
<em>Implementing Event Listeners</em> section carefully if you are trying
to write your own listener.</p>

<p>For event subscribers, there are no surprises. They declare the
lifecycle events in their <code>getSubscribedEvents</code> method and provide
public methods that expect the relevant arguments.</p>

<p>A lifecycle event listener looks like the following:</p>

<pre><code class="php"><br />    &lt;?php
    use Doctrine\Common\Persistence\Event\LifecycleEventArgs;

    class MyEventListener
    {
        public function preUpdate(LifecycleEventArgs $args)
        {
            $entity = $args-&gt;getObject();
            $entityManager = $args-&gt;getObjectManager();

            // perhaps you only want to act on some "Product" entity
            if ($entity instanceof Product) {
                // do something with the Product
            }
        }
    }

</code></pre>

<p>A lifecycle event subscriber may looks like this:</p>

<pre><code class="php"><br />    &lt;?php
    use Doctrine\ORM\Events;
    use Doctrine\Common\EventSubscriber;
    use Doctrine\Common\Persistence\Event\LifecycleEventArgs;

    class MyEventSubscriber implements EventSubscriber
    {
        public function getSubscribedEvents()
        {
            return array(
                Events::postUpdate,
            );
        }

        public function postUpdate(LifecycleEventArgs $args)
        {
            $entity = $args-&gt;getObject();
            $entityManager = $args-&gt;getObjectManager();

            // perhaps you only want to act on some "Product" entity
            if ($entity instanceof Product) {
                // do something with the Product
            }
        }

</code></pre>

<p>.. note::</p>

<pre><code>Lifecycle events are triggered for all entities. It is the responsibility
of the listeners and subscribers to check if the entity is of a type
it wants to handle.
</code></pre>

<p>To register an event listener or subscriber, you have to hook it into the
EventManager that is passed to the EntityManager factory:</p>

<pre><code class="php"><br />    &lt;?php
    $eventManager = new EventManager();
    $eventManager-&gt;addEventListener(array(Events::preUpdate), new MyEventListener());
    $eventManager-&gt;addEventSubscriber(new MyEventSubscriber());

    $entityManager = EntityManager::create($dbOpts, $config, $eventManager);

</code></pre>

<p>You can also retrieve the event manager instance after the
EntityManager was created:</p>

<pre><code class="php"><br />    &lt;?php
    $entityManager-&gt;getEventManager()-&gt;addEventListener(array(Events::preUpdate), new MyEventListener());
    $entityManager-&gt;getEventManager()-&gt;addEventSubscriber(new MyEventSubscriber());

</code></pre>

<p>.. _reference-events-implementing-listeners:</p>

<h2 id="implementing-event-listeners">Implementing Event Listeners</h2>

<p>This section explains what is and what is not allowed during
specific lifecycle events of the UnitOfWork. Although you get
passed the EntityManager in all of these events, you have to follow
these restrictions very carefully since operations in the wrong
event may produce lots of different errors, such as inconsistent
data and lost updates/persists/removes.</p>

<p>For the described events that are also lifecycle callback events
the restrictions apply as well, with the additional restriction
that (prior to version 2.4) you do not have access to the
EntityManager or UnitOfWork APIs inside these events.</p>

<p>prePersist
~~~~~~~~~~</p>

<p>There are two ways for the <code>prePersist</code> event to be triggered.
One is obviously when you call <code>EntityManager#persist()</code>. The
event is also called for all cascaded associations.</p>

<p>There is another way for <code>prePersist</code> to be called, inside the
<code>flush()</code> method when changes to associations are computed and
this association is marked as cascade persist. Any new entity found
during this operation is also persisted and <code>prePersist</code> called
on it. This is called "persistence by reachability".</p>

<p>In both cases you get passed a <code>LifecycleEventArgs</code> instance
which has access to the entity and the entity manager.</p>

<p>The following restrictions apply to <code>prePersist</code>:</p>

<ul>
<li>If you are using a PrePersist Identity Generator such as
sequences the ID value will <em>NOT</em> be available within any
PrePersist events.</li>
<li>Doctrine will not recognize changes made to relations in a prePersist
event. This includes modifications to
collections such as additions, removals or replacement.</li>
</ul>

<p>preRemove</p>

<pre><code><br />The ``preRemove`` event is called on every entity when its passed
to the ``EntityManager#remove()`` method. It is cascaded for all
associations that are marked as cascade delete.

There are no restrictions to what methods can be called inside the
``preRemove`` event, except when the remove method itself was
called during a flush operation.

preFlush
~~~~~~~~

``preFlush`` is called at ``EntityManager#flush()`` before
anything else. ``EntityManager#flush()`` can be called safely
inside its listeners.

```php

    &lt;?php

    use Doctrine\ORM\Event\PreFlushEventArgs;

    class PreFlushExampleListener
    {
        public function preFlush(PreFlushEventArgs $args)
        {
            // ...
        }
    }

```

onFlush
~~~~~~~

OnFlush is a very powerful event. It is called inside
``EntityManager#flush()`` after the changes to all the managed
entities and their associations have been computed. This means, the
``onFlush`` event has access to the sets of:


-  Entities scheduled for insert
-  Entities scheduled for update
-  Entities scheduled for removal
-  Collections scheduled for update
-  Collections scheduled for removal

To make use of the onFlush event you have to be familiar with the
internal UnitOfWork API, which grants you access to the previously
mentioned sets. See this example:

```php

    &lt;?php
    class FlushExampleListener
    {
        public function onFlush(OnFlushEventArgs $eventArgs)
        {
            $em = $eventArgs-&gt;getEntityManager();
            $uow = $em-&gt;getUnitOfWork();

            foreach ($uow-&gt;getScheduledEntityInsertions() as $entity) {

            }

            foreach ($uow-&gt;getScheduledEntityUpdates() as $entity) {

            }

            foreach ($uow-&gt;getScheduledEntityDeletions() as $entity) {

            }

            foreach ($uow-&gt;getScheduledCollectionDeletions() as $col) {

            }

            foreach ($uow-&gt;getScheduledCollectionUpdates() as $col) {

            }
        }
    }

```

The following restrictions apply to the onFlush event:


-  If you create and persist a new entity in ``onFlush``, then
   calling ``EntityManager#persist()`` is not enough.
   You have to execute an additional call to
   ``$unitOfWork-&gt;computeChangeSet($classMetadata, $entity)``.
-  Changing primitive fields or associations requires you to
   explicitly trigger a re-computation of the changeset of the
   affected entity. This can be done by calling
   ``$unitOfWork-&gt;recomputeSingleEntityChangeSet($classMetadata, $entity)``.

postFlush
</code></pre>

<p><code>postFlush</code> is called at the end of <code>EntityManager#flush()</code>.
<code>EntityManager#flush()</code> can <strong>NOT</strong> be called safely inside its listeners.</p>

<pre><code class="php"><br />    &lt;?php

    use Doctrine\ORM\Event\PostFlushEventArgs;

    class PostFlushExampleListener
    {
        public function postFlush(PostFlushEventArgs $args)
        {
            // ...
        }
    }

</code></pre>

<p>preUpdate
~~~~~~~~~</p>

<p>PreUpdate is the most restrictive to use event, since it is called
right before an update statement is called for an entity inside the
<code>EntityManager#flush()</code> method.</p>

<p>Changes to associations of the updated entity are never allowed in
this event, since Doctrine cannot guarantee to correctly handle
referential integrity at this point of the flush operation. This
event has a powerful feature however, it is executed with a
<code>PreUpdateEventArgs</code> instance, which contains a reference to the
computed change-set of this entity.</p>

<p>This means you have access to all the fields that have changed for
this entity with their old and new value. The following methods are
available on the <code>PreUpdateEventArgs</code>:</p>

<ul>
<li><code>getEntity()</code> to get access to the actual entity.</li>
<li><code>getEntityChangeSet()</code> to get a copy of the changeset array.
Changes to this returned array do not affect updating.</li>
<li><code>hasChangedField($fieldName)</code> to check if the given field name
of the current entity changed.</li>
<li><code>getOldValue($fieldName)</code> and <code>getNewValue($fieldName)</code> to
access the values of a field.</li>
<li><code>setNewValue($fieldName, $value)</code> to change the value of a
field to be updated.</li>
</ul>

<p>A simple example for this event looks like:</p>

<pre><code class="php"><br />    &lt;?php
    class NeverAliceOnlyBobListener
    {
        public function preUpdate(PreUpdateEventArgs $eventArgs)
        {
            if ($eventArgs-&gt;getEntity() instanceof User) {
                if ($eventArgs-&gt;hasChangedField('name') &amp;&amp; $eventArgs-&gt;getNewValue('name') == 'Alice') {
                    $eventArgs-&gt;setNewValue('name', 'Bob');
                }
            }
        }
    }

</code></pre>

<p>You could also use this listener to implement validation of all the
fields that have changed. This is more efficient than using a
lifecycle callback when there are expensive validations to call:</p>

<pre><code class="php"><br />    &lt;?php
    class ValidCreditCardListener
    {
        public function preUpdate(PreUpdateEventArgs $eventArgs)
        {
            if ($eventArgs-&gt;getEntity() instanceof Account) {
                if ($eventArgs-&gt;hasChangedField('creditCard')) {
                    $this-&gt;validateCreditCard($eventArgs-&gt;getNewValue('creditCard'));
                }
            }
        }

        private function validateCreditCard($no)
        {
            // throw an exception to interrupt flush event. Transaction will be rolled back.
        }
    }

</code></pre>

<p>Restrictions for this event:</p>

<ul>
<li>Changes to associations of the passed entities are not
recognized by the flush operation anymore.</li>
<li>Changes to fields of the passed entities are not recognized by
the flush operation anymore, use the computed change-set passed to
the event to modify primitive field values, e.g. use
<code>$eventArgs-&gt;setNewValue($field, $value);</code> as in the Alice to Bob example above.</li>
<li>Any calls to <code>EntityManager#persist()</code> or
<code>EntityManager#remove()</code>, even in combination with the UnitOfWork
API are strongly discouraged and don't work as expected outside the
flush operation.</li>
</ul>

<p>postUpdate, postRemove, postPersist
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~</p>

<p>The three post events are called inside <code>EntityManager#flush()</code>.
Changes in here are not relevant to the persistence in the
database, but you can use these events to alter non-persistable items,
like non-mapped fields, logging or even associated classes that are
directly mapped by Doctrine.</p>

<p>postLoad
~~~~~~~~</p>

<p>This event is called after an entity is constructed by the
EntityManager.</p>

<h2 id="entity-listeners">Entity listeners</h2>

<p>.. versionadded:: 2.4</p>

<p>An entity listener is a lifecycle listener class used for an entity.</p>

<ul>
<li>The entity listener's mapping may be applied to an entity class or mapped superclass.</li>
<li><p>An entity listener is defined by mapping the entity class with the corresponding mapping.</p>

<pre><code class="php"><br />    &lt;?php
    namespace MyProject\Entity;

    /** @Entity @EntityListeners({"UserListener"}) */
    class User
    {
        // ....
    }
</code></pre>

<pre><code class="xml"><br />    &lt;doctrine-mapping&gt;
        &lt;entity name="MyProject\Entity\User"&gt;
            &lt;entity-listeners&gt;
                &lt;entity-listener class="UserListener"/&gt;
            &lt;/entity-listeners&gt;
            &lt;!-- .... --&gt;
        &lt;/entity&gt;
    &lt;/doctrine-mapping&gt;
</code></pre>

<pre><code class="yaml"><br />    MyProject\Entity\User:
      type: entity
      entityListeners:
        UserListener:
      # ....
</code></pre></li>
</ul>

<p>.. _reference-entity-listeners:</p>

<p>Entity listeners class
~~~~~~~~~~~~~~~~~~~~~~</p>

<p>An <code>Entity Listener</code> could be any class, by default it should be a class with a no-arg constructor.</p>

<ul>
<li>Different from :ref:<code>reference-events-implementing-listeners</code> an <code>Entity Listener</code> is invoked just to the specified entity</li>
<li>An entity listener method receives two arguments, the entity instance and the lifecycle event.</li>
<li>The callback method can be defined by naming convention or specifying a method mapping.</li>
<li>When a listener mapping is not given the parser will use the naming convention to look for a matching method,
e.g. it will look for a public <code>preUpdate()</code> method if you are listening to the <code>preUpdate</code> event.</li>
<li>When a listener mapping is given the parser will not look for any methods using the naming convention.</li>
</ul>

<pre><code class="php"><br />    &lt;?php
    class UserListener
    {
        public function preUpdate(User $user, PreUpdateEventArgs $event)
        {
            // Do something on pre update.
        }
    }

</code></pre>

<p>To define a specific event listener method (one that does not follow the naming convention)
you need to map the listener method using the event type mapping:</p>

<p>.. configuration-block::</p>

<pre><code>```php

    &lt;?php
    class UserListener
    {
        /** @PrePersist */
        public function prePersistHandler(User $user, LifecycleEventArgs $event) { // ... }

        /** @PostPersist */
        public function postPersistHandler(User $user, LifecycleEventArgs $event) { // ... }

        /** @PreUpdate */
        public function preUpdateHandler(User $user, PreUpdateEventArgs $event) { // ... }

        /** @PostUpdate */
        public function postUpdateHandler(User $user, LifecycleEventArgs $event) { // ... }

        /** @PostRemove */
        public function postRemoveHandler(User $user, LifecycleEventArgs $event) { // ... }

        /** @PreRemove */
        public function preRemoveHandler(User $user, LifecycleEventArgs $event) { // ... }

        /** @PreFlush */
        public function preFlushHandler(User $user, PreFlushEventArgs $event) { // ... }

        /** @PostLoad */
        public function postLoadHandler(User $user, LifecycleEventArgs $event) { // ... }
    }
```

```xml

    &lt;doctrine-mapping&gt;
        &lt;entity name="MyProject\Entity\User"&gt;
             &lt;entity-listeners&gt;
                &lt;entity-listener class="UserListener"&gt;
                    &lt;lifecycle-callback type="preFlush"      method="preFlushHandler"/&gt;
                    &lt;lifecycle-callback type="postLoad"      method="postLoadHandler"/&gt;

                    &lt;lifecycle-callback type="postPersist"   method="postPersistHandler"/&gt;
                    &lt;lifecycle-callback type="prePersist"    method="prePersistHandler"/&gt;

                    &lt;lifecycle-callback type="postUpdate"    method="postUpdateHandler"/&gt;
                    &lt;lifecycle-callback type="preUpdate"     method="preUpdateHandler"/&gt;

                    &lt;lifecycle-callback type="postRemove"    method="postRemoveHandler"/&gt;
                    &lt;lifecycle-callback type="preRemove"     method="preRemoveHandler"/&gt;
                &lt;/entity-listener&gt;
            &lt;/entity-listeners&gt;
            &lt;!-- .... --&gt;
        &lt;/entity&gt;
    &lt;/doctrine-mapping&gt;
```

```yaml

    MyProject\Entity\User:
      type: entity
      entityListeners:
        UserListener:
          preFlush: [preFlushHandler]
          postLoad: [postLoadHandler]

          postPersist: [postPersistHandler]
          prePersist: [prePersistHandler]

          postUpdate: [postUpdateHandler]
          preUpdate: [preUpdateHandler]

          postRemove: [postRemoveHandler]
          preRemove: [preRemoveHandler]
      # ....
```
</code></pre>

<p>Entity listeners resolver
~~~~~~~~~~~~~~~~~~~~~~~~~~
Doctrine invokes the listener resolver to get the listener instance.</p>

<ul>
<li>A resolver allows you register a specific entity listener instance.</li>
<li>You can also implement your own resolver by extending <code>Doctrine\ORM\Mapping\DefaultEntityListenerResolver</code> or implementing <code>Doctrine\ORM\Mapping\EntityListenerResolver</code></li>
</ul>

<p>Specifying an entity listener instance :</p>

<pre><code class="php"><br />    &lt;?php
    // User.php

    /** @Entity @EntityListeners({"UserListener"}) */
    class User
    {
        // ....
    }

    // UserListener.php
    class UserListener
    {
        public function __construct(MyService $service)
        {
            $this-&gt;service = $service;
        }

        public function preUpdate(User $user, PreUpdateEventArgs $event)
        {
            $this-&gt;service-&gt;doSomething($user);
        }
    }

    // register a entity listener.
    $listener = $container-&gt;get('user_listener');
    $em-&gt;getConfiguration()-&gt;getEntityListenerResolver()-&gt;register($listener);

</code></pre>

<p>Implementing your own resolver :</p>

<pre><code class="php"><br />    &lt;?php
    class MyEntityListenerResolver extends \Doctrine\ORM\Mapping\DefaultEntityListenerResolver
    {
        public function __construct($container)
        {
            $this-&gt;container = $container;
        }

        public function resolve($className)
        {
            // resolve the service id by the given class name;
            $id = 'user_listener';

            return $this-&gt;container-&gt;get($id);
        }
    }

    // configure the listener resolver.
    $em-&gt;getConfiguration()-&gt;setEntityListenerResolver($container-&gt;get('my_resolver'));

</code></pre>

<h2 id="load-classmetadata-event">Load ClassMetadata Event</h2>

<p>When the mapping information for an entity is read, it is populated
in to a <code>ClassMetadataInfo</code> instance. You can hook in to this
process and manipulate the instance.</p>

<pre><code class="php"><br />    &lt;?php
    $test = new TestEvent();
    $metadataFactory = $em-&gt;getMetadataFactory();
    $evm = $em-&gt;getEventManager();
    $evm-&gt;addEventListener(Events::loadClassMetadata, $test);

    class TestEvent
    {
        public function loadClassMetadata(\Doctrine\ORM\Event\LoadClassMetadataEventArgs $eventArgs)
        {
            $classMetadata = $eventArgs-&gt;getClassMetadata();
            $fieldMapping = array(
                'fieldName' =&gt; 'about',
                'type' =&gt; 'string',
                'length' =&gt; 255
            );
            $classMetadata-&gt;mapField($fieldMapping);
        }
    }

</code></pre>
