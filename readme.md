# HandsLine Workshop

09.03.2015

----

1 week workshop at [ECAL](http://ecal.ch) with 2nd year Media and Interaction Design

![image](https://raw.githubusercontent.com/kikko/ecal_handline_workshop/master/screens/default.png)

----

*« Le chemin le plus court d'un point à un autre est la ligne droite,
à condition que les deux points soient bien en face l'un de l'autre. »*

Pierre Dac

----

## Brief

```
Design and create an interactive application based on the body tracking technology
that stages the virtual line linking our 2 hands.
```

* Study the role played by this virtual line in different situations, activities..etc and derive them to create new ones

* Work as duos

* The application can't use any other tracking information than the hands position (and eventually their open/closed status).

* It's not mandatory but highly encouranged to implement a multi-user system (up to 2 people)

* Sketch your ideas and present them to the group before implementing prototypes

----

### Some references (TBC)

![MYRON KRUEGER](http://www.inventinginteractive.com/wp-content/uploads/2010/03/VideoPlace4-450x304.jpg)

_Videoplace_ - MYRON KRUEGER - mid-1970s

----

### Geometrical properties

- spacial properties
	* position
	* length
	* angle

- temporal properties
	* velocity
	* acceleration

- shared properties
	* parallelism
	* spreading
	* similitude


----


## Framework

The microsized javascript framework available in this repository aims at getting you started quickly with your ideas.

It integrates :

- [kinect2-socket-client](https://github.com/kikko/kinect2-socket-client) to use **kinect 2** streams and playback files
- [paperJS](http://paperjs.org) for **vector graphics**
- [gsap](http://greensock.com/gsap) for **general purpose animations**
- [matter.js](http://brm.io/matter-js) for rigid-body **physics** computations
- [howler.js](https://github.com/goldfire/howler.js) for cross-platform **audio playback**
- [jquery](https://jquery.com) for **general purpose** js tools and dom manipulation

### Getting Started

First clone the repository using your favorite terminal application (I highly recommend the free [iTerm2](http://iterm2.com) on mac) 
```
git clone https://github.com/kikko/ecal_handline_workshop.git
```

Then enter this directory with `cd ecal_handline_workshop`

Make sure you have [nodejs](http://nodejs.org) installed on your machine and install the http-server module globally :
```
npm install -g http-server
```

Then you can launch a local webserver using the provided command file (you can also double click on the file):
```
./launch.command
```

This should start the server and open a web page with the default application.

To load a different default application you can use the url parameter `?app` for example if you want to load the Balls app :
```
http://localhost:8080?app=balls
```

This should load this demo (press TAB to show the debug view) :
![image](https://raw.githubusercontent.com/kikko/ecal_handline_workshop/master/screens/balls_app.png)

Look at the `js/apps` directory to see the list of available apps.

### Make your own application

To make your own, the easiest way is to navigate to the folder `js/apps` and duplicate the file `js/apps/default.js` in the same folder

You can then use the name you give to that file as a url parameter to load it.

The default app defines 5 functions that are called automatically by the framework :

- `setup()` is called once when the application is loaded
- `update(dt)` is called at ~60hz when the window has the focus
- `onUserIn(id, leftHand, rightHand)` is called when a user starts being tracked
- `onUserOut(id)` is called when a user stops being tracked
- *optional* : `resize(width, height)` is called when the window is resized

### Available features

The best way for now to get the full list of available features from the framework is to look at the examples and the source file `js/handline.vXX.js` 

You can also easily extend the functionalities with external libraris using the `include` util. The example `js/apps/gui.js` provides an example for loading an external lib directly from a CDN :

```javascript
function setup() {
  include([
    "https://cdnjs.cloudflare.com/ajax/libs/dat-gui/0.5/dat.gui.min.js"
  ], function() {
    
    gui = new dat.GUI();
    
    ...etc
  });
}
```

### Publish your app

To publish your app, you must follow the following rules :

- add your `yourappname.js` file in the `js/apps` folder
- if you have to load some external files (sounds, svg..etc), put them in the `assets/yourappname` folder
- commit and push your changes

In order to publish your app easily, you **shouldn't modify**  any other file than your `js/apps/yourappname.js` file (for example files like `index.html`, `css/main.css`...etc)

Also, don't forget to fill in your project details in your app file :
```javascript
function setup() {
  
  app = new HL.App();
  app.setup({
    projectName : 'Default',
    author1 : 'Prenom Nom',
    author2 : 'Prenom Nom'
  });

  ...etc
}
```

### Launching live view

`http://localhost:8080/?app=<yourapp>&socket=ws://<windowsmachine_ip>:9092`

* `<yourapp>` is your application filename, without the `.js` extension 
* `<kinect2socket_ip>` is the ip address of computer running the kinect2-socket application (`localhost` if it's on the same computer)

Pressing tab will bring up the debug view.
You can click on the kinect2-socket panel at the bottom-right to start recording. Clicking again will stop it and ask you for a file name to save.

### Playing back a replay

`http://localhost:8080/?app=<yourapp>&replay=<your_replay>.json.gz`

* `<yourapp>` is your application filename, without the `.js` extension 
* `<your_replay>` is the filename of the replay located in /code/replays _**with**_ the filename extension - usually `.json.gz`