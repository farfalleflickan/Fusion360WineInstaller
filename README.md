# Fusion360WineInstaller

Crude write up. Still WIP.


Step 0:
Clone this repo, create the directory where the wineprefix for Fusion 360 is gonna go and place ```installer.sh``` & ```installer.tar.gz```

Step 1:
run ```installer.sh 1```, it will automatically set up the directory as a prefix and automatically do the winetricks stuff, you will have to press "next" on a couple of prompts...

```winecfg``` will pop up, in case you want to enable the virtual desktop (basically a must on i3wm)

```streamer.exe``` will start automatically and fetch data which will be installed in:
```
$WINEPREFIX"/drive_c/users/"$USER"/Local Settings/Application Data/Autodesk/webdeploy/production/
```

Let it run, after a while the Autodesk Fusion 360 logo should re-appear, along with a new window that should ask for your login credentials. Log in and let it run, a pop up should appear with ```Failed to create graphics renderer...```, press "Ok" and let it run for a while, until ```...retrying (60 sec)``` appears in the terminal (or if nothing happens at all), at this point close the terminal and eventually kill all & any wine process. 
THIS STEP SEEMS TO BREAK RANDOMLY. Haven't quite figured it out. Sometimes it works, sometimes it doesn't. 
If the program just hangs, check if 
```
$WINEPREFIX"/drive_c/users/"$USER"/Local Settings/Application Data/Autodesk/webdeploy/production/
```
is populated. There should be 2 folders. One of them will contain just 3 files, when this happens it's done. At this point kill the process & anything wine related.

Now you should enter the folder with just the 3 files (```FusionLauncher.exe```, an icon and a configuration file) & launch ```FusionLauncher.exe```manually, a login window should/might pop up).

If that STILL doesn't give you the login window... Try removing the

```
$WINEPREFIX"/drive_c/users/"$USER"/Local Settings/Application Data/Autodesk/
```
folder and re-doing this step. 
Last solution that seems to work for me, at least (but I also know these are needed by other people in order to work... might be a dependecies thing? Like having optional wine packages installed...? Not sure).
Delete the folder of your prefix and restart from zero, remove ```corefonts winhttp wininet``` from the installer script and proceed as usual.

Step 2:
Run ```installer.sh 2```, which will install the latest DXVK. DO NOT DO THIS BEFORE STEP 1! DXVK for some reason breaks the login window.

Step 3:
Run ```installer.sh 3```, which actually just launches the Fusion launcher again, but now stuff should... show up? All windows work for me. At this point I **STRONGLY** suggest clicking on your name and going into preferences and setting the graphics driver to **directx9**. Restart the program for this to have an effect.

Step 4:
After setting the graphics driver to directx9, close Fusion, make sure any lingering wine processes are killed and run ```installer.sh 2``` to uninstall DXVK (yes. for some reason even though the graphics are switched to DX9 some stuff tries to run in DX11 if DXVK is present) & make sure the following entries are removed in winecfg:
```
d3d10, d3d10_1, d3d10core, d3d11, dxgi
```
(note that you shouldn't have to do this, as dxvk should takre care of this while uninstalling)

Now everything should work! What you have in the end is effectively a prefix with vcrun2017 installed, that's about it!

DXVK/D3D11 are only needed in order to set Fusion 360 to directx9.

# Notes:

* Using DX9 is a must in order to get the internal web browser going (for example to confirm your subscription as a student or the like). AFAIK (more like, I saw a message about it flash in the terminal and read something along those lines on google but I can't quite recall...) DXVK has yet to implement the ability to draw subwindows or something along those lines, so DX9 it is. 

* I have managed to have this working even better while doing all of the above+messing with lutris (and tkg-4.6, i believe...), for instance I actually managed to get "subscribing as a student" to work (which for the moment it doesn't, the program will just hang), but i've failed to replicate this. Will look into it more in the coming days and do a better guide/write up too...

* In other places I've seen stuff like corefonts, mshttp, msxml3 & msxml6 getting installed, AFAIK this works anyways. I would probably reccomend following Lutris' wine setup [here](https://github.com/lutris/lutris/wiki/Wine-Dependencies), as I feel like that might have had something to do with it.

* Despite what other sources says (WineHQ, other GitHub Repos....) disabling d3d11 seems to just break stuff.

* This has been tested on Arch Linux (5.0.7) using wine-staging 4.6 & dxvk 1.0.3

This is what worked for me, I don't/can't guarantee that it will work for you. I did this write up hoping that it might help another poor soul that, like me, read about people using Fusion 360 on linux but can't manage to get it to work (mostly due to poor instructions/information in general).




# OLD STEPS BELOW (Leaving them for posterity or whatever)


Gonna write this up better but basically...

Step 0:
Clone this repo, create the directory where the wineprefix for Fusion 360 is gonna go and place ```installer.sh``` & ```installer.tar.gz```

Step 1:
run ```installer.sh 1```, it will automatically set up the directory as a prefix and automatically do the winetricks stuff, you will have to press "next" on a couple of prompts...

```streamer.exe``` will start automatically and fetch data which will be installed in:
```
$WINEPREFIX"/drive_c/users/"$USER"/Local Settings/Application Data/Autodesk/webdeploy/production/
```

Watch this folder as streamer.exe will hang and just be there once done, there are no visual cues to this process starting/ending. Basically let it be a couple of minutes until 2 subfolder show up in the ```production``` folder. One of them will contain just 3 files, when this happens it's done. At this point kill the process & anything wine related.

Step 2:
Run ```installer.sh 2```, which will start up winecfg. Find the d3d11 entry in libraries and remove it (this is a must as it will not launch otherwise, it will just crash. DO NOT USE DXVK! In my experience it doesn't work). If you are on i3wm you might want to setup a virtual desktop as Fusion will leave sticky windows on every workspace otherwise. 

The Fusion Launcher will start once you close winecfg and should prompt you to login. Put in your password & stuff and wait a minute or so. The terminal output should start showing messages along the lines of ```...retrying (60 sec)```, at this point kill all wine processes once again.

Step 3:
Run ```installer.sh 3```, which will install latest dxvk in the prefix. Once it's done move on to step 4.

Step 4:
Run ```installer.sh 4```, which actually just launches the Fusion Launcher again, but now stuff should... show up? All windows work for me. At this point I suggest clicking on your name and going into preferences and setting the renderer to directx9. Restart the program for this to have an effect.

Step 5:
Using DX9 is a must in order to get the internal web browser going (for example to confirm your subscription as a student or the like). AFAIK (more like, I saw a message about it flash in the terminal and read something along those lines on google but I can't quite recall...) DXVK has yet to implement the ability to draw subwindows or something along those lines, so dx9 it is. 

Earlier I managed to have this working even better while doing all of the above+messing with lutris (and tkg-4.6, i believe...), for instance I actually managed to get "subscribing as a student" to work (which for the moment it doesn't, the program will just hang). Will look into it more in the coming days and do a better guide/write up too...


This is what worked for me, I don't guarantee that it will work for you. I did this write up hoping that it might help another poor soul that, like me, read about people using Fusion 360 on linux but can't manage to get it to work (mostly due to poor instructions/information in general).
