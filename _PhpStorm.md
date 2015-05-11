###`Detect PSR-0 namespace roots`

Para que PhpStorm pueda configurar correctamente tu proyecto, es recomendable que configuremos los namespaces de `PSR-0`.

Cuando abres un nuevo proyecto Laravel, es posible que PhpStorm automáticamente lo detecte y te muestre el mensaje: `Detect PSR-0 namespace roots`. En ese caso, elige 'configurarlos manualmente'. Y, si no es así, también puedes configurarlos o modificarlos en las opciones: `Settings, Project, Directories`.

Configura los siguientes directorios:

`Tests`:  
- `tests`

`Source Folders`:
- `app`
- `database`
- `config`

`Excluded Folders`:
- `vendor`

`Resource roots`:
- `resources`
- `public\assets`


---------------
Here’s some more gems:

Clicking the closing brace of a block that begins off-screen will display the entire starting line of said block in the top editor gutter. So if you forget the arguments a function consumes, there’s no need to scroll – just select its ending brace and the starting line will appear on the IDE frame.
Open any file with CTRL + Shift + N, or class with CTRL + N, instantly. No scrolling through directory trees, no loading or searching. This is extra handy when you have many files/classes in your project.
Got some messy code from other developers which you can’t look at unless it’s at least in PSR-1? Just run the code auto-format with CTRL+ALT+L on the files, blocks of code, or even entire folders to clean it up according to the coding style du jour.
Any undefined namespaces, redeclared classes, syntax errors and disrespected type hints will glow red. They will be highlighted in the right editor gutter and in the code itself. PHPStorm is your pair programmer – it helps you avoid silly bugs before you even make them!
-------------------
Open a project
Go to Preferences -> Code Style -> PHP -> Set from... -> Predefined Style -> allows you to set your code style guidelines to the styles such as PSR-2 and Zend.
Go to Code -> Reformat Code -> This option will format all of your code to the code style you set. We recently converted our codebase of around 300,000 lines to the PSR-2 standard. It was done in less than 5 minutes.
--------------------
Go to Code -> Reformat Code
Or press ctrl+alt+f if all you want to format is the single file you're working on. Or select a block of text and then press ctrl+alt+f to format just the selection.
--------------------
CTRL + N - Jump to a class
CTRL + SHIFT + N - Jump to a file
Use those all the time.
---------------------
A note on those...
If your class and file names are camel-cased, you can write abbreviations to navigate to it. You can also suffix a line number using : as a separator.
Lets say you have an unhandled exception thrown on line 84 of Acme\ExampleBundle\Form\ShortContactFormType and you need to fix that, you can CTRL-N and write "SCFT:84“ and hit enter.
---------------------
Also you can type file paths into your search for even more accurate searches.
We have a lot of similar named files in our code base and this is a life saver.
enlacepadre
[–]FineWolf 1 punto 1 año atrás 
Namespaces in Find Class... File paths in Find File.
---------------------
Don't forget jump to symbol also - I don't know what the default keys are (i chose eclipse keymap) - I just use Alt + N --> (F file, S symbol, C class)
---------------------
1) Ctrl-Shift-V to access to clipboard ring.
2) Local History of a file can be VERY useful if you break something between VC commits.
3) Alt-Insert brings up the generate menu (to generate getters/setters, and PHPDoc). Setting all of your PHPDoc with proper types will give you much better code insight and warnings about what methods expect.
4) Definitely turn on code inspections on code commit, this will help you find a ton of subtle bugs especially if you keep your PHPDoc up to date.
5) The Right Click Refactor menu (and learning the Extract Method/Extract Variable/Refactor Rename shortcuts) is amazing in its power.
6) Ctrl-A to find the keyboard shortcuts of the hundreds of PHPStorm features.
7) Ctrl-Shift-F Find in files is very powerful and usually very fast (everything gets indexed by PHPStorm).
8) The Regex tester plugin can save a bunch of time (though there are better stand alone tools).
9) Test Restful Web Service is very powerful, though I wish you could use it to debug your own REST services without having to manually setup the Xdebug/ZendDebugger variables.
10) The built in PHPUnit runner (though I wish you could configure the VCS commit to force a run of PHPUnit before allowing a commit).
11) Speaking of which the VCS commit window which allows for things like auto-format/optimize imports/seach for TODO's/run inspections.
12) Live Edit is pretty neat, but more useful if you are developing straight HTML rather than dynamic PHP.
13) The Darcula theme!
There are tons more, but a lot of them have already been mentioned...
------------------------------------
Cmd+click on all the things!
I use Ctrl + B instead
Yup, I changed mine to ctrl + middle click, because I kept hitting it when trying to copy/paste things.
It enables ctrl+click for twig templates and factory pattern ( get('....') ) and lots of other useful things.
http://plugins.jetbrains.com/plugin/?phpStorm&pluginId=7219
--------------------------------------
Composer integration. Setup path to composer.phar in settings and then right click on project -> Composer -> Init / Add package.
---------------------------------------
HPStorm is highly customizable. I recommend spending some time in the Settings tweaking the settings to be just how you like them. You'll simultaneously learn about a bunch of the features.
Middle-clicking function calls, classes and variables goes to the declaration. Middle-click dragging selects in columns, play around with it. Ctrl+P while in the middle of a function call will remind you of the arguments. Ctrl+downarrow (or up) while in autocompletion will break you free of it. PHPStorm error checking and autocompletion is fantastic. Make the most of it by always letting PHPStorm know what class your objects are (with @var).
enlace
[–]polyfractal 1 punto 1 año atrás 
These are awesome, thanks! Ctrl+P is a great find, and I've already used the middle-click drag a dozen times since reading it in this thread.
---------------------------------------------
If you do front-end development, install yourself the CSS-X-Fire addon and add the plugin in your Firefox with Firebug. From there on out every style you change in Firebug can be taken to your CSS/LESS files with a single click.
Also install the addon "key promoter". It will show you the keyboard shortcut everytime you do something with the mouse. Also, if you do the same thing several times during your session it will ask you if you want to add a keyboard shortcut to that function.
If you do HTML/CSS learn to use emmet. Type "table>tr3>td5>a[href="#"]" (without the quotes) into a html file and then hit tab... type "w30" in a css file and then hit tab. Speeds up your development tenfold.
---------------------------------
Live Templates (Zen Coding). In a php file, type forek and then press TAB. For HTML, they implemented emmet: http://docs.emmet.io/
It's damn amazing, and you can create your own live templates.
Another very nice feature is the method generation for getters and setters, as well as for implement / override methods from interfaces or abstract classes. Just press alt+insert in a php file and check it out.
The refactoring features are very awesome too ;)
---------------------------------
If PhpStorm doesn't recognize the class of a variable, you can manually set it with comments.
/** @var $variableName Some_Class_Here */
Better. imho, cause that's how the parameters are described when you comment a method.
/** @var Some_Class_Here $variableName */
enlacepadre
[–]Toast42 2 puntos 1 año atrás 
That's why PHPStorm started defaulting the cursor to the middle; I thought it was a bug. Thanks!
---------------------
http://www.jetbrains.com/phpstorm/webhelp/selecting-text-in-the-editor.html#d230435e441
Once you get used to it, you'll miss it in every other app.
To extend selection from the word at the caret to the piece of code the caret is contained in, use the following shortcuts
Press Ctrl+W to select the word where the caret is currently located.
Press Ctrl+W successively to extend selection to the next containing node (for example, an expression, a paired tag, an entire conditional block, a method body, a class, a group of vararg arguments, etc.)
Note
While extending selection, keep in mind the following:
Pressing Ctrl+W successively in plain text or comments extends the selection first to the current sentence, then to the current paragraph.
Press Ctrl+Shift+W to shrink selection in the reverse order (from the outermost container to the word where the caret currently resides).
----------------------------
Small grievance: IMO, by default, they should map Control-W to the Window->Editor Tabs->Close function, and not the Word Selection function (which is indeed pretty freaking awesome). For a lot of the software I use, especially in my work as a web dev, I'm used to Ctrl-W being the command to close shit. For example: Firefox, Chrome, IE, Safari, Fireworks, Notepad++, Thunderbird, Microsoft Word & Excel... just to name a few, they all use Ctrl-W to close windows/tabs.
Small complaint though, all that stuff is easily personalized under Settings->Keymap.
enlacepadre
[–]Klathmon 3 puntos 1 año atrás 
Going right along with this, is it just me that DESPISES that CTRL-Y deletes a line instead of redoing?
It's second nature for me to sometime go back and forward like 10-15 times and if i blank and hit ctrl-Y it fucks everything up...
---------------------------------
Press TAB will auto complete the top item in a list when you are typing a variable name for example.
-------------
Double clicking a tab expands the file editors to consume all available screen space.
You can middle click a tab to clove it (all UI title bar/ tabs do this!)
You can "rip" editor tabs and other UI components off the main interface and have them as floating windows.
-----------------------
You can set up a connection to an SQL database (anything with a JDBC driver). Check out View -> Tool Windows -> Database if it's already visible on the right side of the window. In recent versions, you can even tunnel the connection over SSH if desired.
What's nifty about this is that it's also able to detect strings that look like SQL queries (as long as you're not building them dynamically). If you have a connection set up then it'll give you autocomplete and check options for your personal database layout.
------------------------
Here's a few. When using PHPDoc typehinting, you can specify mutliple classes by separating them with a | character, this will tell PHPStorm to include methods/member variables from each. One example of where this is useful is PHPUnit mock objects, so you get the code completion for the mocked object, and also code completion for setting up the expecations:
/* @var $somevar My\Class\To\Mock|\PHPUnit_Framework_MockObject_MockObject */
Also, if you use heredoc then you can signal what highlighting should be used for the contents by using different labels, e.g.
$data = <<<XML XML;
will highlight the contents as XML.
Finally, you can open up the contents of a string in a context-sensitive editor simply place the caret inside the string, press alt-enter and then select "Edit {language} fragment" (where {language} is whatever language the string is in, e.g. Javascript/XML etc.). You can then edit with full code completion/highlighting and the changes are mirrored in the original script.
---------------------------
Settings > IDE Settings > Editor > Editor Tabs
Check out the tab closing policy. It automatically closes tabs when you cross a set limit of open tabs.
Settings > IDE Settings > Editor > Colors & Fonts
I love the Darcula color scheme.
CTRL+TAB
to switch between open files (handy when you are switching between two files a lot)
------------------------
Rebind some important keys. If you can handle two stroke commands it is easier and you don't have to worry about conflicting with standard keys most of the time.
I have 3 kinds of custom keys. General, Window, git
For example, to open a file I type: Ctrl-x, f (this brings up the little box where you type part of a file name, select the right file, press enter, it opens.) All of my git commands are launched with Ctrl-G Ctrl-g, r - compare with latest repo version
Two stroke keys mean press the first one by itself, then press the next.
My window(tab management, like close, close all, etc) keys start with ctrl-w
Some of the default key bindings are so odd, like alt-n is the default for the file opening box I mentioned above.
------------------------
Eagerly looking forward to reading more tips/hacks. I've only been using PHPStorm for about a month now, but am already in love. In addition to what others have already mentioned:
Ctrl-Shift-X: open command line, I use it mostly for Composer related stuff since the built in Composer support is still a bit wonky
Ctrl-K: Commit to Git/SVN
Alt-Enter: on most squiggly lines will bring up a menu of potential fixes (e.g. "import class" or "shorten fully qualified name" or "fix docblock", etc)
Integration with PHP Code Sniffer + PHP Mess Detector + PHPUnit + XDebug is magical. My code has never before been so clean and nicely formatted.
---------------------------
As I said in a comment below, Column Selection Mode. You just right click anywhere in your code and turn on "Column Selection Mode" or press Alt+Shift+Insert and you can drag a vertical line and when you type, it types on all those lines. Saves a TON of time when you have to edit a ton of variables/sql/anything that is similar and you just have to make simple changes.

-----------------------------
Does PHPStorm have a command to automatically align variable assignments? For instance, in Sublime Text 2, I select a few lines with variable assignments, hit Control-Alt-A and everything gets tidied up.
For example:
$var = "some value";
$another_var = "another value";
$third_var = "third value";
Becomes:
$var         = "some value";
$another_var = "another value";
$third_var   = "third value";
enlace
[–]sirsosay 3 puntos 1 año atrás 
Settings->Code Style->Other->Align consecutive assignments
Part of code formatting
enlacepadre
--------------------------------
https://www.jetbrains.com/phpstorm/help/using-suggestion-list.html
https://www.jetbrains.com/idea/help/basic-code-completion-completing-names-and-keywords.html
http://jetbrains.dzone.com/articles/top-20-code-completions-in-intellij-idea