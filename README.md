# Quick knowledge Ramda

## Ramda
Biblioteca de Javascript funcional.

### __ 
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
```js
var mapIndexed = R.addIndex(R.map);
mapIndexed((val, idx) => idx + '-' + val, ['f', 'o', 'o', 'b', 'a', 'r']);
//=> ['0-f', '1-o', '2-o', '3-b', '4-a', '5-r']
```

### adjust
`(a → a) → Number → [a] → [a]`
```
R.adjust(R.add(10), 1, [1, 2, 3]);     //=> [1, 12, 3]
R.adjust(R.add(10))(1)([1, 2, 3]);     //=> [1, 12, 3]
```

### all
`(a → Boolean) → [a] → Boolean`
```
var equals3 = R.equals(3);
R.all(equals3)([3, 3, 3, 3]); //=> true
R.all(equals3)([3, 3, 1, 3]); //=> false
```

### allPass
`[(*… → Boolean)] → (*… → Boolean)`
```
var isQueen = R.propEq('rank', 'Q');
var isSpade = R.propEq('suit', '♠︎');
var isQueenOfSpades = R.allPass([isQueen, isSpade]);

isQueenOfSpades({rank: 'Q', suit: '♣︎'}); //=> false
isQueenOfSpades({rank: 'Q', suit: '♠︎'}); //=> true
```

### always
`a → (* → a)`
```
var t = R.always('Tee');
t(); //=> 'Tee'
```

### and
`a → b → a | b`
```
R.and(true, true); //=> true
R.and(true, false); //=> false
R.and(false, true); //=> false
R.and(false, false); //=> false
```

### any
`(a → Boolean) → [a] → Boolean`
```
var lessThan0 = R.flip(R.lt)(0);
var lessThan2 = R.flip(R.lt)(2);
R.any(lessThan0)([1, 2]); //=> false
R.any(lessThan2)([1, 2]); //=> true
```

### anyPass
`[(*… → Boolean)] → (*… → Boolean)`
```
var isClub = R.propEq('suit', '♣');
var isSpade = R.propEq('suit', '♠');
var isBlackCard = R.anyPass([isClub, isSpade]);

isBlackCard({rank: '10', suit: '♣'}); //=> true
isBlackCard({rank: 'Q', suit: '♠'}); //=> true
isBlackCard({rank: 'Q', suit: '♦'}); //=> false
```

### ap
`[a → b] → [a] → [b]`
Apply `f => f (a → b) → f a → f b
(a → b → c) → (a → b) → (a → c)`
```
R.ap([R.multiply(2), R.add(3)], [1,2,3]); //=> [2, 4, 6, 4, 5, 6]
R.ap([R.concat('tasty '), R.toUpper], ['pizza', 'salad']); //=> ["tasty pizza", "tasty salad", "PIZZA", "SALAD"]

// R.ap can also be used as S combinator
// when only two functions are passed
R.ap(R.concat, R.toUpper)('Ramda') //=> 'RamdaRAMDA'
```

### aperture
`Number → [a] → [[a]]`
```
R.aperture(2, [1, 2, 3, 4, 5]); //=> [[1, 2], [2, 3], [3, 4], [4, 5]]
R.aperture(3, [1, 2, 3, 4, 5]); //=> [[1, 2, 3], [2, 3, 4], [3, 4, 5]]
R.aperture(7, [1, 2, 3, 4, 5]); //=> []
```

### append
`a → [a] → [a]`
```
R.append('tests', ['write', 'more']); //=> ['write', 'more', 'tests']
R.append('tests', []); //=> ['tests']
R.append(['tests'], ['write', 'more']); //=> ['write', 'more', ['tests']]
```

### apply 
`(*… → a) → [*] → a`
```
var nums = [1, 2, 3, -99, 42, 6, 7];
R.apply(Math.max, nums); //=> 42
```

### applySpec
`{k: ((a, b, …, m) → v)} → ((a, b, …, m) → {k: v})`
```
var getMetrics = R.applySpec({
  sum: R.add,
  nested: { mul: R.multiply }
});
getMetrics(2, 4); // => { sum: 6, nested: { mul: 8 } }
```

### applyTo
`a → (a → b) → b`
```
var t42 = R.applyTo(42);
t42(R.identity); 
//=> 42
t42(R.add(1)); 
//=> 43
```

### ascend 
`Ord b => (a → b) → a → a → Number`
```
var byAge = R.ascend(R.prop('age'));
var people = [
  // ...
];
var peopleByYoungestFirst = R.sort(byAge, people);
```

### assoc
`String → a → {k: v} → {k: v}`

```
R.assoc('c', 3, {a: 1, b: 2}); 
//=> {a: 1, b: 2, c: 3}
```
### assocPath
`[Idx] → a → {a} → {a}`
`Idx = String | Int`

```
R.assocPath(['a', 'b', 'c'], 42, {a: {b: {c: 0}}}); //=> {a: {b: {c: 42}}}

// Any missing or non-object keys in path will be overridden
R.assocPath(['a', 'b', 'c'], 42, {a: 5}); //=> {a: {b: {c: 42}}}
```

### binary
`(* → c) → (a, b → c)`
```
var takesThreeArgs = function(a, b, c) {
  return [a, b, c];
};
takesThreeArgs.length; //=> 3
takesThreeArgs(1, 2, 3); //=> [1, 2, 3]

var takesTwoArgs = R.binary(takesThreeArgs);
takesTwoArgs.length; //=> 2
// Only 2 arguments are passed to the wrapped function
takesTwoArgs(1, 2, 3); //=> [1, 2, undefined]
```

### bind
`(* → *) → {*} → (* → *)`
```
var log = R.bind(console.log, console);
R.pipe(R.assoc('a', 2), R.tap(log), R.assoc('a', 3))({a: 1}); //=> {a: 3}
// logs {a: 2}
```

### both
`(*… → Boolean) → (*… → Boolean) → (*… → Boolean)`
```
var gt10 = R.gt(R.__, 10)
var lt20 = R.lt(R.__, 20)
var f = R.both(gt10, lt20);
f(15); //=> true
f(30); //=> false
```

### call
`(*… → a),*… → a`
```
R.call(R.add, 1, 2); //=> 3

var indentN = R.pipe(R.repeat(' '),
                     R.join(''),
                     R.replace(/^(?!$)/gm));

var format = R.converge(R.call, [
                            R.pipe(R.prop('indent'), indentN),
                            R.prop('value')
                        ]);

format({indent: 2, value: 'foo\nbar\nbaz\n'}); //=> '  foo\n  bar\n  baz\n'
```

### chain
`Chain m => (a → m b) → m a → m b`
```
var duplicate = n => [n, n];
R.chain(duplicate, [1, 2, 3]); //=> [1, 1, 2, 2, 3, 3]

R.chain(R.append, R.head)([1, 2, 3]); //=> [1, 2, 3, 1]
```

### clamp
`Ord a => a → a → a → a`
```
R.clamp(1, 10, -5) // => 1
R.clamp(1, 10, 15) // => 10
R.clamp(1, 10, 4)  // => 4
```

### clone
`{*} → {*}`
```
var objects = [{}, {}, {}];
var objectsClone = R.clone(objects);
objects === objectsClone; //=> false
objects[0] === objectsClone[0]; //=> false
```

### comparator
`((a, b) → Boolean) → ((a, b) → Number)`
```
var byAge = R.comparator((a, b) => a.age < b.age);
var people = [
  // ...
];
var peopleByIncreasingAge = R.sort(byAge, people);
```

### complement
`(*… → *) → (*… → Boolean)`
```
var isNotNil = R.complement(R.isNil);
isNil(null); //=> true
isNotNil(null); //=> false
isNil(7); //=> false
isNotNil(7); //=> true
```

### compose
`((y → z), (x → y), …, (o → p), ((a, b, …, n) → o)) → ((a, b, …, n) → z)`
```
var classyGreeting = (firstName, lastName) => "The name's " + lastName + ", " + firstName + " " + lastName
var yellGreeting = R.compose(R.toUpper, classyGreeting);
yellGreeting('James', 'Bond'); //=> "THE NAME'S BOND, JAMES BOND"

R.compose(Math.abs, R.add(1), R.multiply(2))(-4) //=> 7
```

