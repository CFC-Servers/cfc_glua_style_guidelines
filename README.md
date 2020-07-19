# CFC Glua Style Guidelines

The official CFC approved Glua styling guidelines as used on most of our repositories.

##  Spacing
- Spaces around operators
  ```lua
  -- Good
  local x = a * b + c

  -- Bad
  local x = a* b+c
  ```
- Spaces inside parentheses and curly braces if they contain content
  ```lua
  -- Good
  local x = ( 3 * myFunc() ) + 5
  local data = { 5, {} }

  -- Bad
  local x = (3 * myFunc( )) + 5
  local data = {5, { }}
  ```
- Spaces after commas
  ```lua
  -- Good
  myFunc( 10, { 3, 5 } )
  
  -- Bad
  myFunc( 10,{ 3,5 } )
  ```
- Indentation should be done with 4 spaces
  ```lua
  -- Good
  if cond then
    myFunc()
  end

  -- Bad
  if cond then
  	myFunc()
  end
  ```
- No spaces inside square brackets
   ```lua
   -- Good
  local val = tab[5] + tab[3]
  local val2 = tab[5 * 3]

  -- Bad
  local val = tab[ 5 ] + tab[ 3 ]
  local val2 = tab[ 5 * 3 ]
  ```
- Single space after comment operators and before if not at start of line
  ```lua
  -- Good
  -- This is a good comment
  local a = 3 -- This is also good
  
  -- Bad
  --This comment is too close to the operator
  local a = 3-- This comment starts too close to the 3
  ```

## Newlines
- Never have more than 2 newlines
  ```lua
  -- Good
  local config = GM.Config


  function GM:Think()
    -- do thing
  end

  -- Bad
  local config = GM.Config



  function GM:Think()
    -- do thing
  end
  ```
- Top level blocks should have either 1 or 2 newlines between them
  ```lua
  -- Good
  local config = GM.Config


  function GM:Think()
    -- do thing
  end

  -- Bad
  local config = GM.Config
  function GM:Think()
    -- do thing
  end
  ```
- Non top level blocks/lines should never have more than 1 newline between them
  ```lua
  -- Good
  function test()
    local a = 3

	print( a )
  end

  -- Bad
  function test()
    local a = 3


	print( a )
  end
  ```
- Returns should have 1 newline before them unless the codeblock is only 1 line
  ```lua
  -- Good
  function test()
    local a = 3

	return a
  end

  function test2()
    return 3
  end

  -- Bad
  function test()
    local a = 3
	return a
  end
  ```
- Code should be split into managable chunks using a single new line
  ```lua
  -- Good
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

  -- Bad
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

## Gmod lua additions
- Operators added by gmod such as `&&` `||` `!` `!=` should never be used
  ```lua
  -- Good
  if a ~= b and not b then end
  
  -- Bad
  if a != b && !b then end
  ```
- GMod style comments should never be used `/* */` and `//`
  ```lua
  -- Good
  --[[
    This line does stuff
  ]]
  do( stuff ) -- Stuff being done

  -- Bad
  /*
    This line does stuff
  */
  do( stuff ) // Stuff being done
  ```
- The use of continue should be avoided if possible
  ```lua
  -- Good
  for k, v in pairs( tab ) do
    if IsValid( v ) then
      v:Remove()
    end
  end

  -- Bad
  for k, v in pairs( tab ) do
    if not IsValid( v ) then continue end
    v:Remove()
  end
  ```

## Naming conventions
- Local variables and functions should always camelCase
  ```lua
  -- Good
  local myVariable = 10

  -- Bad
  local MyVariable = 10
  local my_variable = 20
  ```
- Constants should be SCREAMING_SNAKE
  ```lua
  -- Good
  CATEGORY_NAME = "nothing"
  
  -- Bad
  CategoryName = "nothing"
  categoryName = "nothing"
  category_name = "nothing"
  ```
- Global variables should be written in PascalCase
  ```lua
  -- Good
  GlobalVariable = 10

  -- Bad
  globalVariable = 20
  global_variable = 20
  ```
- Methods for objects should be in PascalCase e.g. the Player and Entity classes
  ```lua
  -- Good
  function classTable:SetHealth( amount )
  end

  -- Bad
  function classTable:setHealth( amount )
  end
  ```
- Where possible table keys should only contain a-z A-Z 0-9 and \_.  they should not start with 0-9.
  ```lua
  -- Good
  myTable.myValue = 4

  -- Bad
  myTable["my-value"] = 4
  ```
