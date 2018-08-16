# Function

## o(两个函数合成，从右到左执行)

`(b → c) → (a → b) → a → c `

```javascript
var classyGreeting = name => "The name's " + name.last + ", " + name.first + " " + name.last
var yellGreeting = R.o(R.toUpper, classyGreeting);
yellGreeting({first: 'James', last: 'Bond'}); //=> "THE NAME'S BOND, JAMES BOND"

R.o(R.multiply(10), R.add(10))(-4) //=> 60
```

## compose(函数合成，从右到左执行)

`((y → z), (x → y), …, (o → p), ((a, b, …, n) → o)) → ((a, b, …, n) → z) `

```javascript
var classyGreeting = (firstName, lastName) => "The name's " + lastName + ", " + firstName + " " + lastName
var yellGreeting = R.compose(R.toUpper, classyGreeting);
yellGreeting('James', 'Bond'); //=> "THE NAME'S BOND, JAMES BOND"

R.compose(Math.abs, R.add(1), R.multiply(2))(-4) //=> 7
```

## composeP(promise函数合成，从右到左执行)

`((y → Promise z), (x → Promise y), …, (a → Promise b)) → (a → Promise z) `

```javascript
//最右侧的函数可以有任意参数，其他函数必须为一元函数
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

## pipe(函数合成，从左到右执行)

`(((a, b, …, n) → o), (o → p), …, (x → y), (y → z)) → ((a, b, …, n) → z) `

```javascript
var f = R.pipe(Math.pow, R.negate, R.inc);

f(3, 4); // -(3^4) + 1
```

## pipeP(promise函数合成，从左到右执行)

`((a → Promise b), (b → Promise c), …, (y → Promise z)) → (a → Promise z) `

```javascript
// 最左侧的函数可以有任意参数，其他函数必须为一元函数
//  followersForUser :: String -> Promise [User]
var followersForUser = R.pipeP(db.getUserById, db.getFollowers);
```

## addIndex(返回一个传递(item, index, list)的列表迭代函数)

`((a … → b) … → [a] → *) → (a …, Int, [a] → b) … → [a] → *) `

```javascript
//参数为一个不将索引或列表成员传递到其回调的列表迭代函数
var mapIndexed = R.addIndex(R.map);
mapIndexed((val, idx) => idx + '-' + val, ['f', 'o', 'o', 'b', 'a', 'r']);
//=> ['0-f', '1-o', '2-o', '3-b', '4-a', '5-r']
```

## always(常量)

`a → (* → a) `

```javascript
var t = R.always('Tee');
t(); //=> 'Tee'
```

## F(false)

`* → Boolean `

```javascript
R.F(); //=> false
```

## T(true)

`* → Boolean`

```javascript
R.T(); //=> true
```

## ap(数组成员执行一组函数)

`[a → b] → [a] → [b] ` `Apply f => f (a → b) → f a → f b ` `(a → b → c) → (a → b) → (a → c) `

```javascript
R.ap([R.multiply(2), R.add(3)], [1,2,3]); //=> [2, 4, 6, 4, 5, 6]
R.ap([R.concat('tasty '), R.toUpper], ['pizza', 'salad']); //=> ["tasty pizza", "tasty salad", "PIZZA", "SALAD"]

// R.ap can also be used as S combinator
// when only two functions are passed
R.ap(R.concat, R.toUpper)('Ramda') //=> 'RamdaRAMDA'
```

## apply(参数数组传入指定函数)

`(*… → a) → [*] → a `

```javascript
var nums = [1, 2, 3, -99, 42, 6, 7];
R.apply(Math.max, nums); //=> 42
```

## unapply(与apply相反)

`([*…] → a) → (*… → a) `

```javascript
R.unapply(JSON.stringify)(1, 2, 3); //=> '[1,2,3]'
```

## memoizeWith(函数结果缓存)

`(*… → String) → (*… → a) → (*… → a) `

```javascript
//第一个参数为fn产生缓存值
//第二个参数为fn记忆函数
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

## call(参数序列传入指定函数)

`(*… → a),*… → a `

```javascript
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

## applySpec(模板函数)

`{k: ((a, b, …, m) → v)} → ((a, b, …, m) → {k: v}) `

```javascript
//返回一个模板函数，该函数会将参数传入模板内的函数执行，然后将执行结果填充到模板
var getMetrics = R.applySpec({
  sum: R.add,
  nested: { mul: R.multiply }
});
getMetrics(2, 4); // => { sum: 6, nested: { mul: 8 } }
```

## juxt(函数组处理)

`[(a, b, …, m) → n] → ((a, b, …, m) → [n]) `

```javascript
var getRange = R.juxt([Math.min, Math.max]);
getRange(3, 4, 9, -3); //=> [-3, 9]
```

## converge(双层函数组处理)

`((x1, x2, …) → z) → [((a, b, …) → x1), ((a, b, …) → x2), …] → (a → b → … → z) `

```javascript
//接受两个参数，第一个参数是函数，第二个参数是函数数组。传入的值先使用第二个参数包含的函数分别处理以后，再用第一个参数处理前一步生成的结果
var average = R.converge(R.divide, [R.sum, R.length])
average([1, 2, 3, 4, 5, 6, 7]) //=> 4

