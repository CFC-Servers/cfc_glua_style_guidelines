# CFC Glua Style Guidelines

The official CFC approved Glua styling guidelines as used on most of our repositories.

#  Spacing
## Spaces around operators

  **Good**
  ```lua
  local x = a * b + c
  ```

  **Bad**
  ```lua
  local x = a* b+c
  ```
## Spaces inside parentheses and curly braces if they contain content

  **Good**
  ```lua
  local x = ( 3 * myFunc() ) + 5
  local data = { 5, {} }
  ```

  **Bad**
  ```lua
  local x = (3 * myFunc( )) + 5
  local data = {5, { }}
  ```
## Spaces after commas

  **Good**
  ```lua
  myFunc( 10, { 3, 5 } )
  ```

  **Bad**
  ```lua
  myFunc( 10,{ 3,5 } )
  ```
## Indentation should be done with 4 spaces

  **Good**
  ```lua
  if cond then
      myFunc()
  end
  ```

  **Bad**
  ```lua
  if cond then
    myFunc()
  end
  ```
## No spaces inside square brackets

  **Good**
  ```lua
  local val = tab[5] + tab[3]
  local val2 = tab[5 * 3]
  ```

  **Bad**
  ```lua
  local val = tab[ 5 ] + tab[ 3 ]
  local val2 = tab[ 5 * 3 ]
  ```
## Single space after comment operators and before if not at start of line

  **Good**
  ```lua
  -- This is a good comment
  local a = 3 -- This is also good
  ```

  **Bad**
  ```lua
  --This comment doesn't have a space before it
  local a = 3-- This comment starts too close to the 3
  ```

# Newlines
## Never have more than 2 newlines

  **Good**
  ```lua
  local config = GM.Config


  function GM:Think()
      -- do thing
  end
  ```

  **Bad**
  ```lua
  local config = GM.Config



  function GM:Think()
      -- do thing
  end
  ```
## Top level blocks should have either 1 or 2 newlines between them

  **Good**
  ```lua
  local config = GM.Config


  function GM:Think()
      -- do thing
  end
  ```

  **Bad**
  ```lua
  local config = GM.Config
  function GM:Think()
      -- do thing
  end
  ```
## Non top level blocks/lines should never have more than 1 newline between them

  **Good**
  ```lua
  function test()
      local a = 3

      print( a )
  end
  ```

  **Bad**
  ```lua
  function test()
      local a = 3


      print( a )
  end
  ```
## Returns should have one newline before them unless the codeblock is only one line

  **Good**
  ```lua
  function test()
      local a = 3

      return a
  end

  function test2()
      return 3
  end
  ```

  **Bad**
  ```lua
  function test()
      local a = 3
      return a
  end
  ```
## Code should be split into managable chunks using a single new line

  **Good**
  ```lua
  function CFCNotifications.resolveFilter( filter )
      if type( filter ) == "Player" then
          filter = { filter }
      end

      if type( filter ) == "table" then
          filter = fWrap( filter )
      end

      filter = filter or player.GetAll
      local players = filter()

      if type( players ) == "Player" then
          players = { players }
      end

      if not players then players = player.GetAll() end

      return players
  end
  ```

  **Bad, far too dense and difficult to read at a glance**
  ```lua
  function CFCNotifications.resolveFilter( filter )
      if type( filter ) == "Player" then
          filter = { filter }
      end
      if type( filter ) == "table" then
          filter = fWrap( filter )
      end
      filter = filter or player.GetAll
      local players = filter()
      if type( players ) == "Player" then
          players = { players }
      end
      if not players then players = player.GetAll() end
      return players
  end
  ```

# Gmod lua additions
## Do not use GMod operators ( `&&` `||` `!` `!=` )

  **Good**
  ```lua
  if a ~= b and not b then end
  ```

  **Bad**
  ```lua
  if a != b && !b then end
  ```
## Do not use GMod style comments ( `/* */` and `//` )

  **Good**
  ```lua
  --[[
    This line does stuff
  ]]
  do( stuff ) -- Stuff being done
  ```

  **Bad, gmod comments aren't recognized by standard lua highlighting**
  ```lua
  /*
    This line does stuff
  */
  do( stuff ) // Stuff being done
  ```
## The use of continue should be avoided if possible

  **Good**
  ```lua
  for k, v in pairs( tab ) do
      if IsValid( v ) then
          v:Remove()
      end
  end
  ```

  **Bad, garry's `continue` is a poor implementation that /might/ be prone to errors**
  ```lua
  for k, v in pairs( tab ) do
      if not IsValid( v ) then continue end
      v:Remove()
  end
  ```

# Naming conventions
## Local variables and functions should always camelCase

  **Good**
  ```lua
  local myVariable = 10
  ```

  **Bad**
  ```lua
  local MyVariable = 10
  local my_variable = 20
  ```
## Constants should be SCREAMING_SNAKE

  **Good**
  ```lua
  CATEGORY_NAME = "nothing"
  local MAXIMUM_VALUE = 25
  ```

  **Bad**
  ```lua
  CategoryName = "nothing"
  categoryName = "nothing"
  category_name = "nothing"
  local maximumValue = 25
  ```
## Global variables should be written in PascalCase

  **Good**
  ```lua
  GlobalVariable = 10
  ```

  **Bad**
  ```lua
  globalVariable = 20
  global_variable = 20
  ```
## Methods for objects should be in PascalCase

  **Good**
  ```lua
  function classTable:SetHealth( amount )
  end
  ```

  **Bad**
  ```lua
  function classTable:setHealth( amount )
  end
  ```