### composeK
`Chain m => ((y → m z), (x → m y), …, (a → m b)) → (a → m z)`
```
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
```

### composeP
`((y → Promise z), (x → Promise y), …, (a → Promise b)) → (a → Promise z)`
```
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
```

### concat 
`[a] → [a] → [a]`
`String → String → String`
```
R.concat('ABC', 'DEF'); // 'ABCDEF'
R.concat([4, 5, 6], [1, 2, 3]); //=> [4, 5, 6, 1, 2, 3]
R.concat([], []); //=> []
```

### cond
`[[(*… → Boolean),(*… → *)]] → (*… → *)`
```
var fn = R.cond([
  [R.equals(0),   R.always('water freezes at 0°C')],
  [R.equals(100), R.always('water boils at 100°C')],
  [R.T,           temp => 'nothing special happens at ' + temp + '°C']
]);
fn(0); //=> 'water freezes at 0°C'
fn(50); //=> 'nothing special happens at 50°C'
fn(100); //=> 'water boils at 100°C'
```

### construct
`(* → {*}) → (* → {*})`
```
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
```

### constructN
`Number → (* → {*}) → (* → {*})`
```
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
```

### contains
`a → [a] → Boolean`
```
R.contains(3, [1, 2, 3]); //=> true
R.contains(4, [1, 2, 3]); //=> false
R.contains({ name: 'Fred' }, [{ name: 'Fred' }]); //=> true
R.contains([42], [[42]]); //=> true
```

### converge
`((x1, x2, …) → z) → [((a, b, …) → x1), ((a, b, …) → x2), …] → (a → b → … → z)`
```
var average = R.converge(R.divide, [R.sum, R.length])
average([1, 2, 3, 4, 5, 6, 7]) //=> 4

var strangeConcat = R.converge(R.concat, [R.toUpper, R.toLower])
strangeConcat("Yodel") //=> "YODELyodel"
```

### countBy
`(a → String) → [a] → {*}`
```
var numbers = [1.0, 1.1, 1.2, 2.0, 3.0, 2.2];
R.countBy(Math.floor)(numbers);    //=> {'1': 3, '2': 2, '3': 1}

var letters = ['a', 'b', 'A', 'a', 'B', 'c'];
R.countBy(R.toLower)(letters);   //=> {'a': 3, 'b': 2, 'c': 1}
```

### curry
`(* → a) → (* → a)`
```
var addFourNumbers = (a, b, c, d) => a + b + c + d;

var curriedAddFourNumbers = R.curry(addFourNumbers);
var f = curriedAddFourNumbers(1, 2);
var g = f(3);
g(4); //=> 10
```

### curryN
`Number → (* → a) → (* → a)`
```
var sumArgs = (...args) => R.sum(args);

var curriedAddFourNumbers = R.curryN(4, sumArgs);
var f = curriedAddFourNumbers(1, 2);
var g = f(3);
g(4); //=> 10
```

### dec
`Number → Number`
```
R.dec(42); 
//=> 41
```

### defaultTo
`a → b → a | b`
```
var defaultTo42 = R.defaultTo(42);

defaultTo42(null);  //=> 42
defaultTo42(undefined);  //=> 42
defaultTo42('Ramda');  //=> 'Ramda'
// parseInt('string') results in NaN
defaultTo42(parseInt('string')); //=> 42
```

### descend
`Ord b => (a → b) → a → a → Number`
```
var byAge = R.descend(R.prop('age'));
var people = [
  // ...
];
var peopleByOldestFirst = R.sort(byAge, people);
```

### difference
`[*] → [*] → [*]`
```
R.difference([1,2,3,4], [7,6,5,4,3]); //=> [1,2]
R.difference([7,6,5,4,3], [1,2,3,4]); //=> [7,6,5]
R.difference([{a: 1}, {b: 2}], [{a: 1}, {c: 3}]) //=> [{b: 2}]
```

### differenceWith
`((a, a) → Boolean) → [a] → [a] → [a]`
```
var cmp = (x, y) => x.a === y.a;
var l1 = [{a: 1}, {a: 2}, {a: 3}];
var l2 = [{a: 3}, {a: 4}];
R.differenceWith(cmp, l1, l2); //=> [{a: 1}, {a: 2}]
```

### dissoc
`String → {k: v} → {k: v}`
```
R.dissoc('b', {a: 1, b: 2, c: 3}); //=> {a: 1, c: 3}
```

### dissocPath
`[Idx] → {k: v} → {k: v}`
`Idx = String | Int`
```
R.dissocPath(['a', 'b', 'c'], {a: {b: {c: 42}}}); //=> {a: {b: {}}}
```

### divide
`Number → Number → Number`
```
R.divide(71, 100); //=> 0.71

var half = R.divide(R.__, 2);
half(42); //=> 21

var reciprocal = R.divide(1);
reciprocal(4);   //=> 0.25
```

### drop
`Number → [a] → [a]`
`Number → String → String`
```
R.drop(1, ['foo', 'bar', 'baz']); //=> ['bar', 'baz']
R.drop(2, ['foo', 'bar', 'baz']); //=> ['baz']
R.drop(3, ['foo', 'bar', 'baz']); //=> []
R.drop(4, ['foo', 'bar', 'baz']); //=> []
R.drop(3, 'ramda');               //=> 'da'
```

### dropLast
`Number → [a] → [a]`
`Number → String → String`
```
R.dropLast(1, ['foo', 'bar', 'baz']); //=> ['foo', 'bar']
R.dropLast(2, ['foo', 'bar', 'baz']); //=> ['foo']
R.dropLast(3, ['foo', 'bar', 'baz']); //=> []
R.dropLast(4, ['foo', 'bar', 'baz']); //=> []
R.dropLast(3, 'ramda');   
//=> 'ra'
```

### dropLastWhile
`(a → Boolean) → [a] → [a]`
`(a → Boolean) → String → String`
```
var lteThree = x => x <= 3;

R.dropLastWhile(lteThree, [1, 2, 3, 4, 3, 2, 1]); //=> [1, 2, 3, 4]

R.dropLastWhile(x => x !== 'd' , 'Ramda'); //=> 'Ramd'
```

### dropRepeats
`[a] → [a]`
```
R.dropRepeats([1, 1, 1, 2, 3, 4, 4, 2, 2]); //=> [1, 2, 3, 4, 2]
```

### dropRepeatsWith
`((a, a) → Boolean) → [a] → [a]`
```
var l = [1, -1, 1, 3, 4, -4, -4, -5, 5, 3, 3];
R.dropRepeatsWith(R.eqBy(Math.abs), l); //=> [1, 3, 4, -5, 3]
```

### dropWhile
`(a → Boolean) → [a] → [a]`
`(a → Boolean) → String → String`
```
var lteTwo = x => x <= 2;

R.dropWhile(lteTwo, [1, 2, 3, 4, 3, 2, 1]); //=> [3, 4, 3, 2, 1]

R.dropWhile(x => x !== 'd' , 'Ramda'); //=> 'da'
```

### either
`(*… → Boolean) → (*… → Boolean) → (*… → Boolean)`
```
var gt10 = x => x > 10;
var even = x => x % 2 === 0;
var f = R.either(gt10, even);
f(101); //=> true
f(8); //=> true
```

### empty
`a → a`
```
R.empty(Just(42));      //=> Nothing()
R.empty([1, 2, 3]);     //=> []
R.empty('unicorns');    //=> ''
R.empty({x: 1, y: 2});  //=> {}
```

### endsWith
`[a] → Boolean`
`String → Boolean`
```
R.endsWith('c', 'abc')                //=> true
R.endsWith('b', 'abc')                //=> false
R.endsWith(['c'], ['a', 'b', 'c'])    //=> true
R.endsWith(['b'], ['a', 'b', 'c'])    //=> false
```

### eqBy
`(a → b) → a → a → Boolean`
```
R.eqBy(Math.abs, 5, -5); //=> true
```

### eqProps
`k → {k: v} → {k: v} → Boolean`
```
var o1 = { a: 1, b: 2, c: 3, d: 4 };
var o2 = { a: 10, b: 20, c: 3, d: 40 };
R.eqProps('a', o1, o2); //=> false
R.eqProps('c', o1, o2); //=> true
```

