# JSFrick

PG version of the JSF*ck encoder made by Martin Kleppe

JSFrick encodes any string or JS into JS that uses only the following non-alphanumeric characters `=_$(){}><~!%^&|*-+[];:?.,\`. characters that that can create strings (``"'`/``) are not used as an aesthetic choice and to provide a bit more difficulty for myself.


## how it works

the principles for generating characters are very similar to [JSF*ck](https://github.com/aemkei/jsfuck) (<- good explanation in the readme), though, the wider set of characters allows for some more concise generation.

### getting characters

I'll only show here ways of getting certain characters that differ from JSF*ck.

The bitwise not character will invert all bits of a number.

```~0 === -1```

We can use an empty array instead of 0 since when coalesced into a number, it has a value of 0.

```~[] === -1```

Taking the negative of this will result in 1.

```-~[] === 1```

Chaining -~ will increase the number by one each time allowing us to get all numbers.

```
-~-~[] === 2
-~-~-~[] === 3
-~-~-~-~[] === 4
-~-~-~-~-~[] === 5
-~-~-~-~-~-~[] === 6
-~-~-~-~-~-~-~[] === 7
(-~-~-~-~[]<<-~[]) === 8 === 4 << 1
+([-~[]]+(+[]))+~[] === 9 === "10"-1
+[] === 0
```

For 8 and 9 we can use less characters by doing a bit shift, or the infamous subtracting from a string trick.

___

To get the characters we are using to encode with (`=_$(){}><~!%^&|*-+[];:?.,`) as a string, we can use them in an arrow function, convert the function to a string, and then access the character we need. 

``` 
((_=>_)+[])[1]      === "="
((_=>_)+[])[0]      === "_"
(($=>$)+[])[0]      === "$"
((()=>{})+[])[0]    === "("
((()=>{})+[])[1]    === ")"
((_=>{})+[])[3]     === "{"
((_=>{})+[])[4]     === "}"
((_=>_)+[])[2]      === ">"
((_=>_<_)+[])[4]    === "<"
((_=>~_)+[])[3]     === "~"
((_=>!_)+[])[3]     === "!"
((_=>_%_)+[])[4]    === "%"
((_=>_^_)+[])[4]    === "^"
((_=>_&_)+[])[4]    === "&"
((_=>_|_)+[])[4]    === "|"
((_=>_*_)+[])[4]    === "*"
((_=>_-_)+[])[4]    === "-"
((_=>_+_)+[])[4]    === "+"
((_=>[])+[])[3]     === "["
((_=>[])+[])[4]     === "]"
((_=>{;})+[])[4]    === ";"
((_=>{_?_:_})+[])[7]=== ":"
((_=>{_?_:_})+[])[5]=== "?"
((_=>{_._})+[])[5]  === "."
((_=>[,])+[])[4]    === ","
```

### character reduction

Since `$` and `_` are valid variable names, creating objects, and setting their properties named after these variables allows for many variables with minimal characters used.

Assigning characters and strings to variables for later use greatly reduces the final character count of encoded JS.

But how can these variables be used if we can't use `var` `let` or `const`? In JS, assigning a value to a variable that hasn't been declared yet will cause the variable to be declared in the global scope, which is usually unwanted, but for our case it will work just fine.

### returning a string

If all that was required was to be able to run a functions we could encode into the following structure:

```
declare variables; Function.constructor( inputted JS )() 
```

But I wanted the capability to evaluate the encoded JS and get a return value if eval is selected, or get a string if eval is not selected. So put everything in a arrow function `()=>{}`


Arrow functions will return a value without having the `return` keyword used, only if there is one statement which means we can only construct and run the inputted JS in the arrow function, but if we want to also declare variables, we have to do so in a way that doesn't prevent us from returning values from the arrow function. Fortunately, we can run code where we would usually pass in arguments. Using the new structure we have:

```
( () => ( Function.constructor(inputted JS) ) (declare objects, assign properties) ()
```

We run the arrow function which returns the constructed function, then we execute it.