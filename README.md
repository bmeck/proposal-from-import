# from ... import ...;

** Champion **: Bradley Farias (@bmeck)

** Stage **: 0

Code completion is painful for the current order of `import` and `from`.
This proposal seeks to add the inverse to make code completion work.

```mjs
from "foo" import {bar};
```

## Explanation

### Status Quo

```mjs
import /*code completion here*/
```

Could be a string for the module specier, or the list of imported binding.

* Tools cannot statically determine the list of possible bindings without the specifier, but it hasn't been added to the code yet.
* Tools can create a good completion by inserting the specifier, but doing so means a programmer needs to reposition the cursor to insert binding names which is the common case of imports.

```mjs
import "foo"/*code completion/programmer moves caret here after typing*/
      ^ code completion caret needs to be here for binding names
```

### Proposed

```mjs
from /*code completion here*/
```

Module specifiers are the only thing that could be here to allow better code completion.

### Imports without bindings

```mjs
import "a";
```

Just for posterity we can add the `from` prefixed form, even though it is more verbose:

```mjs
from "a" import;
```

## Working with other proposals

### Module attributes

The position of module attributes would still be at the end.

```mjs
from "./foo.json" import foo with type="json"
```

## Concerns

* We would be growing the grammar without any new semantics added to the language.
* We would need to have different `[no LineTerminator here]` rules in this form of import.

```mjs
// allowed currently
import
	* as foo
from
  'foo';
```

Due to `from` needing to be a contextual keyword it needs to avoid problems with ASI and would have a grammar like:

```mjs
from [no LineTerminator here] ModuleSpecifier import ImportClause?
```
