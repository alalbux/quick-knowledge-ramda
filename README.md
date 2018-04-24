# Quick knowledge Ramda

## Ramda
Biblioteca de Javascript funcional.

### __ 

A special placeholder value used to specify "gaps" within curried functions, allowing partial application of any combination of arguments, regardless of their positions.

If `g` is a curried ternary function and `_` is `R.__`, the following are equivalent:
```js
g(1, 2, 3)
g(_, 2, 3)(1)
g(_, _, 3)(1)(2)
g(_, _, 3)(1, 2)
g(_, 2, _)(1, 3)
g(_, 2)(1)(3)
g(_, 2)(1, 3)
g(_, 2)(_, 3)(1)
```

```js
var greet = R.replace('{name}', R.__, 'Hello, {name}!');
greet('Alice'); //=> 'Hello, Alice!'
```
### add
`Number → Number → Number`
```js
R.add(2, 3);       //=>  5
R.add(7)(10);      //=> 17
```

### addIndex
`((a … → b) … → [a] → *) → (a …, Int, [a] → b) … → [a] → *)`

fn
A list iteration function that does not pass index or list to its callback
Returns

function An altered list iteration function that passes (item, index, list) to its callback
Creates a new list iteration function from an existing one by adding two new parameters to its callback function: the current index, and the entire list.

This would turn, for instance, R.map function into one that more closely resembles Array.prototype.map. Note that this will only work for functions in which the iteration callback function is the first parameter, and where the list is the last parameter. (This latter might be unimportant if the list parameter is not used.)

var mapIndexed = R.addIndex(R.map);
mapIndexed((val, idx) => idx + '-' + val, ['f', 'o', 'o', 'b', 'a', 'r']);
//=> ['0-f', '1-o', '2-o', '3-b', '4-a', '5-r']



### adjust Added in v0.14.0

(a → a) → Number → [a] → [a]
Parameters

fn
The function to apply.
 idx
The index.
 list
An array-like object whose value at the supplied index will be replaced.
Returns

Array A copy of the supplied array-like object with the element at index `idx` replaced with the value returned by applying `fn` to the existing element.
Applies a function to the value at the given index of an array, returning a new copy of the array with the element at the given index replaced with the result of the function application.

See also update.

R.adjust(R.add(10), 1, [1, 2, 3]);     //=> [1, 12, 3]
R.adjust(R.add(10))(1)([1, 2, 3]);     //=> [1, 12, 3]


### all Added in v0.1.0

(a → Boolean) → [a] → Boolean
Parameters

fn
The predicate function.
 list
The array to consider.
Returns

Boolean `true` if the predicate is satisfied by every element, `false` otherwise.
Returns true if all elements of the list match the predicate, false if there are any that don't.

Dispatches to the all method of the second argument, if present.

Acts as a transducer if a transformer is given in list position.

See also any, none, transduce.

var equals3 = R.equals(3);
R.all(equals3)([3, 3, 3, 3]); //=> true
R.all(equals3)([3, 3, 1, 3]); //=> false


### allPass Added in v0.9.0

[(*… → Boolean)] → (*… → Boolean)
Parameters

predicates
An array of predicates to check
Returns

function The combined predicate
Takes a list of predicates and returns a predicate that returns true for a given list of arguments if every one of the provided predicates is satisfied by those arguments.

The function returned is a curried function whose arity matches that of the highest-arity predicate.

See also anyPass.

var isQueen = R.propEq('rank', 'Q');
var isSpade = R.propEq('suit', '♠︎');
var isQueenOfSpades = R.allPass([isQueen, isSpade]);

isQueenOfSpades({rank: 'Q', suit: '♣︎'}); //=> false
isQueenOfSpades({rank: 'Q', suit: '♠︎'}); //=> true

### always Added in v0.1.0

a → (* → a)
Parameters

val
The value to wrap in a function
Returns

function A Function :: * -> val.
Returns a function that always returns the given value. Note that for non-primitives the value returned is a reference to the original value.

This function is known as const, constant, or K (for K combinator) in other languages and libraries.

var t = R.always('Tee');
t(); //=> 'Tee'


### and Added in v0.1.0

a → b → a | b
Parameters

a
b
Returns

Any the first argument if it is falsy, otherwise the second argument.
Returns true if both arguments are true; false otherwise.

See also both.

R.and(true, true); //=> true
R.and(true, false); //=> false
R.and(false, true); //=> false
R.and(false, false); //=> false


### any Added in v0.1.0

(a → Boolean) → [a] → Boolean
Parameters

fn
The predicate function.
 list
The array to consider.
Returns

Boolean `true` if the predicate is satisfied by at least one element, `false` otherwise.
Returns true if at least one of elements of the list match the predicate, false otherwise.

Dispatches to the any method of the second argument, if present.

Acts as a transducer if a transformer is given in list position.

See also all, none, transduce.

var lessThan0 = R.flip(R.lt)(0);
var lessThan2 = R.flip(R.lt)(2);
R.any(lessThan0)([1, 2]); //=> false
R.any(lessThan2)([1, 2]); //=> true


### anyPass Added in v0.9.0

[(*… → Boolean)] → (*… → Boolean)
Parameters

predicates
An array of predicates to check
Returns

function The combined predicate
Takes a list of predicates and returns a predicate that returns true for a given list of arguments if at least one of the provided predicates is satisfied by those arguments.

The function returned is a curried function whose arity matches that of the highest-arity predicate.

See also allPass.

var isClub = R.propEq('suit', '♣');
var isSpade = R.propEq('suit', '♠');
var isBlackCard = R.anyPass([isClub, isSpade]);

isBlackCard({rank: '10', suit: '♣'}); //=> true
isBlackCard({rank: 'Q', suit: '♠'}); //=> true
isBlackCard({rank: 'Q', suit: '♦'}); //=> false


### ap Added in v0.3.0

[a → b] → [a] → [b]
Apply f => f (a → b) → f a → f b
(a → b → c) → (a → b) → (a → c)
Parameters

applyF
applyX
Returns

*
ap applies a list of functions to a list of values.

Dispatches to the ap method of the second argument, if present. Also treats curried functions as applicatives.

R.ap([R.multiply(2), R.add(3)], [1,2,3]); //=> [2, 4, 6, 4, 5, 6]
R.ap([R.concat('tasty '), R.toUpper], ['pizza', 'salad']); //=> ["tasty pizza", "tasty salad", "PIZZA", "SALAD"]

// R.ap can also be used as S combinator
// when only two functions are passed
R.ap(R.concat, R.toUpper)('Ramda') //=> 'RamdaRAMDA'


### aperture Added in v0.12.0

Number → [a] → [[a]]
Parameters

n
The size of the tuples to create
 list
The list to split into n-length tuples
Returns

Array The resulting list of `n`-length tuples
Returns a new list, composed of n-tuples of consecutive elements. If n is greater than the length of the list, an empty list is returned.

Acts as a transducer if a transformer is given in list position.

See also transduce.

R.aperture(2, [1, 2, 3, 4, 5]); //=> [[1, 2], [2, 3], [3, 4], [4, 5]]
R.aperture(3, [1, 2, 3, 4, 5]); //=> [[1, 2, 3], [2, 3, 4], [3, 4, 5]]
R.aperture(7, [1, 2, 3, 4, 5]); //=> []
append Added in v0.1.0

a → [a] → [a]
Parameters

el
The element to add to the end of the new list.
 list
The list of elements to add a new item to. list.
Returns

Array A new list containing the elements of the old list followed by `el`.
Returns a new list containing the contents of the given list, followed by the given element.

See also prepend.

R.append('tests', ['write', 'more']); //=> ['write', 'more', 'tests']
R.append('tests', []); //=> ['tests']
R.append(['tests'], ['write', 'more']); //=> ['write', 'more', ['tests']]


### apply Added in v0.7.0

(*… → a) → [*] → a
Parameters

fn
The function which will be called with args
 args
The arguments to call fn with
Returns

* result The result, equivalent to `fn(...args)`
Applies function fn to the argument list args. This is useful for creating a fixed-arity function from a variadic function. fn should be a bound function if context is significant.

See also call, unapply.

var nums = [1, 2, 3, -99, 42, 6, 7];
R.apply(Math.max, nums); //=> 42
applySpec Added in v0.20.0

{k: ((a, b, …, m) → v)} → ((a, b, …, m) → {k: v})
Parameters

spec
an object recursively mapping properties to functions for producing the values for these properties.
Returns