### equals
`a → b → Boolean`
```
R.equals(1, 1); //=> true
R.equals(1, '1'); //=> false
R.equals([1, 2, 3], [1, 2, 3]); //=> true

var a = {}; a.v = a;
var b = {}; b.v = b;
R.equals(a, b); //=> true
```

### evolve
`{k: (v → v)} → {k: v} → {k: v}`
```
var tomato  = {firstName: '  Tomato ', data: {elapsed: 100, remaining: 1400}, id:123};
var transformations = {
  firstName: R.trim,
  lastName: R.trim, // Will not get invoked.
  data: {elapsed: R.add(1), remaining: R.add(-1)}
};
R.evolve(transformations, tomato); //=> {firstName: 'Tomato', data: {elapsed: 101, remaining: 1399}, id:123}
```

### F
`* → Boolean`
```
R.F(); //=> false
```

### filter
`f => (a → Boolean) → f a → f a`
```
var isEven = n => n % 2 === 0;

R.filter(isEven, [1, 2, 3, 4]); //=> [2, 4]

R.filter(isEven, {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, d: 4}
```

### find
`(a → Boolean) → [a] → a | undefined`
```
var xs = [{a: 1}, {a: 2}, {a: 3}];
R.find(R.propEq('a', 2))(xs); //=> {a: 2}
R.find(R.propEq('a', 4))(xs); //=> undefined
```

### findIndex
`(a → Boolean) → [a] → Number`
```
var xs = [{a: 1}, {a: 2}, {a: 3}];
R.findIndex(R.propEq('a', 2))(xs); //=> 1
R.findIndex(R.propEq('a', 4))(xs); //=> -1
```

### findLast
`(a → Boolean) → [a] → a | undefined`
```
var xs = [{a: 1, b: 0}, {a:1, b: 1}];
R.findLast(R.propEq('a', 1))(xs); //=> {a: 1, b: 1}
R.findLast(R.propEq('a', 4))(xs); //=> undefined
```

### findLastIndex
`(a → Boolean) → [a] → Number`
```
var xs = [{a: 1, b: 0}, {a:1, b: 1}];
R.findLastIndex(R.propEq('a', 1))(xs); //=> 1
R.findLastIndex(R.propEq('a', 4))(xs); //=> -1
```

### flatten
`[a] → [b]`
```
R.flatten([1, 2, [3, 4], 5, [6, [7, 8, [9, [10, 11], 12]]]]);
//=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
```

### flip
`((a, b, c, …) → z) → (b → a → c → … → z)`
```
var mergeThree = (a, b, c) => [].concat(a, b, c);

mergeThree(1, 2, 3); //=> [1, 2, 3]

R.flip(mergeThree)(1, 2, 3); //=> [2, 1, 3]
```

### forEach
`(a → *) → [a] → [a]`
```
var printXPlusFive = x => console.log(x + 5);
R.forEach(printXPlusFive, [1, 2, 3]); //=> [1, 2, 3]
// logs 6
// logs 7
// logs 8
```

### forEachObjIndexed
`((a, String, StrMap a) → Any) → StrMap a → StrMap a`
```
var printKeyConcatValue = (value, key) => console.log(key + ':' + value);
R.forEachObjIndexed(printKeyConcatValue, {x: 1, y: 2}); //=> {x: 1, y: 2}
// logs x:1
// logs y:2
```

### fromPairs
`[[k,v]] → {k: v}`
```
R.fromPairs([['a', 1], ['b', 2], ['c', 3]]); //=> {a: 1, b: 2, c: 3}
```

### groupBy
`(a → String) → [a] → {String: [a]}`
```
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
```

### groupWith
`((a, a) → Boolean) → [a] → [[a]]`
```
R.groupWith(R.equals, [0, 1, 1, 2, 3, 5, 8, 13, 21])
//=> [[0], [1, 1], [2], [3], [5], [8], [13], [21]]

R.groupWith((a, b) => a + 1 === b, [0, 1, 1, 2, 3, 5, 8, 13, 21])
//=> [[0, 1], [1, 2, 3], [5], [8], [13], [21]]

R.groupWith((a, b) => a % 2 === b % 2, [0, 1, 1, 2, 3, 5, 8, 13, 21])
//=> [[0], [1, 1], [2], [3, 5], [8], [13, 21]]

R.groupWith(R.eqBy(isVowel), 'aestiou')
//=> ['ae', 'st', 'iou']
```

### gt
`Ord a => a → a → Boolean`
```
R.gt(2, 1); //=> true
R.gt(2, 2); //=> false
R.gt(2, 3); //=> false
R.gt('a', 'z'); //=> false
R.gt('z', 'a'); //=> true
```

### gte
`Ord a => a → a → Boolean`
```
R.gte(2, 1); //=> true
R.gte(2, 2); //=> true
R.gte(2, 3); //=> false
R.gte('a', 'z'); //=> false
R.gte('z', 'a'); //=> true
```

### has 
`s → {s: x} → Boolean`
```
var hasName = R.has('name');
hasName({name: 'alice'});   //=> true
hasName({name: 'bob'});     //=> true
hasName({});                //=> false

var point = {x: 0, y: 0};
var pointHas = R.has(R.__, point);
pointHas('x');  //=> true
pointHas('y');  //=> true
pointHas('z');  //=> false
```

### hasIn
`s → {s: x} → Boolean`
```
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
```

### head
`[a] → a | Undefined`
`String → String`
```
R.head(['fi', 'fo', 'fum']); //=> 'fi'
R.head([]); //=> undefined

R.head('abc'); //=> 'a'
R.head(''); //=> ''
```

### identical
`a → a → Boolean`
```
var o = {};
R.identical(o, o); //=> true
R.identical(1, 1); //=> true
R.identical(1, '1'); //=> false
R.identical([], []); //=> false
R.identical(0, -0); //=> false
R.identical(NaN, NaN); //=> true
```

### identity
`a → a`
```
R.identity(1); //=> 1

var obj = {};
R.identity(obj) === obj; //=> true
```

### ifElse
`(*… → Boolean) → (*… → *) → (*… → *) → (*… → *)`
```
var incCount = R.ifElse(
  R.has('count'),
  R.over(R.lensProp('count'), R.inc),
  R.assoc('count', 1)
);
incCount({});           //=> { count: 1 }
incCount({ count: 1 }); //=> { count: 2 }
```

### inc
`Number → Number`
```
R.inc(42); //=> 43
```

### indexBy
`(a → String) → [{k: v}] → {k: {k: v}}`
```
var list = [{id: 'xyz', title: 'A'}, {id: 'abc', title: 'B'}];
R.indexBy(R.prop('id'), list);
//=> {abc: {id: 'abc', title: 'B'}, xyz: {id: 'xyz', title: 'A'}}
```

### indexOf
`a → [a] → Number` 
```
R.indexOf(3, [1,2,3,4]); //=> 2
R.indexOf(10, [1,2,3,4]); //=> -1
```

### init
`[a] → [a]`
`String → String`
```
R.init([1, 2, 3]);  //=> [1, 2]
R.init([1, 2]);     //=> [1]
R.init([1]);        //=> []
R.init([]);         //=> []

R.init('abc');  //=> 'ab'
R.init('ab');   //=> 'a'
R.init('a');    //=> ''
R.init('');     //=> ''
```

### innerJoin
`((a, b) → Boolean) → [a] → [b] → [a]`
```
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
```

### insert
`Number → a → [a] → [a]`
```
R.insert(2, 'x', [1,2,3,4]); //=> [1,2,'x',3,4]
```

### insertAll
`Number → [a] → [a] → [a]`
```
R.insertAll(2, ['x','y','z'], [1,2,3,4]); //=> [1,2,'x','y','z',3,4]
```

### intersection
`[*] → [*] → [*]`
```
R.intersection([1,2,3,4], [7,6,5,4,3]); //=> [4, 3]
```

### intersperse
`a → [a] → [a]`
```
R.intersperse('n', ['ba', 'a', 'a']); //=> ['ba', 'n', 'a', 'n', 'a']
``` 

### into
`a → (b → b) → [c] → a`
```
var numbers = [1, 2, 3, 4];
var transducer = R.compose(R.map(R.add(1)), R.take(2));

R.into([], transducer, numbers); //=> [2, 3]

var intoArray = R.into([]);
intoArray(transducer, numbers); //=> [2, 3]
```

