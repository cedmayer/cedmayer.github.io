---
layout: page
title: Plugins / OiPluginTemplate 
modified: 2014-07-31T13:23:02.362000-04:00
excerpt: "This tutorial supports you to learn how to write and run your own plugins using this project as a template."
author: dev
image:
  feature: banner/b_tracker2.jpg
---


<section id="table-of-contents" class="toc">
  <header>
    <h3>Plugins / OiPluginTemplate Overview</h3>
  </header>
<div id="drawer" markdown="1">
* bla
{:toc} 

</div>
</section><!-- /#table-of-contents -->

---


<a href="/documentation/docu-dev.html" class="btn">Overview</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/concept.html" class="btn">Concept and Architecture</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/plugins.html" class="btn btn-success">Plugins</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/interface.html" class="btn">Server Interface</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/gui.html" class="btn">Model View Control</a>

## Plugin loader

plugin loader

---

##OiPluginTemplate Tutorial

OiPluginTemplate is a predefined Qt project which we recommend you to use if you want to write a plugin for OpenIndy. In the next chapters it is assumed that you know the terminology of OpenIndy (like element, feature, function, sensor etc.). If you do not know the concept of OpenIndy yet, you may want to inform yourself [here](/documentation/docu-dev/concept.html#concept). Read the following steps to learn how to write and run your own plugins using this project as a template.


##Get started

First you need to get a copy of the Qt-project "OiTemplatePlugin" on your local machine. Therefor you have two options available:  

On the one hand you might fork this repository (OiPluginTemplate) and get a copy of it on your local machine. If you are new to GitHub you may want to follow the guidance at [https://help.github.com/articles/fork-a-repo](https://help.github.com/articles/fork-a-repo). 
On the other hand you might also fork and clone the OpenIndy repository. That repository contains a folder `plugins` which may look like this:

![oiPluginFolders](/documentation/images/dev/oiPluginFolders.png) 

To get your own plugin you have to copy the folder `OiPluginTemplate` and paste it again next to it. You may choose any name for your new folder.

No matter which option you choose you get one Qt-project for your own plugin. Every Qt-project is represented by a `*.pro`-file. In your case it is the file `OiTemplatePlugin.pro`. If you choose the first option the folder structure of the Qt-project will look like this:

![oiPluginFolders](/documentation/images/dev/pluginTemplateFolderStructure.png) 

However if you choose the second option the folder structure will look as follows:

![oiPluginFolders](/documentation/images/dev/OiPluginTemplateFolderStructure.png) 

How to work on your own plugin and what you have to take in account is explained in the next chapters.

Furthermore you now need to install Qt Creator. You can download it [here](http://qt-project.org/downloads). To configure Qt Creator you may follow the instructions at the [Qt documentation](http://qt-project.org/doc/qtcreator-3.0/creator-configuring.html).

You are now ready to get started with the implementation of your own plugin. Notice that the content of the folders `src` and `lib`, which came with the template (only if you have chosen option 1 above), must not be changed. You may add other libraries to the `lib` folder, but existing content has to stay as it is if you do not want to get undesirable behaviour.

##Write plugins

OpenIndy provides interfaces for functions, sensors and network adjustments. How to implement them and integrate them in a plugin is described in the next three subchapters. It is explained in detail what you have to take in account when implementing such a plugin. In contrast there is also a step by step guide in the next chapter that leads you through a tutorial where you implement a small plugin with a function and a sensor.  
In general it is important to know that neither the name of the pro-file [`OiTemplatePlugin.pro`](https://github.com/OpenIndy/OiPluginTemplate/blob/master/OiTemplatePlugin.pro) nor the names of the files [`p_factory.h`](https://github.com/OpenIndy/OiPluginTemplate/blob/master/p_factory.h), [`p_factory.cpp`](https://github.com/OpenIndy/OiPluginTemplate/blob/master/p_factory.cpp) and [`metaInfo.json`](https://github.com/OpenIndy/OiPluginTemplate/blob/master/metaInfo.json) must be changed.

### Plugin meta data
{:.no_toc}

The file [`metaInfo.json`](https://github.com/OpenIndy/OiPluginTemplate/blob/master/metaInfo.json) should contain meta information about your plugin. The meaning of each attribute is described in the table below.

<div class="CSSTableGenerator" >
<table>
    <tr>
      <td style="text-align: left">attribute</td>
      <td style="text-align: left">description</td>
    </tr>
    <tr>
      <td style="text-align: left">isOiPlugin</td>
      <td style="text-align: left">Set this parameter to <code>true</code> so that OpenIndy recognises your plugin as valid.</td>
    </tr>
    <tr>
      <td style="text-align: left">name</td>
      <td style="text-align: left">The name of your plugin.</td>
    </tr>
    <tr>
      <td style="text-align: left">pluginVersion</td>
      <td style="text-align: left">The version of your plugin. This version has to correspond with the version <code>OiPlugin_iidd</code> which is defined in the file <a href="https://github.com/OpenIndy/OpenIndy/blob/master/src/plugin/pi_oiplugin.h"><code>pi_oiplugin.h</code></a>.</td>
    </tr>
    <tr>
      <td style="text-align: left">author</td>
      <td style="text-align: left">You can enter your name or the name of your company here.</td>
    </tr>
    <tr>
      <td style="text-align: left">compiler</td>
      <td style="text-align: left">This attribute has to contain the compiler with which you are going to compile your plugin. You might choose <code>MVSC 64bit</code> (Microsoft Visual Studio), <code>minGW</code> (GNU GCC/G++), <code>clang 64bit</code> (Clang/LLVM), <code>intel</code> (Intel ICC/ICPC), <code>intelC</code> (IBM XL C/C++), <code>hewlett</code> (Hewlett-Packard C/aC++), <code>portland</code> (Portland Group PGCC/PGCPP) or <code>oracle</code> (Oracle Solaris Studio). This is important for OpenIndy to know, because a plugin can only be loaded in OpenIndy if OpenIndy has been compiled with the same compiler.</td>
    </tr>
    <tr>
      <td style="text-align: left">operatingSystem</td>
      <td style="text-align: left">Here you have to enter the operating system on which you compile your plugin. There are three options: “windows”, “linux” or “mac os”.</td>
    </tr>
    <tr>
      <td style="text-align: left">description</td>
      <td style="text-align: left">A description of your plugin.</td>
    </tr>
    <tr>
      <td style="text-align: left">dependencies</td>
      <td style="text-align: left">Set this parameter to <code>true</code> if your plugin has some dependencies that have to be copied to the executable of OpenIndy.</td>
    </tr>
    <tr>
      <td style="text-align: left">libPaths</td>
      <td style="text-align: left">If you set the above parameter to <code>true</code> then you can add here as many dependencies as you want. One dependency is represented by a <code>name</code> and a <code>path</code>. Set <code>name</code> to the file name of the library (including the file extension) or the name of a hole folder. That library or folder has to be placed next to your plugin library (e.g. <code>*.dll</code>). You might also enter a hole path to a library like “myLib/dependency.dll”. And in addition set the attribute <code>path</code> to “app” so that the dependency (a library or a folder) will be put next to the executable of OpenIndy.</td>
    </tr>
</table>
</div>
<br><br>

Now that you have set up the Json-file you can start with the actual implementation of your plugin.

### Plugin factory
{:.no_toc}

The [class OiTemplatePlugin](https://github.com/OpenIndy/OiPluginTemplate/blob/master/p_factory.h), which is defined in files [`p_factory.h`](https://github.com/OpenIndy/OiPluginTemplate/blob/master/p_factory.h) and [`p_factory.cpp`](https://github.com/OpenIndy/OiPluginTemplate/blob/master/p_factory.cpp), implements the actual interface for every plugin which itself is defined in the header file [pi_oiplugin.h](https://github.com/OpenIndy/OpenIndy/blob/master/src/plugin/pi_oiplugin.h).

That interface looks like this:

{% highlight c++ %}
class OiPlugin
{
public:
    virtual ~OiPlugin(){}

    virtual QList<Sensor*> createSensors() = 0;
    virtual QList<Function*> createFunctions() = 0;
    virtual QList<NetworkAdjustment*> createNetworkAdjustments() = 0;
    virtual Sensor* createSensor(QString name) = 0;
    virtual Function* createFunction(QString name) = 0;
    virtual NetworkAdjustment* createNetworkAdjustment(QString name) = 0;
};
{% endhighlight %}

As you can see there are six methods which must be implemented. The methods `createSensors`, `createFunctions` and `createNetworkAdjustments` return a list of pointers to sensors, functions and network adjustments respectively. These lists shall contain all sensors, functions and network adjustments which you have implemented in your plugin. The methods `createSensor`, `createFunction` and `createNetworkAdjustment`, which expect the string `name` as a parameter, return only one sensor, function and network adjustment respectively. The parameter `name` represents the name of the sensor, function or network adjustment. So each of these three methods shall return only the proper pointer or NULL if no sensor, function or network adjustment with the given name is available. Certainly this becomes more clear when looking at an example:

Assuming that you have implemented two sensors called "sensor a" and "sensor b" and in addition two functions called "function a" and "function b" before, your implementation of the six methods above might look as follows:

{% highlight c++ %}
QList<Sensor*> OiTemplatePlugin::createSensors(){
    QList<Sensor*> resultSet;
    Sensor *a = new SensorA();
    Sensor *b = new SensorB();
    resultSet.append(a);
    resultSet.append(b);
    return resultSet;
}

QList<Function*> OiTemplatePlugin::createFunctions(){
    QList<Function*> resultSet;
    Function *a = new FunctionA();
    Function *b = new FunctionB();
    resultSet.append(a);
    resultSet.append(b);
    return resultSet;
}

QList<NetworkAdjustment*> OiTemplatePlugin::createNetworkAdjustments(){
    QList<NetworkAdjustment*> resultSet;
    return resultSet;
}

Sensor* OiTemplatePlugin::createSensor(QString name){
    Sensor *result = NULL;
    if(name.compare("sensor a") == 0){
        result = new SensorA();
    }else if(name.compare("sensor b") == 0){
        result = new SensorB();
    }
    return result;
}

Function* OiTemplatePlugin::createFunction(QString name){
    Function *result = NULL;
    if(name.compare("function a") == 0){
        result = new FunctionA();
    }else if(name.compare("function b") == 0){
        result = new FunctionB();
    }
    return result;
}

NetworkAdjustment* OiTemplatePlugin::createNetworkAdjustment(QString name){
    NetworkAdjustment *result = NULL;
    return result;
}
{% endhighlight %}

If you do not add your functions, sensors and network adjustments, which are implemented in your plugin, to the six methods above then OpenIndy will not be able to use them. In other words you can think of the [class OiTemplatePlugin](https://github.com/OpenIndy/OiPluginTemplate/blob/master/p_factory.h) as a factory which creates and delivers function-, sensor- and network adjustment objects from the plugin to OpenIndy as soon as it is required.

### Implement functions, sensors and network adjustments
{:.no_toc}

Now you know how to implement the plugin interface of OpenIndy, what is important to instantiate your function-, sensor- and network adjustment classes and deliver the resulting objects to OpenIndy. To learn how to implement your own functions, sensors and network adjustment you can follow the links below.

* [implement functions](#function)
* [implement sensors](#sensor)
* [implement network adjustments](#network-adjustment)

##Function

### Introduction
{:.no_toc}

Every feature in OpenIndy can have any number of functions. There are construct-, fit- and geodetic functions and furthermore object transformations and system transformations. The list below gives you a review of those function types.

<div class="CSSTableGenerator" >
<table>
    <tr>
      <td style="text-align: left">function type</td>
      <td style="text-align: left">description</td>
    </tr>
    <tr>
      <td style="text-align: left">fit function</td>
      <td style="text-align: left">If overdetermination is present, fit functions have the purpose to calculate the adjusted parameters of a geometry by using observations. So for example a fit function can calculate the center of n xyz-observations.</td>
    </tr>
    <tr>
      <td style="text-align: left">construct function</td>
      <td style="text-align: left">Construct functions are used to calculate geometries by using other geometries. For example the intersection of a line and a plane results in a new point.</td>
    </tr>
    <tr>
      <td style="text-align: left">geodetic function</td>
      <td style="text-align: left">Geodetic functions are intended to calculate special geodetic tasks like spatial intersection etc..</td>
    </tr>
    <tr>
      <td style="text-align: left">object transformation</td>
      <td style="text-align: left">In contrast to fit- and construct functions, object transformations do not define a geometry, but change a previously defined one. A point which has been fit before can for instance be moved along a line by a certain amount.</td>
    </tr>
    <tr>
      <td style="text-align: left">system transformation</td>
      <td style="text-align: left">System transformations are used to calculate the parameters of a transformation from one coordinate system to another one.</td>
    </tr>
</table>
</div>
<br><br>

These function types are implemented as classes which extend the [class Function](https://github.com/OpenIndy/OpenIndy/blob/master/src/function.h). When you want to write your own function in your plugin then you have to extend one of the classes [FitFunction](https://github.com/OpenIndy/OpenIndy/blob/master/src/plugin/pi_fitfunction.h), [ConstructFunction](https://github.com/OpenIndy/OpenIndy/blob/master/src/plugin/pi_constructfunction.h), [GeodeticFunction](https://github.com/OpenIndy/OpenIndy/blob/master/src/plugin/pi_geodeticfunction.h), [ObjectTransformation](https://github.com/OpenIndy/OpenIndy/blob/master/src/plugin/pi_objecttransformation.h) or [SystemTransformation](https://github.com/OpenIndy/OpenIndy/blob/master/src/plugin/pi_systemtransformation.h). Please do not try to extend the [class Function](https://github.com/OpenIndy/OpenIndy/blob/master/src/function.h) itself. While this currently works for some kind of function types for others it does not because additional methods and attributes are necessary which are defined only in the subclasses.

Let us first look at the [class Function](https://github.com/OpenIndy/OpenIndy/blob/master/src/function.h) itself. There are some methods that you always have to override (pure virtual methods). This methods are pure virtual in the five subclasses of the [class Function](https://github.com/OpenIndy/OpenIndy/blob/master/src/function.h), too.:

### Pure virtual methods
{:.no_toc}

{% highlight c++ %}

virtual QList<InputParams> getNeededElements() = 0;
virtual QList<Configuration::FeatureTypes> applicableFor() = 0;
virtual PluginMetaData* getMetaData() = 0;
{% endhighlight %}
The method `getNeededElements` has to return a list of `InputParams`. `InputParams` is a struct and is defined in the upper part of the header file from [class Function](https://github.com/OpenIndy/OpenIndy/blob/master/src/function.h). An input parameter represents an element that is required for the calculation of the actual function. For example if you want to implement a construct function which calculates the intersection of a line and a plane, you might implement the method as follows:

{% highlight c++ %}
QList<InputParams> IntersectLinePlane::getNeededElements(){
    QList<InputParams> result;
    InputParams param1;
    param1.index = 0;
    param1.description = "Select a line to calculate the intersection.";
    param1.infinite = false;
    param1.typeOfElement = Configuration::eLineElement;
    result.append(param1);
    InputParams param2;
    param2.index = 1;
    param2.description = "Select a plane to calculate the intersection.";
    param2.infinite = false;
    param2.typeOfElement = Configuration::ePlaneElement;
    result.append(param2);
    return result;
}
{% endhighlight %}
The [class Configuration](https://github.com/OpenIndy/OpenIndy/blob/master/src/configuration.h) contains the enumeration `ElementTypes` which has a value for each available element. Each input parameter that you add to the result list has to "know" which element type it represents (`typeOfElement`) and how often this element type can be included (`infinite`). If `infinite` is `true` a user can enter as many elements of the specified type as he wants. Otherwise only one element is expected. In addition you can optionally define a description for each input parameter, so that the user of your plugin will know about the use of each parameter.  

The second method `applicableFor` has to return a list of feature types. As well as `ElementTypes`, `FeatureTypes` is an enumeration defined in the [class Configuration](https://github.com/OpenIndy/OpenIndy/blob/master/src/configuration.h) and has one value for each available feature. The result list has to contain every feature type which this function can be assigned to. Assuming you wrote a function called "TranslateByLine" which is able to move a point or a sphere along a line by a specified amount then you have to add the enum values of the features point and sphere to the result list. Your implementation of the method might look like this:
{% highlight c++ %}
QList<Configuration::FeatureTypes> TranslateByLine::applicableFor(){
    QList<Configuration::FeatureTypes> result;
    result.append(Configuration::ePointFeature);
    result.append(Configuration::eSphereFeature);
    return result;
}
{% endhighlight %}
Last but not least there is the method `getMetaData` which has to return a pointer to an object of the type [PluginMetaData](https://github.com/OpenIndy/OpenIndy/blob/master/src/pluginmetadata.h). In the meta data object that is returned in your implementation of this method you can define for example a name and a description for your function. It is not necessary that you fill all attributes that you can find in the [class PluginMetaData](https://github.com/OpenIndy/OpenIndy/blob/master/src/pluginmetadata.h), but a minimal implementation should contain an iid, a name, a description and the name of the plugin which the function belongs to. The name and the description that you chose for your function are later shown in the function plugin loader of OpenIndy (after you have successfully installed your plugin) where users can assign functions to features. As the name of the plugin you have to chose the name which you already defined in the json file (next to the pro file of your Qt project). The iid has a special structure:
{% highlight c++ %}
de.openIndy.Plugin.Function.[TYPE_OF_FUNCTION].[VERSION_OF_PLUGIN]
{% endhighlight %}
Replace `[TYPE_OF_FUNCTION]` by "FitFunction", "ConstructFunction", "GeodeticFunction", "ObjectTransformation" or "SystemTransformation" for your specific case. As `[VERSION_OF_PLUGIN]` please chose the same as you did in your json file, but format it like "v001".  

### Virtual methods
{:.no_toc}

{% highlight c++ %}
virtual bool exec(Station&);
virtual bool exec(CoordinateSystem&);
virtual bool exec(TrafoParam&);
virtual bool exec(Point&);
virtual bool exec(Line&);
virtual bool exec(Plane&);
virtual bool exec(Sphere&);
virtual bool exec(Circle&);
virtual bool exec(Cone&);
virtual bool exec(Cylinder&);
virtual bool exec(Ellipsoid&);
virtual bool exec(Hyperboloid&);
virtual bool exec(Nurbs&);
virtual bool exec(Paraboloid&);
virtual bool exec(PointCloud&);
virtual bool exec(ScalarEntityAngle&);
virtual bool exec(ScalarEntityDistance&);  

virtual QMap<QString, int> getIntegerParameter();
virtual QMap<QString, double> getDoubleParameter();
virtual QMap<QString, QStringList> getStringParameter();  

virtual QStringList getResultProtocol();  

virtual void clear();
virtual void clearResults();
{% endhighlight %}
For each feature that is currently defined in OpenIndy there is an `exec` method in the [class Function](https://github.com/OpenIndy/OpenIndy/blob/master/src/function.h). OpenIndy will call the `exec` method of a function each time a feature which that function is assigned to has to be recalculated. A recalculation of a feature is necessary whenever one or more elements which that feature depends on are changed. This is the case for example when you add a new observation to the fit function of the feature or indirectly when your feature is constructed from another feature which is now changed (for example because of a new observation added). Therefore your `exec` methods have to contain the actual implementation of your algorithm (of course you can subdivide your implementation into your own methods which you then call from the `exec` method). As the `exec` methods are not pure virtual you do not have to implement all of them. If you want to implement the intersection of a line and a plane then of course you only have to implement the `exec` method for the type point. But be sure that your choice of `exec` methods which you want to implement is consistent with how you implemented the method `applicableFor`. Read the part about the implementation of `exec` methods below to get a detailed description on what you have to consider when implementing an `exec` method.  
The methods `getIntegerParameter`, `getDoubleParameter` and `getStringParameter` are meant to give you the chance to define special parameters for your function. As you learned before the method `getNeededElements` allows you to define elements (e.g. observations, points, planes etc.) as input parameters. Your function can use this elements to calculate whatever you want. But in some cases you maybe need to ask an user for special parameters (like wether you shall calculate a transformation with or without scale). Therefore you can implement those three methods and thereby tell OpenIndy to ask users for those parameters. Let us look at an example:
{% highlight c++ %}
QMap<QString, int> TranslateByLine::getIntegerParameter(){
    QMap<QString, int> result;
    QString key = "paramA";
    int defaultValue = 1;
    result.insert(key, defaultValue);
    return result;
}

QMap<QString, double> TranslateByLine::getDoubleParameter(){
    QMap<QString, double> result;
    QString key = "paramB";
    double defaultValue = 12.34;
    result.insert(key, defaultValue);
    return result;
}

QMap<QString, QStringList> TranslateByLine::getStringParameter(){
    QMap<QString, QStringList> result;
    QString key = "paramC";
    QStringList defaultValues;
    defaultValues.append("option A");
    defaultValues.append("option B");
    defaultValues.append("option C");
    result.insert(key, value);
    return result;
}
{% endhighlight %}

You can define three different types of parameters: integer-values, double-values and string-values. Each parameter you define has a key and a default value. In contrast to integer- and double-parameters string-parameters are defined as lists. So you define multiple options and users can select one of those optionse or they can type in a custom string. Default value is always the first entry in the list, in this case that would be the string "option A". We recommend you to use only one single string as a key and define the description of each parameter in the description of the whole function. In OpenIndy your parameters appear as follows:  

<br>
![extra Parameter Example](/documentation/images/dev/extraParameterExample.png)
<br><br>

In the above illustration you can see a string-parameter with the key "invert" and the possible options "yes" and "no".  

Furthermore there is the method `getResultProtocol` which you can override. Your implementation of this method might return a list of strings which shows special results or information on your calculations that otherwise would not have been shown.  

Last but not least you might override the methods `clear` and `clearResults`. As the name implies the method `clearResults` erases all results of the last execution of the function. That concerns for example the statistic of that function. On the contrary the method `clear` erases not only those results, but the whole setup of the function. This also includes all elements which were assigned to the function. If you plan to reimplement this two methods then you might want to take a look at the implementation in the [class Function](https://github.com/OpenIndy/OpenIndy/blob/master/src/function.h).

### Further non-virtual methods
{:.no_toc}

There are some further methods which you cannot reimplement, but which are important to know and use when implementing a function. First of all there is the method `setUseState`. In your implementation you mostly use other elements to calculate one feature. As you will learn in the next part not every element is valid, so it may occur that while most of the given elements are used for calculation some are not. So for each element you should call this method and tell it wether to use the element or not.
Moreover there is the method `isValid` which returns true if all elements that are necessary for calculation are present. This method uses your implementation of `getNeededElements` to decide wether all elements are present or not.

It is also possible to access the console of OpenIndy from the function class. Therefore you can use the method `writeToConsole` by passing your message as a parameter. This might be useful for execptions for example. Furthermore there are methods to get a special element by its id or a whole list of all elements of a special type (e.g. observation, point, plane etc.).

As you can see when looking at the definition of the [class Function](https://github.com/OpenIndy/OpenIndy/blob/master/src/function.h) there are a a few other methods not mentioned yet. Those methods are not important when implementing a function. They are called from OpenIndy to get information on the current setup of the function (e.g. statistic or assigned elements etc.).

### Implementation of `exec` methods
{:.no_toc}

As you have learned recently an `exec` method contains the actual implementation of you algorithm. As input you get a reference to the feature which this function is assigned to and which your implementation has to modify. This input feature may have been solved by previous functions. If so you can access this feature's attributes to get the result of those functions. If the feature is a geometry there is a statistic object, defined in the [class Geometry](https://github.com/OpenIndy/OpenIndy/blob/master/src/geometry.h), with the current statistic of the geometry. Besides that input feature you also need the elements which you defined in your implementation of the method `getNeededElements` for your calculation.

##Sensor

sensor

##Network adjustment

network adjustment

##Plugin implementation - <br>A step by step guide

This section leads you through the implementation of a simple plugin for OpenIndy. You will implement a function and you will learn how to make your plugin ready for the use in OpenIndy.

## - Initial steps

After you have completed the [Get Started](#get-started) chapter you have got a local copy of the OiTemplatePlugin-project. The folder structure of your local project should look like the left image if you have chosen to fork the [OiPluginTemplate](https://github.com/OpenIndy/OiPluginTemplate) repository. Otherwise, if you have forked the [OpenIndy](https://github.com/OpenIndy/OpenIndy) repository, the folder structure of your local project should look like the right image.

<table>
  <tbody>
    <tr>
      <td> <img src="/documentation/images/dev/pluginTemplateFolderStructure.png" /> </td>
      <td> <img src="/documentation/images/dev/OiPluginTemplateFolderStructure.png" /> </td>
    </tr>
  </tbody>
</table>

Before you open the plugin-project you should open and compile the "openIndyLib"-project. That project contains the classes for linear algebra. If you have forked the [OiPluginTemplate](https://github.com/OpenIndy/OiPluginTemplate) repository you can find the `openIndyLib.pro`-file under `/lib/openIndyLib`. In contrast if you have forked the [OpenIndy](https://github.com/OpenIndy/OpenIndy) repository you can find the `openIndyLib.pro`-file under `../../lib/openIndyLib`. 
In both cases it is assumed that you navigate to the `openIndyLib`-folder starting at the folder which is shown in the images above respectively. 

You can open Qt projects (in Qt Creator) by clicking "File" > "Open File or Project..." and selecting your `*.pro`-file. To compile a project select "Build" > "Run qmake" and afterwards "Build" > "Rebuild Project xy".  
<br>
Now, after you have compiled the "openIndyLib"-project, please open the OiTemplatePlugin-project.

### Plugin meta data
{:.no_toc}

Let us first set up the file `metaInfo.json`. Please copy the following lines to your `metaInfo.json`-file:  
{% highlight javascript %}
{
    "isOiPlugin": true,

    "name"  : "Plugin Tutorial",
    "pluginVersion" : "1.0.0",
    "author"  : "OpenIndyOrg",
    "compiler" : "MVSC 64bit",
    "operatingSystem" : "windows",

    "description" : "This is an example plugin. It is meant as a reference for you to create your own plugins.",

    "dependencies" : false,
    "libPaths" : [
        { "name": "dependencies", "path": "app" }
    ]
}
{% endhighlight %}

If you are not working on a Windows system with the Microsoft Visual Studio Compiler you have to replace "windows" or "MSVC 64bit" respectively with the appropriate value. You might replace "windows" by either "linux" or "mac os" if you are working on a Linux or Mac system. The following table gives you a review of all possible values for the compiler.  

<div class="CSSTableGenerator" >
<table>
    <tr>
      <td style="text-align: left">compiler</td>
      <td style="text-align: left">value</td>
    </tr>
    <tr>
      <td style="text-align: left">Microsoft Visual Studio</td>
      <td style="text-align: left">MVSC 64bit</td>
    </tr>
    <tr>
      <td style="text-align: left">GNU GCC/G++</td>
      <td style="text-align: left">minGW</td>
    </tr>
    <tr>
      <td style="text-align: left">Clang/LLVM</td>
      <td style="text-align: left">clang 64bit</td>
    </tr>
    <tr>
      <td style="text-align: left">Intel ICC/ICPC</td>
      <td style="text-align: left">intel</td>
    </tr>
    <tr>
      <td style="text-align: left">IBM XL C/C++</td>
      <td style="text-align: left">intelC</td>
    </tr>
    <tr>
      <td style="text-align: left">Hewlett-Packard C/aC++</td>
      <td style="text-align: left">hewlett</td>
    </tr>
    <tr>
      <td style="text-align: left">Portland Group PGCC/PGCPP</td>
      <td style="text-align: left">portland</td>
    </tr>
    <tr>
      <td style="text-align: left">Oracle Solaris Studio</td>
      <td style="text-align: left">oracle</td>
    </tr>
</table>
</div>
<br>

So for example if you are working on a linux system with the GNU GCC/G++ compiler you have to replace "windows" by "linux" and "MSVC 64bit" by "minGW".  
As there are no dependencies (e.g. external libraries) for this example plugin the parameter "dependencies" is set to `false`.    

You are now ready to start with the implementation of your first function.

## - Your first function

In this subchapter you will implement a function for fitting a point. As input that function expects any number of XYZ-observations. The function then calculates the central point and the covariance matrix of the averaged point.

### Set up the function
{:.no_toc}

Initially you have to create two empty files `pointFit.h` and `pointFit.cpp` next to the `OiTemplatePlugin.pro`-file. Afterwards you can add that two files to your Qt-project by right clicking on the project entry (OiTemplatePlugin) in the project treeview of Qt Creator and selecting the menu item "add existing files". Browse to those two files, `pointFit.h` and `pointFit.cpp`, and add them to your project. They also appear in the `OiTemplatePlugin.pro`-file now, so you do not have to modify it yourself.  

Now you can start implementing your function. Please copy the following lines to the `pointFit.h`-file.
{% highlight c++ %}
#ifndef POINTFIT_H
#define POINTFIT_H

#include "pi_fitfunction.h"
#include "configuration.h"
#include "pluginmetadata.h"

class PointFit : public FitFunction
{

public:
    PluginMetaData* getMetaData();
    QList<InputParams> getNeededElements();
    QList<Configuration::FeatureTypes> applicableFor();

};

#endif // POINTFIT_H
{% endhighlight %}
The corresponding `cpp`-file `pointFit.cpp` has to look as follows:
{% highlight c++ %}
#include "pointFit.h"

PluginMetaData* PointFit::getMetaData(){
    PluginMetaData* metaData = new PluginMetaData();
    metaData->name = "PointFit";
    metaData->pluginName = "Plugin Tutorial";
    metaData->author = "OpenIndyOrg";
    metaData->description = QString("%1 %2")
            .arg("This function calculates an adjusted point.")
            .arg("You can input as many observations as you want which are then used to find the best fit 3D point.");
    metaData->iid = "de.openIndy.Plugin.Function.FitFunction.v001";
    return metaData;
}

QList<InputParams> PointFit::getNeededElements(){
    QList<InputParams> result;
    InputParams param;
    param.index = 0;
    param.description = "Select observations to calculate the best fit point.";
    param.infinite = true;
    param.typeOfElement = Configuration::eObservationElement;
    result.append(param);
    return result;
}

QList<Configuration::FeatureTypes> PointFit::applicableFor(){
    QList<Configuration::FeatureTypes> result;
    result.append(Configuration::ePointFeature);
    return result;
}
{% endhighlight %}

Now you have a minimum implementation of a function. The name and the description you have defined in the method `getMetaData` are important, because they appear later when using your function in OpenIndy. That description tells the users of your function how this function works and what it does. In the method `getNeededElements` it is determined what elements your function needs to be able to calculate something. As your function calculates the center of n observations, of course, you need some observations. This is defined in the parameter `typeOfElement`. 

Again there is a description parameter which will also appear later in OpenIndy. That description tells the user of your function what the element (observation) is used for in the function. The parameter `infinite` determines that the user of your function might add as many observations to the function as he wants. At last there is the method `applicableFor` which tells OpenIndy that your function can only be assigned to a point feature.  

For test purpose you might also compile your plugin now, if you want. There is no actual algorithm implemented, so this function is useless at the moment. Therefor let us continue by adding the algorithm for fitting a point.

### Add the actual functionality to the function
{:.no_toc}

Please add the definition of an `exec` method for a point feature to the header file `pointFit.h`. Your header file should look like this now:
{% highlight c++ %}
#ifndef POINTFIT_H
#define POINTFIT_H

#include "pi_fitfunction.h"
#include "configuration.h"
#include "pluginmetadata.h"

class PointFit : public FitFunction
{

public:
    PluginMetaData* getMetaData();
    QList<InputParams> getNeededElements();
    QList<Configuration::FeatureTypes> applicableFor();

    bool exec(Point&);

};

#endif // POINTFIT_H
{% endhighlight %}
At the very bottom of the corresponding `cpp`-file (`pointFit.cpp`) please add the implementation of that `exec` method as follows:
{% highlight c++ %}
bool PointFit::exec(Point &p){
    if(this->isValid()){

    }
    return false;
}
{% endhighlight %}
The method `isValid`, which is called here, is defined in the function class and checks wether all needed elements are available. In this `exec` method you can now implement the calculation of the central point. The first thing you need is a list of all observations that shall be used for calculation. Therefor please change the implementation of your `exec` method as follows:
{% highlight c++ %}
bool PointFit::exec(Point &p){
    if(this->isValid() && this->featureOrder.contains(0)){
        QList<Observation*> myObservations;
        QList<InputFeature> myObservationIds = this->featureOrder.value(0);
        foreach(InputFeature obs, myObservationIds){
            Observation *myObservation = this->getObservation(obs.id);
            if(myObservation != NULL && myObservation->isValid){
                myObservations.append(myObservation);
            }else{
                this->setUseState(obs.id, false);
            }
        }
        if(myObservations.size() > 0){
            //TODO
        }
    }
    return false;
}
{% endhighlight %}
The attribute `featureOrder` is a map, defined in the function class, which contains the id's of all elements that were assigned to your function by the user of OpenIndy in a special order. In this case that map has only got the key "0", because you only defined one needed element (observations) in the method `getNeededElements`. You get a list of objects, which hold the id's of the observations, by accessing the value of the map with the key "0". 

To get a list with the actual observations you have to iterate through a loop and call the method `getObservation` each time with the id of the observation you are searching for. The result is a pointer to an observation object. You should check for NULL and you should also check wether that observation is valid or not. An observation is valid if it is available in the current coordinate system. If an observation is not valid the method `setUseState` is called to tell OpenIndy that your function won't include that observation in its calculations. To not overfill the `exec` method please define a private method in the header file `pointFit.h` as follows:
{% highlight c++ %}
#ifndef POINTFIT_H
#define POINTFIT_H

#include "pi_fitfunction.h"
#include "configuration.h"
#include "pluginmetadata.h"

class PointFit : public FitFunction
{

public:
    PluginMetaData* getMetaData();
    QList<InputParams> getNeededElements();
    QList<Configuration::FeatureTypes> applicableFor();

    bool exec(Point&);

private:
    bool calculateCentralPoint(Point&, QList<Observation*>);

};

#endif // POINTFIT_H
{% endhighlight %}
Again you can add the following implementation at the very bottom of the `cpp`-file `pointFit.cpp`:
{% highlight c++ %}
bool PointFit::calculateCentralPoint(Point &p, QList<Observation*> myObservations){
    //TODO
    return false;
}
{% endhighlight %}
Now, please replace the `//TODO` in the `exec` method by calling the method you just created and returning its result:
{% highlight c++ %}
return this->calculateCentralPoint(p, myObservations);
{% endhighlight %}
As you will see the classes [OiVec](https://github.com/OpenIndy/OpenIndy/blob/master/lib/openIndyLib/include/oivec.h) and [OiMat](https://github.com/OpenIndy/OpenIndy/blob/master/lib/openIndyLib/include/oimat.h), mentioned before, are used to do all calculations. At first we create and fill the l vector with the XYZ-coordinates of all observations. For this purpose please replace the `//TODO` in the method `calculateCentralPoint` by the following lines:
{% highlight c++ %}
    //set up l vector with all observations
    OiVec l;
    foreach(Observation *obs, myObservations){
        l.add( obs->myXyz.getAt(0) );
        l.add( obs->myXyz.getAt(1) );
        l.add( obs->myXyz.getAt(2) );
    }
    //TODO
{% endhighlight %}
The next step is to set up the A matrix. Again please replace the `//TODO` by the following lines:
{% highlight c++ %}
    OiMat a(l.getSize(), 3);
    for(int i = 0; i < l.getSize(); i++){
        if( (i%3) == 0 ){
            a.setAt(i, Point::unknownX, 1.0);
        }else if( (i%3) == 1 ){
            a.setAt(i, Point::unknownY, 1.0);
        }else if( (i%3) == 2 ){
            a.setAt(i, Point::unknownZ, 1.0);
        }
    }
    //TODO
{% endhighlight %}
The three values `unknownX`, `unknownY` and `unknownZ` are enumeration values. The corresponding enumeration is defined in the [class Point](https://github.com/OpenIndy/OpenIndy/blob/master/src/geometry/point.h). This enumeration values specify the order of unknowns for the A matrix and thus also the order of the covariance matrix. OpenIndy itself and further functions have to know about this order, because otherwise the covariance matrix you calculate would be useless.  
Again please replace the `//TODO` by the following lines:
{% highlight c++ %}
    //calculate coordinates of central point and corresponding statistic
    OiMat n = a.t() * a;
    OiVec c = a.t() * l;
    try{

        OiMat qxx = n.inv();
        OiVec x = qxx * c; //coordinates of central point
        OiVec v = a * x - l;
        x.add(1.0); //x vector as homogeneous coordinates
        //calculate statistic
        double stdv = 0.0;
        if(v.getSize() > 3){
            double sum_vv = 0.0;
            for(int i = 0; i < v.getSize(); i++){
                sum_vv += v.getAt(i) * v.getAt(i);
            }
            stdv = qSqrt(sum_vv / (v.getSize() - 3));
        }else{
            stdv = 0.0;
        }
        //set up result
        p.myStatistic.s0_apriori = 1.0;
        p.myStatistic.s0_aposteriori = stdv;
        p.xyz = x;
        p.myStatistic.qxx = qxx;
        p.myStatistic.v.replace(v);
        p.myStatistic.stdev = stdv;

        //TODO

        p.myStatistic.isValid = true;
        this->myStatistic = p.myStatistic;

        return true;

    }catch(logic_error e){
        this->writeToConsole(e.what());
        return false;
    }catch(runtime_error e){
        this->writeToConsole(e.what());
        return false;
    }
{% endhighlight %}
In the above code snippet the actual algorithm is implemented. The `x` vector holds the coordinates of the central point and the matrix `qxx` is the cofactor matrix of that central point. As the point feature `p`, which your function shall calculate, is passed as a reference you can easily set the results to the reference. Furthermore the point `p` has got an attribute `myStatistic` which you can assign your statistical results to. Not only the point, but also the function itself has got an attribute `myStatistic`. 

After you have set the points statistic you have to assign that statistic object to the statistic object of your function, too. This is because the statistic of the point might later be changed by another function that is assigned to the point. The statistic of a function is only modified by the function itself (and no other functions) and can be reviewed via special dialogs in OpenIndy.  

Now one last step is missing. For the residuals to be displayed you have to fill the attribute `displayResiduals` of the point object. Therefor please replace the `//TODO` by the following snippet:
{% highlight c++ %}
//display the residuals in OpenIndy
for(int i = 0; i < p.myStatistic.v.getSize() / 3; i++){
    Residual r;

    r.addValue("vx", v.getAt(i*3), UnitConverter::eMetric);
    r.addValue("vy", v.getAt(1+i*3), UnitConverter::eMetric);
    r.addValue("vz", v.getAt(2+i*3), UnitConverter::eMetric);

    p.myStatistic.displayResiduals.append(r);
}
{% endhighlight %}
Maybe you ask yourself why OpenIndy does not simply display the residuals which you have previously assigned to the `v` vector. This is because OpenIndy does not no anything about your algorithm. The `v` vector could contain anything in any unit. The residual objects you add to the list `displayResiduals` above define what a residual represents by specifying a header (e.g. "vx") and they also determine the unit in which the residuals are saved.

### Summary
{:.no_toc}

To make sure that your implementation equals the reference implementation of this function you can compare your one to the implementations below.
{% highlight c++ %}
#ifndef POINTFIT_H
#define POINTFIT_H

#include "pi_fitfunction.h"
#include "configuration.h"
#include "pluginmetadata.h"

class PointFit : public FitFunction
{

public:
    PluginMetaData* getMetaData();
    QList<InputParams> getNeededElements();
    QList<Configuration::FeatureTypes> applicableFor();

    bool exec(Point&);

private:
    bool calculateCentralPoint(Point&, QList<Observation*>);

};

#endif // POINTFIT_H
{% endhighlight %}
{% highlight c++ %}
#include "pointFit.h"

PluginMetaData* PointFit::getMetaData(){
    PluginMetaData* metaData = new PluginMetaData();
    metaData->name = "PointFit";
    metaData->pluginName = "Plugin Tutorial";
    metaData->author = "OpenIndyOrg";
    metaData->description = QString("%1 %2")
            .arg("This function calculates an adjusted point.")
            .arg("You can input as many observations as you want which are then used to find the best fit 3D point.");
    metaData->iid = "de.openIndy.Plugin.Function.FitFunction.v001";
    return metaData;
}

QList<InputParams> PointFit::getNeededElements(){
    QList<InputParams> result;
    InputParams param;
    param.index = 0;
    param.description = "Select observations to calculate the best fit point.";
    param.infinite = true;
    param.typeOfElement = Configuration::eObservationElement;
    result.append(param);
    return result;
}

QList<Configuration::FeatureTypes> PointFit::applicableFor(){
    QList<Configuration::FeatureTypes> result;
    result.append(Configuration::ePointFeature);
    return result;
}

bool PointFit::exec(Point &p){
    if(this->isValid() && this->featureOrder.contains(0)){
        QList<Observation*> myObservations;
        QList<InputFeature> myObservationIds = this->featureOrder.value(0);
        foreach(InputFeature obs, myObservationIds){
            Observation *myObservation = this->getObservation(obs.id);
            if(myObservation != NULL && myObservation->isValid){
                myObservations.append(myObservation);
            }else{
                this->setUseState(obs.id, false);
            }
        }
        if(myObservations.size() > 0){
            return this->calculateCentralPoint(p, myObservations);
        }
    }
    return false;
}

bool PointFit::calculateCentralPoint(Point &p, QList<Observation*> myObservations){
    //set up l vector with all observations
    OiVec l;
    foreach(Observation *obs, myObservations){
        l.add( obs->myXyz.getAt(0) );
        l.add( obs->myXyz.getAt(1) );
        l.add( obs->myXyz.getAt(2) );
    }
    //Fill A matrix
    OiMat a(l.getSize(), 3);
    for(int i = 0; i < l.getSize(); i++){
        if( (i%3) == 0 ){
            a.setAt(i, Point::unknownX, 1.0);
        }else if( (i%3) == 1 ){
            a.setAt(i, Point::unknownY, 1.0);
        }else if( (i%3) == 2 ){
            a.setAt(i, Point::unknownZ, 1.0);
        }
    }
    //calculate coordinates of central point and corresponding statistic
    OiMat n = a.t() * a;
    OiVec c = a.t() * l;
    try{

        OiMat qxx = n.inv();
        OiVec x = qxx * c; //coordinates of central point
        OiVec v = a * x - l;
        x.add(1.0); //x vector as homogeneous coordinates
        //calculate statistic
        double stdv = 0.0;
        if(v.getSize() > 3){
            double sum_vv = 0.0;
            for(int i = 0; i < v.getSize(); i++){
                sum_vv += v.getAt(i) * v.getAt(i);
            }
            stdv = qSqrt(sum_vv / (v.getSize() - 3));
        }else{
            stdv = 0.0;
        }
        //set up result
        p.myStatistic.s0_apriori = 1.0;
        p.myStatistic.s0_aposteriori = stdv;
        p.xyz = x;
        p.myStatistic.qxx = qxx;
        p.myStatistic.v.replace(v);
        p.myStatistic.stdev = stdv;

        //display the residuals in OpenIndy
        for(int i = 0; i < p.myStatistic.v.getSize() / 3; i++){
            Residual r;

            r.addValue("vx", v.getAt(i*3), UnitConverter::eMetric);
            r.addValue("vy", v.getAt(1+i*3), UnitConverter::eMetric);
            r.addValue("vz", v.getAt(2+i*3), UnitConverter::eMetric);

            p.myStatistic.displayResiduals.append(r);
        }

        p.myStatistic.isValid = true;
        this->myStatistic = p.myStatistic;

        return true;

    }catch(logic_error e){
        this->writeToConsole(e.what());
        return false;
    }catch(runtime_error e){
        this->writeToConsole(e.what());
        return false;
    }
    return false;
}
{% endhighlight %}
Now you have implemented your first function. With this function you are able to calculate the central point out of any number of XYZ-observations. Next you will learn how to make your plugin ready for the use in OpenIndy.

## - Make your plugin ready

To make your plugin ready you have to update the files `p_factory.h` and `p_factory.cpp` first. Please add the include `#include "pointFit.h"` at the upper part of the file `p_factory.h`. In the file `p_factory.cpp` you can find the following two methods:
{% highlight c++ %}
QList<Function*> OiTemplatePlugin::createFunctions(){
    QList<Function*> resultSet;
    return resultSet;
}
{% endhighlight %}
{% highlight c++ %}
Function* OiTemplatePlugin::createFunction(QString name){
    Function *result = NULL;
    return result;
}
{% endhighlight %}
You have to instantiate your function there, so that OpenIndy is able to work with it. Please change these two methods as follows:
{% highlight c++ %}
QList<Function*> OiTemplatePlugin::createFunctions(){
    QList<Function*> resultSet;
    resultSet.append(new PointFit());
    return resultSet;
}
{% endhighlight %}
{% highlight c++ %}
Function* OiTemplatePlugin::createFunction(QString name){
    Function *result = NULL;
    if(name.compare("PointFit") == 0){
        result = new PointFit();
    }
    return result;
}
{% endhighlight %}
Now please compile your plugin and then it will be ready for the use in OpenIndy. The last step is to learn how your plugin can be installed in OpenIndy.

##Install Plugins

To install a plugin in OpenIndy you first have to compile and run OpenIndy itself. Afterwards in OpenIndy select "Plugin" > "load plugins". This opens a dialog where you can enter the path to the plugin you want to load. After the plugin was checked by OpenIndy you can click "Ok" to load the plugin.  

If you have completed the step by step guide you can now load your own plugin as described above. Then you might create a point feature in OpenIndy and mark it as the active feature. Accordingly select "Function" > "set function" which opens another dialog. Switch to the tab "new function". There you can see the function "PointFit" that you created in your own plugin before.
<br><br>
![function Plugin Loader Tutorial](/documentation/images/dev/functionPluginLoaderTutorial.png)
