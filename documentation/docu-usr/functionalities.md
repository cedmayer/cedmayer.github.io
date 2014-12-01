---
layout: page
title: Other Functionalities
excerpt: "User documentation for OpenIndy - Other Functionalities"
author: usr
image:
  feature: banner/b_smr.jpg
---

<section id="table-of-contents" class="toc">
  <header>
        <h3>Other Functionalities Overview</h3>
  </header>
<div id="drawer" markdown="1">
* bla
{:toc} 

</div>
</section><!-- /#table-of-contents -->

---

<a href="/documentation/docu-usr.html" class="btn">Overview</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/preperation.html" class="btn">Preperation</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/views.html" class="btn">Different Views</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/measurement.html" class="btn">Measurement</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/functionalities.html" class="btn btn-success">Other Functionalities</a>&nbsp;&nbsp;

<br>
Here is an abstract of useful functions, which you might find most useful.


###Watch window

The watch window ![watch window](/documentation/images/icons/watchwindow.png){: style="width: 25px"} can be found in the menu **View > watch window** and is a live stream of the actual readings of the active instrument (e.g. vertical and horizontal angle and distance). It makes it possible to 
watch the current position of the reflector and if it´s moving. With this functionality it is also possible to watch a position while someone works at it and 
corrects it to its nominal value. You always have the option to change the layout settings, the reading type and the displayed values shown in the watch window. The tolerance for the values can be set, too. At the very moment, the sensors value is below this tolerance, the equivalent attribute is highlighted green and not red anymore.
The watch window only displays the actual readings and does not store them. By pressing F3 on the keyboard, you can execute and save a measurement.
<figure >
	<p align="middle"><img src="/documentation/images/usr/watchwindow.png"></a> </p>
	<p align="middle"><i>the watch window</i></p>
</figure>

The plugin developer decides the values that get displayed in the view. So it is dynamic and variable for different tasks.


###Edit nominal data
In the example measurement we imported the nominal data from an ASCII file. It is also possible to create nominal features. To do this, just check the "is nominal" 
option in the create dialog of a feature and specify the destination system to which this nominal feature belongs to. Features are linked to each other by their name in the transformation. You 
can't have more than one actual feature with a identical name but multiple nominal features with the same name, if they have other destination systems (e.g. PART, OBJECTSYSTEM,...). 
While creating the nominal features by hand you have to specify the values by hand for each feature typ.<br> The values of nominal features can be 
edited by a right clicking on the nominal feature in the tableview and choosing the menu "show properties". Then just change the values and apply the dialog. 

###Readings, observations and statistic
If you do a right click on an actual feature and choose the "show properties" dialog, you get more information about this feature. In this dialog you can see the observation, which this feature has with all its attributes like measured values, date, time station, and so on. Also you can see the readings that are created from the observations 
with all their attributes. <br> The last tab is the statistic tab. Here you can find all the statistic of the feature and its functions. 
All statistic is seperated for each function of the feature, which you can switch on the combobox. The statistic of the functions is calculated with 
variance propagation and the resulting arrays can be viewed.

<figure >
	<a href="../images/usr/readings.png"><img src="/documentation/images/usr/readings.png"></a> 
	<p align="middle"><i>readings of the point feature</i></p>
</figure>
<figure >
	<a href="../images/usr/observations.png"><img src="/documentation/images/usr/observations.png"></a> 
	<p align="middle"><i>observations of the point feature</i></p>
</figure>
<figure >
	<a href="../images/usr/statistic.png"><img src="/documentation/images/usr/statistic.png"></a> 
	<p align="middle"><i>statistic of the best fit function of the point feature</i></p>
</figure>



###Station
On a station you can also open the "show properties" dialog. There you can see the sensor configuration of the current active instrumentas.
So you can change the connection parameters, the accuracy and all the other instrument parameters as well as the connection type. You can´t change the instrument in this dialog. To do this you need to click **Station > set instrument**.


###Multiple stations
It is possible to have more stations in OpenIndy, because this is neccessary to solve some measurement tasks. All stations are marked light grey in the 
tableview. The current active station is marked in a darker grey. On default when you start OpenIndy STATION01 is activated. You can switch the active station 
by marking the one you want to activate in the tableview with one left mouse button click. When it´s marked light blue (active feature) go to 
**Station > activate station**. The selected station will then be the active station and you can connect a sensor to it.


###Sensor functionalities
This section gives a brief description of all current implemented sensor functionalities (sensor + totalstation + lasertracker). The implementation itself 
depends on the plugin developer and on the implemented sensor, so it can vary between different sensors.

####Connect
Connects OpenIndy and the sensor

####Disconnect
Disconnects OpenIndy and the sensor

####Initialize
Initializes the sensor

####Home
Sets the sensor to his home position (lasertracker)

####Measure
starts a measurement

####Toggle sight orientation
Changes the sensor from frontside to backside and the other way

####Aim
Aims towards the active feature position. The feature needs to be solved for this functionality, that means it has some valid X Y and Z coordinates.

####Move
The user can specify where the sensor should aim towards

####Change motor state
Enables or disables the motor (lasertracker)

####Compensation
Starts the compensation (lasertracker)

###Change the software settings
In the menu **Settings > view settings** you can change the used units, the displayed attributes of the table view and you see the plugins information like descriptions of the usable functions and sensors of the loaded plugins.