var strangeConcat = R.converge(R.concat, [R.toUpper, R.toLower])
strangeConcat("Yodel") //=> "YODELyodel"
```

## useWith(双层函数组一对一处理)

`((x1, x2, …) → z) → [(a → x1), (b → x2), …] → (a → b → … → z) `

```javascript
//接受两个参数，第一个参数是函数，第二个参数是函数数组。传入的值先使用第二个参数包含的函数一对一对应处理以后，再用第一个参数处理前一步生成的结果
R.useWith(Math.pow, [R.identity, R.identity])(3, 4); //=> 81
R.useWith(Math.pow, [R.identity, R.identity])(3)(4); //=> 81
R.useWith(Math.pow, [R.dec, R.inc])(3, 4); //=> 32
R.useWith(Math.pow, [R.dec, R.inc])(3)(4); //=> 32
```

## applyTo(单参数传入指定函数)

`a → (a → b) → b `

```javascript
var t42 = R.applyTo(42);
t42(R.identity); //=> 42
t42(R.add(1)); //=> 43
```

## comparator(自定义排列的比较函数)

`((a, b) → Boolean) → ((a, b) → Number) `

```javascript
var byAge = R.comparator((a, b) => a.age < b.age);
var people = [{age:12},{age:53},{age:8}];
var peopleByIncreasingAge = R.sort(byAge, people);//=> [{age:8},{age:12},{age:53}]
```

## ascend(升序排列的比较函数 )

`Ord b => (a → b) → a → a → Number `

```javascript
var byAge = R.ascend(R.prop('age'));
var people = [{age:12},{age:53},{age:8}];
var peopleByYoungestFirst = R.sort(byAge, people);//=> [{age:8},{age:12},{age:53}]
```

## descend(降序排列的比较函数 )

`Ord b => (a → b) → a → a → Number `

```javascript
var byAge = R.descend(R.prop('age'));
var people = [{age:12},{age:53},{age:8}];
var peopleByYoungestFirst = R.sort(byAge, people);//=> [{age:53},{age:12},{age:8}]
```

## binary(参数函数执行时，只传入前两个参数 )

`(* → c) → (a, b → c) `

```javascript
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

## nAry(参数函数执行时，只传入前n个参数)

`Number → (* → a) → (* → a) `

```javascript
var takesTwoArgs = (a, b) => [a, b];

takesTwoArgs.length; //=> 2
takesTwoArgs(1, 2); //=> [1, 2]

var takesOneArg = R.nAry(1, takesTwoArgs);
takesOneArg.length; //=> 1
// Only `n` arguments are passed to the wrapped function
takesOneArg(1, 2); //=> [1, undefined]
```

## unary(参数函数执行时，只传入第一个参数)

`(* → b) → (a → b) `

```javascript
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

## bind(绑定上下文函数)

`(* → *) → {*} → (* → *) `

```javascript
var log = R.bind(console.log, console);
R.pipe(R.assoc('a', 2), R.tap(log), R.assoc('a', 3))({a: 1}); //=> {a: 3}
// logs {a: 2}
```

## partial(指定左侧部分参数)

`((a, b, c, …, n) → x) → [a, b, c, …] → ((d, e, f, …, n) → x) `

```javascript
//允许多参数的函数接受一个数组，指定最左边的部分参数。
var multiply2 = (a, b) => a * b;
var double = R.partial(multiply2, [2]);
double(2); //=> 4

var greet = (salutation, title, firstName, lastName) =>
  salutation + ', ' + title + ' ' + firstName + ' ' + lastName + '!';

var sayHello = R.partial(greet, ['Hello']);
var sayHelloToMs = R.partial(sayHello, ['Ms.']);
sayHelloToMs('Jane', 'Jones'); //=> 'Hello, Ms. Jane Jones!'
```

## partialRight(指定右侧部分参数)

`((a, b, c, …, n) → x) → [d, e, f, …, n] → ((a, b, c, …) → x) `

```javascript
//允许多参数的函数接受一个数组，指定最右边的部分参数。
var greet = (salutation, title, firstName, lastName) =>
  salutation + ', ' + title + ' ' + firstName + ' ' + lastName + '!';

var greetMsJaneJones = R.partialRight(greet, ['Ms.', 'Jane', 'Jones']);

greetMsJaneJones('Hello'); //=> 'Hello, Ms. Jane Jones!'
```

## curry(函数柯里化)

`(* → a) → (* → a) `

```javascript
var addFourNumbers = (a, b, c, d) => a + b + c + d;