### invert
`{s: x} → {x: [ s, … ]}`
```
var raceResultsByFirstName = {
  first: 'alice',
  second: 'jake',
  third: 'alice',
};
R.invert(raceResultsByFirstName);
//=> { 'alice': ['first', 'third'], 'jake':['second'] }
```

### invertObj
`{s: x} → {x: s}`
```
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
```

### invoker
`Number → String → (a → b → … → n → Object → *)`
```
var sliceFrom = R.invoker(1, 'slice');
sliceFrom(6, 'abcdefghijklm'); //=> 'ghijklm'
var sliceFrom6 = R.invoker(2, 'slice')(6);
sliceFrom6(8, 'abcdefghijklm'); //=> 'gh'
```

### is
`(* → {*}) → a → Boolean`
```
R.is(Object, {}); //=> true
R.is(Number, 1); //=> true
R.is(Object, 1); //=> false
R.is(String, 's'); //=> true
R.is(String, new String('')); //=> true
R.is(Object, new String('')); //=> true
R.is(Object, 's'); //=> false
R.is(Number, {}); //=> false
```

### isEmpty
`a → Boolean`
```
R.isEmpty([1, 2, 3]);   //=> false
R.isEmpty([]);          //=> true
R.isEmpty('');          //=> true
R.isEmpty(null);        //=> false
R.isEmpty({});          //=> true
R.isEmpty({length: 0}); //=> false
```

### isNil
`* → Boolean` 
```
R.isNil(null); //=> true
R.isNil(undefined); //=> true
R.isNil(0); //=> false
R.isNil([]); //=> false
```

### join
`String → [a] → String`
```
var spacer = R.join(' ');
spacer(['a', 2, 3.4]);   //=> 'a 2 3.4'
R.join('|', [1, 2, 3]);    //=> '1|2|3'
```

### juxt
`[(a, b, …, m) → n] → ((a, b, …, m) → [n])`
```
var getRange = R.juxt([Math.min, Math.max]);
getRange(3, 4, 9, -3); //=> [-3, 9]
```

### keys
`{k: v} → [k]`
```
R.keys({a: 1, b: 2, c: 3}); //=> ['a', 'b', 'c']
```

### keysIn
`{k: v} → [k]`
```
var F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
var f = new F();
R.keysIn(f); //=> ['x', 'y']
```

### last
`[a] → a | Undefined`
`String → String`
```
R.last(['fi', 'fo', 'fum']); //=> 'fum'
R.last([]); //=> undefined

R.last('abc'); //=> 'c'
R.last(''); //=> ''
```

### lastIndexOf
`a → [a] → Number`
```
R.lastIndexOf(3, [-1,3,3,0,1,2,3,4]); //=> 6
R.lastIndexOf(10, [1,2,3,4]); //=> -1
```

### length
`[a] → Number`
```
R.length([]); //=> 0
R.length([1, 2, 3]); //=> 3
```

### lens
`(s → a) → ((a, s) → s) → Lens s a` 
`Lens s a = Functor f => (a → f a) → s → f s`
```
var xLens = R.lens(R.prop('x'), R.assoc('x'));

R.view(xLens, {x: 1, y: 2});            //=> 1
R.set(xLens, 4, {x: 1, y: 2});          //=> {x: 4, y: 2}
R.over(xLens, R.negate, {x: 1, y: 2});  //=> {x: -1, y: 2}
```

### lensIndex
`Number → Lens s a`
`Lens s a = Functor f => (a → f a) → s → f s`
```
var headLens = R.lensIndex(0);

R.view(headLens, ['a', 'b', 'c']);            //=> 'a'
R.set(headLens, 'x', ['a', 'b', 'c']);        //=> ['x', 'b', 'c']
R.over(headLens, R.toUpper, ['a', 'b', 'c']); //=> ['A', 'b', 'c']
```

### lensPath
`[Idx] → Lens s a`
`Idx = String | Int`
`Lens s a = Functor f => (a → f a) → s → f s`
```
var xHeadYLens = R.lensPath(['x', 0, 'y']);

R.view(xHeadYLens, {x: [{y: 2, z: 3}, {y: 4, z: 5}]});
//=> 2
R.set(xHeadYLens, 1, {x: [{y: 2, z: 3}, {y: 4, z: 5}]});
//=> {x: [{y: 1, z: 3}, {y: 4, z: 5}]}
R.over(xHeadYLens, R.negate, {x: [{y: 2, z: 3}, {y: 4, z: 5}]});
//=> {x: [{y: -2, z: 3}, {y: 4, z: 5}]}
```

### lensProp
`String → Lens s a`
`Lens s a = Functor f => (a → f a) → s → f s`
```
var xLens = R.lensProp('x');

R.view(xLens, {x: 1, y: 2});            //=> 1
R.set(xLens, 4, {x: 1, y: 2});          //=> {x: 4, y: 2}
R.over(xLens, R.negate, {x: 1, y: 2});  //=> {x: -1, y: 2}
```

### lift
`(*… → *) → ([*]… → [*])`
```
var madd3 = R.lift((a, b, c) => a + b + c);

madd3([1,2,3], [1,2,3], [1]); //=> [3, 4, 5, 4, 5, 6, 5, 6, 7]

var madd5 = R.lift((a, b, c, d, e) => a + b + c + d + e);

madd5([1,2], [3], [4, 5], [6], [7, 8]); //=> [21, 22, 22, 23, 22, 23, 23, 24]
```

### liftN
`Number → (*… → *) → ([*]… → [*])`
```
var madd3 = R.liftN(3, (...args) => R.sum(args));
madd3([1,2,3], [1,2,3], [1]); //=> [3, 4, 5, 4, 5, 6, 5, 6, 7]
```

### lt
`Ord a => a → a → Boolean`
```
R.lt(2, 1); //=> false
R.lt(2, 2); //=> false
R.lt(2, 3); //=> true
R.lt('a', 'z'); //=> true
R.lt('z', 'a'); //=> false
```

### lte
`Ord a => a → a → Boolean`
```
R.lte(2, 1); //=> false
R.lte(2, 2); //=> true
R.lte(2, 3); //=> true
R.lte('a', 'z'); //=> true
R.lte('z', 'a'); //=> false
```

### map
`Functor f => (a → b) → f a → f b`
```
var double = x => x * 2;

R.map(double, [1, 2, 3]); //=> [2, 4, 6]

R.map(double, {x: 1, y: 2, z: 3}); //=> {x: 2, y: 4, z: 6}
```

### mapAccum 
`((acc, x) → (acc, y)) → acc → [x] → (acc, [y])`
```
var digits = ['1', '2', '3', '4'];
var appender = (a, b) => [a + b, a + b];

R.mapAccum(appender, 0, digits); //=> ['01234', ['01', '012', '0123', '01234']]
```

### mapAccumRight
`((x, acc) → (y, acc)) → acc → [x] → ([y], acc)`
```
var digits = ['1', '2', '3', '4'];
var append = (a, b) => [a + b, a + b];

R.mapAccumRight(append, 5, digits); //=> [['12345', '2345', '345', '45'], '12345']
```

### mapObjIndexed
`((*, String, Object) → *) → Object → Object`
```
var values = { x: 1, y: 2, z: 3 };
var prependKeyAndDouble = (num, key, obj) => key + (num * 2);

R.mapObjIndexed(prependKeyAndDouble, values); //=> { x: 'x2', y: 'y4', z: 'z6' }
```

### match
`RegExp → String → [String | Undefined]`
```
R.match(/([a-z]a)/g, 'bananas'); //=> ['ba', 'na', 'na']
R.match(/a/, 'b'); //=> []
R.match(/a/, null); //=> TypeError: null does not have a method named "match"
```

### mathMod
`Number → Number → Number`
```
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
```

### max
`Ord a => a → a → a`
```
R.max(789, 123); //=> 789
R.max('a', 'b'); //=> 'b'
```

### maxBy
`Ord b => (a → b) → a → a → a`
```
//  square :: Number -> Number
var square = n => n * n;

R.maxBy(square, -3, 2); //=> -3

R.reduce(R.maxBy(square), 0, [3, -5, 4, 1, -2]); //=> -5
R.reduce(R.maxBy(square), 0, []); //=> 0
```

