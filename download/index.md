---
layout: page
title: Latest Version
tags: [download, OpenIndy, QT, GitHub]
comments: true
image:
  feature: banner/b_nacht.jpg
---
<b>Version 0.1 Posted on December XXth, 2014</b>
<br>On this section you can download the latest executable version of OpenIndy.
<br><br>
<a markdown="0" href="" class="btn btn-success">Download Version 0.1</a>
<h1>Sourcecode</h1>
To join our community and support us in the ongoing development, you can also fork the repository of the project on [GitHub](https://github.com/OpenIndy/OpenIndy).
<br><br>
<a markdown="0" href="https://github.com/OpenIndy/OpenIndy" class="btn btn-info">Fork the Repository on GitHub</a>
<h1>Other Downloads</h1>
The development framework of your choice should be QT Creator which is designed to streamline the creation of applications and user interfaces to target multiple platforms with one code base.
<br><br>
<a markdown="0" href="http://qt-project.org/downloads" class="btn btn-info">Download Qt Creator</a>
<br><br>

Build Guide
====

----

IDE
----
As mentioned above, OpenIndy is developed with the [Qt framework](http://qt-project.org/downloads) (Qt libs + Qt Creator IDE).
To configure Qt Creator you may follow the instructions at the [Qt documentation](http://qt-project.org/doc/qtcreator-3.0/creator-configuring.html).

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

For more infos about plugins, check our [OiPluginTemplate Tutorial](/documentation/plugins.html)


