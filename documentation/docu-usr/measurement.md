---
layout: page
title: Measurement
excerpt: "User documentation for OpenIndy."
author: usr
image:
  feature: banner_oi.jpg
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
This section shows one possible way to solve the task. There are also other ways or orders of steps to solve it.

###The measurement task
The task is to check some features of the object vs CAD nominal data. The ten focal points of the drill-holes and the 2 small spheres have to be checked
against the nominal CAD data. The coordinates of the nominal data are in the object coordinate system, so you also have to transform between the instrument
coordinatesystem and the object coordinatesystem. The four marked drill-holes are used as checkpoints for the transformation.
<figure >
	<img src="/documentation/images/usr/object.jpg"></a> 
	<p align="middle"><i>the object that has to be checked</i></p>
</figure>

###Set instrument
The first step is to set and connect an instrument. The instrument is set at the active station. On default STATION01 is the active station and marked grey.
 Other stations, if you have some, are marked light grey. Only one station can be active, but it is possible to switch them. <br>
To set the instrument click **Station > set instrument**.
The following dialog will appear:
<figure >
	<a href="../images/usr/setInstrument.png"><img src="/documentation/images/usr/setInstrument.png"></a> 
	<p align="middle"><i>set instrument dialog</i></p>
</figure>
In this dialog you first have to select your instrument type. In this screenshot it is totalstation. After that click in the table and select the instrument
you want. Here only one sensor is implemented, so there is only one totalstation displayed. After selecting the instrument, you have to set up the connection
settings to the sensor, that will be used to connect with the sensor. You also need to put in the accuracy and other sensor configurations in this dialog.
After you set up the instrument, press Ok. The programm now asks you if you want to connect the sensor automatically, or if you want to connect to it later
 by hand.
 
###Import nominal data
One of the first steps to solve the task is to import the nominal data for the two spheres and drill-holes. To do this. click on **File > import > elemts**.
In the import dialog choose the data format that is ASCII in our example. Then choose the geometry typ. In our case it´s point and sphere, so you have to do it
twice. And as third parameter you need to choose the destination coordinatesystem for the nominal geometries. In our case use the default PART coordinate system.
You can also choose other coordinate systems, if you create them via the toolbar before importing the nominal data.
<figure >
	<p align="middle"><img src="/documentation/images/usr/nominalSphere.png"></a> </p>
	<p align="middle"><i>The nominal spheres file for the measurement task </i></p>
</figure>
<figure >
	<p align="middle"><img src="/documentation/images/usr/nominalPoints.png"></a> </p>
	<p align="middle"><i>The nominal points file for the measurement task</i></p>
</figure>
<figure >
	<a href="../images/usr/importNominals.png"><img src="/documentation/images/usr/importNominals.png"></a> 
	<p align="middle"><i>import nominal dialog</i></p>
</figure><br>


###Create features to be measured
In the next step you need to create the features you want to measure. For this use the toolbar on the left or click on **view > show/hide feature toolbar**
to get the special toolbar for creating features. The standard toolbar on the left has some more functionality, because here you can also create scalar entities
and transformation parameters. In the toolbar on the left there is a button for each feature you can create. Clicking on opens the dialog, where you can put in
your settings and parameters for the features you want to create.

####Measurement config
For each feature you create, you need to specify a measurement configuration. It includes a name, number of measurements, that should be done and averaged,
the reading type (e.g. polar, only distance, only angle,...), two face measurement and so on.
You can specify it while creating the feature, or edit it later. To specify a complete measurement configuration the sensor must be set, to display the supported 
reading types. If the measurement config is not complet, it is displayed red in the tableview.
To specify the measurement config while creating a feature, click on the measurement config button in the create feature dialog or in the create feature toolbar.
To edit the measurement config of an existing feature, click **Station > measurement configuration** or on the measurement configuration in the buttion in
the menu. To edit a measurement config of a feature, you first need to select the feature, by clicking once with the left mouse buttion on it in the tableview.
It then is marked light blue.
<br><br>
For our task you need to create 10 points and 2 spheres. The names of the spheres are "Kugel" and "KugelAmBuegel". The count is 1 for each and they are not
common and not nominal. After creating both of them, they appear in the tableview. Next step, create the ten points for the focal points of the drill-holes.
Click on **Create point** type in the name "p1" count is 10. They are not common and not nominal. After clicking create, ten points will be created with names
p1 to p10. The names are count up and mathed together by input name and the value of count.
<figure >
	<a href="../images/usr/createFeatureToolbar.png"><img src="/documentation/images/usr/createFeatureToolbar.png"></a> 
	<p align="middle"><i>create feature toolbar</i></p>