### mean
`[Number] → Number`
```
R.mean([2, 7, 9]); //=> 6
R.mean([]); //=> NaN
```

### median
`[Number] → Number`
```
R.median([2, 9, 7]); //=> 7
R.median([7, 2, 10, 9]); //=> 8
R.median([]); //=> NaN
```

### memoizeWith
`(*… → String) → (*… → a) → (*… → a)`
```
let count = 0;
const factorial = R.memoizeWith(R.identity, n => {
  count += 1;
  return R.product(R.range(1, n + 1));
});
factorial(5); //=> 120
factorial(5); //=> 120
factorial(5); //=> 120
count; //=> 1
```

### merge
`{k: v} → {k: v} → {k: v}`
```
R.merge({ 'name': 'fred', 'age': 10 }, { 'age': 40 });
//=> { 'name': 'fred', 'age': 40 }

var resetToDefault = R.merge(R.__, {x: 0});
resetToDefault({x: 5, y: 2}); //=> {x: 0, y: 2}
```

### mergeAll
`[{k: v}] → {k: v}`
```
R.mergeAll([{foo:1},{bar:2},{baz:3}]); //=> {foo:1,bar:2,baz:3}
R.mergeAll([{foo:1},{foo:2},{bar:2}]); //=> {foo:2,bar:2}
```

### mergeDeepLeft
`{a} → {a} → {a}`
```
R.mergeDeepLeft({ name: 'fred', age: 10, contact: { email: 'moo@example.com' }},
                { age: 40, contact: { email: 'baa@example.com' }});
//=> { name: 'fred', age: 10, contact: { email: 'moo@example.com' }}
```

### mergeDeepRight
`{a} → {a} → {a}`
```
R.mergeDeepRight({ name: 'fred', age: 10, contact: { email: 'moo@example.com' }},
                 { age: 40, contact: { email: 'baa@example.com' }});
//=> { name: 'fred', age: 40, contact: { email: 'baa@example.com' }}
```

### mergeDeepWith Added in v0.24.0

`((a, a) → a) → {a} → {a} → {a}`
```js
R.mergeDeepWith(R.concat,
                { a: true, c: { values: [10, 20] }},
                { b: true, c: { values: [15, 35] }});
//=> { a: true, b: true, c: { values: [10, 20, 15, 35] }}
```

### mergeDeepWithKey
`((String, a, a) → a) → {a} → {a} → {a}`
```js
let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r
R.mergeDeepWithKey(concatValues,
                   { a: true, c: { thing: 'foo', values: [10, 20] }},
                   { b: true, c: { thing: 'bar', values: [15, 35] }});
//=> { a: true, b: true, c: { thing: 'bar', values: [10, 20, 15, 35] }}
```

### mergeWith
`((a, a) → a) → {a} → {a} → {a}`
```js
R.mergeWith(R.concat,
            { a: true, values: [10, 20] },
            { b: true, values: [15, 35] });
//=> { a: true, b: true, values: [10, 20, 15, 35] }
```

### mergeWithKey
`((String, a, a) → a) → {a} → {a} → {a}`
```js
let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r
R.mergeWithKey(concatValues,
               { a: true, thing: 'foo', values: [10, 20] },
               { b: true, thing: 'bar', values: [15, 35] });
//=> { a: true, b: true, thing: 'bar', values: [10, 20, 15, 35] }
```

### min
`Ord a => a → a → a`
```js
R.min(789, 123); //=> 123
R.min('a', 'b'); //=> 'a'
```

### minBy
`Ord b => (a → b) → a → a → a`
```js
//  square :: Number -> Number
var square = n => n * n;

R.minBy(square, -3, 2); //=> 2

R.reduce(R.minBy(square), Infinity, [3, -5, 4, 1, -2]); //=> 1
R.reduce(R.minBy(square), Infinity, []); //=> Infinity
```

### modulo
`Number → Number → Number`
```js
R.modulo(17, 3); //=> 2
// JS behavior:
R.modulo(-17, 3); //=> -2
R.modulo(17, -3); //=> 2

var isOdd = R.modulo(R.__, 2);
isOdd(42); //=> 0
isOdd(21); //=> 1
```

### multiply
`Number → Number → Number`
```js
var double = R.multiply(2);
var triple = R.multiply(3);
double(3);       //=>  6
triple(4);       //=> 12
R.multiply(2, 5);  //=> 10
```

### nAry
`Number → (* → a) → (* → a)`
```js
var takesTwoArgs = (a, b) => [a, b];

takesTwoArgs.length; //=> 2
takesTwoArgs(1, 2); //=> [1, 2]

var takesOneArg = R.nAry(1, takesTwoArgs);
takesOneArg.length; //=> 1
// Only `n` arguments are passed to the wrapped function
takesOneArg(1, 2); //=> [1, undefined]
```

### negate
`Number → Number`
```js
R.negate(42); //=> -42
```

### none
`(a → Boolean) → [a] → Boolean`
```js
var isEven = n => n % 2 === 0;
var isOdd = n => n % 2 === 1;

R.none(isEven, [1, 3, 5, 7, 9, 11]); //=> true
R.none(isOdd, [1, 3, 5, 7, 8, 11]); //=> false
```

### not
`* → Boolean`
```js
R.not(true); //=> false
R.not(false); //=> true
R.not(0); //=> true
R.not(1); //=> false
```

### nth
`Number → [a] → a | Undefined`
`Number → String → String`
```js
var list = ['foo', 'bar', 'baz', 'quux'];
R.nth(1, list); //=> 'bar'
R.nth(-1, list); //=> 'quux'
R.nth(-99, list); //=> undefined

R.nth(2, 'abc'); //=> 'c'
R.nth(3, 'abc'); //=> ''
```

### nthArg
`Number → *… → *`
```js
R.nthArg(1)('a', 'b', 'c'); //=> 'b'
R.nthArg(-1)('a', 'b', 'c'); //=> 'c'
```

### o
`(b → c) → (a → b) → a → c`
```js
var classyGreeting = name => "The name's " + name.last + ", " + name.first + " " + name.last
var yellGreeting = R.o(R.toUpper, classyGreeting);
yellGreeting({first: 'James', last: 'Bond'}); //=> "THE NAME'S BOND, JAMES BOND"

R.o(R.multiply(10), R.add(10))(-4) //=> 60
```

### objOf
`String → a → {String:a}`
```js
var matchPhrases = R.compose(
  R.objOf('must'),
  R.map(R.objOf('match_phrase'))
);
matchPhrases(['foo', 'bar', 'baz']); //=> {must: [{match_phrase: 'foo'}, {match_phrase: 'bar'}, {match_phrase: 'baz'}]}
```

### of
`a → [a]`
```js
R.of(null); //=> [null]
R.of([42]); //=> [[42]]
```

### omit 
`[String] → {String: *} → {String: *}`
```js
R.omit(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, c: 3}
```

### once
`(a… → b) → (a… → b)`
```js
var addOneOnce = R.once(x => x + 1);
addOneOnce(10); //=> 11
addOneOnce(addOneOnce(50)); //=> 11
```

### or
`a → b → a | b`
```js
R.or(true, true); //=> true
R.or(true, false); //=> true
R.or(false, true); //=> true
R.or(false, false); //=> false
```

### over
`Lens s a → (a → a) → s → s` 
`Lens s a = Functor f => (a → f a) → s → f s`
```js
var headLens = R.lensIndex(0);

R.over(headLens, R.toUpper, ['foo', 'bar', 'baz']); //=> ['FOO', 'bar', 'baz']
```

### pair
`a → b → (a,b)`
```js
R.pair('foo', 'bar'); //=> ['foo', 'bar']
```

### partial
`((a, b, c, …, n) → x) → [a, b, c, …] → ((d, e, f, …, n) → x)`
```js
var multiply2 = (a, b) => a * b;
var double = R.partial(multiply2, [2]);
double(2); //=> 4

var greet = (salutation, title, firstName, lastName) =>
  salutation + ', ' + title + ' ' + firstName + ' ' + lastName + '!';

var sayHello = R.partial(greet, ['Hello']);
var sayHelloToMs = R.partial(sayHello, ['Ms.']);
sayHelloToMs('Jane', 'Jones'); //=> 'Hello, Ms. Jane Jones!'
```

