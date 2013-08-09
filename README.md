clone.js nano
=====

The true prototype-based nano-framework.
This is all sources:

    function clone(/** Object */proto, /** ObjectLiteralOnly */properdies){
        properdies.__proto__ = proto;
        return properdies;
    }
    
### What is the clone?

`clone()` function produces new objects — clones.  
**Clone — this is the lazy shallow copy**, i.e., it is actually not a copy, it's just a reference to the object,
with one difference: if you will add/replace any of its properties, it would not affect the cloned object (prototype).  
All JavaScript objects are clones of `Object.prototype` (except itself and objects, created by `Object.create(null)`).

### How to use

Forget about classes.    
Instead of creating class (function), create prototype (object):

    var duck$ = {
        quack: function(){
            console.log( this.firstName +" "+ this.lastName +": Quack-quack!");
        }
    };

*The classic way:*

    var Duck = function(firstName, lastName){
        this.firstName = firstName;
        this.lastName = lastName;
    }
    Duck.prototype.quack = function(){
        console.log( this.firstName +" "+ this.lastName +": Quack-quack!");
    }

Inheritance is simple (talkingDuck$ extends duck$):

    var talkingDuck$ = clone(duck$, {
        quack: function(){
            duck$.quack.call(this);
            console.log("My name is "+ this.name +"!");
        }
    });

*The classic way:*

    var TalkingDuck = function(name){
        Duck.apply(this, arguments);
    }
    TalkingDuck.prototype = new Duck;
    TalkingDuck.prototype.constructor = TalkingDuck;
    TalkingDuck.prototype.quack = function(){
        Duck.prototype.quack.call(this);
        console.log("My name is "+ this.name +"!");
    }

Forget about the `new` operator, use `clone` to create instances:

    var donald = clone(talkingDuck$, {firstName: "Donald", lastName: "Duck"});
    donald.quack();// Donald Duck: Quack-quack! 
                   // My name is Donald!

*The classic way:*

    var donald = new TalkingDuck("Donald", "Duck");
    donald.quack();// Donald Duck: Quack-quack! 
                   // My name is Donald!

Forget about the `instanceof` operator, use JS native `.isPrototypeOf()` method instead:

    duck$.isPrototypeOf(donald);// true

*The classic way:*

    donald instanceof Duck;// true
