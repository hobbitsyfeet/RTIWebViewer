# RTIWebViewer

Thanks to Gianpaolo Palma for providing this useful tool.

Email: gianpaolo.palma@isti.cnr.it

http://vcg.isti.cnr.it/rti/webviewer.php

== Description =======================================================================
WebRtiViewer is a viewer of high resolution RTI images (LRGB-PTM, RGB-PTM, HSH).
for the web. It allows also the visualization of common images in the format
JPEG, PNG and TIF. It is available under the GNU General Public License version 3.

In order to show the image on the web, it must be preprocess with the command line 
tool webGLRTIMaker that creates the multi-resolution format to load in your web server. 
In the folders "webGLRTIMaker-win-x86" and "webGLRTIMaker-win-x64" there are the 32
and 64 bit versions of the tool for Windows. The folder "webGLRTIMaker-src" contains
the source code. It requires a recent version of the framework QT.


== PREPROCESSING =======================================================================
To preprocess the image you must execute the following command line:

webGLRTIMaker.exe pathtothefile -q 90

where we have the filepath of the image and a quality value (in example 90).
The quality can accept value in the range 0-100 (100 = maximum quality).
The tool creates a new folder (with the same name of the image) in the folder of the
image with all the data to store in the web server. At this point you can copy
the new folder in your web server.


 
== VIEWER ==============================================================================
The folder "webViewer" contains the code of the HTML5 RTIViewer. To simply show an image
you must add the following code in your HTML page:

	<div id="viewerContainer">
		<script  type="text/javascript">
			createRtiViewer("viewerContainer", "./webrti", 900, 600); 
		</script>
	</div>
	
where the function "createRTIViewer" takes the following parameters:
1) the id of the div tag that must contain the viewer ("viewerContainer" in the example);
2) the path to the folder that contains the proprocessed image ("webrti" in the example);
3) the size in pixel (width and height) of the viewport of the viewer (900 and 600 in
   the example)

Please note that a web server MUST be running in order to view this. React js takes care
of this. Otherwise you can use things like Apache or other web servers.

==REACT JS ==============================================================================
To include this with React JS, you need to include the <script> components in the header
of viewer.html into React js's Index.html. This acts as a replacement for Import, and allows
the Javascript functions and CSS to be accessed anywhere in the project such as CreateRtiViewer(). 
It will look like this in index.html under Public/:

	<body>
    		<link type="text/css" href="css/ui-lightness/jquery-ui-1.10.3.custom.css" rel="Stylesheet">
		<link type="text/css" href="css/webrtiviewer.css" rel="Stylesheet">
		<script type="text/javascript" src="js/jquery-1.11.3.min.js"></script>
		<script type="text/javascript" src="js/jquery-ui.min.js"></script>
		<script type="text/javascript" src="js/pep.min.js"></script>
		
		<script type="text/javascript" src="spidergl/spidergl.js"></script>
		<script type="text/javascript" src="spidergl/multires.js"></script>
	</body>

Often, a component will be created independent of other project components. We created a
default class extending component and defininf a ComponentDidMount which runs the component when it
is 'mounted' into React. The Whole class looks like this:

import React, { Component } from "react";

export default class RTIViewer extends Component {

    componentDidMount(){
        var script = document.createElement("script");
        script.innerHTML = `createRtiViewer("viewerContainer", "./webrti/", 900, 600);`
        var container = document.getElementById("viewerContainer");
        container.appendChild(script)
    }

    render() {
        return (
            <div id="viewerContainer"></div>
        );
      }
}

If you want to dynamically add a path to the viewer, you can add a prop variable which can
change on the fly to load new RTI (ptm) images. That would look something like this:

	script.innerHTML = `createRtiViewer("viewerContainer", "${this.props.rti}", 900, 600);`


Once this is complete, you need to add the component to App.js, looking like so:

import RTIViewer from "./components/RTIViewer"

	export default class App extends Component {
	  render() {
	    return (

		<div className="App">
		  <RTIViewer/>
		</div>
	    )
	  }
	}

== ACKNOWLEDGEMENT =======================================================================
If you use the viewer in your site please send me an email (gianpaolo.palma@isti.cnr.it)
with the link of your site. 
