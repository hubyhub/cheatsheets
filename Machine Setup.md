Install [Nodepad++](https://notepad-plus-plus.org/download/v7.5.5.html)<br>
Install [7zip](https://www.7-zip.org/)<br>
Install [Git](https://git-scm.com/downloads)<br>
Install MobaXTerm<br>
Install Browsers (chrome, IE, FF, Opera)<br>
Install [Webstorm](https://download.jetbrains.com/webstorm/WebStorm-2017.3.5.exe)<br>
Install [IntelliJ 14.1](https://download.jetbrains.com/idea/ideaIU-14.1.7.exe)<br>
Install [ScreenToGif](http://www.screentogif.com/)<br>
Install [Irfanview](http://www.irfanview.com/)<br>
Install [Skype](https://www.skype.com/de/)<br>
Install [XAMPP](https://www.apachefriends.org/de/index.html)<br>
Install [Insomnia](https://insomnia.rest/download/)
Install [SVN](https://tortoisesvn.net/downloads.de.html) <br>
Install [Sourcetree](https://www.sourcetreeapp.com/)<br>	

Add Network locations:
```
# Right-Click on Computer(in explorer) add a network location:<br>
\\files.whatever.local\whatever
```

Event Viewer:
```
%windir%\system32\eventvwr.msc /s 
```

Services: 
`%windir%\system32\services.msc`
	
	
## Install [Node.js](https://nodejs.org/en/) (npm)
[Download node.js](https://nodejs.org/en/)

```bash
npm install gulp-cli -g	
npm install rollup -g	
npm install bower -g 	
npm install less -g     # in Webstorm add File->Settings->Tools->FileWatchers->Less
```

## IntelliJ
1. Install "FileWatchers": "Ctrl"+ "Alt"+ "S"  --> Plugins --> Install Jetbrains Plugins --> "File Watchers"
2. File->Settings->Tools->File Watchers->Less
3. Keymap (CTRL+ALT+S) use default
	* change Undo: CTRL+Z
	* change Redo: CTRL+Y
	* Comment with Line Comment: Ctrl+k, Ctrl +c
	* Comment with Block Comment: Ctrl+k, Ctrl + Shift +c


## Show extensions & show protected system files
`Explorer--> Organize --> Folder Search options --> View -->`<br>
[ ] Hide extensions for know file types<br>
[ ] Hide protected operating sytem files<br>
