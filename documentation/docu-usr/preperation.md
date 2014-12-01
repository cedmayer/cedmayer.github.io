---
layout: page
title: Preperation
excerpt: "User documentation for OpenIndy - Preperation"
author: usr
image:
  feature: banner/b_smr.jpg
---

<section id="table-of-contents" class="toc">
  <header>
    <h3>Preperation Overview</h3>
  </header>
<div id="drawer" markdown="1">
* bla
{:toc} 

</div>
</section><!-- /#table-of-contents -->

---

<a href="/documentation/docu-usr.html" class="btn">Overview</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/preperation.html" class="btn btn-success">Preperation</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/views.html" class="btn">Different Views</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/measurement.html" class="btn">Measurement</a>&nbsp;&nbsp;<a href="/documentation/docu-usr/functionalities.html" class="btn">Other Functionalities</a>&nbsp;&nbsp;

<br>

###Load plugins in OpenIndy

After starting OpenIndy, only the default plugin and its containing sensor functionalities (sensor + totalstation + lasertracker) is implemented.<br>
If you want to use other sensors and functions that are not implemented yet, you need to load the corresponding plugin in OpenIndy.<br>
To load another plugin click on the menu **Plugin > load plugin**. After this a new window opens, where you have to select the location of your plugin. After accepting the folder dialog, OpenIndy shows you the metadata of the selected plugin and checks if it is valid. Press Ok and then ok in the window to load it. OpenIndy will then automatically copy the plugin and all its dependencies to the plugin directory, so that you can work with it.
The next time you start OpenIndy, the plugins will be loaded automatically.
<figure >
	<a href="../images/usr/loadPlugin.png"><img src="/documentation/images/usr/loadPlugin.png"></a> 
	<p align="middle"><i>The "load plugin" dialog</i></p>
</figure>