</figure>
<figure >
	<a href="../images/usr/createPointDialog.png"><img src="/documentation/images/usr/createPointDialog.png"></a> 
	<p align="middle"><i>create point dialog</i></p>
</figure>
<br>

###Add functions
To solve a feature it is neccessary to add functions to it. Every function has input parameters, that you have to assign to it (e.g. obervations or a plane and a 
distance). For our task we first add a "Best Fit function" to the features we measure. For this, select a feature and click **Function > set function** or use the
button. Then choose your function by double clicking it. There are only functions displayed, that can be applied to the selected feature at this state.
After double clicking the next view appears, where you can add or remove the input parameters that the function needs. Also you see all functions that are 
applied to this feature in the order they are performed.
in our case we first add the best fit function and then measure the feature, so all observations of the feature are used for the best fit immediately. 
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

You can remove functions by righ clicking them in the function menu and choosing **delete function**.

###Measuring features
After the features are created, functions are applied and the sensor is set and connected, we can start measuring. For this select the feature you want 
to measure and click on **measure** in the sensor control pad. You can find it by clicking **View > sensor control** or by clicking on the button.
<figure >
	<a href="../images/usr/measure.png"><img src="/documentation/images/usr/measure.png"></a> 
	<p align="middle"><i>measure the current selected feature (p1)</i></p>
</figure>

Because we could not measure the focal point of the drillholes directly, we need to solve the problem by applying some functions.
<figure >
	<p align="middle"><img src="/documentation/images/usr/offSetProblem.JPG"></a> </p>
	<p align="middle"><i>unknown offset problem</i></p>
</figure>

To solve this problem, we measure the ground plane, and move it on its own normal vector with the value of the reflector radius. To do this use the function 
"ShiftPlane". 
After this we project our drill-hole points in the plane and have the offset corrected values we want.

####Scalar entity distance
To specify the distance value for moving the ground plane you need to crate a scalar entity distance by clicking on the button **Create Scalar entity**.
Choose a name, count is 1 and the value is the radius of the reflector. After creating you can solve the unknown offset problem.

###Transformation parameter
After measuring all neccessary features, we need to transform our measurements, because the nominal data for the feature is in the PART system whereas 
the actual feature data is in the instrument coordinate system. In order to do this, we need to create a transformation parameter via the button 
**Create transformation parameter**. In the dialog you have to give this parameter set a name and a start and destination coordinate system. After creating 
the parameter set it is displayed in its own tableview. Here you can select it by clicking one on it with the left mouse button. You can also edit the parameters 
by hand if you double click the transformation parameter set.

####Apply a function to the transformation parameter
After you created the transformation parameter you can set some values by hand (see paragraph above), or you can calculate parameters by applying a function 
to the trafo parameter feature. The default plugin, that we used for this measurement task, contains a 7 parameter helmert transformation, that we used in this 
case. So select the transformation parameter and go to the set function dialog, where you can apply the function to the feature.

####Apply checkpoints to the 7 parameter transformation
Now after selecting the function we need to apply the input parameter to the function. Here we need to apply the nominal (imported at the beginning) 
and actual (measured) checkpoints, specified at the beginning of the guide (p1,p3,p5,p7). After this step apply the dialog and the transformation parameter will be
calculated and applied on the feature.
<figure >
	<a href="../images/usr/7parameterTransformation.png"><img src="/documentation/images/usr/7parameterTransformation.png"></a> 
	<p align="middle"><i>applying checkpoints (nominal and actual) to the function</i></p>
</figure>

So now it is possible to transform between STATION01 and PART. You can do this, by switching the active coordinate system 
in the combobox right above the feature tableview. This combobox includes all existing coordinate systems whether instrument systems or part systems.

###Save and load project.
Now after the actual values can be transformed and compared with the nominal values of the features, it is possible to save all the work done in this 
project. For this click **File > save as** and specify a path and folder where OpenIndy should save the project.<br>
OpenIndy saves the project in an openindyXML.xml file. Here all features, their relations, observations, functions, ... are stored, so that it is possible to 
restore the complete project on a later time. To load and restore a project click **File > open project**.
<figure >
	<a href="../images/usr/exampleProjectFile.png"><img src="/documentation/images/usr/exampleProjectFile.png"></a> 
	<p align="middle"><i>extract of a project xml file</i></p>
</figure>