var curriedAddFourNumbers = R.curry(addFourNumbers);
var f = curriedAddFourNumbers(1, 2);
var g = f(3);
g(4); //=> 10
```

## curryN(可变函数柯里化)

`Number → (* → a) → (* → a) `

```javascript
var sumArgs = (...args) => R.sum(args);
var curriedAddFourNumbers = R.curryN(4, sumArgs);
var f = curriedAddFourNumbers(1, 2);
var g = f(3);
g(4); //=> 10

var sumArgs = (...args) => R.sum(args);
var curriedAddFourNumbers = R.curryN(3, sumArgs);
var f = curriedAddFourNumbers(1, 2);
var g = f(3);
g(4); //=> TypeError: g is not a function

var sumArgs = (...args) => R.sum(args);
var curriedAddFourNumbers = R.curryN(5, sumArgs);
var f = curriedAddFourNumbers(1, 2);
var g = f(3);
g(4); //=> function()
```

## uncurryN(反柯里化)

`Number → (a → b) → (a → c) `

```javascript
var addFour = a => b => c => d => a + b + c + d;

var uncurriedAddFour = R.uncurryN(4, addFour);
uncurriedAddFour(1, 2, 3, 4); //=> 10
```

## empty(返回与指定值同类型的空值)

`a → a `

```javascript
R.empty(Just(42));      //=> Nothing()
R.empty([1, 2, 3]);     //=> []
R.empty('unicorns');    //=> ''
R.empty({x: 1, y: 2});  //=> {}
```

## flip(翻转前两个参数传入顺序)

`((a, b, c, …) → z) → (b → a → c → … → z) `

```javascript
var mergeThree = (a, b, c) => [].concat(a, b, c);

mergeThree(1, 2, 3); //=> [1, 2, 3]

R.flip(mergeThree)(1, 2, 3); //=> [2, 1, 3]
```

## identity(占位函数)

`a → a `

```javascript
//不进行任何运算，只返回传入的参数，作为默认或占位符使用
R.identity(1); //=> 1

var obj = {};
R.identity(obj) === obj; //=> true
```

## invoker(引用函数)

`Number → String → (a → b → … → n → Object → *) `

```javascript
//第一个参数为引入的函数的参数个数
//第二个参数为引入的函数的名称
var sliceFrom = R.invoker(1, 'slice');
sliceFrom(6, 'abcdefghijklm'); //=> 'ghijklm'
var sliceFrom6 = R.invoker(2, 'slice')(6);
sliceFrom6(8, 'abcdefghijklm'); //=> 'gh'
```

## construct(构造函数)

`(* → {*}) → (* → {*}) `

```javascript
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

## constructN(可变构造函数)

`Number → (* → {*}) → (* → {*}) `

```javascript
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

## lift(提升)

`(*… → *) → ([*]… → [*]) `

```javascript
var madd3 = R.lift((a, b, c) => a + b + c);

madd3([1,2,3], [1,3], [2,5]); //=> [
						    //=> 	4, 7, 
                              //=>     6, 9, 
                              //=>     5, 8,
                              //=>     7, 10, 
                              //=>     6, 9,
                              //=>     8, 11
						    //=> ]
var madd5 = R.lift((a, b, c, d, e) => a + b + c + d + e);

madd5([1,2], [3], [4, 5], [6], [7, 8]); //=> [21, 22, 22, 23, 22, 23, 23, 24]
```

## liftN(可变提升)

`Number → (*… → *) → ([*]… → [*]) `

```javascript
var madd3 = R.liftN(3, (...args) => R.sum(args));
madd3([1,2,3], [1,2,3], [1]); //=> [3, 4, 5, 4, 5, 6, 5, 6, 7]
```

## nthArg(返回指定索引处的参数)

`Number → *… → * `

```javascript
R.nthArg(1)('a', 'b', 'c'); //=> 'b'
R.nthArg(-1)('a', 'b', 'c'); //=> 'c'
```

## __(占位符)

```javascript
var greet = R.replace('{name}', R.__, 'Hello, {name}!');
greet('Alice'); //=> 'Hello, Alice!'
```

## of(返回包含所提供值的单数组)

`a → [a] `

```javascript
R.of(null); //=> [null]
R.of([42]); //=> [[42]]
```

## once(只运行一次函数)

`(a… → b) → (a… → b) `

```javascript
var addOneOnce = R.once(x => x + 1);
addOneOnce(10); //=> 11
addOneOnce(addOneOnce(50)); //=> 11
```

## tap(值变换)

`(a → *) → a → a `

```javascript
var sayX = x => console.log('x is ' + x);
R.tap(sayX, 100); //=> 100
// logs 'x is 100'
```

## tryCatch(try catch)

`(…x → a) → ((e, …x) → a) → (…x → a) `

```javascript
//第一个参数为tryer
//第二个参数为catcher
R.tryCatch(R.prop('x'), R.F)({x: true}); //=> true
R.tryCatch(R.prop('x'), R.F)(null);      //=> false
```

