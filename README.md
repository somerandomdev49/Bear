<h1 align="center"> Bear </h1>
<p align="center">Bear Language Specification</p>

A Functional/(?) language. Everything is based around "transformation" of data.

A basic program: Add `1` to each element of a list.
(In code: `.` - `this` or `current` or whatever)
```fsharp
.. -> theList -> map { [1, .] -> @Add } -> ..
```
Maps `theList` to sum of a list of 1 and current element (eg. `[[1, 2], [3, 1]] -> [3, 4]`) `@Add` adds first two elements of a list in the input. In general, `@` `Operator` does something on first two elements on a list.

Example code:
```fsharp

//============[ lisp example ]============//

import Symbol // For Symbol and SymbolContext

const myContext SymbolContext

parse -> trim -> [head, .] -> & {
  	.0 -> ( [., '('] -> equals ) = [parseInside, .] -> match .1 {
		')' -> .0
		 _  -> throw "Expected ')'"
	};

  	.0 -> isalpha = ${
		until .
		{ not (head -> isalpha) }
		{ <$ head |> tail }
	} -> accumulate -> SymbolIn myContext;

	.0 -> [([., '-'] -> equal), isdigit] -> or = ${
		until .
		{ not (head -> isdigit) }
		{ <$ head |> tail  }
	} -> accumulate -> number;

   	true = throw (["Unexpected character: '", ., "'."] -> @Concat);
};

exec -> & {
  typeof List = (.0 -> exec -> cast Func) .1:
  typeof Symbol = myContext;
  typeof Number = .;
  true = throw (["Unknown type: '", ., "'."] -> @Concat);
}

lisp -> void -> parse -> exec;

//============[ Hello World ]============//
hello -> void -> "Hello, World!" -> stdout;

//============[ Calculator ]============//
// use match same as here, because I cant change the lisp one.
math -> [.0, .1, .2] -> & { tail -> true = @(.0 -> operator) };
// or with syntax sugar:
math -> [.1, .2] |> @(.0 -> operator)


main -> math;
```
`a |> b = a -> & { true = b }`


Implementation?
for example: `TransparentArrow : Arrow { void Move(Obj prev, Obj next) { next.Input = prev; next.Exec(prev.Previous); } }`
and `BasicArrow : Arrow { void Move(Obj prev, Obj next) { next.Input = prev; next.Exec(prev); } }`



