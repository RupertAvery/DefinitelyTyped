# DateJS Definitions Usage Notes

**DISCLAIMER**: I am not affiliated in any way with the authors of DateJS and I did not contribute to the original source code and as such the naming conventions and abstractions presented are in no way 'official' in any sense and may be subject to change. If you find the naming conventions incorrect please feel free to suggest an alternative.

**NOTE:** Please make sure you have the latest revision of DateJS from the [DateJS repository build branch](https://code.google.com/p/datejs/source/browse/trunk/#trunk%2Fbuild) in order to use certain features on the SugarPak extension. 

A more updated fork exists at <https://github.com/abritinthebay/datejs/> which fixes some issues such as a [maximum stack size error](https://code.google.com/p/datejs/issues/detail?id=143)

## Referencing DateJS definition files in your code

Add the following line to your code to make the base DateJS definitions [as defined in the the API documentation](https://code.google.com/p/datejs/wiki/APIDocumentation) available.

`/// <reference path="path/to/datejs.d.ts" />`

In order to use the SugarPak syntactic sugar extention definitions, add this line:

`/// <reference path="path/to/sugarpak.d.ts" />`

This will let you do things such as:

`(1).days().ago()`

and 

`Date.today().next().year().is().friday()`

all with intellisense support in your IDE.

## Using the DateJS types

Since DateJS extends the inherent base *Date* type we cannot cast the static Date class as a *DateJS*. Instead, declare a global var and type it:

`var DateJS: IDateJSStatic = <any>Date;`

## Notes on SugarPak Type Definitions

The SugarPak API does not create or return objects that can be typed, (i.e. instances of objects that explicitly contain the functions), but rather sets the internal state on a DateJS instance and returns itself (`this`) and relies on subsequent functions to check on the current internal state.

The type definitions provided expose this state as various interfaces and effectively limit what API calls are 'available' after a state-setting function so that nonsensical function calls can be avoided.

For instance:

`Date.today().add(2).monday()`

Although this is 'valid' in DateJS and will not throw an error, it does not actually move the current date to the second following monday, instead `monday()` resets the state and simply moves the current date to the next Monday.

The type definitions prevent this from occurring by typing the result of `add(n)` to an interface that exposes a specific set of functions that make sense in the current context. 
