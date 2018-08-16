# String

## _trim(去除左右两侧空白)

`String → String `

```javascript
R.trim('   xyz  '); //=> 'xyz'
R.map(R.trim, R.split(',', 'x, y, z')); //=> ['x', 'y', 'z']
```

## match(正则匹配)

`RegExp → String → [String | Undefined] `

```javascript
R.match(/([a-z]a)/g, 'bananas'); //=> ['ba', 'na', 'na']
R.match(/a/, 'b'); //=> []
R.match(/a/, null); //=> TypeError: null does not have a method named "match"
```

## replace(替换)

`RegExp|String → String → String → String `

```javascript
R.replace('foo', 'bar', 'foo foo foo'); //=> 'bar foo foo'
R.replace(/foo/, 'bar', 'foo foo foo'); //=> 'bar foo foo'

// Use the "g" (global) flag to replace all occurrences:
R.replace(/foo/g, 'bar', 'foo foo foo'); //=> 'bar bar bar'
```

## split(字符串分割)

`(String | RegExp) → String → [String] `

```javascript
var pathComponents = R.split('/');
R.tail(pathComponents('/usr/local/bin/node')); //=> ['usr', 'local', 'bin', 'node']

R.split('.', 'a.b.c.xyz.d'); //=> ['a', 'b', 'c', 'xyz', 'd']
```

## test(正则匹配判断)

`RegExp → String → Boolean `

```javascript
R.test(/^x/, 'xyz'); //=> true
R.test(/^y/, 'xyz'); //=> false
```

## toLower(字符串小写)

`String → String `

```javascript
R.toLower('XYZ'); //=> 'xyz'
```

## toUpper(字符串大写)

`String → String `

```javascript
R.toUpper('abc'); //=> 'ABC'
```

## toString(转化为字符串)

`* → String` 

```javascript
R.toString(42); //=> '42'
R.toString('abc'); //=> '"abc"'
R.toString([1, 2, 3]); //=> '[1, 2, 3]'
R.toString({foo: 1, bar: 2, baz: 3}); //=> '{"bar": 2, "baz": 3, "foo": 1}'
R.toString(new Date('2001-02-03T04:05:06Z')); //=> 'new Date("2001-02-03T04:05:06.000Z")'
```

