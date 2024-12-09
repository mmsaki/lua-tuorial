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