### partialRight
`((a, b, c, …, n) → x) → [d, e, f, …, n] → ((a, b, c, …) → x)`
```js
var greet = (salutation, title, firstName, lastName) =>
  salutation + ', ' + title + ' ' + firstName + ' ' + lastName + '!';

var greetMsJaneJones = R.partialRight(greet, ['Ms.', 'Jane', 'Jones']);

greetMsJaneJones('Hello'); //=> 'Hello, Ms. Jane Jones!'
```

### partition
`Filterable f => (a → Boolean) → f a → [f a, f a]`
```js
R.partition(R.contains('s'), ['sss', 'ttt', 'foo', 'bars']);
// => [ [ 'sss', 'bars' ],  [ 'ttt', 'foo' ] ]

R.partition(R.contains('s'), { a: 'sss', b: 'ttt', foo: 'bars' });
// => [ { a: 'sss', foo: 'bars' }, { b: 'ttt' }  ]
```

### path
`[Idx] → {a} → a | Undefined`
`Idx = String | Int`
```js
R.path(['a', 'b'], {a: {b: 2}}); //=> 2
R.path(['a', 'b'], {c: {b: 2}}); //=> undefined
```

### pathEq
`[Idx] → a → {a} → Boolean`
`Idx = String | Int`
```js
var user1 = { address: { zipCode: 90210 } };
var user2 = { address: { zipCode: 55555 } };
var user3 = { name: 'Bob' };
var users = [ user1, user2, user3 ];
var isFamous = R.pathEq(['address', 'zipCode'], 90210);
R.filter(isFamous, users); //=> [ user1 ]
```

### pathOr
`a → [Idx] → {a} → a`
`Idx = String | Int`
```js
R.pathOr('N/A', ['a', 'b'], {a: {b: 2}}); //=> 2
R.pathOr('N/A', ['a', 'b'], {c: {b: 2}}); //=> "N/A"
```

### pathSatisfies
`(a → Boolean) → [Idx] → {a} → Boolean`
`Idx = String | Int`
```js
R.pathSatisfies(y => y > 0, ['x', 'y'], {x: {y: 2}}); //=> true
```

### pick
`[k] → {k: v} → {k: v}`
```js
R.pick(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, d: 4}
R.pick(['a', 'e', 'f'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1}
```

### pickAll
`[k] → {k: v} → {k: v}`
```js
R.pickAll(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, d: 4}
R.pickAll(['a', 'e', 'f'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, e: undefined, f: undefined}
```

### pickBy
`((v, k) → Boolean) → {k: v} → {k: v}`
```js
var isUpperCase = (val, key) => key.toUpperCase() === key;
R.pickBy(isUpperCase, {a: 1, b: 2, A: 3, B: 4}); //=> {A: 3, B: 4}
```
### pipe
`(((a, b, …, n) → o), (o → p), …, (x → y), (y → z)) → ((a, b, …, n) → z)`
```js
var f = R.pipe(Math.pow, R.negate, R.inc);

f(3, 4); // -(3^4) + 1
```

### pipeK
`Chain m => ((a → m b), (b → m c), …, (y → m z)) → (a → m z)`
```js
// R.pipeK(f, g, h) is equivalent to R.pipe(f, R.chain(g), R.chain(h)).

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
```

### pipeP
`((a → Promise b), (b → Promise c), …, (y → Promise z)) → (a → Promise z)`
```js
//  followersForUser :: String -> Promise [User]
var followersForUser = R.pipeP(db.getUserById, db.getFollowers);
```

### pluck
`Functor f => k → f {k: v} → f v`
```js
R.pluck('a')([{a: 1}, {a: 2}]); //=> [1, 2]
R.pluck(0)([[1, 2], [3, 4]]);   //=> [1, 3]
R.pluck('val', {a: {val: 3}, b: {val: 5}}); //=> {a: 3, b: 5}
```

### prepend
`a → [a] → [a]`
```js
R.prepend('fee', ['fi', 'fo', 'fum']); //=> ['fee', 'fi', 'fo', 'fum']
```

### product
`[Number] → Number`
```js
R.product([2,4,6,8,100,1]); //=> 38400
```

### project
`[k] → [{k: v}] → [{k: v}]`
```js
var abby = {name: 'Abby', age: 7, hair: 'blond', grade: 2};
var fred = {name: 'Fred', age: 12, hair: 'brown', grade: 7};
var kids = [abby, fred];
R.project(['name', 'grade'], kids); //=> [{name: 'Abby', grade: 2}, {name: 'Fred', grade: 7}]
```

### prop
`s → {s: a} → a | Undefined`
```js
R.prop('x', {x: 100}); //=> 100
R.prop('x', {}); //=> undefined
```

### propEq
`String → a → Object → Boolean`
```js
var abby = {name: 'Abby', age: 7, hair: 'blond'};
var fred = {name: 'Fred', age: 12, hair: 'brown'};
var rusty = {name: 'Rusty', age: 10, hair: 'brown'};
var alois = {name: 'Alois', age: 15, disposition: 'surly'};
var kids = [abby, fred, rusty, alois];
var hasBrownHair = R.propEq('hair', 'brown');
R.filter(hasBrownHair, kids); //=> [fred, rusty]
```

### propIs
`Type → String → Object → Boolean`
```js
R.propIs(Number, 'x', {x: 1, y: 2});  //=> true
R.propIs(Number, 'x', {x: 'foo'});    //=> false
R.propIs(Number, 'x', {});            //=> false
```

### propOr
`a → String → Object → a`
```js
var alice = {
  name: 'ALICE',
  age: 101
};
var favorite = R.prop('favoriteLibrary');
var favoriteWithDefault = R.propOr('Ramda', 'favoriteLibrary');

favorite(alice);  //=> undefined
favoriteWithDefault(alice);  //=> 'Ramda'
```

### props
`[k] → {k: v} → [v]`
```js
R.props(['x', 'y'], {x: 1, y: 2}); //=> [1, 2]
R.props(['c', 'a', 'b'], {b: 2, a: 1}); //=> [undefined, 1, 2]

var fullName = R.compose(R.join(' '), R.props(['first', 'last']));
fullName({last: 'Bullet-Tooth', age: 33, first: 'Tony'}); //=> 'Tony Bullet-Tooth'
```

### propSatisfies
`(a → Boolean) → String → {String: a} → Boolean`
```js
R.propSatisfies(x => x > 0, 'x', {x: 1, y: 2}); //=> true
```

### range
`Number → Number → [Number]`
```js
R.range(1, 5);    //=> [1, 2, 3, 4]
R.range(50, 53);  //=> [50, 51, 52]
```

### reduce
`((a, b) → a) → a → [b] → a`
```js 
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
```

### reduceBy
`((a, b) → a) → a → (b → String) → [b] → {String: a}`
```js
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
```

### reduced
`a → *`
```js
R.reduce(
 (acc, item) => item > 3 ? R.reduced(acc) : acc.concat(item),
 [],
 [1, 2, 3, 4, 5]) // [1, 2, 3]
```

### reduceRight
`((a, b) → b) → b → [a] → b`
```js
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
```

### reduceWhile
`((a, b) → Boolean) → ((a, b) → a) → a → [b] → a`
```js
var isOdd = (acc, x) => x % 2 === 1;
var xs = [1, 3, 5, 60, 777, 800];
R.reduceWhile(isOdd, R.add, 0, xs); //=> 9

var ys = [2, 4, 6]
R.reduceWhile(isOdd, R.add, 111, ys); //=> 111
```

### reject
`Filterable f => (a → Boolean) → f a → f a`
```js
var isOdd = (n) => n % 2 === 1;

R.reject(isOdd, [1, 2, 3, 4]); //=> [2, 4]

R.reject(isOdd, {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, d: 4}
```

### remove
`Number → Number → [a] → [a]`
```js
R.remove(2, 3, [1,2,3,4,5,6,7,8]); //=> [1,2,6,7,8]
```

### repeat
`a → n → [a]`
```
R.repeat('hi', 5); //=> ['hi', 'hi', 'hi', 'hi', 'hi']

var obj = {};
var repeatedObjs = R.repeat(obj, 5); //=> [{}, {}, {}, {}, {}]
repeatedObjs[0] === repeatedObjs[1]; //=> true
```