## Table keys
### Table keys should only contain `a-z A-Z 0-9` and `\_.` they should not start with `0-9`

  **Good**
  ```lua
  myTable.myValue = 4
  ```

  **Bad, accessing the table value requires brackets and quotes because of the hyphen**
  ```lua
  myTable["my-value"] = 4
  ```
## Only use `_` as a variable to "throwaway" values that will not be used

  **Good**
  ```lua
  for _, ply in pairs( player.GetAll() ) do
      local _, shouldKill = ply:GetKillData()

      if shouldKill then
          ply:Kill()
      end
  end
  ```

  **Bad, `k` isn't used**
  ```lua
  for k, ply in pairs( player.GetAll() ) do
      local canKill, shouldKill = ply:GetKillData()

      if shouldKill then
          ply:Kill()
      end
  end
  ```
## Hook naming:
  - Hook identifiers should be named as such `Organization_AddonName_HookPurpose`
  - The hook event name should not be included in the identifier.
    For example, you should not do `ORG_MyAddon_OnDisconnect` or even `ORG_MyAddon_CleanupPropsOnDisconnect`. But`ORG_MyAddon_CleanupProps` would be appropriate. The "HookPurpose" should state what a function does without restating information provided by the event name.
  - Hook event names should be named as such `Organization_EventName`

# General
## Quotations
### You may use single or double quotes, but be consistent

  **Good, quote usage is consistent**
  ```lua
  myFunc( "hello ", "world!" )
  ```

  **Bad, different quotes are used interchangeably**
  ```lua
  myFunc( "hello ", 'world!' )
  ```
## Do not use redundant parentheses

  **Good**
  ```lua
  if x == y then
  ```

  **Bad, these parenthesis are not necessary**
  ```lua
  if (x == y) then
  ```

## Multiline tables
### Elements should begin on the next line and the last line should contain only a closing bracket. Elements inside should be indented once.
  ```lua
  tbl = {
      key = x,
      key2 = y
  }
  ```
## Multi line function calls should be written similarly
  ```lua
  myFunc(
      "First arg",
      secondArg,
      { third, arg }
  )
  ```
## Return early from functions

  **Good**
  ```lua
  function test()
      if not myThing then return end

      -- Do a bunch of real complicated things
  end
  ```

  **Bad, adds an extra level of indentation**
  ```lua
  function test()
      if myThing then
          -- Do a bunch of real complicated things
      end
  end
  ```
## Magic numbers should be pulled out into meaningful variables

  **Good**
  ```lua
  local maxX = 25

  function checkX( x )
      return x > maxX
  end
  ```

  **Bad, the significance of `25` is unknown without a meaningful variable name**
  ```lua
  function checkX( x )
      return x > 25
  end
  ```
## Complex expressions should be written on multiple lines with meaningful variable names

  **Good, each step of the equation is named and done individually. The math is easy to follow**
  ```lua
  local widthModifier = amount * damageMult
  local age = 1 - lifetime / duration
  local width = widthModifier * age
  ```

  **Bad, this math is difficult to figure out from a glance**
  ```lua
  local width = (amount * 5) * (1 - lifetime / duration)
  ```
## Never use semicolons

  **Good**
  ```lua
  local a = 3
  print( a )
  ```

  **Bad**
  ```lua
  local a = 3; print( a )
  ```
## Make use of existing constants where possible

  **Good**
  ```lua
  -- Good
  x = y * math.pi
  radians = math.rad( deg )
  ```

  **Bad**
  ```lua
  x = y * 3.142
  radians = deg * ( 3.142 / 180 )
  ```
## Unnecessarily long conditions should be avoided.
### Conditions can be pulled out into meaningful variable names to avoid this.

  **Good, each check is clear and it's easy to follow the reasoning**
  ```lua
  local entValid = Isvalid( ent )
  local entOwner = ent:GetCPPIOwner()
  local ownerVehicleIsVisible = entOwner:GetVehicle():GetColor().a > 0
  local ownerCapable = entOwner:IsAdmin() or entOwner.canDoThing

  if entValid and ownerVehicleIsVisible and ownerCapable then
      -- doThing
  end
  ```

  **Bad, the conditions are difficult to follow and require some unpacking**
  ```lua
  if IsValid( ent ) and ent:GetCPPIOwner():GetVehicle():GetColor().a > 0
    and ( ent:GetCPPIOwner():IsAdmin() or ent:GetCPPIOwner().canDoThing ) then
      -- do thing
  end
  ```
## Lines should be around 110 characters long at the most

  **Good**
  ```lua
  if IsValid( ent ) and ent:IsPlayer() and ent:IsAdmin() then
      local hue = ( CurTime() * 10 ) % 360
      local color = HSVToColor( hue, 1, 1 ) )

      ent:SetColor( color )
  end
  ```

  **Bad, this line is too long and could easily be split into multiple, easy-to-follow lines**
  ```lua
  if IsValid( ent ) and ent:IsPlayer() and ent:IsAdmin() then ent:SetColor( HSVToColor( ( CurTime() * 10 ) % 360, 1, 1 ) ) ) end
  ```

# Numbers
- Don't define numbers like this `.69` instead do `0.69`
- No leading 0s, dont do `0420` instead do `420`
- No trailing 0s, dont do `0.200` instead do `0.2`

#  Commenting
## Do not add useless comments
### Good variable and function names can make comments unecessary. Strive for self commenting code. Save comments for complicated code that may not be clear on its own.

  **Good**
  ```lua
  for _, ply in pairs( players )
      print( ply )
      if ply:Alive() then ply:Kill() end
  end
  ```

  **Bad, the code explains itself without comments**
  ```lua
  for _, v in pairs( stuff ) -- loop through players
      print( v ) -- print the player
      if v:Alive() then v:Kill() end -- kill player if player is alive
  end
  ```
