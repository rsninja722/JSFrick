# JSFrick
PG version of the JSF*ck compiler made by Martin Kleppe

## goals
- create a compiler that compiles any JS into JS that only uses the following characters: `!$%^&*()_+{}|:<>?-=[]\;,.~`
- keep compiled character count low
- have compiled code run well enough to eventually compile the compiler into JSFrick

## main advantage of JSFrick over JSF*ck

`$` and `_` are valid variable names, creating objects, and setting their properties named after these variables allows for many variables with minimal characters used.

assigning characters and strings to variables for later use greatly reduces the final character count of compiled code.


### progress

```
_._ = +[]; (0)

_.$ = -~[]; (1)

$._ = -~-~[]; (2)

$.$ = -~-~-~[]; (3)

__._ = -~-~-~-~[]; (4)

__.$ = -~-~-~-~-~[]; (5)

_$._ = -~-~-~-~-~-~[]; (6)

_$.$ = -~-~-~-~-~-~-~[]; (7)

$_._ = -~-~-~-~[]<<-~[]; (8)

$_.$ = +([-~[]]+(+[]))+~[]; (9)

$$._ = ![]+[]; (false)

$$.$ = !![]+[]; (true)

_.__ = $._[_.$]; (a) dependencies: false,1

_._$ = $.$[$.$]; (e) dependencies: true,3

_.$_ = $._[_._]; (f) dependencies: false,0

_.$$ = $._[$._]; (l) dependencies: false,2

$.__ = $.$[_.$]; (r) dependencies: true,1

$._$ = $._[$.$]; (s) dependencies: false,3

$.$_ = $.$[_._]; (t) dependencies: true,0

$.$$ = $.$[$._]; (u) dependencies: true,2
```