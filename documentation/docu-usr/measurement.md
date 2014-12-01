---
layout: page
title: Measurement
excerpt: "User documentation for OpenIndy - The Measurement"
author: usr
image:
  feature: banner/b_smr.jpg
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>Measurement Overview</h3>
  </header>
<div id="drawer" markdown="1">
* bla
{:toc} 

</div>
</section><!-- /#table-of-contents -->

---

<a href="/documentation/docu-usr.html" class="btn">Overview</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/preperation.html" class="btn">Preperation</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/views.html" class="btn">Different Views</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/measurement.html" class="btn btn-success">Measurement</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/functionalities.html" class="btn">Other Functionalities</a>&nbsp;&nbsp;

<br>
This section shows one possible way to solve an example task. There are also other ways or orders of steps to solve it.

###The measurement task
The task is to check some features of the object vs CAD nominal data. Ten focal points of drill-holes and two small spheres have to be checked against a nominal CAD data. The coordinates of the nominal data are in the object coordinate system, so you also have to transform between the instrument
coordinatesystem and the object coordinatesystem. The four marked drill-holes are used as checkpoints for the transformation.
<figure >
	<img src="/documentation/images/usr/object.jpg"></a> 
	<p align="middle"><i>the object that has to be checked</i></p>
</figure>

###Set instrument
The first step is to set and connect an instrument. The instrument is set at the active station. On default, STATION01 is the active station and is marked in dark grey.
Other stations, if you have some, are marked light grey. Only one station can be active, but it is possible to switch them. <br>
To set the instrument click **Station > set instrument**.
The following dialog will appear:
<figure >
	<p align="middle"><a href="../images/usr/setInstrument.png"><img src="/documentation/images/usr/setInstrument.png"></a> </p>
	<p align="middle"><i>set instrument dialog</i></p>
</figure>
In this dialog you first have to select your sensor type, which is totalstation in the screenshot. After that you choose the instrument by selecting it in the table view. Currently there is only one sensor implemented, so only one totalstation is displayed in the screenshot. After selecting the instrument, you have to set up the **connection** settings to the sensor, that will be used to connect with the sensor. The connection parameter in OpenIndy have to be the same as the one you set up for your instrument.
You also need to put in the **accuracy** and other **sensor configurations** in this dialog.
After you set up the instrument, press Ok. The programm now asks you if you want to connect the sensor automatically, or if you want to connect to it later by hand.
 
###Import nominal data
One of the first steps to solve the task is to import the nominal data (are tagged as brown rows) for the two spheres and drill-holes. To do this. click on **File > import > elements**.
In the import dialog choose the data format which is ASCII in our example and the geometry typ. In our case there is a point and sphere file, so you have to import both of the geometry types. As third parameter you need to choose the destination coordinatesystem for the nominal geometries. In our case use the default PART coordinate system, which is the coordinate system of the not yet transformed plain. 
You can also choose other coordinate systems, if you create them via the toolbar by clicking on ![create coordinate system](/documentation/images/icons/coordinateSystem.png){: style="width: 25px"} before importing the nominal data.
<br><br>
<i>The nominal spheres file for the measurement task:</i><br>
KugelAmBuegel -54.181 -88.773 10.210 12.666<br>
Kugel 152.446 88.805 10.123 12.791<br><br>
<i>The nominal points file for the measurement task:</i><br>
p1 -50.737 0.082 0.000<br>
p2 -35.867 36.062 0.000<br>
p3 0.096 50.816 0.000<br>
p4 35.924 35.920 0.000<br>
p5 50.796 0.012 0.000<br>
p6 35.861 -35.981 0.000<br>
p7 -0.086 -50.734 0.000<br>
p8 -36.015 -35.797 0.000<br>
p9 0.000 0.000 0.000<br>
p10 104.199 0.000 0.000

<figure >
	<a href="../images/usr/importNominals.png"><img src="/documentation/images/usr/importNominals.png"></a> 
	<p align="middle"><i>import nominal dialog</i></p>
