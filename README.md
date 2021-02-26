# timer.nvim

### Motivation

I want to see diagnostics errors in normal mode, when I stay on a line for some time. I also want to see func signature
while typing right away when typing.

So I set `CursorHold` and `CursorHoldI` autocommands to view helps.

But wait, they both being triggered within `updatetime` timeout!

Not anymore...

### Install

- packer.nvim

```
use 'alex-popov-tech/timer.nvim'
```

### Usage

Plugin is pretty minimalistic:

```
require "timer".add( -- require function and call it
    function() -- pass your function
        local next = math.random(1000, 3000)
        print("hello, next tick in " .. next .. "ms...")
        return next -- return next 'tick' time to wait
    end
)
```

<div align='center'>
<img src="https://user-images.githubusercontent.com/21224705/109360007-1e5d0080-788f-11eb-9562-170135c328d3.gif" />
</div>

So all you need to do is to pass your function and return a number of milliseconds to wait before your func will be
called again.

### Real world examples

Those are from my [dotfiles](https://github.com/alex-popov-tech/.dotfiles/blob/master/home/.config/nvim/lua/lsp/on_attach.lua)

- show signature help in insert mode when available
  ```
  require "timer".add(
      function()
          if vim.fn.mode() == "i" then
              require("lspsaga.signaturehelp").signature_help()
          end
          return 2000
      end
  )
  ```
  <div align='center'>
  <img src="https://user-images.githubusercontent.com/21224705/109360125-549a8000-788f-11eb-9d57-df41e38e6fae.gif" />
  </div>

- show line diagnostic in normal mode
  ```
    require "timer".add(
      function()
          if vim.fn.mode() == "n" then
              require "lspsaga.diagnostic".show_line_diagnostics()
          end
          return 2000
      end
    )
  ```
  <div align='center'>
  <img src="https://user-images.githubusercontent.com/21224705/109359897-e9e94480-788e-11eb-993d-3f158b1d2062.gif" />
  </div>

### Advanced examples

You can use clojures to achieve more dynamics:
```
local tick = true -- control flag
require "timer".add(
    function()
        if tick == true then
            print("tick")
        else
            print("tack")
        end
        tick = not tick -- update control flag on each tick
        return 1000
    end
)
```

<div align='center'>
<img src="https://user-images.githubusercontent.com/21224705/109360168-65e38c80-788f-11eb-9fbe-70eb85f12e23.gif" />
</div>

And you even can use that to control frequency of 'ticks':
```
local count = 1 -- keep counter outside of tick
require "timer".add(
    function()
        local currentTimer = count
        count = count + 1 -- update it each tick
        print("next tick in " .. currentTimer .. "sec")
        return currentTimer * 1000
    end
)
```

<div align='center'>
<img src="https://user-images.githubusercontent.com/21224705/109360658-4436d500-7890-11eb-9c2f-174738d80fe2.gif" />
</div>

# License

MIT