### replace
`RegExp|String → String → String → String`
```js
R.replace('foo', 'bar', 'foo foo foo'); //=> 'bar foo foo'
R.replace(/foo/, 'bar', 'foo foo foo'); //=> 'bar foo foo'

// Use the "g" (global) flag to replace all occurrences:
R.replace(/foo/g, 'bar', 'foo foo foo'); //=> 'bar bar bar'
```

### reverse 
`[a] → [a]`
`String → String`
```js
R.reverse([1, 2, 3]);  //=> [3, 2, 1]
R.reverse([1, 2]);     //=> [2, 1]
R.reverse([1]);        //=> [1]
R.reverse([]);         //=> []

R.reverse('abc');      //=> 'cba'
R.reverse('ab');       //=> 'ba'
R.reverse('a');        //=> 'a'
R.reverse('');         //=> ''
```

### scan
`((a, b) → a) → a → [b] → [a]`
```js
var numbers = [1, 2, 3, 4];
var factorials = R.scan(R.multiply, 1, numbers); //=> [1, 1, 2, 6, 24]
```

### sequence
`(Applicative f, Traversable t) => (a → f a) → t (f a) → f (t a)`
```js
R.sequence(Maybe.of, [Just(1), Just(2), Just(3)]);   //=> Just([1, 2, 3])
R.sequence(Maybe.of, [Just(1), Just(2), Nothing()]); //=> Nothing()

R.sequence(R.of, Just([1, 2, 3])); //=> [Just(1), Just(2), Just(3)]
R.sequence(R.of, Nothing());       //=> [Nothing()]
```

### set
`Lens s a → a → s → s`
`Lens s a = Functor f => (a → f a) → s → f s`
```
var xLens = R.lensProp('x');

R.set(xLens, 4, {x: 1, y: 2});  //=> {x: 4, y: 2}
R.set(xLens, 8, {x: 1, y: 2});  //=> {x: 8, y: 2}
```

### slice
`Number → Number → [a] → [a]`
`Number → Number → String → String`
```
R.slice(1, 3, ['a', 'b', 'c', 'd']);        //=> ['b', 'c']
R.slice(1, Infinity, ['a', 'b', 'c', 'd']); //=> ['b', 'c', 'd']
R.slice(0, -1, ['a', 'b', 'c', 'd']);       //=> ['a', 'b', 'c']
R.slice(-3, -1, ['a', 'b', 'c', 'd']);      //=> ['b', 'c']
R.slice(0, 3, 'ramda');                     //=> 'ram'
```

### sort 
`((a, a) → Number) → [a] → [a]`
```
var diff = function(a, b) { return a - b; };
R.sort(diff, [4,2,7,5]); //=> [2, 4, 5, 7]
```

### sortBy 
`Ord b => (a → b) → [a] → [a]`
```
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
```

### sortWith
`[(a, a) → Number] → [a] → [a]`
```
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
 ```
 
 ### split
`(String | RegExp) → String → [String]`
```
var pathComponents = R.split('/');
R.tail(pathComponents('/usr/local/bin/node')); //=> ['usr', 'local', 'bin', 'node']

R.split('.', 'a.b.c.xyz.d'); //=> ['a', 'b', 'c', 'xyz', 'd']
```

### splitAt
`Number → [a] → [[a], [a]]`
`Number → String → [String, String]`
```
R.splitAt(1, [1, 2, 3]);          //=> [[1], [2, 3]]
R.splitAt(5, 'hello world');      //=> ['hello', ' world']
R.splitAt(-1, 'foobar');          //=> ['fooba', 'r']
```

### splitEvery
`Number → [a] → [[a]]`
`Number → String → [String]`
```
R.splitEvery(3, [1, 2, 3, 4, 5, 6, 7]); //=> [[1, 2, 3], [4, 5, 6], [7]]
R.splitEvery(3, 'foobarbaz'); //=> ['foo', 'bar', 'baz']
```

### splitWhen
`(a → Boolean) → [a] → [[a], [a]]`
```
R.splitWhen(R.equals(2), [1, 2, 3, 1, 2, 3]);   //=> [[1], [2, 3, 1, 2, 3]]
```

### startsWith
`[a] → Boolean`
`String → Boolean`
```
R.startsWith('a', 'abc')                //=> true
R.startsWith('b', 'abc')                //=> false
R.startsWith(['a'], ['a', 'b', 'c'])    //=> true
R.startsWith(['b'], ['a', 'b', 'c'])    //=> false
```

### subtract
`Number → Number → Number`
```
R.subtract(10, 8); //=> 2

var minus5 = R.subtract(R.__, 5);
minus5(17); //=> 12

var complementaryAngle = R.subtract(90);
complementaryAngle(30); //=> 60
complementaryAngle(72); //=> 18
```

### sum
`[Number] → Number`
```
R.sum([2,4,6,8,100,1]); //=> 121
```

### symmetricDifference
`[*] → [*] → [*]`
```
R.symmetricDifference([1,2,3,4], [7,6,5,4,3]); //=> [1,2,7,6,5]
R.symmetricDifference([7,6,5,4,3], [1,2,3,4]); //=> [7,6,5,1,2]
```

### symmetricDifferenceWith
`((a, a) → Boolean) → [a] → [a] → [a]`
```
var eqA = R.eqBy(R.prop('a'));
var l1 = [{a: 1}, {a: 2}, {a: 3}, {a: 4}];
var l2 = [{a: 3}, {a: 4}, {a: 5}, {a: 6}];
R.symmetricDifferenceWith(eqA, l1, l2); //=> [{a: 1}, {a: 2}, {a: 5}, {a: 6}]
```

### T
`* → Boolean`
```
R.T(); //=> true
```

### tail
`[a] → [a]`
`String → String`
```
R.tail([1, 2, 3]);  //=> [2, 3]
R.tail([1, 2]);     //=> [2]
R.tail([1]);        //=> []
R.tail([]);         //=> []

R.tail('abc');  //=> 'bc'
R.tail('ab');   //=> 'b'
R.tail('a');    //=> ''
R.tail('');     //=> ''
```

#### take
`Number → [a] → [a]`
`Number → String → String`
```
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
```

### takeLast
`Number → [a] → [a]`
`Number → String → String`
```
R.takeLast(1, ['foo', 'bar', 'baz']); //=> ['baz']
R.takeLast(2, ['foo', 'bar', 'baz']); //=> ['bar', 'baz']
R.takeLast(3, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.takeLast(4, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.takeLast(3, 'ramda');               //=> 'mda'
```

### takeLastWhile
`(a → Boolean) → [a] → [a]`
`(a → Boolean) → String → String`
```
var isNotOne = x => x !== 1;

R.takeLastWhile(isNotOne, [1, 2, 3, 4]); //=> [2, 3, 4]

R.takeLastWhile(x => x !== 'R' , 'Ramda'); //=> 'amda'
```

### takeWhile
`(a → Boolean) → [a] → [a]`
`(a → Boolean) → String → String`
```
var isNotFour = x => x !== 4;

R.takeWhile(isNotFour, [1, 2, 3, 4, 3, 2, 1]); //=> [1, 2, 3]

R.takeWhile(x => x !== 'd' , 'Ramda'); //=> 'Ram'
```

### tap 
`(a → *) → a → a`
```
var sayX = x => console.log('x is ' + x);
R.tap(sayX, 100); //=> 100
// logs 'x is 100'
```

### test
`RegExp → String → Boolean`
```
R.test(/^x/, 'xyz'); //=> true
R.test(/^y/, 'xyz'); //=> false
```

### times
`(Number → a) → Number → [a]`
```
R.times(R.identity, 5); //=> [0, 1, 2, 3, 4]
```

### toLower
`String → String`
```
R.toLower('XYZ'); //=> 'xyz'
```

### toPairs
`{String: *} → [[String,*]]`
```
R.toPairs({a: 1, b: 2, c: 3}); //=> [['a', 1], ['b', 2], ['c', 3]]
```

### toPairsIn
`{String: *} → [[String,*]]`
```
var F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
var f = new F();
R.toPairsIn(f); //=> [['x','X'], ['y','Y']]
```

### toString
`* → String`
```
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
```

### toUpper
`String → String`
```
R.toUpper('abc'); //=> 'ABC'
```