- Only use `_` as a variable to "throwaway" values that will not be used
  ```lua
  -- Good
  for _, ply in pairs( player.GetAll() ) do
    local _, shouldKill = ply:GetKillData()
    if shouldKill then
      ply:Kill()
    end
  end

  -- Bad
  for _, ply in pairs( player.GetAll() ) do
    local canKill, shouldKill = ply:GetKillData()
    if shouldKill then
      ply:Kill()
    end
  end
  ```
- Hook naming:
  - Hook identifiers should be named as such `Organization_AddonName_HookPurpose`
  - The hook event name should not be included in the identifier
  - Hook event names should be named as such `Organization_EventName`

## general
- When picking between `"`  and `'` you must be consistent across the entire project
  Good:
  ```lua
  -- Good
  myFunc( "hello ", "world!" )

  -- Bad
  myFunc( "hello ", 'world!' )
  ```
- Do not use redundant parentheses  
  ```lua
  -- Good
  if x == y then

  -- Bad
  if (x == y) then
  ```

- When writing a multiline table, elements should begin on the next line and the last line should contain only a closing bracket. Elements inside should be indented once.
  ```lua
  tbl = {
      key = x,
      key2 = y
  }
  ```
- Multi line function calls should be written similarly
  ```lua
  myFunc(
    "First arg",
    secondArg,
    { third, arg }
  )
  ```
- Returning early from a function is encouraged to avoid unnecessary scope
  ```lua
  -- Good
  function test()
    if not myThing then return end

    -- Do a bunch of real complicated things
  end

  -- Bad
  function test()
    if myThing then
      -- Do a bunch of real complicated things
    end
  end
  ```
- Magic numbers should be pulled out into meaningful variables
  ```lua
  -- Good
  local maxX = 25

  function checkX( x )
    return x > maxX
  end

  -- Bad
  function checkX( x )
    return x > 25
  end
  ```
- Complex expressions should be written on multiple lines with meaningful variable names
  ```lua
  -- Good
  local gridX = idx % itemsPerColumn
  local offset = ( outerWidth - innerWidth ) * 0.5
  local panelX = x + offset + gridX * gridSize

  -- Bad
  local panelX = x + ( outerWidth - innerWidth ) * 0.5 + ( idx % itemsPerColumn ) * gridSize
  ```
- Never use semicolons
  ```lua
  -- Good
  local a = 3
  print( a )

  -- Bad
  local a = 3; print( a )
  ```
- Don't redefine constants that already exist
  ```lua
  -- Good
  x = y * math.pi
  radians = math.rad( deg )

  -- Bad
  x = y * 3.142
  radians = deg * ( 3.142 / 180 )
  ```
- Unnecessarilly long conditions should be avoided. Conditions can be pulled out into meaningful variable names to avoid this.
  ```lua
  -- Good
  local entValid = Isvalid( ent )
  local entOwner = ent:GetCPPIOwner()
  local ownerVehicleIsVisible = entOwner:GetVehicle():GetColor().a > 0
  local ownerCapable = entOwner:IsAdmin() or entOwner.canDoThing
  
  if entValid and ownerVehicleIsVisible and ownerCapable then
    -- doThing
  end

  -- Bad
  if IsValid( ent ) and ent:GetCPPIOwner():GetVehicle():GetColor().a > 0
    and ( ent:GetCPPIOwner():IsAdmin() or ent:GetCPPIOwner().canDoThing ) then
    -- do thing
  end
  ```
- Lines should be around 110 characters long at the most
  ```lua
  -- Good
  if IsValid( ent ) and ent:IsPlayer() and ent:IsAdmin() then
    local hue = ( CurTime() * 10 ) % 360
    local color = HSVToColor( hue, 1, 1 ) )
    ent:SetColor( color )
  end

  -- Bad
  if IsValid( ent ) and ent:IsPlayer() and ent:IsAdmin() then ent:SetColor( HSVToColor( ( CurTime() * 10 ) % 360, 1, 1 ) ) ) end
  ```

## Numbers
- Don't define numbers like this `.69` instead do `0.69`
- No leading 0s dont do `0420` instead do `420`
- No trailing 0s dont do `0.200` instead do `0.2`

##  Commenting
- Do not add useless comments, good variable and function names can make comments unecessary. Strive for self commenting code.  
  ```lua
  -- Good
  for _, ply in pairs( players )
      print( ply )
      if ply:Alive() then ply:Kill() end
  end

  -- Bad
  for _, v in pairs( stuff ) -- loop through players
      print( v ) -- print the player
      if v:Alive() then v:Kill() end -- kill player if player is alive
  end
  ```
