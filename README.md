# [CindyXR-Interactions](https://maine-imre.github.io/CindyXR-Interactions/)
This repository has a few examples of spatial interactions with spatial diagrams developed by the IMRE Lab at the University of Maine.
This project builds on CindyJS and the CindyXR plugin.  In particular, we use [CindyJS]
(https://github.com/CindyJS/CindyJS) to render spatial diagrams and use the [CindyXR](https://github.com/chrismile/CindyJS) plugin to access *select* and *squeeze* events from controllers, along with their positions in 3-spac to made the diagrams interactable.  Thank you [@chrismile](https://github.com/chrismile) for developing the event system that makes this possible!


## Examples
* [Shearing-Triangle](https://maine-imre.github.io/CindyXR-Interactions/examples/01_Shearing_Triangle.html)
* [Shearing-Pyramid](https://maine-imre.github.io/CindyXR-Interactions/examples/02_Shearing_Pyramid.html)


### Accessing Examples with HTC Vive (or Similar OpenVR)
* Start SteamVR, connect HMD
* Using a recent version of Firefox, open an html from https://maine-imre.github.io/CindyXR-Interactions/
* Allow VR in Firefox
* The webpage should load and connect to the HMD


### Accessing Examples with WebXR device emulator browser extension
 * Using a recent version of Firefox, install WebXr Emulator extension
 * Open an html from https://maine-imre.github.io/CindyXR-Interactions/
 * Allow VR in Firefox
 * Right click on the page.  Choose "Inspect Element"
 * In the Firefox Developer Tools, choose "WebXR"
 * The WebXR tab will allow you to control the HMD and Controllers, while the content is rendered in the browser page.
 
 ### Accessing Examples with WebXR all-in-one device (Oculus Quest or similar)
  * Open te Oculus Browser or Firefox Reality on the device
  * Navigate to https://maine-imre.github.io/CindyXR-Interactions/

## Getting Started
This repository has a few examples of spatial interactions with spatial diagrams developed by the IMRE Lab at the University of Maine.
Each example will have functionality mapped to 

### For Development of new Scenes
Take the ``examples/template.html`` and add code in the ``init``, ``xrdraw``, ``xrsqueezehold``, and ``xrselecthold`` scripts.
These scripts are all written in CindyScript.  See [CindyScript docs](https://github.com/CindyJS/CindyJS/blob/master/ref/createCindy.md).

### Cloning Instructions
We use git and git-submodule to include a recent deployment of CindyJS with the CindyXR plugin. 
Note that the CindyJS/Deploy repository (subomdule) is unstable and for development only.
For a stable solution, build CindyJS on your web server or use a release from the CindyJS repository.

* git clone git@github.com:maine-imre/CindyXR
* git submodule init
* git pull --recurse-submodules


### Rendering in XR
CindyXR supports an xr-draw script, using draw3D commands.  We plan to write some helpful functions that extend draw3D to structs with geometric element data.

### Interactions
CindyXR gives us a set of data from the controller. We use position data and the select and squeeze events.  This reduces button mappings to two buttons per controller, but makes cross-device accessibility easy.

### GitHub Pages
We deploy our examples using GitHub pages.  Once an example is stable it will be moved to the master branch and deployed into the GitHub.io site.
No changes to the source are necessary: in both cases, address the ``Cindy.js`` source with ``../deploy/Cindy.js`` to make it accessible locally for offline work and available locally 

### To make development available on the site
- Make an html page in the **examples** folder
- Make a pull request to master
- Add a link in the examples section to the html page
- GitHub pages will include the new content within 10 minutes of the pull request being accepted.

# Template Example
In this section, we walk through the content in our ``template.html`` and a place to get started with CindyXR scenes.

## Header
The header includes references to CindyJS, CindyXR and Cindy3D in ``../deploy/``.  If you are using CindyGL, add a reference here.

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>CindyXR Template</title>
    <style type="text/css">
        body {
            margin: 0px;
            padding: 0px;
        }

        #CSCanvas {
            width: 800px; height: 800px;
        }
    </style>
    <script type="text/javascript" src="../deploy/Cindy.js"></script>
    <script type="text/javascript" src="../deploy/Cindy3D.js"></script>
    <script type="text/javascript" src="../deploy/CindyXR.js"></script>
```

## CindyScripts
A series of CindyScripts follow with tags of the form ``<script id="NAME" type="text/x-cindyscript"> SCRIPT HERE </script>``

### init
In this script, we setup a XR Scene, make references to CindyXR and Cindy3D dependencies and define any global varaibles.  For example, we might add a list of the positions of vertices if we were rendring a polygon.

```
    <script id="init" type="text/x-cindyscript">
        use("Cindy3D");
        use("CindyXR");
        initxrcindy3d(referencemode->"local");

    </script>
```

### xrdraw
The xrdraw event handles all of the rendering for CindyJS - mapping to both the Xr device display and the webpage on the computer monitor.

#### draw3Dxrinputsources

The ``draw3Dxrinputsources`` function takes the data from each controller and renders a colored sphere at the controller's location.  We hope to update this to include a better visualization of the controller in the future.  The color of the sphere chnages based on the use of *select* and *squeeze* buttons.

#### begin3D / end3D
These funtions are called before and after working with 3D drawing functions in CindyJS.
We set the default size (width) for inscriptions and the background color.
Within this pair of functions, make calls to draw any geometric figures using ``draw3D``.
Finally, we make a call to ``draw3Dxrinputsources`` and pass in the controller data from the ``getxrinputsoruces`` function.

```
 <script id="xrdraw" type="text/x-cindyscript">
        //Function Definitions:
        draw3Dxrinputsources(xrinputsources) :=(
            repeat(length(xrinputsources), i,
                inputsource = xrinputsources_i;
                // We only want input sources regarding tracked controllers.
                if (inputsource.targetRayMode == "tracked-pointer",
                    // Draw a ball at the position of the controller and color it depending on whether button 1 is pressed.
                    isbuttonpressed = inputsource.gamepad.buttons_1.pressed;
                    color = if(isbuttonpressed, hue(0.1), hue(0.5));
                    color3d(color);
                    size3d(.3);
                    draw3d(inputsource.gripSpaceTransform.position);
                );
            );
         );

        //begin applet
        begin3d();

        //environment parameters
            size3d(.1);

		//set bg color
			background3d([0.9,0.9,0.9]);


//ADD CODE HERE TO MAKE INSCRIPTIONS

	    //draw tracked controllers
	        draw3Dxrinputsources(getxrinputsources());

        end3d();
    </script>
```

### xrselecthold
This event is called every frame that the *select* button on the device is held down.
This should be used for selecting figure components, but can be mapped to any function of the controller's position.
```
    <script id="xrselecthold" type="text/x-cindyscript">
      //select interaction
    </script>
```


### xrsqueezehold
This event is called every frame that the *squeeze* button on the device is held down.
This should be used for moving figure components, but can be mapped to any function of the controller's position.
```
    <script id="xrsqueezehold" type="text/x-cindyscript">
      //squeeze interaction
    </script>
```


### Example ``xrsqueezehold`` for moving vertices (xrselecthold or xrsqueezehold)
This example event reassigns sufficiently close vertices to the location of the controller in 3-space. This is a crude implementation of dragging.

We plan to expand this to handle more sophisticated dragging of a variety of geometric elements, including choosing the closest object instead of all objects within a predetermined radius.
```
    <script id="xrsqueezehold" type="text/x-cindyscript">
        //drag the verticies when proximal and holding the squeeze button
        tolerance = .5;  //maximum effective distance in meters
        controllerPosition = inputsource.gripSpaceTransform.position;  //position of the controller
        repeat(length(vertices), i, //loop through all vertices in the list (assigned in init)
            if(|controllerPosition - vertices_i| < tolerance, //if the vertex is sufficently close to the controller
                vertices_i = controllerPosition; //assign the vertex to the controller's position
            );
        );
    </script>
```

## BODY
The body contains a reference to CindyJS and frames it within the page.
```

        <script type="text/javascript">
            CindyJS({canvasname:"CSCanvas",scripts:"*"});
        </script>
        </head>

        <body>
            <canvas id="Cindy3D" style="border: none;" width="632" height="452"></canvas>
            <div id="CSCanvas" style="width:50px; height:50px; border:none"></div>
        </body>

    </html>
 ```
