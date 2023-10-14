# FNF Modcharting Tools
Just a thing I made to make modcharting easier, should be easy to add to most engines.
Still very WIP and not everything is supported yet!

## Features
### Modifier system for easing in and out effects
![](https://github.com/TheZoroForce240/FNF-Modcharting-Tools/blob/main/readme/modifiers.gif)
### Custom Sustain Renderer (using FlxStrip for stretchy sustains)
### Multiple playfields that can have their own positions and modifiers
![](https://github.com/TheZoroForce240/FNF-Modcharting-Tools/blob/main/readme/sustains.gif)
### Custom Modifier Support via Hscript
![](https://github.com/TheZoroForce240/FNF-Modcharting-Tools/blob/main/readme/custommods.gif)
### Support for multiple engines
- Base Game (not tested)
- [Psych Engine](https://github.com/ShadowMario/FNF-PsychEngine) (working 0.6.3 and 0.7.x, includes lua functions)
- [Leather Engine](https://github.com/Leather128/LeatherEngine) (working 0.4.2)
- [Kade Engine](https://github.com/KadeDev/Kade-Engine) (not tested)
- [Yoshi Engine](https://github.com/YoshiCrafter29/YoshiCrafterEngine) (not tested)
- [Forever Engine Legacy](https://github.com/Yoshubs/Forever-Engine-Legacy) (not tested)
- [FPS Plus](https://github.com/ThatRozebudDude/FPS-Plus-Public) (not tested)


## Credits 
- [Original FNF Team](https://github.com/ninjamuffin99/Funkin) - They made the game
- [NotITG](https://www.noti.tg/) - Inspiration (made me love modcharts lol)
- [OpenITG](https://github.com/openitg/openitg) - Math used for some modifiers
- [TheZoroForce240](https://github.com/TheZoroForce240/FNF-Modcharting-Tools) - Creator of modcharting tools base
- [Vortex2Oblivion](https://github.com/Vortex2Oblivion) - Helper from modcharting tools
- [Manu614](https://github.com/Manu614) - Helper from modcharting tools
- [UncertainProd](https://github.com/UncertainProd) - Helper from modcharting tools
- [Joalor64GH](https://github.com/Joalor64GH) - Helper from modcharting tools
- [Edwhak_KB](https://github.com/EdwhakKB) - Added some modifiers and fixed stuff + skewNotes
- [Glowsoony](https://github.com/glowsoony) - Help with some 0.7.X stuff! + skewNotes too
- [Slushi_Github](https://github.com/Slushi-Github) - Help with reorganisation with haxelib edition

## Installation 
You need the most recent version of HaxeFlixel for it to work. (5.2.1 as of writing)
### With Source:
1. Install the haxelib by typing `haxelib git fnf-modcharting-tools https://github.com/EdwhakKB/FNF-Modcharting-Tools` in the console
2. Now you only need to make a few small additions to get everything working,
- In MusicBeatState.hx:
```haxe

class MusicBeatState extends modcharting.ModchartMusicBeatState
{
  
```
- In PlayState.hx:
```haxe
import modcharting.ModchartFuncs;
import modcharting.NoteMovement;
import modcharting.PlayfieldRenderer;
  
```
```haxe

override public function create()
{

  //Add this before camfollow stuff and after strumLineNotes and notes have been made
  playfieldRenderer = new PlayfieldRenderer(strumLineNotes, notes, this);
  playfieldRenderer.cameras = [camHUD];
  add(playfieldRenderer);
  add(grpNoteSplashes); /*place splashes in front (add this if the engine has splashes).
  If you have added this: remove(or something) the add(grpNoteSplashes); which is by default below the add(strumLineNotes);*/

  //if you use PSYCH 0.6.3 use this code
  ModchartFuncs.loadLuaFunctions(this); //add this if you want lua functions in scripts
  //being used in psych engine as an example

callOnLuas('onCreatePost', []);
      
```

- If Psych and it's 0.7.0 in FunkinLua.hx, if Leather then ModchartUtilities.hx:

```haxe
// at the start of the HX
import modcharting.ModchartFuncs; //to fix any crash lmao
// (at the bottom of create())
#if desktop DiscordClient.addLuaCallbacks(this); #end
ModchartFuncs.loadLuaFunctions(this); //add this if you want lua functions in scripts
//being used in psych engine as an example

callOnLuas('onCreate', []);

```
```haxe

public function startCountdown():Void
{
  generateStaticArrows(0);
  generateStaticArrows(1);
  
  //add after generating strums
  NoteMovement.getDefaultStrumPos(this);

```

- In StrumNote.hx:
```haxe
//Import FlxSkewedSprite at the top
import flixel.addons.effects.FlxSkewedSprite;

//change "FlxSprite" to "FlxSkewedSprite"
class StrumNote extends FlxSkewedSprite

```

- In Note.hx:
```haxe
//Import FlxSkewedSprite at the top
import flixel.addons.effects.FlxSkewedSprite;

//change "FlxSprite" to "FlxSkewedSprite"
class Note extends FlxSkewedSprite
{
  //add these 2 variables for the renderer
  public var mesh:modcharting.SustainStrip = null;
  public var z:Float = 0;

```

- In FunkinLua.hx (0.7.X exclusive!):
```haxe

class FunkinLua
{
  //add this variable bellow "public var closed:Bool = false;"
  	public static var instance:FunkinLua = null;

```

- In Conductor.hx (0.7.X exclusive!):
```haxe

class Conductor
{
  //add this variable at the end of the code (but inside the class)
  	public static function changeBPM(newBpm:Float)
	  {
		bpm = newBpm;

		crochet = calculateCrochet(bpm);
		stepCrochet = crochet / 4;
	  }

```
- In Project.xml:
```xml
<!--Set this to the engine you're using!-->
<define name="PSYCH" />

<haxelib name="fnf-modcharting-tools" />

```
You need to define which engine you're using to fix compiling issues, or it would default to base game settings (downscroll won't work etc).
Available ones: PSYCH, KADE(notTested), LEATHER, FOREVER_LEGACY(notTested), YOSHI(notTested), FPSPLUS(notTested)

Note: If you use psych engine you should add this

```xml

<define name="PSYCHVERSION" value="0.7"/>

```

to get 0.7+ and up
leave it as another value to use 0.6.3 edition 


3. Now if your game compiles successfully then you should be all good to go.