function A function that returns an object of the same structure as `spec', with each property set to the value returned by calling its associated function with the supplied arguments.
Given a spec object recursively mapping properties to functions, creates a function producing an object of the same structure, by mapping each property to the result of calling its associated function with the supplied arguments.

See also converge, juxt.

var getMetrics = R.applySpec({
  sum: R.add,
  nested: { mul: R.multiply }
});
getMetrics(2, 4); // => { sum: 6, nested: { mul: 8 } }


### applyTo Added in v0.25.0

a → (a → b) → b
Parameters

x
The value
 f
The function to apply
Returns

* The result of applying `f` to `x`
Takes a value and applies a function to it.

This function is also known as the thrush combinator.

var t42 = R.applyTo(42);
t42(R.identity); //=> 42
t42(R.add(1)); //=> 43


### ascend Added in v0.23.0

Ord b => (a → b) → a → a → Number
Parameters

fn
A function of arity one that returns a value that can be compared
 a
The first item to be compared.
 b
The second item to be compared.
Returns

Number `-1` if fn(a) < fn(b), `1` if fn(b) < fn(a), otherwise `0`
Makes an ascending comparator function out of a function that returns a value that can be compared with < and >.

See also descend.

var byAge = R.ascend(R.prop('age'));
var people = [
  // ...
];
var peopleByYoungestFirst = R.sort(byAge, people);

### assoc Added in v0.8.0

String → a → {k: v} → {k: v}
Parameters

prop
The property name to set
 val
The new value
 obj
The object to clone
Returns

Object A new object equivalent to the original except for the changed property.
Makes a shallow clone of an object, setting or overriding the specified property with the given value. Note that this copies and flattens prototype properties onto the new object as well. All non-primitive properties are copied by reference.

See also dissoc.

R.assoc('c', 3, {a: 1, b: 2}); //=> {a: 1, b: 2, c: 3}

### assocPath Added in v0.8.0

[Idx] → a → {a} → {a}
Idx = String | Int
Parameters

path
the path to set
 val
The new value
 obj
The object to clone
Returns

Object A new object equivalent to the original except along the specified path.
Makes a shallow clone of an object, setting or overriding the nodes required to create the given path, and placing the specific value at the tail end of that path. Note that this copies and flattens prototype properties onto the new object as well. All non-primitive properties are copied by reference.

See also dissocPath.

R.assocPath(['a', 'b', 'c'], 42, {a: {b: {c: 0}}}); //=> {a: {b: {c: 42}}}

// Any missing or non-object keys in path will be overridden
R.assocPath(['a', 'b', 'c'], 42, {a: 5}); //=> {a: {b: {c: 42}}}


### binary Added in v0.2.0

(* → c) → (a, b → c)
Parameters

fn
The function to wrap.
Returns

function A new function wrapping `fn`. The new function is guaranteed to be of arity 2.
Wraps a function of any arity (including nullary) in a function that accepts exactly 2 parameters. Any extraneous parameters will not be passed to the supplied function.

See also nAry, unary.

var takesThreeArgs = function(a, b, c) {
  return [a, b, c];
};
takesThreeArgs.length; //=> 3
takesThreeArgs(1, 2, 3); //=> [1, 2, 3]

var takesTwoArgs = R.binary(takesThreeArgs);
takesTwoArgs.length; //=> 2
// Only 2 arguments are passed to the wrapped function
takesTwoArgs(1, 2, 3); //=> [1, 2, undefined]

### bind Added in v0.6.0

(* → *) → {*} → (* → *)
Parameters

fn
The function to bind to context
 thisObj
The context to bind fn to
Returns

function A function that will execute in the context of `thisObj`.
Creates a function that is bound to a context. Note: R.bind does not provide the additional argument-binding capabilities of Function.prototype.bind.

See also partial.

var log = R.bind(console.log, console);
R.pipe(R.assoc('a', 2), R.tap(log), R.assoc('a', 3))({a: 1}); //=> {a: 3}
// logs {a: 2}


### both Added in v0.12.0

(*… → Boolean) → (*… → Boolean) → (*… → Boolean)
Parameters

f
A predicate
 g
Another predicate
Returns

function a function that applies its arguments to `f` and `g` and `&&`s their outputs together.
A function which calls the two provided functions and returns the && of the results. It returns the result of the first function if it is false-y and the result of the second function otherwise. Note that this is short-circuited, meaning that the second function will not be invoked if the first returns a false-y value.

In addition to functions, R.both also accepts any fantasy-land compatible applicative functor.

See also and.

var gt10 = R.gt(R.__, 10)
var lt20 = R.lt(R.__, 20)
var f = R.both(gt10, lt20);
f(15); //=> true
f(30); //=> false
### call Added in v0.9.0

(*… → a),*… → a
Parameters

fn
The function to apply to the remaining arguments.
 args
Any number of positional arguments.
Returns

*
Returns the result of calling its first argument with the remaining arguments. This is occasionally useful as a converging function for R.converge: the first branch can produce a function while the remaining branches produce values to be passed to that function as its arguments.

See also apply.

R.call(R.add, 1, 2); //=> 3

var indentN = R.pipe(R.repeat(' '),
                     R.join(''),
                     R.replace(/^(?!$)/gm));

var format = R.converge(R.call, [
                            R.pipe(R.prop('indent'), indentN),
                            R.prop('value')
                        ]);

format({indent: 2, value: 'foo\nbar\nbaz\n'}); //=> '  foo\n  bar\n  baz\n'


### chain Added in v0.3.0

Chain m => (a → m b) → m a → m b
Parameters

fn
The function to map with
 list
The list to map over
Returns

Array The result of flat-mapping `list` with `fn`
chain maps a function over a list and concatenates the results. chain is also known as flatMap in some libraries

Dispatches to the chain method of the second argument, if present, according to the FantasyLand Chain spec.

var duplicate = n => [n, n];
R.chain(duplicate, [1, 2, 3]); //=> [1, 1, 2, 2, 3, 3]

R.chain(R.append, R.head)([1, 2, 3]); //=> [1, 2, 3, 1]

### clamp Added in v0.20.0

Ord a => a → a → a → a
Parameters

minimum
The lower limit of the clamp (inclusive)
 maximum
The upper limit of the clamp (inclusive)
 value
Value to be clamped
Returns

Number Returns `minimum` when `val < minimum`, `maximum` when `val > maximum`, returns `val` otherwise
Restricts a number to be within a range.

Also works for other ordered types such as Strings and Dates.

R.clamp(1, 10, -5) // => 1
R.clamp(1, 10, 15) // => 10
R.clamp(1, 10, 4)  // => 4
clone Added in v0.1.0

{*} → {*}
Parameters

value
The object or array to clone
Returns

* A deeply cloned copy of `val`
Creates a deep copy of the value which may contain (nested) Arrays and Objects, Numbers, Strings, Booleans and Dates. Functions are assigned by reference rather than copied

Dispatches to a clone method if present.

var objects = [{}, {}, {}];
var objectsClone = R.clone(objects);
objects === objectsClone; //=> false
objects[0] === objectsClone[0]; //=> false

### comparator Added in v0.1.0

((a, b) → Boolean) → ((a, b) → Number)
Parameters

pred
A predicate function of arity two which will return true if the first argument is less than the second, false otherwise
Returns

function A Function :: a -> b -> Int that returns `-1` if a < b, `1` if b < a, otherwise `0`
Makes a comparator function out of a function that reports whether the first element is less than the second.

var byAge = R.comparator((a, b) => a.age < b.age);
var people = [
  // ...
];
var peopleByIncreasingAge = R.sort(byAge, people);

### complement Added in v0.12.0

(*… → *) → (*… → Boolean)
Parameters

f
Returns

function
Takes a function f and returns a function g such that if called with the same arguments when f returns a "truthy" value, g returns false and when f returns a "falsy" value g returns true.

R.complement may be applied to any functor

See also not.

var isNotNil = R.complement(R.isNil);
isNil(null); //=> true
isNotNil(null); //=> false
isNil(7); //=> false
isNotNil(7); //=> true

### compose Added in v0.1.0

((y → z), (x → y), …, (o → p), ((a, b, …, n) → o)) → ((a, b, …, n) → z)
Parameters

...functions
The functions to compose
Returns

function
Performs right-to-left function composition. The rightmost function may have any arity; the remaining functions must be unary.

Note: The result of compose is not automatically curried.

See also pipe.

var classyGreeting = (firstName, lastName) => "The name's " + lastName + ", " + firstName + " " + lastName
var yellGreeting = R.compose(R.toUpper, classyGreeting);
yellGreeting('James', 'Bond'); //=> "THE NAME'S BOND, JAMES BOND"

R.compose(Math.abs, R.add(1), R.multiply(2))(-4) //=> 7

### composeK Added in v0.16.0

Chain m => ((y → m z), (x → m y), …, (a → m b)) → (a → m z)
Parameters

...functions
The functions to compose
Returns

function
Returns the right-to-left Kleisli composition of the provided functions, each of which must return a value of a type supported by chain.

R.composeK(h, g, f) is equivalent to R.compose(R.chain(h), R.chain(g), f).

See also pipeK.

//  get :: String -> Object -> Maybe *
 var get = R.curry((propName, obj) => Maybe(obj[propName]))

 //  getStateCode :: Maybe String -> Maybe String
 var getStateCode = R.composeK(
   R.compose(Maybe.of, R.toUpper),
   get('state'),
   get('address'),
   get('user'),
 );
 getStateCode({"user":{"address":{"state":"ny"}}}); //=> Maybe.Just("NY")
 getStateCode({}); //=> Maybe.Nothing()
composeP Added in v0.10.0

((y → Promise z), (x → Promise y), …, (a → Promise b)) → (a → Promise z)
Parameters

functions
The functions to compose
Returns

function
Performs right-to-left composition of one or more Promise-returning functions. The rightmost function may have any arity; the remaining functions must be unary.

See also pipeP.

var db = {
  users: {
    JOE: {
      name: 'Joe',
      followers: ['STEVE', 'SUZY']
    }
  }
}

// We'll pretend to do a db lookup which returns a promise
var lookupUser = (userId) => Promise.resolve(db.users[userId])
var lookupFollowers = (user) => Promise.resolve(user.followers)
lookupUser('JOE').then(lookupFollowers)

//  followersForUser :: String -> Promise [UserId]
var followersForUser = R.composeP(lookupFollowers, lookupUser);
followersForUser('JOE').then(followers => console.log('Followers:', followers))
// Followers: ["STEVE","SUZY"]

### concat Added in v0.1.0

[a] → [a] → [a]
String → String → String
Parameters

firstList
The first list
 secondList
The second list
Returns

Array A list consisting of the elements of `firstList` followed by the elements of `secondList`.
Returns the result of concatenating the given lists or strings.

Note: R.concat expects both arguments to be of the same type, unlike the native Array.prototype.concat method. It will throw an error if you concat an Array with a non-Array value.

Dispatches to the concat method of the first argument, if present. Can also concatenate two members of a fantasy-land compatible semigroup.

R.concat('ABC', 'DEF'); // 'ABCDEF'
R.concat([4, 5, 6], [1, 2, 3]); //=> [4, 5, 6, 1, 2, 3]
R.concat([], []); //=> []
### cond Added in v0.6.0

[[(*… → Boolean),(*… → *)]] → (*… → *)
Parameters

pairs
A list of [predicate, transformer]
Returns

function
Returns a function, fn, which encapsulates if/else, if/else, ... logic. R.cond takes a list of [predicate, transformer] pairs. All of the arguments to fn are applied to each of the predicates in turn until one returns a "truthy" value, at which point fn returns the result of applying its arguments to the corresponding transformer. If none of the predicates matches, fn returns undefined.

var fn = R.cond([
  [R.equals(0),   R.always('water freezes at 0°C')],
  [R.equals(100), R.always('water boils at 100°C')],
  [R.T,           temp => 'nothing special happens at ' + temp + '°C']
]);
fn(0); //=> 'water freezes at 0°C'
fn(50); //=> 'nothing special happens at 50°C'
fn(100); //=> 'water boils at 100°C'
construct Added in v0.1.0

(* → {*}) → (* → {*})
Parameters

fn
The constructor function to wrap.
Returns

function A wrapped, curried constructor function.
Wraps a constructor function inside a curried function that can be called with the same arguments and returns the same type.

See also invoker.

// Constructor function
function Animal(kind) {
  this.kind = kind;
};
Animal.prototype.sighting = function() {
  return "It's a " + this.kind + "!";
}

var AnimalConstructor = R.construct(Animal)

// Notice we no longer need the 'new' keyword:
AnimalConstructor('Pig'); //=> {"kind": "Pig", "sighting": function (){...}};

var animalTypes = ["Lion", "Tiger", "Bear"];
var animalSighting = R.invoker(0, 'sighting');
var sightNewAnimal = R.compose(animalSighting, AnimalConstructor);
R.map(sightNewAnimal, animalTypes); //=> ["It's a Lion!", "It's a Tiger!", "It's a Bear!"]
constructN Added in v0.4.0

Number → (* → {*}) → (* → {*})
Parameters

n
The arity of the constructor function.
 Fn
The constructor function to wrap.
Returns

function A wrapped, curried constructor function.
Wraps a constructor function inside a curried function that can be called with the same arguments and returns the same type. The arity of the function returned is specified to allow using variadic constructor functions.

// Variadic Constructor function
function Salad() {
  this.ingredients = arguments;
}

Salad.prototype.recipe = function() {
  var instructions = R.map(ingredient => 'Add a dollop of ' + ingredient, this.ingredients);
  return R.join('\n', instructions);
};

var ThreeLayerSalad = R.constructN(3, Salad);

// Notice we no longer need the 'new' keyword, and the constructor is curried for 3 arguments.
var salad = ThreeLayerSalad('Mayonnaise')('Potato Chips')('Ketchup');

console.log(salad.recipe());
// Add a dollop of Mayonnaise
// Add a dollop of Potato Chips
// Add a dollop of Ketchup
contains Added in v0.1.0

a → [a] → Boolean
Parameters

a
The item to compare against.
 list
The array to consider.
Returns

Boolean `true` if an equivalent item is in the list, `false` otherwise.
Returns true if the specified value is equal, in R.equals terms, to at least one element of the given list; false otherwise.

See also any.

R.contains(3, [1, 2, 3]); //=> true
R.contains(4, [1, 2, 3]); //=> false
R.contains({ name: 'Fred' }, [{ name: 'Fred' }]); //=> true
R.contains([42], [[42]]); //=> true
converge Added in v0.4.2

((x1, x2, …) → z) → [((a, b, …) → x1), ((a, b, …) → x2), …] → (a → b → … → z)
Parameters

after
A function. after will be invoked with the return values of fn1 and fn2 as its arguments.
 functions
A list of functions.
Returns

function A new function.
Accepts a converging function and a list of branching functions and returns a new function. When invoked, this new function is applied to some arguments, each branching function is applied to those same arguments. The results of each branching function are passed as arguments to the converging function to produce the return value.

See also useWith.

var average = R.converge(R.divide, [R.sum, R.length])
average([1, 2, 3, 4, 5, 6, 7]) //=> 4

var strangeConcat = R.converge(R.concat, [R.toUpper, R.toLower])
strangeConcat("Yodel") //=> "YODELyodel"
countBy Added in v0.1.0

(a → String) → [a] → {*}
Parameters

fn
The function used to map values to keys.
 list
The list to count elements from.
Returns

Object An object mapping keys to number of occurrences in the list.
Counts the elements of a list according to how many match each value of a key generated by the supplied function. Returns an object mapping the keys produced by fn to the number of occurrences in the list. Note that all keys are coerced to strings because of how JavaScript objects work.

Acts as a transducer if a transformer is given in list position.

var numbers = [1.0, 1.1, 1.2, 2.0, 3.0, 2.2];
R.countBy(Math.floor)(numbers);    //=> {'1': 3, '2': 2, '3': 1}

var letters = ['a', 'b', 'A', 'a', 'B', 'c'];
R.countBy(R.toLower)(letters);   //=> {'a': 3, 'b': 2, 'c': 1}
curry Added in v0.1.0

(* → a) → (* → a)
Parameters

fn
The function to curry.
Returns

function A new, curried function.
Returns a curried equivalent of the provided function. The curried function has two unusual capabilities. First, its arguments needn't be provided one at a time. If f is a ternary function and g is R.curry(f), the following are equivalent:

g(1)(2)(3)
g(1)(2, 3)
g(1, 2)(3)
g(1, 2, 3)
Secondly, the special placeholder value R.__ may be used to specify "gaps", allowing partial application of any combination of arguments, regardless of their positions. If g is as above and _ is R.__, the following are equivalent:

g(1, 2, 3)
g(_, 2, 3)(1)
g(_, _, 3)(1)(2)
g(_, _, 3)(1, 2)
g(_, 2)(1)(3)
g(_, 2)(1, 3)
g(_, 2)(_, 3)(1)
See also curryN.

var addFourNumbers = (a, b, c, d) => a + b + c + d;

var curriedAddFourNumbers = R.curry(addFourNumbers);
var f = curriedAddFourNumbers(1, 2);
var g = f(3);
g(4); //=> 10
curryN Added in v0.5.0

Number → (* → a) → (* → a)
Parameters

length
The arity for the returned function.
 fn
The function to curry.
Returns

function A new, curried function.
Returns a curried equivalent of the provided function, with the specified arity. The curried function has two unusual capabilities. First, its arguments needn't be provided one at a time. If g is R.curryN(3, f), the following are equivalent:

g(1)(2)(3)
g(1)(2, 3)
g(1, 2)(3)
g(1, 2, 3)
Secondly, the special placeholder value R.__ may be used to specify "gaps", allowing partial application of any combination of arguments, regardless of their positions. If g is as above and _ is R.__, the following are equivalent:

g(1, 2, 3)
g(_, 2, 3)(1)
g(_, _, 3)(1)(2)
g(_, _, 3)(1, 2)
g(_, 2)(1)(3)
g(_, 2)(1, 3)
g(_, 2)(_, 3)(1)
See also curry.

var sumArgs = (...args) => R.sum(args);

var curriedAddFourNumbers = R.curryN(4, sumArgs);
var f = curriedAddFourNumbers(1, 2);
var g = f(3);
g(4); //=> 10
dec Added in v0.9.0

Number → Number
Parameters

n
Returns

Number n - 1
Decrements its argument.

See also inc.

R.dec(42); //=> 41
defaultTo Added in v0.10.0

a → b → a | b
Parameters

default
The default value.
 val
val will be returned instead of default unless val is null, undefined or NaN.
Returns

* The second value if it is not `null`, `undefined` or `NaN`, otherwise the default value
Returns the second argument if it is not null, undefined or NaN; otherwise the first argument is returned.

var defaultTo42 = R.defaultTo(42);

defaultTo42(null);  //=> 42
defaultTo42(undefined);  //=> 42
defaultTo42('Ramda');  //=> 'Ramda'
// parseInt('string') results in NaN
defaultTo42(parseInt('string')); //=> 42
descend Added in v0.23.0

Ord b => (a → b) → a → a → Number
Parameters

fn
A function of arity one that returns a value that can be compared
 a
The first item to be compared.
 b
The second item to be compared.
Returns

Number `-1` if fn(a) > fn(b), `1` if fn(b) > fn(a), otherwise `0`
Makes a descending comparator function out of a function that returns a value that can be compared with < and >.

See also ascend.

var byAge = R.descend(R.prop('age'));
var people = [
  // ...
];
var peopleByOldestFirst = R.sort(byAge, people);
difference Added in v0.1.0

[*] → [*] → [*]
Parameters

list1
The first list.
 list2
The second list.
Returns

Array The elements in `list1` that are not in `list2`.
Finds the set (i.e. no duplicates) of all elements in the first list not contained in the second list. Objects and Arrays are compared in terms of value equality, not reference equality.

See also differenceWith, symmetricDifference, symmetricDifferenceWith, without.

R.difference([1,2,3,4], [7,6,5,4,3]); //=> [1,2]
R.difference([7,6,5,4,3], [1,2,3,4]); //=> [7,6,5]
R.difference([{a: 1}, {b: 2}], [{a: 1}, {c: 3}]) //=> [{b: 2}]
differenceWith Added in v0.1.0

((a, a) → Boolean) → [a] → [a] → [a]
Parameters

pred
A predicate used to test whether two items are equal.
 list1
The first list.
 list2
The second list.
Returns

Array The elements in `list1` that are not in `list2`.
Finds the set (i.e. no duplicates) of all elements in the first list not contained in the second list. Duplication is determined according to the value returned by applying the supplied predicate to two list elements.

See also difference, symmetricDifference, symmetricDifferenceWith.

var cmp = (x, y) => x.a === y.a;
var l1 = [{a: 1}, {a: 2}, {a: 3}];
var l2 = [{a: 3}, {a: 4}];
R.differenceWith(cmp, l1, l2); //=> [{a: 1}, {a: 2}]
dissoc Added in v0.10.0

String → {k: v} → {k: v}
Parameters

prop
The name of the property to dissociate
 obj
The object to clone
Returns

Object A new object equivalent to the original but without the specified property
Returns a new object that does not contain a prop property.

See also assoc.

R.dissoc('b', {a: 1, b: 2, c: 3}); //=> {a: 1, c: 3}
dissocPath Added in v0.11.0

[Idx] → {k: v} → {k: v}
Idx = String | Int
Parameters

path
The path to the value to omit
 obj
The object to clone
Returns

Object A new object without the property at path
Makes a shallow clone of an object, omitting the property at the given path. Note that this copies and flattens prototype properties onto the new object as well. All non-primitive properties are copied by reference.

See also assocPath.

R.dissocPath(['a', 'b', 'c'], {a: {b: {c: 42}}}); //=> {a: {b: {}}}
divide Added in v0.1.0

Number → Number → Number
Parameters

a
The first value.
 b
The second value.
Returns

Number The result of `a / b`.
Divides two numbers. Equivalent to a / b.

See also multiply.

R.divide(71, 100); //=> 0.71

var half = R.divide(R.__, 2);
half(42); //=> 21

var reciprocal = R.divide(1);
reciprocal(4);   //=> 0.25
drop Added in v0.1.0

Number → [a] → [a]
Number → String → String
Parameters

n
list
Returns

* A copy of list without the first `n` elements
Returns all but the first n elements of the given list, string, or transducer/transformer (or object with a drop method).

Dispatches to the drop method of the second argument, if present.

See also take, transduce, dropLast, dropWhile.

R.drop(1, ['foo', 'bar', 'baz']); //=> ['bar', 'baz']
R.drop(2, ['foo', 'bar', 'baz']); //=> ['baz']
R.drop(3, ['foo', 'bar', 'baz']); //=> []
R.drop(4, ['foo', 'bar', 'baz']); //=> []
R.drop(3, 'ramda');               //=> 'da'
dropLast Added in v0.16.0

Number → [a] → [a]
Number → String → String
Parameters

n
The number of elements of list to skip.
 list
The list of elements to consider.
Returns

Array A copy of the list with only the first `list.length - n` elements
Returns a list containing all but the last n elements of the given list.

See also takeLast, drop, dropWhile, dropLastWhile.

R.dropLast(1, ['foo', 'bar', 'baz']); //=> ['foo', 'bar']
R.dropLast(2, ['foo', 'bar', 'baz']); //=> ['foo']
R.dropLast(3, ['foo', 'bar', 'baz']); //=> []
R.dropLast(4, ['foo', 'bar', 'baz']); //=> []
R.dropLast(3, 'ramda');               //=> 'ra'
dropLastWhile Added in v0.16.0

(a → Boolean) → [a] → [a]
(a → Boolean) → String → String
Parameters

predicate
The function to be called on each element
 xs
The collection to iterate over.
Returns

Array A new array without any trailing elements that return `falsy` values from the `predicate`.
Returns a new list excluding all the tailing elements of a given list which satisfy the supplied predicate function. It passes each value from the right to the supplied predicate function, skipping elements until the predicate function returns a falsy value. The predicate function is applied to one argument: (value).

See also takeLastWhile, addIndex, drop, dropWhile.

var lteThree = x => x <= 3;

R.dropLastWhile(lteThree, [1, 2, 3, 4, 3, 2, 1]); //=> [1, 2, 3, 4]

R.dropLastWhile(x => x !== 'd' , 'Ramda'); //=> 'Ramd'
dropRepeats Added in v0.14.0

[a] → [a]
Parameters

list
The array to consider.
Returns

Array `list` without repeating elements.
Returns a new list without any consecutively repeating elements. R.equals is used to determine equality.

Acts as a transducer if a transformer is given in list position.

See also transduce.

R.dropRepeats([1, 1, 1, 2, 3, 4, 4, 2, 2]); //=> [1, 2, 3, 4, 2]
dropRepeatsWith Added in v0.14.0

((a, a) → Boolean) → [a] → [a]
Parameters

pred
A predicate used to test whether two items are equal.
 list
The array to consider.
Returns

Array `list` without repeating elements.
Returns a new list without any consecutively repeating elements. Equality is determined by applying the supplied predicate to each pair of consecutive elements. The first element in a series of equal elements will be preserved.

Acts as a transducer if a transformer is given in list position.

See also transduce.

var l = [1, -1, 1, 3, 4, -4, -4, -5, 5, 3, 3];
R.dropRepeatsWith(R.eqBy(Math.abs), l); //=> [1, 3, 4, -5, 3]
dropWhile Added in v0.9.0

(a → Boolean) → [a] → [a]
(a → Boolean) → String → String
Parameters

fn
The function called per iteration.
 xs
The collection to iterate over.
Returns

Array A new array.
Returns a new list excluding the leading elements of a given list which satisfy the supplied predicate function. It passes each value to the supplied predicate function, skipping elements while the predicate function returns true. The predicate function is applied to one argument: (value).

Dispatches to the dropWhile method of the second argument, if present.

Acts as a transducer if a transformer is given in list position.

See also takeWhile, transduce, addIndex.

var lteTwo = x => x <= 2;

R.dropWhile(lteTwo, [1, 2, 3, 4, 3, 2, 1]); //=> [3, 4, 3, 2, 1]

R.dropWhile(x => x !== 'd' , 'Ramda'); //=> 'da'
either Added in v0.12.0

(*… → Boolean) → (*… → Boolean) → (*… → Boolean)
Parameters

f
a predicate
 g
another predicate
Returns

function a function that applies its arguments to `f` and `g` and `||`s their outputs together.
A function wrapping calls to the two functions in an || operation, returning the result of the first function if it is truth-y and the result of the second function otherwise. Note that this is short-circuited, meaning that the second function will not be invoked if the first returns a truth-y value.

In addition to functions, R.either also accepts any fantasy-land compatible applicative functor.

See also or.

var gt10 = x => x > 10;
var even = x => x % 2 === 0;
var f = R.either(gt10, even);
f(101); //=> true
f(8); //=> true
empty Added in v0.3.0

a → a
Parameters

x
Returns

*
Returns the empty value of its argument's type. Ramda defines the empty value of Array ([]), Object ({}), String (''), and Arguments. Other types are supported if they define <Type>.empty, <Type>.prototype.empty or implement the FantasyLand Monoid spec.

Dispatches to the empty method of the first argument, if present.

R.empty(Just(42));      //=> Nothing()
R.empty([1, 2, 3]);     //=> []
R.empty('unicorns');    //=> ''
R.empty({x: 1, y: 2});  //=> {}
endsWith Added in v0.24.0

[a] → Boolean
String → Boolean
Parameters

suffix
list
Returns

Boolean
Checks if a list ends with the provided values

R.endsWith('c', 'abc')                //=> true
R.endsWith('b', 'abc')                //=> false
R.endsWith(['c'], ['a', 'b', 'c'])    //=> true
R.endsWith(['b'], ['a', 'b', 'c'])    //=> false
eqBy Added in v0.18.0

(a → b) → a → a → Boolean
Parameters

f
x
y
Returns

Boolean
Takes a function and two values in its domain and returns true if the values map to the same value in the codomain; false otherwise.

R.eqBy(Math.abs, 5, -5); //=> true
eqProps Added in v0.1.0

k → {k: v} → {k: v} → Boolean
Parameters

prop
The name of the property to compare
 obj1
obj2
Returns

Boolean
Reports whether two objects have the same value, in R.equals terms, for the specified property. Useful as a curried predicate.

var o1 = { a: 1, b: 2, c: 3, d: 4 };
var o2 = { a: 10, b: 20, c: 3, d: 40 };
R.eqProps('a', o1, o2); //=> false
R.eqProps('c', o1, o2); //=> true
equals Added in v0.15.0

a → b → Boolean
Parameters

a
b
Returns

Boolean
Returns true if its arguments are equivalent, false otherwise. Handles cyclical data structures.

Dispatches symmetrically to the equals methods of both arguments, if present.

R.equals(1, 1); //=> true
R.equals(1, '1'); //=> false
R.equals([1, 2, 3], [1, 2, 3]); //=> true

var a = {}; a.v = a;
var b = {}; b.v = b;
R.equals(a, b); //=> true
evolve Added in v0.9.0

{k: (v → v)} → {k: v} → {k: v}
Parameters

transformations
The object specifying transformation functions to apply to the object.
 object
The object to be transformed.
Returns

Object The transformed object.
Creates a new object by recursively evolving a shallow copy of object, according to the transformation functions. All non-primitive properties are copied by reference.

A transformation function will not be invoked if its corresponding key does not exist in the evolved object.

var tomato  = {firstName: '  Tomato ', data: {elapsed: 100, remaining: 1400}, id:123};
var transformations = {
  firstName: R.trim,
  lastName: R.trim, // Will not get invoked.
  data: {elapsed: R.add(1), remaining: R.add(-1)}
};
R.evolve(transformations, tomato); //=> {firstName: 'Tomato', data: {elapsed: 101, remaining: 1399}, id:123}
F Added in v0.9.0

* → Boolean
Parameters

Returns

Boolean
A function that always returns false. Any passed in parameters are ignored.

See also always, T.

R.F(); //=> false
filter Added in v0.1.0

Filterable f => (a → Boolean) → f a → f a
Parameters

pred
filterable
Returns

Array Filterable
Takes a predicate and a Filterable, and returns a new filterable of the same type containing the members of the given filterable which satisfy the given predicate. Filterable objects include plain objects or any object that has a filter method such as Array.

Dispatches to the filter method of the second argument, if present.

Acts as a transducer if a transformer is given in list position.

See also reject, transduce, addIndex.

var isEven = n => n % 2 === 0;

R.filter(isEven, [1, 2, 3, 4]); //=> [2, 4]

R.filter(isEven, {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, d: 4}
find Added in v0.1.0

(a → Boolean) → [a] → a | undefined
Parameters

fn
The predicate function used to determine if the element is the desired one.
 list
The array to consider.
Returns

Object The element found, or `undefined`.
Returns the first element of the list which matches the predicate, or undefined if no element matches.

Dispatches to the find method of the second argument, if present.

Acts as a transducer if a transformer is given in list position.

See also transduce.

var xs = [{a: 1}, {a: 2}, {a: 3}];
R.find(R.propEq('a', 2))(xs); //=> {a: 2}
R.find(R.propEq('a', 4))(xs); //=> undefined
findIndex Added in v0.1.1

(a → Boolean) → [a] → Number
Parameters

fn
The predicate function used to determine if the element is the desired one.
 list
The array to consider.
Returns

Number The index of the element found, or `-1`.
Returns the index of the first element of the list which matches the predicate, or -1 if no element matches.

Acts as a transducer if a transformer is given in list position.

See also transduce.

var xs = [{a: 1}, {a: 2}, {a: 3}];
R.findIndex(R.propEq('a', 2))(xs); //=> 1
R.findIndex(R.propEq('a', 4))(xs); //=> -1
findLast Added in v0.1.1

(a → Boolean) → [a] → a | undefined
Parameters

fn
The predicate function used to determine if the element is the desired one.
 list
The array to consider.
Returns

Object The element found, or `undefined`.
Returns the last element of the list which matches the predicate, or undefined if no element matches.

Acts as a transducer if a transformer is given in list position.

See also transduce.

var xs = [{a: 1, b: 0}, {a:1, b: 1}];
R.findLast(R.propEq('a', 1))(xs); //=> {a: 1, b: 1}
R.findLast(R.propEq('a', 4))(xs); //=> undefined
findLastIndex Added in v0.1.1

(a → Boolean) → [a] → Number
Parameters

fn
The predicate function used to determine if the element is the desired one.
 list
The array to consider.
Returns

Number The index of the element found, or `-1`.
Returns the index of the last element of the list which matches the predicate, or -1 if no element matches.

Acts as a transducer if a transformer is given in list position.

See also transduce.

var xs = [{a: 1, b: 0}, {a:1, b: 1}];
R.findLastIndex(R.propEq('a', 1))(xs); //=> 1
R.findLastIndex(R.propEq('a', 4))(xs); //=> -1
flatten Added in v0.1.0

[a] → [b]
Parameters

list
The array to consider.
Returns

Array The flattened list.
Returns a new list by pulling every item out of it (and all its sub-arrays) and putting them in a new array, depth-first.

See also unnest.

R.flatten([1, 2, [3, 4], 5, [6, [7, 8, [9, [10, 11], 12]]]]);
//=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
flip Added in v0.1.0

((a, b, c, …) → z) → (b → a → c → … → z)
Parameters

fn
The function to invoke with its first two parameters reversed.
Returns

* The result of invoking `fn` with its first two parameters' order reversed.
Returns a new function much like the supplied one, except that the first two arguments' order is reversed.

var mergeThree = (a, b, c) => [].concat(a, b, c);

mergeThree(1, 2, 3); //=> [1, 2, 3]

R.flip(mergeThree)(1, 2, 3); //=> [2, 1, 3]
forEach Added in v0.1.1

(a → *) → [a] → [a]
Parameters

fn
The function to invoke. Receives one argument, value.
 list
The list to iterate over.
Returns

Array The original list.
Iterate over an input list, calling a provided function fn for each element in the list.

fn receives one argument: (value).

Note: R.forEach does not skip deleted or unassigned indices (sparse arrays), unlike the native Array.prototype.forEach method. For more details on this behavior, see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#Description

Also note that, unlike Array.prototype.forEach, Ramda's forEach returns the original array. In some libraries this function is named each.

Dispatches to the forEach method of the second argument, if present.

See also addIndex.

var printXPlusFive = x => console.log(x + 5);
R.forEach(printXPlusFive, [1, 2, 3]); //=> [1, 2, 3]
// logs 6
// logs 7
// logs 8
forEachObjIndexed Added in v0.23.0

((a, String, StrMap a) → Any) → StrMap a → StrMap a
Parameters

fn
The function to invoke. Receives three argument, value, key, obj.
 obj
The object to iterate over.
Returns

Object The original object.
Iterate over an input object, calling a provided function fn for each key and value in the object.

fn receives three argument: (value, key, obj).

var printKeyConcatValue = (value, key) => console.log(key + ':' + value);
R.forEachObjIndexed(printKeyConcatValue, {x: 1, y: 2}); //=> {x: 1, y: 2}
// logs x:1
// logs y:2
fromPairs Added in v0.3.0

[[k,v]] → {k: v}
Parameters

pairs
An array of two-element arrays that will be the keys and values of the output object.
Returns

Object The object made by pairing up `keys` and `values`.
Creates a new object from a list key-value pairs. If a key appears in multiple pairs, the rightmost pair is included in the object.

See also toPairs, pair.

R.fromPairs([['a', 1], ['b', 2], ['c', 3]]); //=> {a: 1, b: 2, c: 3}
groupBy Added in v0.1.0

(a → String) → [a] → {String: [a]}
Parameters

fn
Function :: a -> String
 list
The array to group
Returns

Object An object with the output of `fn` for keys, mapped to arrays of elements that produced that key when passed to `fn`.
Splits a list into sub-lists stored in an object, based on the result of calling a String-returning function on each element, and grouping the results according to values returned.

Dispatches to the groupBy method of the second argument, if present.

Acts as a transducer if a transformer is given in list position.

See also transduce.

var byGrade = R.groupBy(function(student) {
  var score = student.score;
  return score < 65 ? 'F' :
         score < 70 ? 'D' :
         score < 80 ? 'C' :
         score < 90 ? 'B' : 'A';
});
var students = [{name: 'Abby', score: 84},
                {name: 'Eddy', score: 58},
                // ...
                {name: 'Jack', score: 69}];
byGrade(students);
// {
//   'A': [{name: 'Dianne', score: 99}],
//   'B': [{name: 'Abby', score: 84}]
//   // ...,
//   'F': [{name: 'Eddy', score: 58}]
// }
groupWith Added in v0.21.0

((a, a) → Boolean) → [a] → [[a]]
Parameters

fn
Function for determining whether two given (adjacent) elements should be in the same group
 list
The array to group. Also accepts a string, which will be treated as a list of characters.
Returns

List A list that contains sublists of elements, whose concatenations are equal to the original list.
Takes a list and returns a list of lists where each sublist's elements are all satisfied pairwise comparison according to the provided function. Only adjacent elements are passed to the comparison function.

R.groupWith(R.equals, [0, 1, 1, 2, 3, 5, 8, 13, 21])
//=> [[0], [1, 1], [2], [3], [5], [8], [13], [21]]

R.groupWith((a, b) => a + 1 === b, [0, 1, 1, 2, 3, 5, 8, 13, 21])
//=> [[0, 1], [1, 2, 3], [5], [8], [13], [21]]

R.groupWith((a, b) => a % 2 === b % 2, [0, 1, 1, 2, 3, 5, 8, 13, 21])
//=> [[0], [1, 1], [2], [3, 5], [8], [13, 21]]

R.groupWith(R.eqBy(isVowel), 'aestiou')
//=> ['ae', 'st', 'iou']
gt Added in v0.1.0

Ord a => a → a → Boolean
Parameters

a
b
Returns

Boolean
Returns true if the first argument is greater than the second; false otherwise.

See also lt.

R.gt(2, 1); //=> true
R.gt(2, 2); //=> false
R.gt(2, 3); //=> false
R.gt('a', 'z'); //=> false
R.gt('z', 'a'); //=> true
gte Added in v0.1.0

Ord a => a → a → Boolean
Parameters

a
b
Returns

Boolean
Returns true if the first argument is greater than or equal to the second; false otherwise.

See also lte.

R.gte(2, 1); //=> true
R.gte(2, 2); //=> true
R.gte(2, 3); //=> false
R.gte('a', 'z'); //=> false
R.gte('z', 'a'); //=> true
has Added in v0.7.0

s → {s: x} → Boolean
Parameters

prop
The name of the property to check for.
 obj
The object to query.
Returns

Boolean Whether the property exists.
Returns whether or not an object has an own property with the specified name

var hasName = R.has('name');
hasName({name: 'alice'});   //=> true
hasName({name: 'bob'});     //=> true
hasName({});                //=> false

var point = {x: 0, y: 0};
var pointHas = R.has(R.__, point);
pointHas('x');  //=> true
pointHas('y');  //=> true
pointHas('z');  //=> false
hasIn Added in v0.7.0

s → {s: x} → Boolean
Parameters

prop
The name of the property to check for.
 obj
The object to query.
Returns

Boolean Whether the property exists.
Returns whether or not an object or its prototype chain has a property with the specified name

function Rectangle(width, height) {
  this.width = width;
  this.height = height;
}
Rectangle.prototype.area = function() {
  return this.width * this.height;
};

var square = new Rectangle(2, 2);
R.hasIn('width', square);  //=> true
R.hasIn('area', square);  //=> true
head Added in v0.1.0

[a] → a | Undefined
String → String
Parameters

list
Returns

*
Returns the first element of the given list or string. In some libraries this function is named first.

See also tail, init, last.

R.head(['fi', 'fo', 'fum']); //=> 'fi'
R.head([]); //=> undefined

R.head('abc'); //=> 'a'
R.head(''); //=> ''
identical Added in v0.15.0

a → a → Boolean
Parameters

a
b
Returns

Boolean
Returns true if its arguments are identical, false otherwise. Values are identical if they reference the same memory. NaN is identical to NaN; 0 and -0 are not identical.

var o = {};
R.identical(o, o); //=> true
R.identical(1, 1); //=> true
R.identical(1, '1'); //=> false
R.identical([], []); //=> false
R.identical(0, -0); //=> false
R.identical(NaN, NaN); //=> true
identity Added in v0.1.0

a → a
Parameters

x
The value to return.
Returns

* The input value, `x`.
A function that does nothing but return the parameter supplied to it. Good as a default or placeholder function.

R.identity(1); //=> 1

var obj = {};
R.identity(obj) === obj; //=> true
ifElse Added in v0.8.0

(*… → Boolean) → (*… → *) → (*… → *) → (*… → *)
Parameters

condition
A predicate function
 onTrue
A function to invoke when the condition evaluates to a truthy value.
 onFalse
A function to invoke when the condition evaluates to a falsy value.
Returns

function A new unary function that will process either the `onTrue` or the `onFalse` function depending upon the result of the `condition` predicate.
Creates a function that will process either the onTrue or the onFalse function depending upon the result of the condition predicate.

See also unless, when.

var incCount = R.ifElse(
  R.has('count'),
  R.over(R.lensProp('count'), R.inc),
  R.assoc('count', 1)
);
incCount({});           //=> { count: 1 }
incCount({ count: 1 }); //=> { count: 2 }
inc Added in v0.9.0

Number → Number
Parameters

n
Returns

Number n + 1
Increments its argument.

See also dec.

R.inc(42); //=> 43
indexBy Added in v0.19.0

(a → String) → [{k: v}] → {k: {k: v}}
Parameters

fn
Function :: a -> String
 array
The array of objects to index
Returns

Object An object indexing each array element by the given property.
Given a function that generates a key, turns a list of objects into an object indexing the objects by the given key. Note that if multiple objects generate the same value for the indexing key only the last value will be included in the generated object.

Acts as a transducer if a transformer is given in list position.

var list = [{id: 'xyz', title: 'A'}, {id: 'abc', title: 'B'}];
R.indexBy(R.prop('id'), list);
//=> {abc: {id: 'abc', title: 'B'}, xyz: {id: 'xyz', title: 'A'}}
indexOf Added in v0.1.0

a → [a] → Number
Parameters

target
The item to find.
 xs
The array to search in.
Returns

Number the index of the target, or -1 if the target is not found.
Returns the position of the first occurrence of an item in an array, or -1 if the item is not included in the array. R.equals is used to determine equality.

See also lastIndexOf.

R.indexOf(3, [1,2,3,4]); //=> 2
R.indexOf(10, [1,2,3,4]); //=> -1
init Added in v0.9.0

[a] → [a]
String → String
Parameters

list
Returns

*
Returns all but the last element of the given list or string.

See also last, head, tail.

R.init([1, 2, 3]);  //=> [1, 2]
R.init([1, 2]);     //=> [1]
R.init([1]);        //=> []
R.init([]);         //=> []

R.init('abc');  //=> 'ab'
R.init('ab');   //=> 'a'
R.init('a');    //=> ''
R.init('');     //=> ''
innerJoin Added in v0.24.0

((a, b) → Boolean) → [a] → [b] → [a]
Parameters

pred
xs
ys
Returns

Array
Takes a predicate pred, a list xs, and a list ys, and returns a list xs' comprising each of the elements of xs which is equal to one or more elements of ys according to pred.

pred must be a binary function expecting an element from each list.

xs, ys, and xs' are treated as sets, semantically, so ordering should not be significant, but since xs' is ordered the implementation guarantees that its values are in the same order as they appear in xs. Duplicates are not removed, so xs' may contain duplicates if xs contains duplicates.

See also intersection.

R.innerJoin(
  (record, id) => record.id === id,
  [{id: 824, name: 'Richie Furay'},
   {id: 956, name: 'Dewey Martin'},
   {id: 313, name: 'Bruce Palmer'},
   {id: 456, name: 'Stephen Stills'},
   {id: 177, name: 'Neil Young'}],
  [177, 456, 999]
);
//=> [{id: 456, name: 'Stephen Stills'}, {id: 177, name: 'Neil Young'}]
insert Added in v0.2.2

Number → a → [a] → [a]
Parameters

index
The position to insert the element
 elt
The element to insert into the Array
 list
The list to insert into
Returns

Array A new Array with `elt` inserted at `index`.
Inserts the supplied element into the list, at the specified index. Note that this is not destructive: it returns a copy of the list with the changes. No lists have been harmed in the application of this function.

R.insert(2, 'x', [1,2,3,4]); //=> [1,2,'x',3,4]
insertAll Added in v0.9.0

Number → [a] → [a] → [a]
Parameters

index
The position to insert the sub-list
 elts
The sub-list to insert into the Array
 list
The list to insert the sub-list into
Returns

Array A new Array with `elts` inserted starting at `index`.
Inserts the sub-list into the list, at the specified index. Note that this is not destructive: it returns a copy of the list with the changes. No lists have been harmed in the application of this function.

R.insertAll(2, ['x','y','z'], [1,2,3,4]); //=> [1,2,'x','y','z',3,4]
intersection Added in v0.1.0

[*] → [*] → [*]
Parameters

list1
The first list.
 list2
The second list.
Returns

Array The list of elements found in both `list1` and `list2`.
Combines two lists into a set (i.e. no duplicates) composed of those elements common to both lists.

See also innerJoin.

R.intersection([1,2,3,4], [7,6,5,4,3]); //=> [4, 3]
intersperse Added in v0.14.0

a → [a] → [a]
Parameters

separator
The element to add to the list.
 list
The list to be interposed.
Returns

Array The new list.
Creates a new list with the separator interposed between elements.

Dispatches to the intersperse method of the second argument, if present.

R.intersperse('n', ['ba', 'a', 'a']); //=> ['ba', 'n', 'a', 'n', 'a']
into Added in v0.12.0

a → (b → b) → [c] → a
Parameters

acc
The initial accumulator value.
 xf
The transducer function. Receives a transformer and returns a transformer.
 list
The list to iterate over.
Returns

* The final, accumulated value.
Transforms the items of the list with the transducer and appends the transformed items to the accumulator using an appropriate iterator function based on the accumulator type.

The accumulator can be an array, string, object or a transformer. Iterated items will be appended to arrays and concatenated to strings. Objects will be merged directly or 2-item arrays will be merged as key, value pairs.

The accumulator can also be a transformer object that provides a 2-arity reducing iterator function, step, 0-arity initial value function, init, and 1-arity result extraction function result. The step function is used as the iterator function in reduce. The result function is used to convert the final accumulator into the return type and in most cases is R.identity. The init function is used to provide the initial accumulator.

The iteration is performed with R.reduce after initializing the transducer.

var numbers = [1, 2, 3, 4];
var transducer = R.compose(R.map(R.add(1)), R.take(2));

R.into([], transducer, numbers); //=> [2, 3]

var intoArray = R.into([]);
intoArray(transducer, numbers); //=> [2, 3]
invert Added in v0.9.0

{s: x} → {x: [ s, … ]}
Parameters

obj
The object or array to invert
Returns

Object out A new object with keys in an array.
Same as R.invertObj, however this accounts for objects with duplicate values by putting the values into an array.

See also invertObj.

var raceResultsByFirstName = {
  first: 'alice',
  second: 'jake',
  third: 'alice',
};
R.invert(raceResultsByFirstName);
//=> { 'alice': ['first', 'third'], 'jake':['second'] }
invertObj Added in v0.9.0

{s: x} → {x: s}
Parameters

obj
The object or array to invert
Returns

Object out A new object
Returns a new object with the keys of the given object as values, and the values of the given object, which are coerced to strings, as keys. Note that the last key found is preferred when handling the same value.

See also invert.

var raceResults = {
  first: 'alice',
  second: 'jake'
};
R.invertObj(raceResults);
//=> { 'alice': 'first', 'jake':'second' }

// Alternatively:
var raceResults = ['alice', 'jake'];
R.invertObj(raceResults);
//=> { 'alice': '0', 'jake':'1' }
invoker Added in v0.1.0

Number → String → (a → b → … → n → Object → *)
Parameters

arity
Number of arguments the returned function should take before the target object.
 method
Name of the method to call.
Returns

function A new curried function.
Turns a named method with a specified arity into a function that can be called directly supplied with arguments and a target object.

The returned function is curried and accepts arity + 1 parameters where the final parameter is the target object.

See also construct.

var sliceFrom = R.invoker(1, 'slice');
sliceFrom(6, 'abcdefghijklm'); //=> 'ghijklm'
var sliceFrom6 = R.invoker(2, 'slice')(6);
sliceFrom6(8, 'abcdefghijklm'); //=> 'gh'
is Added in v0.3.0

(* → {*}) → a → Boolean
Parameters

ctor
A constructor
 val
The value to test
Returns

Boolean
See if an object (val) is an instance of the supplied constructor. This function will check up the inheritance chain, if any.

R.is(Object, {}); //=> true
R.is(Number, 1); //=> true
R.is(Object, 1); //=> false
R.is(String, 's'); //=> true
R.is(String, new String('')); //=> true
R.is(Object, new String('')); //=> true
R.is(Object, 's'); //=> false
R.is(Number, {}); //=> false
isEmpty Added in v0.1.0

a → Boolean
Parameters

x
Returns

Boolean
Returns true if the given value is its type's empty value; false otherwise.

See also empty.

R.isEmpty([1, 2, 3]);   //=> false
R.isEmpty([]);          //=> true
R.isEmpty('');          //=> true
R.isEmpty(null);        //=> false
R.isEmpty({});          //=> true
R.isEmpty({length: 0}); //=> false
isNil Added in v0.9.0

* → Boolean
Parameters

x
The value to test.
Returns

Boolean `true` if `x` is `undefined` or `null`, otherwise `false`.
Checks if the input value is null or undefined.

R.isNil(null); //=> true
R.isNil(undefined); //=> true
R.isNil(0); //=> false
R.isNil([]); //=> false
join Added in v0.1.0

String → [a] → String
Parameters

separator
The string used to separate the elements.
 xs
The elements to join into a string.
Returns

String str The string made by concatenating `xs` with `separator`.
Returns a string made by inserting the separator between each element and concatenating all the elements into a single string.

See also split.

var spacer = R.join(' ');
spacer(['a', 2, 3.4]);   //=> 'a 2 3.4'
R.join('|', [1, 2, 3]);    //=> '1|2|3'
juxt Added in v0.19.0

[(a, b, …, m) → n] → ((a, b, …, m) → [n])
Parameters

fns
An array of functions
Returns

function A function that returns a list of values after applying each of the original `fns` to its parameters.
juxt applies a list of functions to a list of values.

See also applySpec.

var getRange = R.juxt([Math.min, Math.max]);
getRange(3, 4, 9, -3); //=> [-3, 9]
keys Added in v0.1.0

{k: v} → [k]
Parameters

obj
The object to extract properties from
Returns

Array An array of the object's own properties.
Returns a list containing the names of all the enumerable own properties of the supplied object. Note that the order of the output array is not guaranteed to be consistent across different JS platforms.

See also keysIn, values.

R.keys({a: 1, b: 2, c: 3}); //=> ['a', 'b', 'c']
keysIn Added in v0.2.0

{k: v} → [k]
Parameters

obj
The object to extract properties from
Returns

Array An array of the object's own and prototype properties.
Returns a list containing the names of all the properties of the supplied object, including prototype properties. Note that the order of the output array is not guaranteed to be consistent across different JS platforms.

See also keys, valuesIn.

var F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
var f = new F();
R.keysIn(f); //=> ['x', 'y']
last Added in v0.1.4

[a] → a | Undefined
String → String
Parameters

list
Returns

*
Returns the last element of the given list or string.

See also init, head, tail.

R.last(['fi', 'fo', 'fum']); //=> 'fum'
R.last([]); //=> undefined

R.last('abc'); //=> 'c'
R.last(''); //=> ''
lastIndexOf Added in v0.1.0

a → [a] → Number
Parameters

target
The item to find.
 xs
The array to search in.
Returns

Number the index of the target, or -1 if the target is not found.
Returns the position of the last occurrence of an item in an array, or -1 if the item is not included in the array. R.equals is used to determine equality.

See also indexOf.

R.lastIndexOf(3, [-1,3,3,0,1,2,3,4]); //=> 6
R.lastIndexOf(10, [1,2,3,4]); //=> -1
length Added in v0.3.0

[a] → Number
Parameters

list
The array to inspect.
Returns

Number The length of the array.
Returns the number of elements in the array by returning list.length.

R.length([]); //=> 0
R.length([1, 2, 3]); //=> 3
lens Added in v0.8.0

(s → a) → ((a, s) → s) → Lens s a
Lens s a = Functor f => (a → f a) → s → f s
Parameters

getter
setter
Returns

Lens
Returns a lens for the given getter and setter functions. The getter "gets" the value of the focus; the setter "sets" the value of the focus. The setter should not mutate the data structure.

See also view, set, over, lensIndex, lensProp.

var xLens = R.lens(R.prop('x'), R.assoc('x'));

R.view(xLens, {x: 1, y: 2});            //=> 1
R.set(xLens, 4, {x: 1, y: 2});          //=> {x: 4, y: 2}
R.over(xLens, R.negate, {x: 1, y: 2});  //=> {x: -1, y: 2}
lensIndex Added in v0.14.0

Number → Lens s a
Lens s a = Functor f => (a → f a) → s → f s
Parameters

n
Returns

Lens
Returns a lens whose focus is the specified index.

See also view, set, over.

var headLens = R.lensIndex(0);

R.view(headLens, ['a', 'b', 'c']);            //=> 'a'
R.set(headLens, 'x', ['a', 'b', 'c']);        //=> ['x', 'b', 'c']
R.over(headLens, R.toUpper, ['a', 'b', 'c']); //=> ['A', 'b', 'c']
lensPath Added in v0.19.0

[Idx] → Lens s a
Idx = String | Int
Lens s a = Functor f => (a → f a) → s → f s
Parameters

path
The path to use.
Returns

Lens
Returns a lens whose focus is the specified path.

See also view, set, over.

var xHeadYLens = R.lensPath(['x', 0, 'y']);

R.view(xHeadYLens, {x: [{y: 2, z: 3}, {y: 4, z: 5}]});
//=> 2
R.set(xHeadYLens, 1, {x: [{y: 2, z: 3}, {y: 4, z: 5}]});
//=> {x: [{y: 1, z: 3}, {y: 4, z: 5}]}
R.over(xHeadYLens, R.negate, {x: [{y: 2, z: 3}, {y: 4, z: 5}]});
//=> {x: [{y: -2, z: 3}, {y: 4, z: 5}]}
lensProp Added in v0.14.0

String → Lens s a
Lens s a = Functor f => (a → f a) → s → f s
Parameters

k
Returns

Lens
Returns a lens whose focus is the specified property.

See also view, set, over.

var xLens = R.lensProp('x');

R.view(xLens, {x: 1, y: 2});            //=> 1
R.set(xLens, 4, {x: 1, y: 2});          //=> {x: 4, y: 2}
R.over(xLens, R.negate, {x: 1, y: 2});  //=> {x: -1, y: 2}
lift Added in v0.7.0

(*… → *) → ([*]… → [*])
Parameters

fn
The function to lift into higher context
Returns

function The lifted function.
"lifts" a function of arity > 1 so that it may "map over" a list, Function or other object that satisfies the FantasyLand Apply spec.

See also liftN.

var madd3 = R.lift((a, b, c) => a + b + c);

madd3([1,2,3], [1,2,3], [1]); //=> [3, 4, 5, 4, 5, 6, 5, 6, 7]

var madd5 = R.lift((a, b, c, d, e) => a + b + c + d + e);

madd5([1,2], [3], [4, 5], [6], [7, 8]); //=> [21, 22, 22, 23, 22, 23, 23, 24]
liftN Added in v0.7.0

Number → (*… → *) → ([*]… → [*])
Parameters

fn
The function to lift into higher context
Returns

function The lifted function.
"lifts" a function to be the specified arity, so that it may "map over" that many lists, Functions or other objects that satisfy the FantasyLand Apply spec.

See also lift, ap.

var madd3 = R.liftN(3, (...args) => R.sum(args));
madd3([1,2,3], [1,2,3], [1]); //=> [3, 4, 5, 4, 5, 6, 5, 6, 7]
lt Added in v0.1.0

Ord a => a → a → Boolean
Parameters

a
b
Returns

Boolean
Returns true if the first argument is less than the second; false otherwise.

See also gt.

R.lt(2, 1); //=> false
R.lt(2, 2); //=> false
R.lt(2, 3); //=> true
R.lt('a', 'z'); //=> true
R.lt('z', 'a'); //=> false
lte Added in v0.1.0

Ord a => a → a → Boolean
Parameters

a
b
Returns

Boolean
Returns true if the first argument is less than or equal to the second; false otherwise.

See also gte.

R.lte(2, 1); //=> false
R.lte(2, 2); //=> true
R.lte(2, 3); //=> true
R.lte('a', 'z'); //=> true
R.lte('z', 'a'); //=> false
map Added in v0.1.0

Functor f => (a → b) → f a → f b
Parameters

fn
The function to be called on every element of the input list.
 list
The list to be iterated over.
Returns

Array The new list.
Takes a function and a functor, applies the function to each of the functor's values, and returns a functor of the same shape.

Ramda provides suitable map implementations for Array and Object, so this function may be applied to [1, 2, 3] or {x: 1, y: 2, z: 3}.

Dispatches to the map method of the second argument, if present.

Acts as a transducer if a transformer is given in list position.

Also treats functions as functors and will compose them together.

See also transduce, addIndex.

var double = x => x * 2;

R.map(double, [1, 2, 3]); //=> [2, 4, 6]

R.map(double, {x: 1, y: 2, z: 3}); //=> {x: 2, y: 4, z: 6}
mapAccum Added in v0.10.0

((acc, x) → (acc, y)) → acc → [x] → (acc, [y])
Parameters

fn
The function to be called on every element of the input list.
 acc
The accumulator value.
 list
The list to iterate over.
Returns

* The final, accumulated value.
The mapAccum function behaves like a combination of map and reduce; it applies a function to each element of a list, passing an accumulating parameter from left to right, and returning a final value of this accumulator together with the new list.

The iterator function receives two arguments, acc and value, and should return a tuple [acc, value].

See also addIndex, mapAccumRight.

var digits = ['1', '2', '3', '4'];
var appender = (a, b) => [a + b, a + b];

R.mapAccum(appender, 0, digits); //=> ['01234', ['01', '012', '0123', '01234']]
mapAccumRight Added in v0.10.0

((x, acc) → (y, acc)) → acc → [x] → ([y], acc)
Parameters

fn
The function to be called on every element of the input list.
 acc
The accumulator value.
 list
The list to iterate over.
Returns

* The final, accumulated value.
The mapAccumRight function behaves like a combination of map and reduce; it applies a function to each element of a list, passing an accumulating parameter from right to left, and returning a final value of this accumulator together with the new list.

Similar to mapAccum, except moves through the input list from the right to the left.

The iterator function receives two arguments, value and acc, and should return a tuple [value, acc].

See also addIndex, mapAccum.

var digits = ['1', '2', '3', '4'];
var append = (a, b) => [a + b, a + b];

R.mapAccumRight(append, 5, digits); //=> [['12345', '2345', '345', '45'], '12345']
mapObjIndexed Added in v0.9.0

((*, String, Object) → *) → Object → Object
Parameters

fn
obj
Returns

Object
An Object-specific version of map. The function is applied to three arguments: (value, key, obj). If only the value is significant, use map instead.

See also map.

var values = { x: 1, y: 2, z: 3 };
var prependKeyAndDouble = (num, key, obj) => key + (num * 2);

R.mapObjIndexed(prependKeyAndDouble, values); //=> { x: 'x2', y: 'y4', z: 'z6' }
match Added in v0.1.0

RegExp → String → [String | Undefined]
Parameters

rx
A regular expression.
 str
The string to match against
Returns

Array The list of matches or empty array.
Tests a regular expression against a String. Note that this function will return an empty array when there are no matches. This differs from String.prototype.match which returns null when there are no matches.

See also test.

R.match(/([a-z]a)/g, 'bananas'); //=> ['ba', 'na', 'na']
R.match(/a/, 'b'); //=> []
R.match(/a/, null); //=> TypeError: null does not have a method named "match"
mathMod Added in v0.3.0

Number → Number → Number
Parameters

m
The dividend.
 p
the modulus.
Returns

Number The result of `b mod a`.
mathMod behaves like the modulo operator should mathematically, unlike the % operator (and by extension, R.modulo). So while -17 % 5 is -2, mathMod(-17, 5) is 3. mathMod requires Integer arguments, and returns NaN when the modulus is zero or negative.

See also modulo.

R.mathMod(-17, 5);  //=> 3
R.mathMod(17, 5);   //=> 2
R.mathMod(17, -5);  //=> NaN
R.mathMod(17, 0);   //=> NaN
R.mathMod(17.2, 5); //=> NaN
R.mathMod(17, 5.3); //=> NaN

var clock = R.mathMod(R.__, 12);
clock(15); //=> 3
clock(24); //=> 0

var seventeenMod = R.mathMod(17);
seventeenMod(3);  //=> 2
seventeenMod(4);  //=> 1
seventeenMod(10); //=> 7
max Added in v0.1.0

Ord a => a → a → a
Parameters

a
b
Returns

*
Returns the larger of its two arguments.

See also maxBy, min.

R.max(789, 123); //=> 789
R.max('a', 'b'); //=> 'b'
maxBy Added in v0.8.0

Ord b => (a → b) → a → a → a
Parameters

f
a
b
Returns

*
Takes a function and two values, and returns whichever value produces the larger result when passed to the provided function.

See also max, minBy.

//  square :: Number -> Number
var square = n => n * n;

R.maxBy(square, -3, 2); //=> -3

R.reduce(R.maxBy(square), 0, [3, -5, 4, 1, -2]); //=> -5
R.reduce(R.maxBy(square), 0, []); //=> 0
mean Added in v0.14.0

[Number] → Number
Parameters

list
Returns

Number
Returns the mean of the given list of numbers.

See also median.

R.mean([2, 7, 9]); //=> 6
R.mean([]); //=> NaN
median Added in v0.14.0

[Number] → Number
Parameters

list
Returns

Number
Returns the median of the given list of numbers.

See also mean.

R.median([2, 9, 7]); //=> 7
R.median([7, 2, 10, 9]); //=> 8
R.median([]); //=> NaN
memoize Added in v0.1.0

Deprecated since v0.25.0
(*… → a) → (*… → a)
Parameters

fn
The function to memoize.
Returns

function Memoized version of `fn`.
Creates a new function that, when invoked, caches the result of calling fn for a given argument set and returns the result. Subsequent calls to the memoized fn with the same argument set will not result in an additional call to fn; instead, the cached result for that set of arguments will be returned.

See also memoizeWith.

let count = 0;
const factorial = R.memoize(n => {
  count += 1;
  return R.product(R.range(1, n + 1));
});
factorial(5); //=> 120
factorial(5); //=> 120
factorial(5); //=> 120
count; //=> 1
memoizeWith Added in v0.24.0

(*… → String) → (*… → a) → (*… → a)
Parameters

fn
The function to generate the cache key.
 fn
The function to memoize.
Returns

function Memoized version of `fn`.
A customisable version of R.memoize. memoizeWith takes an additional function that will be applied to a given argument set and used to create the cache key under which the results of the function to be memoized will be stored. Care must be taken when implementing key generation to avoid clashes that may overwrite previous entries erroneously.

See also memoize.

let count = 0;
const factorial = R.memoizeWith(R.identity, n => {
  count += 1;
  return R.product(R.range(1, n + 1));
});
factorial(5); //=> 120
factorial(5); //=> 120
factorial(5); //=> 120
count; //=> 1
merge Added in v0.1.0

{k: v} → {k: v} → {k: v}
Parameters

l
r
Returns

Object
Create a new object with the own properties of the first object merged with the own properties of the second object. If a key exists in both objects, the value from the second object will be used.

See also mergeDeepRight, mergeWith, mergeWithKey.

R.merge({ 'name': 'fred', 'age': 10 }, { 'age': 40 });
//=> { 'name': 'fred', 'age': 40 }

var resetToDefault = R.merge(R.__, {x: 0});
resetToDefault({x: 5, y: 2}); //=> {x: 0, y: 2}
mergeAll Added in v0.10.0

[{k: v}] → {k: v}
Parameters

list
An array of objects
Returns

Object A merged object.
Merges a list of objects together into one object.

See also reduce.

R.mergeAll([{foo:1},{bar:2},{baz:3}]); //=> {foo:1,bar:2,baz:3}
R.mergeAll([{foo:1},{foo:2},{bar:2}]); //=> {foo:2,bar:2}
mergeDeepLeft Added in v0.24.0

{a} → {a} → {a}
Parameters

lObj
rObj
Returns

Object
Creates a new object with the own properties of the first object merged with the own properties of the second object. If a key exists in both objects:

and both values are objects, the two values will be recursively merged
otherwise the value from the first object will be used.
See also merge, mergeDeepRight, mergeDeepWith, mergeDeepWithKey.

R.mergeDeepLeft({ name: 'fred', age: 10, contact: { email: 'moo@example.com' }},
                { age: 40, contact: { email: 'baa@example.com' }});
//=> { name: 'fred', age: 10, contact: { email: 'moo@example.com' }}
mergeDeepRight Added in v0.24.0

{a} → {a} → {a}
Parameters

lObj
rObj
Returns

Object
Creates a new object with the own properties of the first object merged with the own properties of the second object. If a key exists in both objects:

and both values are objects, the two values will be recursively merged
otherwise the value from the second object will be used.
See also merge, mergeDeepLeft, mergeDeepWith, mergeDeepWithKey.

R.mergeDeepRight({ name: 'fred', age: 10, contact: { email: 'moo@example.com' }},
                 { age: 40, contact: { email: 'baa@example.com' }});
//=> { name: 'fred', age: 40, contact: { email: 'baa@example.com' }}
mergeDeepWith Added in v0.24.0

((a, a) → a) → {a} → {a} → {a}
Parameters

fn
lObj
rObj
Returns

Object
Creates a new object with the own properties of the two provided objects. If a key exists in both objects:

and both associated values are also objects then the values will be recursively merged.
otherwise the provided function is applied to associated values using the resulting value as the new value associated with the key. If a key only exists in one object, the value will be associated with the key of the resulting object.
See also mergeWith, mergeDeep, mergeDeepWithKey.

R.mergeDeepWith(R.concat,
                { a: true, c: { values: [10, 20] }},
                { b: true, c: { values: [15, 35] }});
//=> { a: true, b: true, c: { values: [10, 20, 15, 35] }}
mergeDeepWithKey Added in v0.24.0

((String, a, a) → a) → {a} → {a} → {a}
Parameters

fn
lObj
rObj
Returns

Object
Creates a new object with the own properties of the two provided objects. If a key exists in both objects:

and both associated values are also objects then the values will be recursively merged.
otherwise the provided function is applied to the key and associated values using the resulting value as the new value associated with the key. If a key only exists in one object, the value will be associated with the key of the resulting object.
See also mergeWithKey, mergeDeep, mergeDeepWith.

let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r
R.mergeDeepWithKey(concatValues,
                   { a: true, c: { thing: 'foo', values: [10, 20] }},
                   { b: true, c: { thing: 'bar', values: [15, 35] }});
//=> { a: true, b: true, c: { thing: 'bar', values: [10, 20, 15, 35] }}
mergeWith Added in v0.19.0

((a, a) → a) → {a} → {a} → {a}
Parameters

fn
l
r
Returns

Object
Creates a new object with the own properties of the two provided objects. If a key exists in both objects, the provided function is applied to the values associated with the key in each object, with the result being used as the value associated with the key in the returned object.

See also mergeDeepWith, merge, mergeWithKey.

R.mergeWith(R.concat,
            { a: true, values: [10, 20] },
            { b: true, values: [15, 35] });
//=> { a: true, b: true, values: [10, 20, 15, 35] }
mergeWithKey Added in v0.19.0

((String, a, a) → a) → {a} → {a} → {a}
Parameters

fn
l
r
Returns

Object
Creates a new object with the own properties of the two provided objects. If a key exists in both objects, the provided function is applied to the key and the values associated with the key in each object, with the result being used as the value associated with the key in the returned object.

See also mergeDeepWithKey, merge, mergeWith.

let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r
R.mergeWithKey(concatValues,
               { a: true, thing: 'foo', values: [10, 20] },
               { b: true, thing: 'bar', values: [15, 35] });
//=> { a: true, b: true, thing: 'bar', values: [10, 20, 15, 35] }
min Added in v0.1.0

Ord a => a → a → a
Parameters

a
b
Returns

*
Returns the smaller of its two arguments.

See also minBy, max.

R.min(789, 123); //=> 123
R.min('a', 'b'); //=> 'a'
minBy Added in v0.8.0

Ord b => (a → b) → a → a → a
Parameters

f
a
b
Returns

*
Takes a function and two values, and returns whichever value produces the smaller result when passed to the provided function.

See also min, maxBy.

//  square :: Number -> Number
var square = n => n * n;

R.minBy(square, -3, 2); //=> 2

R.reduce(R.minBy(square), Infinity, [3, -5, 4, 1, -2]); //=> 1
R.reduce(R.minBy(square), Infinity, []); //=> Infinity
modulo Added in v0.1.1

Number → Number → Number
Parameters

a
The value to the divide.
 b
The pseudo-modulus
Returns

Number The result of `b % a`.
Divides the first parameter by the second and returns the remainder. Note that this function preserves the JavaScript-style behavior for modulo. For mathematical modulo see mathMod.

See also mathMod.

R.modulo(17, 3); //=> 2
// JS behavior:
R.modulo(-17, 3); //=> -2
R.modulo(17, -3); //=> 2

var isOdd = R.modulo(R.__, 2);
isOdd(42); //=> 0
isOdd(21); //=> 1
multiply Added in v0.1.0

Number → Number → Number
Parameters

a
The first value.
 b
The second value.
Returns

Number The result of `a * b`.
Multiplies two numbers. Equivalent to a * b but curried.

See also divide.

var double = R.multiply(2);
var triple = R.multiply(3);
double(3);       //=>  6
triple(4);       //=> 12
R.multiply(2, 5);  //=> 10
nAry Added in v0.1.0

Number → (* → a) → (* → a)
Parameters

n
The desired arity of the new function.
 fn
The function to wrap.
Returns

function A new function wrapping `fn`. The new function is guaranteed to be of arity `n`.
Wraps a function of any arity (including nullary) in a function that accepts exactly n parameters. Any extraneous parameters will not be passed to the supplied function.

See also binary, unary.

var takesTwoArgs = (a, b) => [a, b];

takesTwoArgs.length; //=> 2
takesTwoArgs(1, 2); //=> [1, 2]

var takesOneArg = R.nAry(1, takesTwoArgs);
takesOneArg.length; //=> 1
// Only `n` arguments are passed to the wrapped function
takesOneArg(1, 2); //=> [1, undefined]
negate Added in v0.9.0

Number → Number
Parameters

n
Returns

Number
Negates its argument.

R.negate(42); //=> -42
none Added in v0.12.0

(a → Boolean) → [a] → Boolean
Parameters

fn
The predicate function.
 list
The array to consider.
Returns

Boolean `true` if the predicate is not satisfied by every element, `false` otherwise.
Returns true if no elements of the list match the predicate, false otherwise.

Dispatches to the any method of the second argument, if present.

See also all, any.

var isEven = n => n % 2 === 0;
var isOdd = n => n % 2 === 1;

R.none(isEven, [1, 3, 5, 7, 9, 11]); //=> true
R.none(isOdd, [1, 3, 5, 7, 8, 11]); //=> false
not Added in v0.1.0

* → Boolean
Parameters

a
any value
Returns

Boolean the logical inverse of passed argument.
A function that returns the ! of its argument. It will return true when passed false-y value, and false when passed a truth-y one.

See also complement.

R.not(true); //=> false
R.not(false); //=> true
R.not(0); //=> true
R.not(1); //=> false
nth Added in v0.1.0

Number → [a] → a | Undefined
Number → String → String
Parameters

offset
list
Returns

*
Returns the nth element of the given list or string. If n is negative the element at index length + n is returned.

var list = ['foo', 'bar', 'baz', 'quux'];
R.nth(1, list); //=> 'bar'
R.nth(-1, list); //=> 'quux'
R.nth(-99, list); //=> undefined

R.nth(2, 'abc'); //=> 'c'
R.nth(3, 'abc'); //=> ''
nthArg Added in v0.9.0

Number → *… → *
Parameters

n
Returns

function
Returns a function which returns its nth argument.

R.nthArg(1)('a', 'b', 'c'); //=> 'b'
R.nthArg(-1)('a', 'b', 'c'); //=> 'c'
o Added in v0.24.0

(b → c) → (a → b) → a → c
Parameters

f
g
Returns

function
o is a curried composition function that returns a unary function. Like compose, o performs right-to-left function composition. Unlike compose, the rightmost function passed to o will be invoked with only one argument.

See also compose, pipe.

var classyGreeting = name => "The name's " + name.last + ", " + name.first + " " + name.last
var yellGreeting = R.o(R.toUpper, classyGreeting);
yellGreeting({first: 'James', last: 'Bond'}); //=> "THE NAME'S BOND, JAMES BOND"

R.o(R.multiply(10), R.add(10))(-4) //=> 60
objOf Added in v0.18.0

String → a → {String:a}
Parameters

key
val
Returns

Object
Creates an object containing a single key:value pair.

See also pair.

var matchPhrases = R.compose(
  R.objOf('must'),
  R.map(R.objOf('match_phrase'))
);
matchPhrases(['foo', 'bar', 'baz']); //=> {must: [{match_phrase: 'foo'}, {match_phrase: 'bar'}, {match_phrase: 'baz'}]}
of Added in v0.3.0

a → [a]
Parameters

x
any value
Returns

Array An array wrapping `x`.
Returns a singleton array containing the value provided.

Note this of is different from the ES6 of; See https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of

R.of(null); //=> [null]
R.of([42]); //=> [[42]]
omit Added in v0.1.0

[String] → {String: *} → {String: *}
Parameters

names
an array of String property names to omit from the new object
 obj
The object to copy from
Returns

Object A new object with properties from `names` not on it.
Returns a partial copy of an object omitting the keys specified.

See also pick.

R.omit(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, c: 3}
once Added in v0.1.0

(a… → b) → (a… → b)
Parameters

fn
The function to wrap in a call-only-once wrapper.
Returns

function The wrapped function.
Accepts a function fn and returns a function that guards invocation of fn such that fn can only ever be called once, no matter how many times the returned function is invoked. The first value calculated is returned in subsequent invocations.

var addOneOnce = R.once(x => x + 1);
addOneOnce(10); //=> 11
addOneOnce(addOneOnce(50)); //=> 11
or Added in v0.1.0

a → b → a | b
Parameters

a
b
Returns

Any the first argument if truthy, otherwise the second argument.
Returns true if one or both of its arguments are true. Returns false if both arguments are false.

See also either.

R.or(true, true); //=> true
R.or(true, false); //=> true
R.or(false, true); //=> true
R.or(false, false); //=> false
over Added in v0.16.0

Lens s a → (a → a) → s → s
Lens s a = Functor f => (a → f a) → s → f s
Parameters

lens
v
x
Returns

*
Returns the result of "setting" the portion of the given data structure focused by the given lens to the result of applying the given function to the focused value.

See also prop, lensIndex, lensProp.

var headLens = R.lensIndex(0);

R.over(headLens, R.toUpper, ['foo', 'bar', 'baz']); //=> ['FOO', 'bar', 'baz']
pair Added in v0.18.0

a → b → (a,b)
Parameters

fst
snd
Returns

Array
Takes two arguments, fst and snd, and returns [fst, snd].

See also objOf, of.

R.pair('foo', 'bar'); //=> ['foo', 'bar']
partial Added in v0.10.0

((a, b, c, …, n) → x) → [a, b, c, …] → ((d, e, f, …, n) → x)
Parameters

f
args
Returns

function
Takes a function f and a list of arguments, and returns a function g. When applied, g returns the result of applying f to the arguments provided initially followed by the arguments provided to g.

See also partialRight.

var multiply2 = (a, b) => a * b;
var double = R.partial(multiply2, [2]);
double(2); //=> 4

var greet = (salutation, title, firstName, lastName) =>
  salutation + ', ' + title + ' ' + firstName + ' ' + lastName + '!';

var sayHello = R.partial(greet, ['Hello']);
var sayHelloToMs = R.partial(sayHello, ['Ms.']);
sayHelloToMs('Jane', 'Jones'); //=> 'Hello, Ms. Jane Jones!'
partialRight Added in v0.10.0

((a, b, c, …, n) → x) → [d, e, f, …, n] → ((a, b, c, …) → x)
Parameters

f
args
Returns

function
Takes a function f and a list of arguments, and returns a function g. When applied, g returns the result of applying f to the arguments provided to g followed by the arguments provided initially.

See also partial.

var greet = (salutation, title, firstName, lastName) =>
  salutation + ', ' + title + ' ' + firstName + ' ' + lastName + '!';

var greetMsJaneJones = R.partialRight(greet, ['Ms.', 'Jane', 'Jones']);

greetMsJaneJones('Hello'); //=> 'Hello, Ms. Jane Jones!'
partition Added in v0.1.4

Filterable f => (a → Boolean) → f a → [f a, f a]
Parameters

pred
A predicate to determine which side the element belongs to.
 filterable
the list (or other filterable) to partition.
Returns

Array An array, containing first the subset of elements that satisfy the predicate, and second the subset of elements that do not satisfy.
Takes a predicate and a list or other Filterable object and returns the pair of filterable objects of the same type of elements which do and do not satisfy, the predicate, respectively. Filterable objects include plain objects or any object that has a filter method such as Array.

See also filter, reject.

R.partition(R.contains('s'), ['sss', 'ttt', 'foo', 'bars']);
// => [ [ 'sss', 'bars' ],  [ 'ttt', 'foo' ] ]

R.partition(R.contains('s'), { a: 'sss', b: 'ttt', foo: 'bars' });
// => [ { a: 'sss', foo: 'bars' }, { b: 'ttt' }  ]
path Added in v0.2.0

[Idx] → {a} → a | Undefined
Idx = String | Int
Parameters

path
The path to use.
 obj
The object to retrieve the nested property from.
Returns

* The data at `path`.
Retrieve the value at a given path.

See also prop.

R.path(['a', 'b'], {a: {b: 2}}); //=> 2
R.path(['a', 'b'], {c: {b: 2}}); //=> undefined
pathEq Added in v0.7.0

[Idx] → a → {a} → Boolean
Idx = String | Int
Parameters

path
The path of the nested property to use
 val
The value to compare the nested property with
 obj
The object to check the nested property in
Returns

Boolean `true` if the value equals the nested object property, `false` otherwise.
Determines whether a nested path on an object has a specific value, in R.equals terms. Most likely used to filter a list.

var user1 = { address: { zipCode: 90210 } };
var user2 = { address: { zipCode: 55555 } };
var user3 = { name: 'Bob' };
var users = [ user1, user2, user3 ];
var isFamous = R.pathEq(['address', 'zipCode'], 90210);
R.filter(isFamous, users); //=> [ user1 ]
pathOr Added in v0.18.0

a → [Idx] → {a} → a
Idx = String | Int
Parameters

d
The default value.
 p
The path to use.
 obj
The object to retrieve the nested property from.
Returns

* The data at `path` of the supplied object or the default value.
If the given, non-null object has a value at the given path, returns the value at that path. Otherwise returns the provided default value.

R.pathOr('N/A', ['a', 'b'], {a: {b: 2}}); //=> 2
R.pathOr('N/A', ['a', 'b'], {c: {b: 2}}); //=> "N/A"
pathSatisfies Added in v0.19.0

(a → Boolean) → [Idx] → {a} → Boolean
Idx = String | Int
Parameters

pred
propPath
obj
Returns

Boolean
Returns true if the specified object property at given path satisfies the given predicate; false otherwise.

See also propSatisfies, path.

R.pathSatisfies(y => y > 0, ['x', 'y'], {x: {y: 2}}); //=> true
pick Added in v0.1.0

[k] → {k: v} → {k: v}
Parameters

names
an array of String property names to copy onto a new object
 obj
The object to copy from
Returns

Object A new object with only properties from `names` on it.
Returns a partial copy of an object containing only the keys specified. If the key does not exist, the property is ignored.

See also omit, props.

R.pick(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, d: 4}
R.pick(['a', 'e', 'f'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1}
pickAll Added in v0.1.0

[k] → {k: v} → {k: v}
Parameters

names
an array of String property names to copy onto a new object
 obj
The object to copy from
Returns

Object A new object with only properties from `names` on it.
Similar to pick except that this one includes a key: undefined pair for properties that don't exist.

See also pick.

R.pickAll(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, d: 4}
R.pickAll(['a', 'e', 'f'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, e: undefined, f: undefined}
pickBy Added in v0.8.0

((v, k) → Boolean) → {k: v} → {k: v}
Parameters

pred
A predicate to determine whether or not a key should be included on the output object.
 obj
The object to copy from
Returns

Object A new object with only properties that satisfy `pred` on it.
Returns a partial copy of an object containing only the keys that satisfy the supplied predicate.

See also pick, filter.

var isUpperCase = (val, key) => key.toUpperCase() === key;
R.pickBy(isUpperCase, {a: 1, b: 2, A: 3, B: 4}); //=> {A: 3, B: 4}
pipe Added in v0.1.0

(((a, b, …, n) → o), (o → p), …, (x → y), (y → z)) → ((a, b, …, n) → z)
Parameters

functions
Returns

function
Performs left-to-right function composition. The leftmost function may have any arity; the remaining functions must be unary.

In some libraries this function is named sequence.

Note: The result of pipe is not automatically curried.

See also compose.

var f = R.pipe(Math.pow, R.negate, R.inc);

f(3, 4); // -(3^4) + 1
pipeK Added in v0.16.0

Chain m => ((a → m b), (b → m c), …, (y → m z)) → (a → m z)
Parameters

Returns

function
Returns the left-to-right Kleisli composition of the provided functions, each of which must return a value of a type supported by chain.

R.pipeK(f, g, h) is equivalent to R.pipe(f, R.chain(g), R.chain(h)).

See also composeK.

//  parseJson :: String -> Maybe *
//  get :: String -> Object -> Maybe *

//  getStateCode :: Maybe String -> Maybe String
var getStateCode = R.pipeK(
  parseJson,
  get('user'),
  get('address'),
  get('state'),
  R.compose(Maybe.of, R.toUpper)
);

getStateCode('{"user":{"address":{"state":"ny"}}}');
//=> Just('NY')
getStateCode('[Invalid JSON]');
//=> Nothing()
pipeP Added in v0.10.0

((a → Promise b), (b → Promise c), …, (y → Promise z)) → (a → Promise z)
Parameters

functions
Returns

function
Performs left-to-right composition of one or more Promise-returning functions. The leftmost function may have any arity; the remaining functions must be unary.

See also composeP.

//  followersForUser :: String -> Promise [User]
var followersForUser = R.pipeP(db.getUserById, db.getFollowers);
pluck Added in v0.1.0

Functor f => k → f {k: v} → f v
Parameters

key
The key name to pluck off of each object.
 f
The array or functor to consider.
Returns

Array The list of values for the given key.
Returns a new list by plucking the same named property off all objects in the list supplied.

pluck will work on any functor in addition to arrays, as it is equivalent to R.map(R.prop(k), f).

See also props.

R.pluck('a')([{a: 1}, {a: 2}]); //=> [1, 2]
R.pluck(0)([[1, 2], [3, 4]]);   //=> [1, 3]
R.pluck('val', {a: {val: 3}, b: {val: 5}}); //=> {a: 3, b: 5}
prepend Added in v0.1.0

a → [a] → [a]
Parameters

el
The item to add to the head of the output list.
 list
The array to add to the tail of the output list.
Returns

Array A new array.
Returns a new list with the given element at the front, followed by the contents of the list.

See also append.

R.prepend('fee', ['fi', 'fo', 'fum']); //=> ['fee', 'fi', 'fo', 'fum']
product Added in v0.1.0

[Number] → Number
Parameters

list
An array of numbers
Returns

Number The product of all the numbers in the list.
Multiplies together all the elements of a list.

See also reduce.

R.product([2,4,6,8,100,1]); //=> 38400
project Added in v0.1.0

[k] → [{k: v}] → [{k: v}]
Parameters

props
The property names to project
 objs
The objects to query
Returns

Array An array of objects with just the `props` properties.
Reasonable analog to SQL select statement.

var abby = {name: 'Abby', age: 7, hair: 'blond', grade: 2};
var fred = {name: 'Fred', age: 12, hair: 'brown', grade: 7};
var kids = [abby, fred];
R.project(['name', 'grade'], kids); //=> [{name: 'Abby', grade: 2}, {name: 'Fred', grade: 7}]
prop Added in v0.1.0

s → {s: a} → a | Undefined
Parameters

p
The property name
 obj
The object to query
Returns

* The value at `obj.p`.
Returns a function that when supplied an object returns the indicated property of that object, if it exists.

See also path.

R.prop('x', {x: 100}); //=> 100
R.prop('x', {}); //=> undefined
propEq Added in v0.1.0

String → a → Object → Boolean
Parameters

name
val
obj
Returns

Boolean
Returns true if the specified object property is equal, in R.equals terms, to the given value; false otherwise. You can test multiple properties with R.where.

See also whereEq, propSatisfies, equals.

var abby = {name: 'Abby', age: 7, hair: 'blond'};
var fred = {name: 'Fred', age: 12, hair: 'brown'};
var rusty = {name: 'Rusty', age: 10, hair: 'brown'};
var alois = {name: 'Alois', age: 15, disposition: 'surly'};
var kids = [abby, fred, rusty, alois];
var hasBrownHair = R.propEq('hair', 'brown');
R.filter(hasBrownHair, kids); //=> [fred, rusty]
propIs Added in v0.16.0

Type → String → Object → Boolean
Parameters

type
name
obj
Returns

Boolean
Returns true if the specified object property is of the given type; false otherwise.

See also is, propSatisfies.

R.propIs(Number, 'x', {x: 1, y: 2});  //=> true
R.propIs(Number, 'x', {x: 'foo'});    //=> false
R.propIs(Number, 'x', {});            //=> false
propOr Added in v0.6.0

a → String → Object → a
Parameters

val
The default value.
 p
The name of the property to return.
 obj
The object to query.
Returns

* The value of given property of the supplied object or the default value.
If the given, non-null object has an own property with the specified name, returns the value of that property. Otherwise returns the provided default value.

var alice = {
  name: 'ALICE',
  age: 101
};
var favorite = R.prop('favoriteLibrary');
var favoriteWithDefault = R.propOr('Ramda', 'favoriteLibrary');

favorite(alice);  //=> undefined
favoriteWithDefault(alice);  //=> 'Ramda'
props Added in v0.1.0

[k] → {k: v} → [v]
Parameters

ps
The property names to fetch
 obj
The object to query
Returns

Array The corresponding values or partially applied function.
Acts as multiple prop: array of keys in, array of values out. Preserves order.

R.props(['x', 'y'], {x: 1, y: 2}); //=> [1, 2]
R.props(['c', 'a', 'b'], {b: 2, a: 1}); //=> [undefined, 1, 2]

var fullName = R.compose(R.join(' '), R.props(['first', 'last']));
fullName({last: 'Bullet-Tooth', age: 33, first: 'Tony'}); //=> 'Tony Bullet-Tooth'
propSatisfies Added in v0.16.0

(a → Boolean) → String → {String: a} → Boolean
Parameters

pred
name
obj
Returns

Boolean
Returns true if the specified object property satisfies the given predicate; false otherwise. You can test multiple properties with R.where.

See also where, propEq, propIs.

R.propSatisfies(x => x > 0, 'x', {x: 1, y: 2}); //=> true
range Added in v0.1.0

Number → Number → [Number]
Parameters

from
The first number in the list.
 to
One more than the last number in the list.
Returns

Array The list of numbers in tthe set `[a, b)`.
Returns a list of numbers from from (inclusive) to to (exclusive).

R.range(1, 5);    //=> [1, 2, 3, 4]
R.range(50, 53);  //=> [50, 51, 52]
reduce Added in v0.1.0

((a, b) → a) → a → [b] → a
Parameters

fn
The iterator function. Receives two values, the accumulator and the current element from the array.
 acc
The accumulator value.
 list
The list to iterate over.
Returns

* The final, accumulated value.
Returns a single item by iterating through the list, successively calling the iterator function and passing it an accumulator value and the current value from the array, and then passing the result to the next call.

The iterator function receives two values: (acc, value). It may use R.reduced to shortcut the iteration.

The arguments' order of reduceRight's iterator function is (value, acc).

Note: R.reduce does not skip deleted or unassigned indices (sparse arrays), unlike the native Array.prototype.reduce method. For more details on this behavior, see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce#Description

Dispatches to the reduce method of the third argument, if present. When doing so, it is up to the user to handle the R.reduced shortcuting, as this is not implemented by reduce.

See also reduced, addIndex, reduceRight.

R.reduce(R.subtract, 0, [1, 2, 3, 4]) // => ((((0 - 1) - 2) - 3) - 4) = -10
//          -               -10
//         / \              / \
//        -   4           -6   4
//       / \              / \
//      -   3   ==>     -3   3
//     / \              / \
//    -   2           -1   2
//   / \              / \
//  0   1            0   1
reduceBy Added in v0.20.0

((a, b) → a) → a → (b → String) → [b] → {String: a}
Parameters

valueFn
The function that reduces the elements of each group to a single value. Receives two values, accumulator for a particular group and the current element.
 acc
The (initial) accumulator value for each group.
 keyFn
The function that maps the list's element into a key.
 list
The array to group.
Returns

Object An object with the output of `keyFn` for keys, mapped to the output of `valueFn` for elements which produced that key when passed to `keyFn`.
Groups the elements of the list according to the result of calling the String-returning function keyFn on each element and reduces the elements of each group to a single value via the reducer function valueFn.

This function is basically a more general groupBy function.

Acts as a transducer if a transformer is given in list position.

See also groupBy, reduce.

var reduceToNamesBy = R.reduceBy((acc, student) => acc.concat(student.name), []);
var namesByGrade = reduceToNamesBy(function(student) {
  var score = student.score;
  return score < 65 ? 'F' :
         score < 70 ? 'D' :
         score < 80 ? 'C' :
         score < 90 ? 'B' : 'A';
});
var students = [{name: 'Lucy', score: 92},
                {name: 'Drew', score: 85},
                // ...
                {name: 'Bart', score: 62}];
namesByGrade(students);
// {
//   'A': ['Lucy'],
//   'B': ['Drew']
//   // ...,
//   'F': ['Bart']
// }
reduced Added in v0.15.0

a → *
Parameters

x
The final value of the reduce.
Returns

* The wrapped value.
Returns a value wrapped to indicate that it is the final value of the reduce and transduce functions. The returned value should be considered a black box: the internal structure is not guaranteed to be stable.

Note: this optimization is unavailable to functions not explicitly listed above. For instance, it is not currently supported by reduceRight.

See also reduce, transduce.

R.reduce(
 (acc, item) => item > 3 ? R.reduced(acc) : acc.concat(item),
 [],
 [1, 2, 3, 4, 5]) // [1, 2, 3]
reduceRight Added in v0.1.0

((a, b) → b) → b → [a] → b
Parameters

fn
The iterator function. Receives two values, the current element from the array and the accumulator.
 acc
The accumulator value.
 list
The list to iterate over.
Returns

* The final, accumulated value.
Returns a single item by iterating through the list, successively calling the iterator function and passing it an accumulator value and the current value from the array, and then passing the result to the next call.

Similar to reduce, except moves through the input list from the right to the left.

The iterator function receives two values: (value, acc), while the arguments' order of reduce's iterator function is (acc, value).

Note: R.reduceRight does not skip deleted or unassigned indices (sparse arrays), unlike the native Array.prototype.reduceRight method. For more details on this behavior, see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight#Description

See also reduce, addIndex.

R.reduceRight(R.subtract, 0, [1, 2, 3, 4]) // => (1 - (2 - (3 - (4 - 0)))) = -2
//    -               -2
//   / \              / \
//  1   -            1   3
//     / \              / \
//    2   -     ==>    2  -1
//       / \              / \
//      3   -            3   4
//         / \              / \
//        4   0            4   0
reduceWhile Added in v0.22.0

((a, b) → Boolean) → ((a, b) → a) → a → [b] → a
Parameters

pred
The predicate. It is passed the accumulator and the current element.
 fn
The iterator function. Receives two values, the accumulator and the current element.
 a
The accumulator value.
 list
The list to iterate over.
Returns

* The final, accumulated value.
Like reduce, reduceWhile returns a single item by iterating through the list, successively calling the iterator function. reduceWhile also takes a predicate that is evaluated before each step. If the predicate returns false, it "short-circuits" the iteration and returns the current value of the accumulator.

See also reduce, reduced.

var isOdd = (acc, x) => x % 2 === 1;
var xs = [1, 3, 5, 60, 777, 800];
R.reduceWhile(isOdd, R.add, 0, xs); //=> 9

var ys = [2, 4, 6]
R.reduceWhile(isOdd, R.add, 111, ys); //=> 111
reject Added in v0.1.0

Filterable f => (a → Boolean) → f a → f a
Parameters

pred
filterable
Returns

Array
The complement of filter.

Acts as a transducer if a transformer is given in list position. Filterable objects include plain objects or any object that has a filter method such as Array.

See also filter, transduce, addIndex.

var isOdd = (n) => n % 2 === 1;

R.reject(isOdd, [1, 2, 3, 4]); //=> [2, 4]

R.reject(isOdd, {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, d: 4}
remove Added in v0.2.2

Number → Number → [a] → [a]
Parameters

start
The position to start removing elements
 count
The number of elements to remove
 list
The list to remove from
Returns

Array A new Array with `count` elements from `start` removed.
Removes the sub-list of list starting at index start and containing count elements. Note that this is not destructive: it returns a copy of the list with the changes. No lists have been harmed in the application of this function.

R.remove(2, 3, [1,2,3,4,5,6,7,8]); //=> [1,2,6,7,8]
repeat Added in v0.1.1

a → n → [a]
Parameters

value
The value to repeat.
 n
The desired size of the output list.
Returns

Array A new array containing `n` `value`s.
Returns a fixed list of size n containing a specified identical value.

See also times.

R.repeat('hi', 5); //=> ['hi', 'hi', 'hi', 'hi', 'hi']

var obj = {};
var repeatedObjs = R.repeat(obj, 5); //=> [{}, {}, {}, {}, {}]
repeatedObjs[0] === repeatedObjs[1]; //=> true
replace Added in v0.7.0

RegExp|String → String → String → String
Parameters

pattern
A regular expression or a substring to match.
 replacement
The string to replace the matches with.
 str
The String to do the search and replacement in.
Returns

String The result.
Replace a substring or regex match in a string with a replacement.

R.replace('foo', 'bar', 'foo foo foo'); //=> 'bar foo foo'
R.replace(/foo/, 'bar', 'foo foo foo'); //=> 'bar foo foo'

// Use the "g" (global) flag to replace all occurrences:
R.replace(/foo/g, 'bar', 'foo foo foo'); //=> 'bar bar bar'
reverse Added in v0.1.0

[a] → [a]
String → String
Parameters

list
Returns

Array
Returns a new list or string with the elements or characters in reverse order.

R.reverse([1, 2, 3]);  //=> [3, 2, 1]
R.reverse([1, 2]);     //=> [2, 1]
R.reverse([1]);        //=> [1]
R.reverse([]);         //=> []

R.reverse('abc');      //=> 'cba'
R.reverse('ab');       //=> 'ba'
R.reverse('a');        //=> 'a'
R.reverse('');         //=> ''
scan Added in v0.10.0

((a, b) → a) → a → [b] → [a]
Parameters

fn
The iterator function. Receives two values, the accumulator and the current element from the array
 acc
The accumulator value.
 list
The list to iterate over.
Returns

Array A list of all intermediately reduced values.
Scan is similar to reduce, but returns a list of successively reduced values from the left

See also reduce.

var numbers = [1, 2, 3, 4];
var factorials = R.scan(R.multiply, 1, numbers); //=> [1, 1, 2, 6, 24]
sequence Added in v0.19.0

(Applicative f, Traversable t) => (a → f a) → t (f a) → f (t a)
Parameters

of
traversable
Returns

*
Transforms a Traversable of Applicative into an Applicative of Traversable.

Dispatches to the sequence method of the second argument, if present.

See also traverse.

R.sequence(Maybe.of, [Just(1), Just(2), Just(3)]);   //=> Just([1, 2, 3])
R.sequence(Maybe.of, [Just(1), Just(2), Nothing()]); //=> Nothing()

R.sequence(R.of, Just([1, 2, 3])); //=> [Just(1), Just(2), Just(3)]
R.sequence(R.of, Nothing());       //=> [Nothing()]
set Added in v0.16.0

Lens s a → a → s → s
Lens s a = Functor f => (a → f a) → s → f s
Parameters

lens
v
x
Returns

*
Returns the result of "setting" the portion of the given data structure focused by the given lens to the given value.

See also prop, lensIndex, lensProp.

var xLens = R.lensProp('x');

R.set(xLens, 4, {x: 1, y: 2});  //=> {x: 4, y: 2}
R.set(xLens, 8, {x: 1, y: 2});  //=> {x: 8, y: 2}
slice Added in v0.1.4

Number → Number → [a] → [a]
Number → Number → String → String
Parameters

fromIndex
The start index (inclusive).
 toIndex
The end index (exclusive).
 list
Returns

*
Returns the elements of the given list or string (or object with a slice method) from fromIndex (inclusive) to toIndex (exclusive).

Dispatches to the slice method of the third argument, if present.

R.slice(1, 3, ['a', 'b', 'c', 'd']);        //=> ['b', 'c']
R.slice(1, Infinity, ['a', 'b', 'c', 'd']); //=> ['b', 'c', 'd']
R.slice(0, -1, ['a', 'b', 'c', 'd']);       //=> ['a', 'b', 'c']
R.slice(-3, -1, ['a', 'b', 'c', 'd']);      //=> ['b', 'c']
R.slice(0, 3, 'ramda');                     //=> 'ram'
sort Added in v0.1.0

((a, a) → Number) → [a] → [a]
Parameters

comparator
A sorting function :: a -> b -> Int
 list
The list to sort
Returns

Array a new array with its elements sorted by the comparator function.
Returns a copy of the list, sorted according to the comparator function, which should accept two values at a time and return a negative number if the first value is smaller, a positive number if it's larger, and zero if they are equal. Please note that this is a copy of the list. It does not modify the original.

var diff = function(a, b) { return a - b; };
R.sort(diff, [4,2,7,5]); //=> [2, 4, 5, 7]
sortBy Added in v0.1.0

Ord b => (a → b) → [a] → [a]
Parameters

fn
list
The list to sort.
Returns

Array A new list sorted by the keys generated by `fn`.
Sorts the list according to the supplied function.

var sortByFirstItem = R.sortBy(R.prop(0));
var sortByNameCaseInsensitive = R.sortBy(R.compose(R.toLower, R.prop('name')));
var pairs = [[-1, 1], [-2, 2], [-3, 3]];
sortByFirstItem(pairs); //=> [[-3, 3], [-2, 2], [-1, 1]]
var alice = {
  name: 'ALICE',
  age: 101
};
var bob = {
  name: 'Bob',
  age: -10
};
var clara = {
  name: 'clara',
  age: 314.159
};
var people = [clara, bob, alice];
sortByNameCaseInsensitive(people); //=> [alice, bob, clara]
sortWith Added in v0.23.0

[(a, a) → Number] → [a] → [a]
Parameters

functions
A list of comparator functions.
 list
The list to sort.
Returns

Array A new list sorted according to the comarator functions.
Sorts a list according to a list of comparators.

var alice = {
  name: 'alice',
  age: 40
};
var bob = {
  name: 'bob',
  age: 30
};
var clara = {
  name: 'clara',
  age: 40
};
var people = [clara, bob, alice];
var ageNameSort = R.sortWith([
  R.descend(R.prop('age')),
  R.ascend(R.prop('name'))
]);
ageNameSort(people); //=> [alice, clara, bob]
split Added in v0.1.0

(String | RegExp) → String → [String]
Parameters

sep
The pattern.
 str
The string to separate into an array.
Returns

Array The array of strings from `str` separated by `str`.
Splits a string into an array of strings based on the given separator.

See also join.

var pathComponents = R.split('/');
R.tail(pathComponents('/usr/local/bin/node')); //=> ['usr', 'local', 'bin', 'node']

R.split('.', 'a.b.c.xyz.d'); //=> ['a', 'b', 'c', 'xyz', 'd']
splitAt Added in v0.19.0

Number → [a] → [[a], [a]]
Number → String → [String, String]
Parameters

index
The index where the array/string is split.
 array
The array/string to be split.
Returns

Array
Splits a given list or string at a given index.

R.splitAt(1, [1, 2, 3]);          //=> [[1], [2, 3]]
R.splitAt(5, 'hello world');      //=> ['hello', ' world']
R.splitAt(-1, 'foobar');          //=> ['fooba', 'r']
splitEvery Added in v0.16.0

Number → [a] → [[a]]
Number → String → [String]
Parameters

n
list
Returns

Array
Splits a collection into slices of the specified length.

R.splitEvery(3, [1, 2, 3, 4, 5, 6, 7]); //=> [[1, 2, 3], [4, 5, 6], [7]]
R.splitEvery(3, 'foobarbaz'); //=> ['foo', 'bar', 'baz']
splitWhen Added in v0.19.0

(a → Boolean) → [a] → [[a], [a]]
Parameters

pred
The predicate that determines where the array is split.
 list
The array to be split.
Returns

Array
Takes a list and a predicate and returns a pair of lists with the following properties:

the result of concatenating the two output lists is equivalent to the input list;
none of the elements of the first output list satisfies the predicate; and
if the second output list is non-empty, its first element satisfies the predicate.
R.splitWhen(R.equals(2), [1, 2, 3, 1, 2, 3]);   //=> [[1], [2, 3, 1, 2, 3]]
startsWith Added in v0.24.0

[a] → Boolean
String → Boolean
Parameters

prefix
list
Returns

Boolean
Checks if a list starts with the provided values

R.startsWith('a', 'abc')                //=> true
R.startsWith('b', 'abc')                //=> false
R.startsWith(['a'], ['a', 'b', 'c'])    //=> true
R.startsWith(['b'], ['a', 'b', 'c'])    //=> false
subtract Added in v0.1.0

Number → Number → Number
Parameters

a
The first value.
 b
The second value.
Returns

Number The result of `a - b`.
Subtracts its second argument from its first argument.

See also add.

R.subtract(10, 8); //=> 2

var minus5 = R.subtract(R.__, 5);
minus5(17); //=> 12

var complementaryAngle = R.subtract(90);
complementaryAngle(30); //=> 60
complementaryAngle(72); //=> 18
sum Added in v0.1.0

[Number] → Number
Parameters

list
An array of numbers
Returns

Number The sum of all the numbers in the list.
Adds together all the elements of a list.

See also reduce.

R.sum([2,4,6,8,100,1]); //=> 121
symmetricDifference Added in v0.19.0

[*] → [*] → [*]
Parameters

list1
The first list.
 list2
The second list.
Returns

Array The elements in `list1` or `list2`, but not both.
Finds the set (i.e. no duplicates) of all elements contained in the first or second list, but not both.

See also symmetricDifferenceWith, difference, differenceWith.

R.symmetricDifference([1,2,3,4], [7,6,5,4,3]); //=> [1,2,7,6,5]
R.symmetricDifference([7,6,5,4,3], [1,2,3,4]); //=> [7,6,5,1,2]
symmetricDifferenceWith Added in v0.19.0

((a, a) → Boolean) → [a] → [a] → [a]
Parameters

pred
A predicate used to test whether two items are equal.
 list1
The first list.
 list2
The second list.
Returns

Array The elements in `list1` or `list2`, but not both.
Finds the set (i.e. no duplicates) of all elements contained in the first or second list, but not both. Duplication is determined according to the value returned by applying the supplied predicate to two list elements.

See also symmetricDifference, difference, differenceWith.

var eqA = R.eqBy(R.prop('a'));
var l1 = [{a: 1}, {a: 2}, {a: 3}, {a: 4}];
var l2 = [{a: 3}, {a: 4}, {a: 5}, {a: 6}];
R.symmetricDifferenceWith(eqA, l1, l2); //=> [{a: 1}, {a: 2}, {a: 5}, {a: 6}]
T Added in v0.9.0

* → Boolean
Parameters

Returns

Boolean
A function that always returns true. Any passed in parameters are ignored.

See also always, F.

R.T(); //=> true
tail Added in v0.1.0

[a] → [a]
String → String
Parameters

list
Returns

*
Returns all but the first element of the given list or string (or object with a tail method).

Dispatches to the slice method of the first argument, if present.

See also head, init, last.

R.tail([1, 2, 3]);  //=> [2, 3]
R.tail([1, 2]);     //=> [2]
R.tail([1]);        //=> []
R.tail([]);         //=> []

R.tail('abc');  //=> 'bc'
R.tail('ab');   //=> 'b'
R.tail('a');    //=> ''
R.tail('');     //=> ''
take Added in v0.1.0

Number → [a] → [a]
Number → String → String
Parameters

n
list
Returns

*
Returns the first n elements of the given list, string, or transducer/transformer (or object with a take method).

Dispatches to the take method of the second argument, if present.

See also drop.

R.take(1, ['foo', 'bar', 'baz']); //=> ['foo']
R.take(2, ['foo', 'bar', 'baz']); //=> ['foo', 'bar']
R.take(3, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.take(4, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.take(3, 'ramda');               //=> 'ram'

var personnel = [
  'Dave Brubeck',
  'Paul Desmond',
  'Eugene Wright',
  'Joe Morello',
  'Gerry Mulligan',
  'Bob Bates',
  'Joe Dodge',
  'Ron Crotty'
];

var takeFive = R.take(5);
takeFive(personnel);
//=> ['Dave Brubeck', 'Paul Desmond', 'Eugene Wright', 'Joe Morello', 'Gerry Mulligan']
takeLast Added in v0.16.0

Number → [a] → [a]
Number → String → String
Parameters

n
The number of elements to return.
 xs
The collection to consider.
Returns

Array
Returns a new list containing the last n elements of the given list. If n > list.length, returns a list of list.length elements.

See also dropLast.

R.takeLast(1, ['foo', 'bar', 'baz']); //=> ['baz']
R.takeLast(2, ['foo', 'bar', 'baz']); //=> ['bar', 'baz']
R.takeLast(3, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.takeLast(4, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.takeLast(3, 'ramda');               //=> 'mda'
takeLastWhile Added in v0.16.0

(a → Boolean) → [a] → [a]
(a → Boolean) → String → String
Parameters

fn
The function called per iteration.
 xs
The collection to iterate over.
Returns

Array A new array.
Returns a new list containing the last n elements of a given list, passing each value to the supplied predicate function, and terminating when the predicate function returns false. Excludes the element that caused the predicate function to fail. The predicate function is passed one argument: (value).

See also dropLastWhile, addIndex.

var isNotOne = x => x !== 1;

R.takeLastWhile(isNotOne, [1, 2, 3, 4]); //=> [2, 3, 4]

R.takeLastWhile(x => x !== 'R' , 'Ramda'); //=> 'amda'
takeWhile Added in v0.1.0

(a → Boolean) → [a] → [a]
(a → Boolean) → String → String
Parameters

fn
The function called per iteration.
 xs
The collection to iterate over.
Returns

Array A new array.
Returns a new list containing the first n elements of a given list, passing each value to the supplied predicate function, and terminating when the predicate function returns false. Excludes the element that caused the predicate function to fail. The predicate function is passed one argument: (value).

Dispatches to the takeWhile method of the second argument, if present.

Acts as a transducer if a transformer is given in list position.

See also dropWhile, transduce, addIndex.

var isNotFour = x => x !== 4;

R.takeWhile(isNotFour, [1, 2, 3, 4, 3, 2, 1]); //=> [1, 2, 3]

R.takeWhile(x => x !== 'd' , 'Ramda'); //=> 'Ram'
tap Added in v0.1.0

(a → *) → a → a
Parameters

fn
The function to call with x. The return value of fn will be thrown away.
 x
Returns

* `x`.
Runs the given function with the supplied object, then returns the object.

Acts as a transducer if a transformer is given as second parameter.

var sayX = x => console.log('x is ' + x);
R.tap(sayX, 100); //=> 100
// logs 'x is 100'
test Added in v0.12.0

RegExp → String → Boolean
Parameters

pattern
str
Returns

Boolean
Determines whether a given string matches a given regular expression.

See also match.

R.test(/^x/, 'xyz'); //=> true
R.test(/^y/, 'xyz'); //=> false
times Added in v0.2.3

(Number → a) → Number → [a]
Parameters

fn
The function to invoke. Passed one argument, the current value of n.
 n
A value between 0 and n - 1. Increments after each function call.
Returns

Array An array containing the return values of all calls to `fn`.
Calls an input function n times, returning an array containing the results of those function calls.

fn is passed one argument: The current value of n, which begins at 0 and is gradually incremented to n - 1.

See also repeat.

R.times(R.identity, 5); //=> [0, 1, 2, 3, 4]
toLower Added in v0.9.0

String → String
Parameters

str
The string to lower case.
Returns

String The lower case version of `str`.
The lower case version of a string.

See also toUpper.

R.toLower('XYZ'); //=> 'xyz'
toPairs Added in v0.4.0

{String: *} → [[String,*]]
Parameters

obj
The object to extract from
Returns

Array An array of key, value arrays from the object's own properties.
Converts an object into an array of key, value arrays. Only the object's own properties are used. Note that the order of the output array is not guaranteed to be consistent across different JS platforms.

See also fromPairs.

R.toPairs({a: 1, b: 2, c: 3}); //=> [['a', 1], ['b', 2], ['c', 3]]
toPairsIn Added in v0.4.0

{String: *} → [[String,*]]
Parameters

obj
The object to extract from
Returns

Array An array of key, value arrays from the object's own and prototype properties.
Converts an object into an array of key, value arrays. The object's own properties and prototype properties are used. Note that the order of the output array is not guaranteed to be consistent across different JS platforms.

var F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
var f = new F();
R.toPairsIn(f); //=> [['x','X'], ['y','Y']]
toString Added in v0.14.0

* → String
Parameters

val
Returns

String
Returns the string representation of the given value. eval'ing the output should result in a value equivalent to the input value. Many of the built-in toString methods do not satisfy this requirement.

If the given value is an [object Object] with a toString method other than Object.prototype.toString, this method is invoked with no arguments to produce the return value. This means user-defined constructor functions can provide a suitable toString method. For example:

function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function() {
  return 'new Point(' + this.x + ', ' + this.y + ')';
};

R.toString(new Point(1, 2)); //=> 'new Point(1, 2)'
R.toString(42); //=> '42'
R.toString('abc'); //=> '"abc"'
R.toString([1, 2, 3]); //=> '[1, 2, 3]'
R.toString({foo: 1, bar: 2, baz: 3}); //=> '{"bar": 2, "baz": 3, "foo": 1}'
R.toString(new Date('2001-02-03T04:05:06Z')); //=> 'new Date("2001-02-03T04:05:06.000Z")'
toUpper Added in v0.9.0

String → String
Parameters

str
The string to upper case.
Returns

String The upper case version of `str`.
The upper case version of a string.

See also toLower.

R.toUpper('abc'); //=> 'ABC'
transduce Added in v0.12.0

(c → c) → ((a, b) → a) → a → [b] → a
Parameters

xf
The transducer function. Receives a transformer and returns a transformer.
 fn
The iterator function. Receives two values, the accumulator and the current element from the array. Wrapped as transformer, if necessary, and used to initialize the transducer
 acc
The initial accumulator value.
 list
The list to iterate over.
Returns

* The final, accumulated value.
Initializes a transducer using supplied iterator function. Returns a single item by iterating through the list, successively calling the transformed iterator function and passing it an accumulator value and the current value from the array, and then passing the result to the next call.

The iterator function receives two values: (acc, value). It will be wrapped as a transformer to initialize the transducer. A transformer can be passed directly in place of an iterator function. In both cases, iteration may be stopped early with the R.reduced function.

A transducer is a function that accepts a transformer and returns a transformer and can be composed directly.

A transformer is an an object that provides a 2-arity reducing iterator function, step, 0-arity initial value function, init, and 1-arity result extraction function, result. The step function is used as the iterator function in reduce. The result function is used to convert the final accumulator into the return type and in most cases is R.identity. The init function can be used to provide an initial accumulator, but is ignored by transduce.

The iteration is performed with R.reduce after initializing the transducer.

See also reduce, reduced, into.

var numbers = [1, 2, 3, 4];
var transducer = R.compose(R.map(R.add(1)), R.take(2));
R.transduce(transducer, R.flip(R.append), [], numbers); //=> [2, 3]

var isOdd = (x) => x % 2 === 1;
var firstOddTransducer = R.compose(R.filter(isOdd), R.take(1));
R.transduce(firstOddTransducer, R.flip(R.append), [], R.range(0, 100)); //=> [1]
transpose Added in v0.19.0

[[a]] → [[a]]
Parameters

list
A 2D list
Returns

Array A 2D list
Transposes the rows and columns of a 2D list. When passed a list of n lists of length x, returns a list of x lists of length n.

R.transpose([[1, 'a'], [2, 'b'], [3, 'c']]) //=> [[1, 2, 3], ['a', 'b', 'c']]
R.transpose([[1, 2, 3], ['a', 'b', 'c']]) //=> [[1, 'a'], [2, 'b'], [3, 'c']]

// If some of the rows are shorter than the following rows, their elements are skipped:
R.transpose([[10, 11], [20], [], [30, 31, 32]]) //=> [[10, 20, 30], [11, 31], [32]]
traverse Added in v0.19.0

(Applicative f, Traversable t) => (a → f a) → (a → f b) → t a → f (t b)
Parameters

of
f
traversable
Returns

*
Maps an Applicative-returning function over a Traversable, then uses sequence to transform the resulting Traversable of Applicative into an Applicative of Traversable.

Dispatches to the traverse method of the third argument, if present.

See also sequence.

// Returns `Nothing` if the given divisor is `0`
safeDiv = n => d => d === 0 ? Nothing() : Just(n / d)

R.traverse(Maybe.of, safeDiv(10), [2, 4, 5]); //=> Just([5, 2.5, 2])
R.traverse(Maybe.of, safeDiv(10), [2, 0, 5]); //=> Nothing
trim Added in v0.6.0

String → String
Parameters

str
The string to trim.
Returns

String Trimmed version of `str`.
Removes (strips) whitespace from both ends of the string.

R.trim('   xyz  '); //=> 'xyz'
R.map(R.trim, R.split(',', 'x, y, z')); //=> ['x', 'y', 'z']
tryCatch Added in v0.20.0

(…x → a) → ((e, …x) → a) → (…x → a)
Parameters

tryer
The function that may throw.
 catcher
The function that will be evaluated if tryer throws.
Returns

function A new function that will catch exceptions and send then to the catcher.
tryCatch takes two functions, a tryer and a catcher. The returned function evaluates the tryer; if it does not throw, it simply returns the result. If the tryer does throw, the returned function evaluates the catcher function and returns its result. Note that for effective composition with this function, both the tryer and catcher functions must return the same type of results.

R.tryCatch(R.prop('x'), R.F)({x: true}); //=> true
R.tryCatch(R.prop('x'), R.F)(null);      //=> false
type Added in v0.8.0

(* → {*}) → String
Parameters

val
The value to test
Returns

String
Gives a single-word string description of the (native) type of a value, returning such answers as 'Object', 'Number', 'Array', or 'Null'. Does not attempt to distinguish user Object types any further, reporting them all as 'Object'.

R.type({}); //=> "Object"
R.type(1); //=> "Number"
R.type(false); //=> "Boolean"
R.type('s'); //=> "String"
R.type(null); //=> "Null"
R.type([]); //=> "Array"
R.type(/[A-z]/); //=> "RegExp"
R.type(() => {}); //=> "Function"
R.type(undefined); //=> "Undefined"
unapply Added in v0.8.0

([*…] → a) → (*… → a)
Parameters

fn
Returns

function
Takes a function fn, which takes a single array argument, and returns a function which:

takes any number of positional arguments;
passes these arguments to fn as an array; and
returns the result.
In other words, R.unapply derives a variadic function from a function which takes an array. R.unapply is the inverse of R.apply.

See also apply.

R.unapply(JSON.stringify)(1, 2, 3); //=> '[1,2,3]'
unary Added in v0.2.0

(* → b) → (a → b)
Parameters

fn
The function to wrap.
Returns

function A new function wrapping `fn`. The new function is guaranteed to be of arity 1.
Wraps a function of any arity (including nullary) in a function that accepts exactly 1 parameter. Any extraneous parameters will not be passed to the supplied function.

See also binary, nAry.

var takesTwoArgs = function(a, b) {
  return [a, b];
};
takesTwoArgs.length; //=> 2
takesTwoArgs(1, 2); //=> [1, 2]

var takesOneArg = R.unary(takesTwoArgs);
takesOneArg.length; //=> 1
// Only 1 argument is passed to the wrapped function
takesOneArg(1, 2); //=> [1, undefined]
uncurryN Added in v0.14.0

Number → (a → b) → (a → c)
Parameters

length
The arity for the returned function.
 fn
The function to uncurry.
Returns

function A new function.
Returns a function of arity n from a (manually) curried function.

See also curry.

var addFour = a => b => c => d => a + b + c + d;

var uncurriedAddFour = R.uncurryN(4, addFour);
uncurriedAddFour(1, 2, 3, 4); //=> 10
unfold Added in v0.10.0

(a → [b]) → * → [b]
Parameters

fn
The iterator function. receives one argument, seed, and returns either false to quit iteration or an array of length two to proceed. The element at index 0 of this array will be added to the resulting array, and the element at index 1 will be passed to the next call to fn.
 seed
The seed value.
Returns

Array The final list.
Builds a list from a seed value. Accepts an iterator function, which returns either false to stop iteration or an array of length 2 containing the value to add to the resulting list and the seed to be used in the next call to the iterator function.

The iterator function receives one argument: (seed).

var f = n => n > 50 ? false : [-n, n + 10];
R.unfold(f, 10); //=> [-10, -20, -30, -40, -50]
union Added in v0.1.0

[*] → [*] → [*]
Parameters

as
The first list.
 bs
The second list.
Returns

Array The first and second lists concatenated, with duplicates removed.
Combines two lists into a set (i.e. no duplicates) composed of the elements of each list.

R.union([1, 2, 3], [2, 3, 4]); //=> [1, 2, 3, 4]
unionWith Added in v0.1.0

((a, a) → Boolean) → [*] → [*] → [*]
Parameters

pred
A predicate used to test whether two items are equal.
 list1
The first list.
 list2
The second list.
Returns

Array The first and second lists concatenated, with duplicates removed.
Combines two lists into a set (i.e. no duplicates) composed of the elements of each list. Duplication is determined according to the value returned by applying the supplied predicate to two list elements.

See also union.

var l1 = [{a: 1}, {a: 2}];
var l2 = [{a: 1}, {a: 4}];
R.unionWith(R.eqBy(R.prop('a')), l1, l2); //=> [{a: 1}, {a: 2}, {a: 4}]
uniq Added in v0.1.0

[a] → [a]
Parameters

list
The array to consider.
Returns

Array The list of unique items.
Returns a new list containing only one copy of each element in the original list. R.equals is used to determine equality.

R.uniq([1, 1, 2, 1]); //=> [1, 2]
R.uniq([1, '1']);     //=> [1, '1']
R.uniq([[42], [42]]); //=> [[42]]
uniqBy Added in v0.16.0

(a → b) → [a] → [a]
Parameters

fn
A function used to produce a value to use during comparisons.
 list
The array to consider.
Returns

Array The list of unique items.
Returns a new list containing only one copy of each element in the original list, based upon the value returned by applying the supplied function to each list element. Prefers the first item if the supplied function produces the same value on two items. R.equals is used for comparison.

R.uniqBy(Math.abs, [-1, -5, 2, 10, 1, 2]); //=> [-1, -5, 2, 10]
uniqWith Added in v0.2.0

((a, a) → Boolean) → [a] → [a]
Parameters

pred
A predicate used to test whether two items are equal.
 list
The array to consider.
Returns

Array The list of unique items.
Returns a new list containing only one copy of each element in the original list, based upon the value returned by applying the supplied predicate to two list elements. Prefers the first item if two items compare equal based on the predicate.

var strEq = R.eqBy(String);
R.uniqWith(strEq)([1, '1', 2, 1]); //=> [1, 2]
R.uniqWith(strEq)([{}, {}]);       //=> [{}]
R.uniqWith(strEq)([1, '1', 1]);    //=> [1]
R.uniqWith(strEq)(['1', 1, 1]);    //=> ['1']
unless Added in v0.18.0

(a → Boolean) → (a → a) → a → a
Parameters

pred
A predicate function
 whenFalseFn
A function to invoke when the pred evaluates to a falsy value.
 x
An object to test with the pred function and pass to whenFalseFn if necessary.
Returns

* Either `x` or the result of applying `x` to `whenFalseFn`.
Tests the final argument by passing it to the given predicate function. If the predicate is not satisfied, the function will return the result of calling the whenFalseFn function with the same argument. If the predicate is satisfied, the argument is returned as is.

See also ifElse, when.

let safeInc = R.unless(R.isNil, R.inc);
safeInc(null); //=> null
safeInc(1); //=> 2
unnest Added in v0.3.0

Chain c => c (c a) → c a
Parameters

list
Returns

*
Shorthand for R.chain(R.identity), which removes one level of nesting from any Chain.

See also flatten, chain.

R.unnest([1, [2], [[3]]]); //=> [1, 2, [3]]
R.unnest([[1, 2], [3, 4], [5, 6]]); //=> [1, 2, 3, 4, 5, 6]
until Added in v0.20.0

(a → Boolean) → (a → a) → a → a
Parameters

pred
A predicate function
 fn
The iterator function
 init
Initial value
Returns

* Final value that satisfies predicate
Takes a predicate, a transformation function, and an initial value, and returns a value of the same type as the initial value. It does so by applying the transformation until the predicate is satisfied, at which point it returns the satisfactory value.

R.until(R.gt(R.__, 100), R.multiply(2))(1) // => 128


### update

Number → a → [a] → [a]
Parameters

idx
The index to update.
 x
The value to exist at the given index of the returned array.
 list
The source array-like object to be updated.
Returns

Array A copy of `list` with the value at index `idx` replaced with `x`.
Returns a new copy of the array with the element at the provided index replaced with the given value.

See also adjust.

R.update(1, 11, [0, 1, 2]);     //=> [0, 11, 2]
R.update(1)(11)([0, 1, 2]);     //=> [0, 11, 2]


### useWith 
((x1, x2, …) → z) → [(a → x1), (b → x2), …] → (a → b → … → z)
Parameters

fn
The function to wrap.
 transformers
A list of transformer functions
Returns

function The wrapped function.
Accepts a function fn and a list of transformer functions and returns a new curried function. When the new function is invoked, it calls the function fn with parameters consisting of the result of calling each supplied handler on successive arguments to the new function.

If more arguments are passed to the returned function than transformer functions, those arguments are passed directly to fn as additional parameters. If you expect additional arguments that don't need to be transformed, although you can ignore them, it's best to pass an identity function so that the new function reports the correct arity.

See also converge.

R.useWith(Math.pow, [R.identity, R.identity])(3, 4); //=> 81
R.useWith(Math.pow, [R.identity, R.identity])(3)(4); //=> 81
R.useWith(Math.pow, [R.dec, R.inc])(3, 4); //=> 32
R.useWith(Math.pow, [R.dec, R.inc])(3)(4); //=> 32
values Added in v0.1.0

{k: v} → [v]
Parameters

obj
The object to extract values from
Returns

Array An array of the values of the object's own properties.
Returns a list of all the enumerable own properties of the supplied object. Note that the order of the output array is not guaranteed across different JS platforms.

See also valuesIn, keys.

R.values({a: 1, b: 2, c: 3}); //=> [1, 2, 3]


### valuesIn

{k: v} → [v]
Parameters

obj
The object to extract values from
Returns

Array An array of the values of the object's own and prototype properties.
Returns a list of all the properties, including prototype properties, of the supplied object. Note that the order of the output array is not guaranteed to be consistent across different JS platforms.

See also values, keysIn.

var F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
var f = new F();
R.valuesIn(f); //=> ['X', 'Y']

### view

Lens s a → s → a
Lens s a = Functor f => (a → f a) → s → f s
Parameters

lens
x
Returns

*
Returns a "view" of the given data structure, determined by the given lens. The lens's focus determines which portion of the data structure is visible.

See also prop, lensIndex, lensProp.

var xLens = R.lensProp('x');

R.view(xLens, {x: 1, y: 2});  //=> 1
R.view(xLens, {x: 4, y: 2});  //=> 4

### when
(a → Boolean) → (a → a) → a → a
Parameters

pred
A predicate function
 whenTrueFn
A function to invoke when the condition evaluates to a truthy value.
 x
An object to test with the pred function and pass to whenTrueFn if necessary.
Returns

* Either `x` or the result of applying `x` to `whenTrueFn`.
Tests the final argument by passing it to the given predicate function. If the predicate is satisfied, the function will return the result of calling the whenTrueFn function with the same argument. If the predicate is not satisfied, the argument is returned as is.

See also ifElse, unless.

// truncate :: String -> String
var truncate = R.when(
  R.propSatisfies(R.gt(R.__, 10), 'length'),
  R.pipe(R.take(10), R.append('…'), R.join(''))
);
truncate('12345');         //=> '12345'
truncate('0123456789ABC'); //=> '0123456789…'


### where

```{String: (* → Boolean)} → {String: *} → Boolean```
Parameters

spec
testObj
Returns

Boolean
Takes a spec object and a test object; returns true if the test satisfies the spec. Each of the spec's own properties must be a predicate function. Each predicate is applied to the value of the corresponding property of the test object. where returns true if all the predicates return true, false otherwise.

where is well suited to declaratively expressing constraints for other functions such as filter and find.

See also propSatisfies, whereEq.
```
// pred :: Object -> Boolean
var pred = R.where({
  a: R.equals('foo'),
  b: R.complement(R.equals('bar')),
  x: R.gt(R.__, 10),
  y: R.lt(R.__, 20)
});

