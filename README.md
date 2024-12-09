# Lua Tutorial

Learning Lua following tutorials by [Teej](https://x.com/teej_dv/status/1863919864953418087/video/1)

## Comments
```lua
-- This is a comment. It starts with two dashes
--[[ This is also
    a comment.

    But it spans multiple lines!
--]]
```

## Variables: Simple Literals
```lua
local number = 5

local string = "hello, world"
local single = 'this works'
local crazy = [[ This
    is multi line and literal ]]

local truth, lies = true, false

local nothing = nil
```

## Variables: Functions

- Example 1

```lua
local greet = function(name)
    print("Hello!", name)
end

local greet = function(name)
    -- .. is string concatenation
    print("Greetings, " .. name .. "!")
end
```

- Example 2

```lua
local higher_order = function(value)
    return function(another)
        return value + another
    end
end

local add_one = higher_order(1)
print("add_one(2) ->", add_one(2))

-- Output
-- add_one(2) -> 3
```

## Variables: Tables

Tables are like a list ...

```lua
local list = { "first", 2, false, function() print("Fourth!") end }
print("Yup, 1-indexed:", list[1])
print("Fourth is 4 ..:", list[4]())
-- Output
-- Yuo, 1-indexed: first
-- Fourth!
-- Fourth is 4 ..:
```

Tables being used as a map ...

```lua
local t = {
    literal_key = "a string",
    ["an expression"] = "also works",
    [function() end] = true
}

print("literal_key    : ", t.literal_key)
print("an expression  : ", t["an expression"])
print("function() end : ", t[function() end])

-- Output
-- literal_key   : a string
-- an expression : also works
-- function() end: nil
```

## Control Flow: `for`

```lua
local favorite_accounts = { "msakiart", "teej_dv", "ThePrimeagen" }
for index = 1, #favorite_accounts do
    print(index, favorite_accounts[index])
end

for index, value in ipairs(favorite_accounts) do
    print(index, value)
end
-- Output
-- 1 msakiart
-- 2 teej_dv
-- 3 ThePrimeagen
```

> NOTE: Length does not work over maps

```lua
local reading_scores = { msakiart = 10, teej_dv = "N/A" }
for index = 1, #reading_scores do
    print(reading_scores[index])
end
-- This Doesn't print anything - the "length" of the array is 0.
-- We aren't using it as an array, we're using it as a map!
```

- Use `pairs` to index inside hash tables

```lua
local reading_scores = { msakiart = 9.5, teej_dv = "N/A" }
for key, value in pairs(reading_scores) do
    print(key, value)
end
-- Output
-- msakiart 9.5
-- teej_dv N/A
```

## Control Flow: `if`

```lua
local function action(loves_coffee)
    if loves_coffee then
        print("Check out `sh terminal.shop` - it's cool")
    else
        print("Check out `sh terminal.shop` - it's still cool")
    end
end

-- "falsey": nil, false
action() -- same as: action(nil)
action(false)

-- Everything else is "truthy"
action(true)
action(0)
action({})

-- Output
-- Check out `sh terminal.shop` - it's still cool
-- Check out `sh terminal.shop` - it's still cool
-- Check out `sh terminal.shop` - it's cool
-- Check out `sh terminal.shop` - it's cool
-- Check out `sh terminal.shop` - it's cool
```

## Modules

Modules are just files. Nothing special about modules.

```lua
-- foo.lua
local M = {}
M.cool_function = function() end
return M
```

```lua
-- bar.lua
local foo = require("foo")
foo.cool_function()
```

## Function: Multiple Returns

```lua
local returns_four_variables = function()
    return 1, 2, 3, 4
end

first, second, last = returns_four_variables()

print("first: ", first)
print("second: ", second)
print("last: ", last)
-- the `4` is discarded :'(

-- Output
-- first: 1
-- second: 2
-- last: 3
```

- Value spreading ...
    > Some values can get lost when they are unpacked into other functions

```lua
local variable_arguments = function( ... )
    local arguments = { ... }
    for i, v in ipairs(arguments) do print(i,v) end
    return unpack(arguments)
end

print("===============")
print("1:", variable_arguments("hello", "world", "!"))
print("===============")
print("2:", variable_arguments("hello", "world", "!"), "<lost>")

-- Output
-- ================
-- 1: Hello
-- 2: world
-- 3: !
-- 1: Hello world !
-- ================
-- 1: Hello
-- 2: world
-- 3: !
-- 2: Hello <lost>
```

## Functions: Calling

String shorthand

> If the function takes a single string we can ignore the parentesis

```lua
local single_string = function(s)
    return s .. " - WOW!"
end

local x = single_string("hi")
local y = single_string "hi"
print(x, y)
-- Output
-- hi - WOW !hi - WOW!
```

Table Shorthand

```lua
local setup = function(opts)
    if opts.default == nil then
        opts.default = 17
    end

    print(opts.default, opts.other)
end

setup { default = 12, other = false }
setup { other = true }
-- Output
-- 12 false
-- 17 true
```

## Functions: Colon Functions

When we define a function on a table we can use the colon syntax:

```lua
local MyTable = {}

-- Both these functions describe the same thing, creating a method in a table
function MyTable.something(self, ... ) end
function MyTable:something( ... ) end
```

## Metatables

Can we add two tables in lua? ...

> If you want to add two tables you have to implement a `__add` method on the table

```lua
local vector_mt = {}
vector_mt.__add = function(left, right)
    return setmetatable({
        left[1] + right[1],
        left[2] + right[2],
        left[3] + right[3],
    }, vector_mt)
end

local v1 = setmetatable({ 3, 1, 5 }, vector_mt)
local v2 = setmetatable({ -3, 2, 2 }, vector_mt)
local v3 = v1 + v2
vim.print(v3[1], v3[2], v3[3])
vim.print(v3 + v3)
-- Output
-- 0 3 7
-- 0 6 14, <metatable>
```

Implement indexing inside a table ( metatables )

```lua
local fib_mt = {
    __index = function(self, key)
        if key < 2 then return 1 end
        -- Update the table, to save intermediate results ( caching )
        self[key] = self[key - 2] + self[key - 1]
        -- Return the result
        return self[key]
    end
}

local fib = setmetatable({}, fib_mt)

print(fib[5])
print(fib[1000])
-- Output
-- 7
-- ~ 2 ** 208
```
