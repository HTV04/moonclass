# moonclass
A simple function wrapper for Moonscript/Yuescript's class system, written in Moonscript.

Moonscript class usage is pretty limited on the Lua side of things, because Moonscript generates all the code that creates a class every time a class is created. To fix this, moonclass takes advantage of the fact that Moonscript classes are just fancy tables. When moonclass is compiled to Lua, it contains all the code that creates and extends classes, and its functions return tables that would be considered classes to Moonscript code.

In theory, once compiled to Lua via moonclass, you can use Moonscript's class system without Moonscript at all ;)

## Usage
```moonscript
class newClass
	new: =>
		print "I'm a new class!"

	foo: 5
	bar: 10

extendedClass = class extends newClass
	foo: 10
	bar: 5
```

is functionally equivalent to:

```lua
moonclass = require "moonclass"

local newClass = moonclass.new(
	"newClass", -- Class name
	function(self) -- Constructor
		return print "I'm a new class!"
 	end,
	{ -- Values
		foo = 5,
		bar = 10
	}
)

local extendedClass = moonclass.extend(
	newClass, -- Parent class
	"extendedClass", -- Class name
	{ -- Values
		foo = 10,
		bar = 5
	}
)
```

All other functionality using classes in Moonscript is pretty easy to work with on the Lua side. Operators like `@variable` and `@@variable` just expand to `self.variable` and `self.__class.variable`, respectively. You can access values from Moonscript class tables like you would any other table, as well.