</figure>

###Create features to be measured
In the next step you need to create the features you want to measure. For this use the toolbar on the left or click on **view > show/hide feature toolbar**
to get the special toolbar for creating features. The standard toolbar on the left has some more functionality, because here you can also create scalar entities
and transformation parameters. In the toolbar on the left there is a button for each feature you can create. Clicking on one of them opens the creation dialog, where you can put in
your settings and parameters for the features you want to create.

####Measurement config
For each feature you create, you need to specify a measurement configuration. It includes a name, number of measurements, that should be done and averaged,
the reading type (e.g. polar, only distance, only angle,...), two face measurement and more.
You can specify it while creating the feature, or edit it later. To specify a complete measurement configuration the sensor must be set to display the supported 
reading types. If the measurement config is not completely set, the feature rows are tagged red in the tableview.
To specify the measurement config while creating a feature, click on the measurement config button in the create feature dialog or in the create feature toolbar.
To edit the measurement config of an existing feature, click **Station > measurement configuration** or the ![measurement config](/documentation/images/icons/measurementconfig.png){: style="width: 25px"} icon in the menu. To edit a measurement config of a feature, you first need to select the feature, by clicking on it in the tableview. It will be highlighted in a blue color.
<br><br>
For the task you need to create 10 points and 2 spheres. The name of the actual measurement features has to be the same as the corresponding nominal one. Click on **Create sphere** ![create sphere](/documentation/images/icons/sphere_5e8acf.png){: style="width: 25px"} and create "Kugel" and "KugelAmBuegel". The count, which defines the number of features to create, should be 1. The spheres are neither common nor nominal. The function of your choice is SphereFromPoints.  Functions will be explained in the next paragraph. After creating both of them, they appear in the tableview. The next step is to create the ten points for the focal points of the drill-holes.
Click on **Create point** ![create point](/documentation/images/icons/point_5e8acf.png){: style="width: 25px"}, type in the name "p1" and set the count = 10. They are not common and not nominal. Leave the default function as BestFitPoint.  After clicking on create, ten points will be created and automatically be named after the count number from p1 to p10.
<figure >
	<p align="middle"><a href="../images/usr/createFeatureDialog.png"><img src="/documentation/images/usr/createFeatureDialog.png"></a> </p>
	<p align="middle"><i>create feature dialog</i></p>
</figure>
<figure >
	<a href="../images/usr/createPointDialog.png"><img src="/documentation/images/usr/createPointDialog.png"></a> 
	<p align="middle"><i>create point dialog</i></p>
</figure>
<br>

###Add functions
To solve a feature it is neccessary to add functions to it. You already set the default function in the create feature dialog. Every function has input parameters, that you have to assign to it (e.g. obervations or a plane and a 
distance). For our task we added a "Best Fit function" to the features we measure in the creation dialog. To do this manually, select a feature and click **Function > set function** or use the ![set function](/documentation/images/icons/function.png){: style="width: 20px"}
icon. Then choose your function by clicking it. There are only functions displayed, that can be applied to the selected feature at this state.
After double clicking the next view appears, where you can add or remove the input parameters that the function needs. Also you see all functions that are 
applied to this feature in the order they are performed.
In our case we first add the best fit function and then measure the feature, so all observations of the feature are used for the best fit immediately. 
It is also possible to measure the feature first and then add the function. This way you have to assign the observations of the feature to the function via 
the treeviews in the function menu.
<figure >
	<a href="../images/usr/chooseFunction.png"><img src="/documentation/images/usr/chooseFunction.png"></a> 
	<p align="middle"><i>choose function</i></p>
</figure>
<figure >
	<a href="../images/usr/assignInputParameter.png"><img src="/documentation/images/usr/assignInputParameter.png"></a> 
	<p align="middle"><i>add input parameter. In this case an observation to the best fit function</i></p>
</figure>

You can remove functions by doing a right click on them in the function menu and choosing **delete function**.

