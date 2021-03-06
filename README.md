# Open Source Architecture Workflows

Notes on open source workflows within the architecture/building/construction industry.

## Contents

 * Software
 * Surveying
   * Tested process: Traditional surveyor PDF to Blender via Inkscape/SVG
   * Untested process: Opendronemap point cloud survey
 * Shadow modeling
 * Construction related CNC fabrication work
 * Structural and thermal analysis

## Software

 * [Blender](http://www.blender.org/) open source cross-platform 3D modeling and animation program
 * [Inkscape](http://www.inkscape.org/) open source cross-platform vector editing program with SVG as its native format
 * [Open Drone Map](https://www.opendronemap.org/) open source point cloud generation from images or video
 * [OpenSFM](https://github.com/mapillary/OpenSfM/) open source structure from motion - ie. precise camera-position-over-time generation from images or video (can also support GPS feed)
 * [Meshlab](https://www.meshlab.net/) open source system for processing and editing 3D triangular meshes.
It provides a set of tools for editing, cleaning, healing, inspecting, rendering, texturing and converting meshes. It offers features for processing raw data produced by 3D digitization tools/devices and for preparing models for 3D printing.
 * [Point Cloud Library](https://pointclouds.org/) standalone, large scale, open project for 2D/3D image and point cloud processing

## Surveying

### Deciding on a survey type

When deciding on a survey type, the main factors are probably legal requirements, accuracy/coverage and cost.

Many jurisdictions have legal requirements that licensed surveyors output are submitted in order to approve construction work, which eliminates the option of self-survey. However, __traditional surveyors__ are often expensive, slow (2 weeks to get data is considered fast in some markets!) and low resolution, and may have other problems such as spotty coverage and inaccurate approximations interspersed with precise positioning information. While their primary point readings are accurate, their coverage of the space will be really basic and spotty.

__DIY surveying__ with video output from a phone or a drone is possible with open source photogrammetry tools, and will result in point cloud data which can be further processed. Photogrammetry based point cloud surveys are also the norm for large commercial and insturial sites which require large scale surveying. Commercial __point cloud surveyors__ increasingly tend to use cloud based services (drone operator uploads video to third party, waits, downloads dataset). When executed propertly, point clouds provide far better coverage of a site at good accuracy.

All surveys may be complicated by certain types of features (eg. dense foliage).

Finally, you may not need a survey at all. Satellite images can be obtained online for free, and existing site plans can often be obtained from local governments, which increasingly have GIS systems or web GIS portals for this purpose.

![image](surveying.svg)

#### Tested process: Traditional surveyor PDF to Blender via Inkscape/SVG

To convert a vector PDF from a traditional surveyor to an SVG file that can be imported to Blender without losing any data, you can use the following manual process. Once you have the hang of it, it's not too slow, and gives you the benefit of the freedom to resolve issues with the supplied vector data in an appropriate environment before importing to Blender.

First, open the PDF file in Blender. You will get an import dialog like this.

![image](inkscape-1-pdf-import.jpg)

Import the file, then press __Ctrl+Shift+L__ to show the layers palette, or select __Layer|Layers...__ from the menu.

Now set the existing PDF contents layer's name to "Source" and lock it by clicking the open padlock icon. Change its opacity to 30% to facilitate tracing.  You should see it lighten a little bit. 

![image](inkscape-2a-layers.jpg)

Then press the plus icon to make a new layer, named "Terrain". Select the "Terrain" layer. Your layers should now look like this.

![image](inkscape-2b-layers.jpg)

Now you are going to zoom in and start to trace the data provided by the surveyor, while normalizing it to useful closed vector shapes and noting the associated heights. To begin, select the Bezier drawing tool with __Shift+F6__ or by clicking the icon.

![image](inkscape-3-bezier-tool.jpg)

Now zoom in to the image so that you can see clearly one single area described by the surveyor as a whole vector shape. For example, the top left corner of your survey. The view should look something like this.

![image](tutorial-zoomed-in.jpg)

You should then draw the bezier out, noting Inkscape's ability to snap accurately to the PDF contents, and wind up with a fully enclosed shape like this.
![image](tutorial-drawn-path.jpg)

Zoomed in further, the shape now looks like this.

![image](tutorial-drawn-path-only.jpg)

Press __Ctrl+Shift+F__ or select the __Fill and Stroke__ panel, then set the fill to black and the stroke to X (none). The shape should now look like this.

![image](tutorial-drawn-path-only-solid.jpg)

We now want to set the properties of the object to store the height as indicated by the surveyor, using the __Object Properties__ panel (__Shift+Ctrl+O__). The ID field text will be visible and associated with the object when we import the SVG we create from the PDF in Inkscape in to our target Blender scene, so we want to set it to the number of meters above sea level, eg. '145'. If you have more than one object with the same height, just call it '145 (unique string here)' like '145 iaousdiau' or '145 bottom of garden' to maintain the uniqueness restraint.

![image](tutorial-path-object-properties.jpg)

Repeat the process to include all terrain heights as fully enclosed vectors, store the SVG, and import in to Blender as follows.

First, ensure that you have the __Import-Export: Scalable Vector Graphics (SVG) 1.1 Format__ add-on enabled in __Blender Preferences__' __Add-ons__ section.

![image](tutorial-blender-svg-addon.jpg)

Now select __File|Import|Scalable Vector Graphics__.

![image](tutorial-blender-import.jpg)

After importing you should see the height text associated with each terrain object in the __Scene Collection__ view.

![image](tutorial-blender-imported.jpg)

The overall drawing should be visible in the main view.

![image](tutorial-blender-imported-2.jpg)

Now you simply need to scale each vector to the appropriate height. The fast way to do this is to first determine the vertical point (meters above sea level) that you want to model as zero. For example if most of your heights range from 140-160 meters above sea level then you might choose 140 meters above sea level as your vertical zero point.

Then select the vector shape in question, press tab to toggle to the edit mode, press A to select all vertices on the vector shape, execute your extrusion using the keyboard (S+Z+(number-of-meters-above-sea-level-less-the-height-of-your-vertical-origin-point)+enter) (eg. 145-140=5, so S+Z+5+ENTER), exit edit mode back to object mode with tab, select the next vector and repeat. You can cover a large number of vectors quite quickly this way. 

In the end you will have a reasonably detailed model, potentially better than the original survey as you have considered the errors and omissions within it and resolve them during your vector cleanup.

![image](tutorial-result-simple.jpg)

With a little more work you can add trees, nearby features and so forth, then have a good basis for moving forward on any conceptual design or renovations.

![image](tutorial-result-trees.jpg)

### Untested process: OpenDroneMap Point Cloud Survey

First get a drone or a camera (or smartphone) ready to take numerous photographs or video. The benefit of a drone is you can get more angles, higher angles, and move more quickly. The drawback is you have to own one, and there are legal issues flying them in many places. As with any photography, you should worry about lighting: choose a light but cloudy day without harsh shadows if available.  Next, read [DJI's guide to making successful maps](https://support.dronedeploy.com/docs/making-successful-maps).

You can use either [command line ODM](https://www.opendronemap.org/odm/), [webODM](https://www.opendronemap.org/webodm/), or [LiveODM](https://www.opendronemap.org/liveodm/) to do the processing.

When you fly (or walk with a smartphone!), you may consider using [ground control points](https://github.com/mapillary/OpenSfM/blob/master/doc/source/gcp.rst) (GCP). If you do this, webODM provides a workflow for inputting the GCPs whereas command line ODM does not, so you may be better off going with webODM, despite it being newer and less polished.

Finally you should be able to import the results to Blender, possibly via [Meshlab](https://www.meshlab.net/) for post-processing and [Point Cloud Library](https://pointclouds.org/).


## Shadow modeling

Blender is ideal for shadow models. Once you have your structure and terrain in to Blender, install the __Lighting: Sun Position__ add-on.

Once it is configured with your site coordinates and true north offset (available either on the survey or via mobile phone applications), you can then use time of year and time of day modification controls to set up screenshots of the top orthographic view. Paste these in to Inkscape and trace significant features such as major shadow lines. You can do this multiple times with different features excluded from the scene to create very accurate shadow models to greatly exceed the expectations of local regulators who are used to simplistic output from commercial modeling tools. For example, in order to show the minimum impact of a built environment change, you might indicate shadows cast by existing foliage or fences separately.

Here is an example screenshot.

![image](tutorial-shadows-example-screenshot.jpg)


## Construction related CNC fabrication work

There are definitely a lot of open source programs capable of producing output for CNC systems which are used to prepare materials for construction, including but not limited to timber, metal, plastic and glass. In practice, however, normally these systems are owned by a third party supplier and specifications are sent to them as drawings, after which they re-digitize the drawings as files for their CNC systems.

Low hanging fruit for CNC are things like sheet metal cutting (usually done with fiber lasers), wooden sheet cutting, timber and metal beam cutting, hole drilling and thread tapping. Classic examples would be windows, CNC cut benchtops, laser cut privacy screens, stairs, aluminium window modules and roller shutter units.


## Structural and Thermal Analysis

There is a [osarch meeting with a video demo](https://peertube.social/videos/watch/e93aaf55-b987-4769-8bac-b57f174e7ccc) showing how to use salome meca to perform structural modeling, eg. for earthquake simulation.

To learn yourself, see [learning material for Code Aster and Salome Meca](https://wiki.osarch.org/index.php?title=Learning_Material_for_Code_Aster_and_Salome_Meca).

Alternatively or in addition, you can consider paying one of these consultancies to set it up for you: [simulease](https://simulease.com/), [deltacad code aster services](http://www.code-aster-services.com/).
