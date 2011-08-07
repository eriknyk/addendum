
Addendum - Annotations support for PHP
======================================

DocBlock/JavaDoc annotations support for PHP5. Supporting single and multi valued annotations accessible through extended Reflection API.

Example annotations:
--------------------

    @SimpleAnnotation
    @SingleValuedAnnotation(true)
    @SingleValuedAnnotation(-3.141592)
    @SingleValuedAnnotation('Hello World!')
    @SingleValuedAnnotationWithArray({1, 2, 3})
    @MultiValuedAnnotation(key = 'value', anotherKey = false, andMore = 1234)


Addendum annotation basics
--------------------------

Creating your first annotation
Annotations in Addendum are simple classes. To make a new Persistent annotation all you need to do is:

class Persistent extends Annotation {}
NOTE: Make sure that annotation class name starts with an uppercase letter.

Annotating a class
With this defined annotation, you can annotate a different class/method/property with it. Annotating in Addendum is done by creating a doc block comment with annotation syntax you might know from Java.

    /** @Persistent */
    class Person {
       // some code
    }
NOTE: Please make sure that there are two asterisks at beginning. Doc blocks differ from normal comments.

Accessing annotations
---------------------

Annotations of a class are accessed through extended reflection API. A reflecting class can be created in two ways:

    $reflection = new ReflectionAnnotatedClass('Person'); // by class name

    $person = new Person();
    $reflection = new ReflectionAnnotatedClass($person); // by instance
    
To find out if a class is annotated by Persistent annotation use:

    $reflection->hasAnnotation('Persistent'); // true

To access method/property annotations you can use ReflectionAnnotatedMethod and ReflectionAnnotatedProperty.

Valued annotations
Single valued annotation
An annotation can also hold a value. Let us create a Table annotation to demonstrate this feature.

    class Table extends Annotation {}

Now let us annotate a class with a valued annotation.

    /** @Table("people") */
    class Person {
       // some code
    }
This value can be then accessed through reflection API

    $reflection = new ReflectionAnnotatedClass('Person'); // by class name    
    $reflection->getAnnotation('Table')->value; // contains string "people"

Multi valued annotation
-----------------------

Annotations can also hold multiple values. A multi valued annotation can be defined easily like this

    class Secured extends Annotation {
        public $role;
        public $level;
    }
Multi valued annotations are used like this:

    /** @Secured(role = "admin", level = 2) */
    class Administration {
        // some code
    }
To access these field just use extended reflection API.

$reflection = new ReflectionAnnotatedClass('Administration'); // by class name
$annotation = $reflection->getAnnotation('Secured');
$annotation->role; // contains string "admin"
$annotation->level; // contains integer "2"
Array values in annotations
Annotations can even hold arrays of values using {} syntax. For example:

class RolesAllowed extends Annotation {}

    /** @RolesAllowed({'admin', 'web-editor'}) */
    class CMS {
     // some code
    }

    $reflection = new ReflectionAnnotatedClass('CMS');
    $annotation = $reflection->getAnnotation('RolesAllowed');
    $annotation->value; // contains array('admin', 'web-editor')
    
Of course you can also use associative arrays.

    @Annotation({key1 = 1, key2 = 2, key3 = 3})
Or even mix them and use nested arrays any way you like!

    @Annotation({key1 = 1, 2, 3, {4, key = 5}})
