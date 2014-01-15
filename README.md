# Chariot (beta)

Embedded Lua HTML Templates.

This version is heavily modified by nikkiii to make template declarations shorter. Because of our use of { and }, you WILL need to escape any embedded css/javascript braces with a backslash.

## Features

- Embed Lua code with `{ code }`
- Escapes html by default with `{{code}}`
- Unescaped buffering with `{- code -}`
- Import templates with `{( "template name" )}`
- Static caching of intermediate Lua code
- Customizable open and close tags
- Customizable base template folder
- Customizable default extension
- Includes

## Usage

```lua
  local chariot = require('chariot')
  local c = chariot:new()

  -- 2 arguments:
  -- load template from file
  -- filename is used as cache key
  c.render(filename, model)
  -- => string

  -- 3 arguments:
  -- supply template string
  -- a filename is still needed for caching and including
  c.render(template, filename, model)
  -- => string
```

## Options

```lua
  local chariot = require('chariot')
  local c = chariot:new()

  -- cache templates
  c.cache = true
  -- print generated functions
  c.debug = false
  -- template open tag
  c.open = '{'
  -- template close tag
  c.close = '}'
  -- base template folder
  c.base = ''
  -- default extension
  c.extension = 'html'
```

## Includes

Includes are relative to the calling template file. Included files also benefit
from caching.

```
{( "header" )}
{ if self.user then }
  <h1>{{self.user}}</h1>
{ end }
{( "mobile/footer" )}
```

## Example

```lua
local chariot = require('chariot')
local c = chariot:new()

local template = [[
{ if self.user then }
  <h1>{{ self.user }}</h1>
  {- self.code -}
{ end }
]]

print(c:render('index.html', template, {
  user = '<strong>Breakfast</strong>',
  code = '<script>alert("boo")</script>'
}))
```

outputs:

```html
<h1>&lt;strong&gt;Breakfast&lt;&#x2F;strong&gt;</h1>
<script>alert("boo")</script>
```

## License

(The MIT License)

Copyright (c) 2013 Chomping Pixels &lt;breakfast@chompingpixels.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
