Naming things in code is one of the most critical states in documenting your code. For one thing, you have to do it, unlike all other forms of documentation that are optional :D. For another thing, its a form that is sewn through out your code, unlike block comments and readmes that are centralized and abstracted from your code entirely, respectively. 

This is about naming specifically not code conventions - for consistency I'll use CamelCase, but not because I think its relevant to a good name which way you swing. 

I have a few principles of naming that have done a lot for me over the years.

1. **Pay attention to whether you are naming a verb or a noun.** Properties will of course not have verbal names: `LocalOrigin`, `Voxels`, etc. are good names for properties but bad names for methods -- that is, methods that are truly methods. If a call changes other properties (I.e., have side effects) or are Idempotent (like GetCurrentTime) then it is a proper method; if its just a property accessor as in 

```` javascript

var myObj = {
    _foo = 2,

   Foo(n){ if (arguments.length > 0) { _foo = Number(n); } return this._foo; }

}

````

Foo is in this case