###Measuring features
After the features are created, functions are applied and the sensor is set and connected, we can start measuring the features. For this, select the feature you want 
to measure and click on **measure** in the sensor control pad. You can find it by clicking **View > sensor control** or by clicking on the ![sensor control pad](/documentation/images/icons/sensorcontrol.png){: style="width: 25px"} icon.

<figure >
	<a href="../images/usr/measure.png"><img src="/documentation/images/usr/measure.png"></a> 
	<p align="middle"><i>measure the current selected feature (p1)</i></p>
</figure>

You will notice, that the measured points are displayed as not yet solved (columns are tagged yellow). Because we could not measure the focal point of the drillholes directly, we need to solve the problem by applying some functions.
<figure >
	<p align="middle"><img src="/documentation/images/usr/offSetProblem.JPG"></a> </p>
	<p align="middle"><i>unknown offset problem</i></p>
</figure>

For this, we measure the ground plane, and move it on its own normal vector with the value of the reflector radius. To do this use the function 
"ShiftPlane". 
After this we project our drill-hole points in the plane and have the offset corrected values we want.

####Scalar entity distance 
To specify the distance value for moving the ground plane you need to crate a scalar entity distance by clicking on the icon **Create Scalar Entity** ![create scalar entity](/documentation/images/icons/scalarEntities.png){: style="width: 25px"} in the feature toolbar.
Choose a name, count is 1 and the value is the radius of the reflector. After creating you can solve the unknown offset problem.

###Transformation parameter
After measuring all neccessary features, we need to transform our measurements, because the nominal data for the feature is in the PART system whereas 
the actual feature data is in the instrument coordinate system. In order to do this, we need to create a transformation parameter via the button 
**Create transformation parameter** ![create transformation parameter](/documentation/images/icons/trafoParam.png){: style="width: 25px"}. In the dialog you have to give this transformation parameter a name and set a start and destination coordinate system. After creating 
the parameter set it is displayed in its own tableview tab "transformation parameter". Here you can select it by clicking on it. You can also edit the parameters 
by hand if you choose "show properties" after right clicking the transformation parameter set.

####Apply a function to the transformation parameter
{:.no_toc}
After you created the transformation parameter you can set some values by hand (see paragraph above), or you can calculate parameters by applying a function 
to the transformation parameter feature. Click on **Function > set function** and click on the tab "new function". The default plugin, that we used for this measurement task, contains a 7 parameter helmert transformation. Select the transformation parameter by double clicking it and go back to the function configuration dialog, where you can apply the function to the feature.

####Apply checkpoints to the 7 parameter transformation
{:.no_toc}
After selecting the function, the input parameter have to be applied to the function. In the available elements window, we need to apply the nominal (imported at the beginning) 
and actual (measured) checkpoints, specified at the beginning of the guide (p1,p3,p5,p7). Apply the dialog and now the transformation parameter will be calculated and applied on the feature.

<figure >
	<a href="../images/usr/7parameterTransformation.png"><img src="/documentation/images/usr/7parameterTransformation.png"></a> 
	<p align="middle"><i>applying checkpoints (nominal and actual) to the function</i></p>
</figure>

After this step it is important to change the "use" column of the transformation parameter feature from false to true by double clicking the matching field, so that the parameter can be applied as transformation on the coordinate systems.<br>
Now it is possible to transform between STATION01 and PART. You can do this, by switching the active coordinate system in the combobox right above the feature tableview. This combobox includes all existing coordinate systems whether instrument systems or part systems.


###Save and load project
After the actual values are transformed and compared with the nominal values of the features, you should save the project by clicking on **File > save as** and specify a path and folder.
OpenIndy saves the project in an openindyXML.xml file where all features, their relations, observations, functions and other parameters are stored, so that it is possible to 
restore the complete project at a later time. To load and restore a project click **File > open project**.
<figure >
	<a href="../images/usr/exampleProjectFile.png"><img src="/documentation/images/usr/exampleProjectFile.png"></a> 
	<p align="middle"><i>extract of a project xml file</i></p>
</figure>
