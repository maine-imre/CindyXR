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
