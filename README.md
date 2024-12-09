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

-- output
-- add_one(2) -> 3
```

