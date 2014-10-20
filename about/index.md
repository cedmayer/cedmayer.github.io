---
layout: page
title: About OpenIndy
tags: [about, OpenIndy, industrial, measurement, surveying, metrology, laser, tracker, tacheometer, tachymeter, Industrie, Vermessung]
modified: 2014-08-08T20:53:07.573882-04:00
comments: true
image:
  feature: banner_oi_proto.jpg
---

OpenIndy is a metrology software solution that can be extended by [plugins](https://github.com/OpenIndy/OiPluginTemplate). The project started in 2013 as a student project in the Department of Geoinformatics and Surveying ([HS Mainz](https://www.hs-mainz.de/technology/geoinformatics-and-surveying/index.html){:target="blank"}). Our primary goal is to attract students and to jointly develop and learn. For more information take a look at the [OpenIndy Documentation](/documentation).


OpenIndy is all about:
----
* much wow
* very nicely
* so money-saving



<a markdown="0" href="https://github.com/OpenIndy/OpenIndy" class="btn">Join & Fork The Project On GitHub</a>

{% comment %} 
    FRAGE: Aktuell öffnen sich Links im gleichen Fenster. 
    Sollen die Links in einem blank Fenster geöffnet werden?? (name)[url]{:target="blank"}
{% endcomment %}


Installation Guide
====

----

IDE
----
OpenIndy is developed with the Qt framework (Qt libs + Qt Creator IDE). You can download the framework [here](http://qt-project.org/downloads).

Dependencies
------------

- [openIndyLib](https://github.com/OpenIndy/OpenIndy/wiki/openIndy-lib-(linear-algebra))
- [amadillo c++ linear algebra library](http://arma.sourceforge.net)
- [BLAS/LAPACK](http://www.netlib.org/lapack/)
- [Qt](http://qt-project.org)
- [sqlite](https://sqlite.org)

Build
-----
The easiest way to build OpenIndy is to use the Qt Creator. You can build OpenIndy in `debug` or `release` mode.  

First you have to build the openIndyLib. It includes basic mathematic functionalities such as linear algebra algorithms.

* Qt Creator ----> `lib/openIndyLib/openIndyLib.pro`

Then build OpenIndy and copy the dependencies. 

* Qt Creator ----> `openindy.pro`
* Copy the openIndyLib (`lib/openIndyLib/bin/debug` || `lib/openIndyLib/bin/release`) 
and the oisystemdb.sqlite (`db/`) to `bin/debug` || `bin/release`. 
On Mac OS, you can find the binaries under `openindy/contents/MacOs`
* Include LAPACK/BLAS


BLAS and LAPACK
----------------

If using Linux:

* Use the Terminal (command line): `sudo apt-get install liblapack-dev`

If using Windows:

* Copy all dll's from `lib/armadillo-3.910.0/examples/lib_win64` 
to `bin/debug` || `bin/release`

If using Mac OS:

* Copy `lib/armadillo-3.910.0/examples/framework_mac/Accelerate.framework` to the Mac OS Library `/Library/Frameworks`

OpenGL and GLU
----------------

If using Linux:

* Use the Terminal (command line): 
    * `sudo apt-get install build-essential`
    * `sudo apt-get install libx11-xcb-dev libglu1-mesa-dev libxrender-dev`
     

Default Plugin
---------------
You can find the default plugin under `plugins/OiDefaultPlugin`. After you've built the plugin you can embed it via the GUI (`Plugin -> load plugins`). Also you can find a template for a plugin in the folder, which you can copy and start developing your own plugin.





