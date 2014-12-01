---
layout: page
title: Different Views
excerpt: "User documentation for OpenIndy - Different Views"
author: usr
image:
  feature: banner/b_smr.jpg
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>GUI Overview</h3>
  </header>
<div id="drawer" markdown="1">
* bla
{:toc} 

</div>
</section><!-- /#table-of-contents -->

---

<a href="/documentation/docu-usr.html" class="btn">Overview</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/preperation.html" class="btn">Preperation</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/views.html" class="btn btn-success">Different Views</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/measurement.html" class="btn">Measurement</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/functionalities.html" class="btn">Other Functionalities</a>&nbsp;&nbsp;

<br>
The graphical user interface of OpenIndy is made according to the principle of "one model, many views". So OpenIndy has some internal data logik, where it stores all dependencies, features and other things. The user has different possibilities to view this data and work with it. The following sections will display these views and describe them.

###Short Introducing of the graphical user interface (GUI)

The most frequently used functions are displayed as icons on a feature toolbar, allowing quick and easy access to the measurement information.
The forms-based graphical interaction helps you defining your measurement quickly and easily. 
Each form tab is labeled with its content, preventing you from getting lost in the defining process.


### Tableview

The tableview is a tabular representation of the features and their attributes. Each row represents one feature (e.g. one point) and the columns represent the metadata, attributes and values, number of observations for this geometry, measurement configuration and the functions of this feature.
The status of each row is represented in different colors:<br>
light gray = the colored feature type is a station<br>
dark gray = currently activated station<br>
light blue = active feature(s)<br>
white = measurement feature<br>
brown = nominal feature<br>
red = measurement config is not completely set
yellow columns = feature/attribute isn't solved yet<br>



<figure>
	<a href="/documentation/images/dev/tableview.png"><img src="/documentation/images/dev/tableview.png"></a>
	<p align="middle"><i>The table view</i></p>
</figure>


###Graphic view

The 3D graphic view gives the opportunity for a graphical representation of the features and their dependencies. The graphic view also contains a small tree view that contains all features and their attributes.
<figure>
	<a href="/documentation/images/dev/graphicView.png"><img src="/documentation/images/dev/graphicView.png"></a>
	<p align="middle"><i>The graphic view</i></p>
</figure>

###Console

The console actually is used as a output device for information, warnings and errors of currently executed functions and actions as well as interactions.
<figure>
	<a href="/documentation/images/dev/console.png"><img src="/documentation/images/dev/console.png"></a>
	<p align="middle"><i>The console</i></p>
</figure>

