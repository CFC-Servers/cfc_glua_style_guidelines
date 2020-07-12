# cfc_glua_style_guidelines

##  Spacing
- spaces around operators
- spaces inside parentheses if they contain content
- spaces after commas
- spaces inside curly braces if they contain content
- indentation should be done with 4 spaces
- no spaces inside square brackets

## Newlines
- never have more than 2 newlines
- top level blocks should have at least 1 newline between them
- returns should have 1 newline before them

## garry bad
- operators added by gmod such as `&&` `||` `!` `!=` should never be used
- gmod style comments should never be used `/* */` and `//`
- the use of continue should be avoided if possible

## naming
- local variables and functions should always camelCase
- constants should be SCREAMING_SNAKE
- global variables should be written in PascalCase
- methods for objects should be in PascalCase e.g. the Player and Entity classes
- where possible table keys should only contain a-z A-Z 0-9 and \_.  they should not start with 0-9.
- only use `_` as a variable to "throwaway" values that will not be used

## general
- when picking between `"`  and `'` you must be consistent across the entire project
- do not use redundant parentheses  
  good:  
  ```lua
  if x == y then
  ```
  bad: 
  ```lua
  if (x == y) then
  ```

- when writing a multiline table, elements should begin on the next line and the last line should contain only a closing bracket. Elements inside should be indented once
  ```lua
  tbl = {
      key = x,
      key2 = y
  }
  ```
- multi line function calls should be written similarly 
- returning early from a function is encouraged to avoid unnecessary scope
- magic numbers should be pulled out into meaningful variables
- complex expressions should be written on multiple lines with meaningful variable names
- never use semicolons
- don't redefine constants that already exist
  bad:  
  ```lua
  x = y * 3.142
  radians = deg * ( 3.142 / 180 )
  ```
  good: 
  ```lua
  x = y * math.pi
  radians = math.rad( deg )
  ```
- unnecessarilly long conditions should be avoided. conditions can be pulled out into meaningful variable names to  avoid this.
- lines should be around 110 characters long at the most

## Numbers
- dont define numbers like this `.69` instead do `0.69`
- no leading 0s dont do `0420` instead do `420`
- no trailing 0s dont do `0.200` instead do `0.2`

# Gmod
## naming hooks
- hook identifiers should be named as such `Organization_AddonName_HookPurpose`
- the hook event name should not be included in the identifier
- hook event names should be named as such `Organization_EventName`

##  commenting
- do not add useless comments, good variable and function names can make comments unecessary. Strive for self commenting code.  
  bad:  
  ```lua
  for k, v in pairs(stuff) -- loop through stuff
      print(v) -- print v
      if k == "u" then return end -- return on u
  end
  ```
  good:
  ```lua
  for k, v in pairs(stuff)
      print(v)
      if k == "u" then return end
  end
  ```