### transduce
`(c → c) → ((a, b) → a) → a → [b] → a`
```
var numbers = [1, 2, 3, 4];
var transducer = R.compose(R.map(R.add(1)), R.take(2));
R.transduce(transducer, R.flip(R.append), [], numbers); //=> [2, 3]

var isOdd = (x) => x % 2 === 1;
var firstOddTransducer = R.compose(R.filter(isOdd), R.take(1));
R.transduce(firstOddTransducer, R.flip(R.append), [], R.range(0, 100)); //=> [1]
```

### transpose
`[[a]] → [[a]]`
```
R.transpose([[1, 'a'], [2, 'b'], [3, 'c']]) //=> [[1, 2, 3], ['a', 'b', 'c']]
R.transpose([[1, 2, 3], ['a', 'b', 'c']]) //=> [[1, 'a'], [2, 'b'], [3, 'c']]

// If some of the rows are shorter than the following rows, their elements are skipped:
R.transpose([[10, 11], [20], [], [30, 31, 32]]) //=> [[10, 20, 30], [11, 31], [32]]
```

### traverse
`(Applicative f, Traversable t) => (a → f a) → (a → f b) → t a → f (t b)`
```
// Returns `Nothing` if the given divisor is `0`
safeDiv = n => d => d === 0 ? Nothing() : Just(n / d)

R.traverse(Maybe.of, safeDiv(10), [2, 4, 5]); //=> Just([5, 2.5, 2])
R.traverse(Maybe.of, safeDiv(10), [2, 0, 5]); //=> Nothing
```

### trim
`String → String`
```
R.trim('   xyz  '); //=> 'xyz'
R.map(R.trim, R.split(',', 'x, y, z')); //=> ['x', 'y', 'z']
```

### tryCatch
`(…x → a) → ((e, …x) → a) → (…x → a)`
```
R.tryCatch(R.prop('x'), R.F)({x: true}); //=> true
R.tryCatch(R.prop('x'), R.F)(null);      //=> false
```

### type
`(* → {*}) → String`
```
R.type({}); //=> "Object"
R.type(1); //=> "Number"
R.type(false); //=> "Boolean"
R.type('s'); //=> "String"
R.type(null); //=> "Null"
R.type([]); //=> "Array"
R.type(/[A-z]/); //=> "RegExp"
R.type(() => {}); //=> "Function"
R.type(undefined); //=> "Undefined"
```

### unapply
`([*…] → a) → (*… → a)`
```
R.unapply(JSON.stringify)(1, 2, 3); //=> '[1,2,3]'
```

### unary
`(* → b) → (a → b)`
```
var takesTwoArgs = function(a, b) {
  return [a, b];
};
takesTwoArgs.length; //=> 2
takesTwoArgs(1, 2); //=> [1, 2]

var takesOneArg = R.unary(takesTwoArgs);
takesOneArg.length; //=> 1
// Only 1 argument is passed to the wrapped function
takesOneArg(1, 2); //=> [1, undefined]
```

### uncurryN
`Number → (a → b) → (a → c)`
```
var addFour = a => b => c => d => a + b + c + d;

var uncurriedAddFour = R.uncurryN(4, addFour);
uncurriedAddFour(1, 2, 3, 4); //=> 10
```

### unfold
`(a → [b]) → * → [b]`
```
var f = n => n > 50 ? false : [-n, n + 10];
R.unfold(f, 10); //=> [-10, -20, -30, -40, -50]
```

### union
`[*] → [*] → [*]`
```
R.union([1, 2, 3], [2, 3, 4]); //=> [1, 2, 3, 4]
```

### unionWith
`((a, a) → Boolean) → [*] → [*] → [*]`
```
var l1 = [{a: 1}, {a: 2}];
var l2 = [{a: 1}, {a: 4}];
R.unionWith(R.eqBy(R.prop('a')), l1, l2); //=> [{a: 1}, {a: 2}, {a: 4}]
```

### uniq
`[a] → [a]`
```
R.uniq([1, 1, 2, 1]); //=> [1, 2]
R.uniq([1, '1']);     //=> [1, '1']
R.uniq([[42], [42]]); //=> [[42]]
```

### uniqBy
`(a → b) → [a] → [a]`
```
R.uniqBy(Math.abs, [-1, -5, 2, 10, 1, 2]); //=> [-1, -5, 2, 10]
```

### uniqWith
`((a, a) → Boolean) → [a] → [a]`
```
var strEq = R.eqBy(String);
R.uniqWith(strEq)([1, '1', 2, 1]); //=> [1, 2]
R.uniqWith(strEq)([{}, {}]);       //=> [{}]
R.uniqWith(strEq)([1, '1', 1]);    //=> [1]
R.uniqWith(strEq)(['1', 1, 1]);    //=> ['1']
```

### unless
`(a → Boolean) → (a → a) → a → a`
```
let safeInc = R.unless(R.isNil, R.inc);
safeInc(null); //=> null
safeInc(1); //=> 2
```

### unnest
`Chain c => c (c a) → c a`
```
R.unnest([1, [2], [[3]]]); //=> [1, 2, [3]]
R.unnest([[1, 2], [3, 4], [5, 6]]); //=> [1, 2, 3, 4, 5, 6]
```

### until
`(a → Boolean) → (a → a) → a → a`
```
R.until(R.gt(R.__, 100), R.multiply(2))(1) // => 128
```

### update
`Number → a → [a] → [a]`
```
R.update(1, 11, [0, 1, 2]);     //=> [0, 11, 2]
R.update(1)(11)([0, 1, 2]);     //=> [0, 11, 2]
```

### useWith 
`((x1, x2, …) → z) → [(a → x1), (b → x2), …] → (a → b → … → z)`
```
R.useWith(Math.pow, [R.identity, R.identity])(3, 4); //=> 81
R.useWith(Math.pow, [R.identity, R.identity])(3)(4); //=> 81
R.useWith(Math.pow, [R.dec, R.inc])(3, 4); //=> 32
R.useWith(Math.pow, [R.dec, R.inc])(3)(4); //=> 32
```

### values
`{k: v} → [v]`
```
R.values({a: 1, b: 2, c: 3}); //=> [1, 2, 3]
```

### valuesIn
`{k: v} → [v]`
```
var F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
var f = new F();
R.valuesIn(f); //=> ['X', 'Y']
```

### view
`Lens s a → s → a`
`Lens s a = Functor f => (a → f a) → s → f s`
```
var xLens = R.lensProp('x');

R.view(xLens, {x: 1, y: 2});  //=> 1
R.view(xLens, {x: 4, y: 2});  //=> 4
```

### when
`(a → Boolean) → (a → a) → a → a`
```
// truncate :: String -> String
var truncate = R.when(
  R.propSatisfies(R.gt(R.__, 10), 'length'),
  R.pipe(R.take(10), R.append('…'), R.join(''))
);
truncate('12345');         //=> '12345'
truncate('0123456789ABC'); //=> '0123456789…'
```

### where
```{String: (* → Boolean)} → {String: *} → Boolean```
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
```js
// pred :: Object -> Boolean
var pred = R.whereEq({a: 1, b: 2});

pred({a: 1});              //=> false
pred({a: 1, b: 2});        //=> true
pred({a: 1, b: 2, c: 3});  //=> true
pred({a: 1, b: 1});        //=> false
```

### without
`[a] → [a] → [a]`
```
R.without([1, 2], [1, 2, 1, 3, 4]); //=> [3, 4]
```

### xprod
`[a] → [b] → [[a,b]]`
```
R.xprod([1, 2], ['a', 'b']); //=> [[1, 'a'], [1, 'b'], [2, 'a'], [2, 'b']]
```

### zip
`[a] → [b] → [[a,b]]`
```
R.zip([1, 2, 3], ['a', 'b', 'c']); //=> [[1, 'a'], [2, 'b'], [3, 'c']]
```

### zipObj
```js
[String] → [*] → {String: *}
```
```js
R.zipObj(['a', 'b', 'c'], [1, 2, 3]); //=> {a: 1, b: 2, c: 3}
```

### zipWith
`((a, b) → c) → [a] → [b] → [c]`
```js
var f = (x, y) => {
  // ...
};
R.zipWith(f, [1, 2, 3], ['a', 'b', 'c']);
//=> [f(1, 'a'), f(2, 'b'), f(3, 'c')]

```