pred({a: 'foo', b: 'xxx', x: 11, y: 19}); //=> true
pred({a: 'xxx', b: 'xxx', x: 11, y: 19}); //=> false
pred({a: 'foo', b: 'bar', x: 11, y: 19}); //=> false
pred({a: 'foo', b: 'xxx', x: 10, y: 19}); //=> false
pred({a: 'foo', b: 'xxx', x: 11, y: 20}); //=> false
```

### whereEq

```js
{String: *} → {String: *} → Boolean
```
Parameters

spec
testObj
Returns

Boolean
Takes a spec object and a test object; returns true if the test satisfies the spec, false otherwise. An object satisfies the spec if, for each of the spec's own properties, accessing that property of the object gives the same value (in R.equals terms) as accessing that property of the spec.

whereEq is a specialization of where.

See also propEq, where.

```js
// pred :: Object -> Boolean
var pred = R.whereEq({a: 1, b: 2});

pred({a: 1});              //=> false
pred({a: 1, b: 2});        //=> true
pred({a: 1, b: 2, c: 3});  //=> true
pred({a: 1, b: 1});        //=> false
```
### without

[a] → [a] → [a]
Parameters

list1
The values to be removed from list2.
 list2
The array to remove values from.
Returns

Array The new array without values in `list1`.
Returns a new list without values in the first argument. R.equals is used to determine equality.

Acts as a transducer if a transformer is given in list position.

See also transduce, difference.

R.without([1, 2], [1, 2, 1, 3, 4]); //=> [3, 4]

### xprod

[a] → [b] → [[a,b]]
Parameters

as
The first list.
 bs
The second list.
Returns

Array The list made by combining each possible pair from `as` and `bs` into pairs (`[a, b]`).
Creates a new list out of the two supplied by creating each possible pair from the lists.

R.xprod([1, 2], ['a', 'b']); //=> [[1, 'a'], [1, 'b'], [2, 'a'], [2, 'b']]

### zip
[a] → [b] → [[a,b]]
Parameters

list1
The first array to consider.
 list2
The second array to consider.
Returns

Array The list made by pairing up same-indexed elements of `list1` and `list2`.
Creates a new list out of the two supplied by pairing up equally-positioned items from both lists. The returned list is truncated to the length of the shorter of the two input lists. Note: zip is equivalent to zipWith(function(a, b) { return [a, b] }).

R.zip([1, 2, 3], ['a', 'b', 'c']); //=> [[1, 'a'], [2, 'b'], [3, 'c']]

### zipObj
```js
[String] → [*] → {String: *}
```
Parameters
keys
The array that will be properties on the output object.
 values
The list of values on the output object.
Returns

Object The object made by pairing up same-indexed elements of `keys` and `values`.
Creates a new object out of a list of keys and a list of values. Key/value pairing is truncated to the length of the shorter of the two lists. Note: zipObj is equivalent to pipe(zip, fromPairs).
```js
R.zipObj(['a', 'b', 'c'], [1, 2, 3]); //=> {a: 1, b: 2, c: 3}
```
### zipWith

((a, b) → c) → [a] → [b] → [c]
Parameters
fn - The function used to combine the two elements into one value.
list1 - The first array to consider.
list2 - The second array to consider.

Returns
Array The list made by combining same-indexed elements of `list1` and `list2` using `fn`.
Creates a new list out of the two supplied by applying the function to each equally-positioned pair in the lists. The returned list is truncated to the length of the shorter of the two input lists.
```js
var f = (x, y) => {
  // ...
};
R.zipWith(f, [1, 2, 3], ['a', 'b', 'c']);
//=> [f(1, 'a'), f(2, 'b'), f(3, 'c')]

```
