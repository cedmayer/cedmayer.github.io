---
layout: page
title: Concept and Architecture 
excerpt: "Developer documentation for OpenIndy - Concept and Architecture"
author: dev
image:
  feature: banner/b_tracker2.jpg
---


<section id="table-of-contents" class="toc">
  <header>
    <h3>Concept and Architecture Overview</h3>
  </header>
<div id="drawer" markdown="1">
* bla
{:toc} 

</div>
</section><!-- /#table-of-contents -->

---


<a href="/documentation/docu-dev.html" class="btn">Overview</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/concept.html" class="btn btn-success">Concept and Architecture</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/plugins.html" class="btn">Plugins</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/interface.html" class="btn">Server Interface</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/gui.html" class="btn">Model View Control</a>

##  Concept
The following section explains the concept and special features of OpenIndy.

> "Metrology is the science of measurement. Metrology includes all theoretical and practical aspects of measurement. The word comes from Greek μέτρον (metron), "measure" + "λόγος" (logos), amongst others meaning "speech, oration, discourse, quote, study, calculation, reason". In Ancient Greek the term μετρολογία (metrologia) meant "theory of ratios"." 
[(wikipedia)](http://en.wikipedia.org/wiki/Metrology){:target="blank"}

Requirements for OpenIndy:

* Controlling of 3D - Measurement Systems (laser tracker, tachymeter, etc) [(plugins)](/documentation/docu-dev/plugins.html)
* Open data exchange format (openindyXML.xml)
* Various algorithms to fit, construct and manipulate geometries [(plugins)](/documentation/docu-dev/plugins.html)
* Algorithms for the measurement analysis are easy to add or change [(plugins)](/documentation/docu-dev/plugins.html)
* Easy to use development framework for students 
* Easy to use GUI, which provides a deep insight into the measurement data.

### OpenIndy architecture
{:.no_toc}

![OpenIndy Architecture](/documentation/images/dev/openIndyArchitecture.png) <br><br>


OpenIndy is a typical desktop application. All functionality and sensor integration will be realized by [plugins](/documentation/docu-dev/plugins.html).
That way you can configure OpenIndy for your custom task pane. The backend is a SQLite database that stores the user-defined configuration. The collected and processed data are stored in a XML format ([openIndyXML](/documentation/docu-dev/interface.html#openindy-xml-schema)).

![OpenIndy class diagram](/documentation/images/dev/classDiagramSimple.png)

OpenIndy has three major object groups:

 * [Sensor](#sensor)
 * [Feature](#feature)
 * [Function](#function)

A sensor generates readings. A reading is an original measurement value of the sensor. For example a polar reading is described by two angles and a distance. If possible the reading will be transformed to an observation. An observation is a 3D coordinate that knows the reading from which it was generated. Each observation can belong to one or more geometries and can be changed by system tranformations.

A feature is anything which can be manipulated by functions. OpenIndy uses features to describe all elements in an abstract way that you need to solve your task. For example you want to measure a pipeline. So you will need the feature cylinder ([geometry](#feature-geometry)) to reconstruct the pipeline. In addition to the feature, you need a function(fit or construct) that solves the cylinder.

### Feature and function
{:.no_toc}

![feature and function](/documentation/images/dev/functionConcept.png) <br><br>

OpenIndy is [feature](#feature) based. Every feature can be changed by [functions](#function). For example, you can solve a geometry with a fit function, and then rotate the geometry using a transformation function. Each function generates a statistics object with information about the accuracy and passes it to the next function. Thus, a consistent error propagation is possible.

---

## Feature

![feature types](/documentation/images/dev/featureTypes.png)

There are four kinds of feature:

* [Geometry](#feature-geometry)
* [Station](#feature-station)
* [Transformation parameter](#feature-transformation-parameter)
* [Coordinate systems](#feature-coordinate-system)

OpenIndy describes the "world" through features. A feature has all the necessary attributes to describe it. But a feature doesn't know how to get valid values for his attributes. To calculate values for the attributes of the feature, OpenIndy uses [functions](#function). Functions uses other features or [observations](#sensor) to solve "his" feature.
<br><br>
For example:
<br><br>
A point feature ([geometry](#feature-geometry)) knows his attributes (x,y,z), but it doesn't know how to get them. You set a best fit function for the point. This functions uses any number of observations to solve the attributes of the point.
<br><br>
Common attributes of any feature:

{% highlight c++ %}
class Feature : public Element
{

public:
    virtual ~Feature();

    QString name;
    QString group;
    bool isUpdated;
    bool isSolved;
    QList<Function*> functionList;
    QList<FeatureWrapper*> usedFor; //features which need this feature to recalc
    QList<FeatureWrapper*> previouslyNeeded; //features which are needed to recalc this feature
    Configuration::eColor displayColor;
};
{% endhighlight %}

<div class="CSSTableGenerator" >
<table>
<tr>
<td align="left">attribute</td>
<td align="left">description</td>
</tr>
<tr>
<td align="left">group</td>
<td align="left">name of the group to which the feature belongs to</td>
</tr>
<tr>
<td align="left">isUpdated</td>
<td align="left">true if the feature was successfully transformed into another coordinate system</td>
</tr>
<tr>
<td align="left">isSolved</td>
<td align="left">true if a function solves the feature</td>
</tr>
<tr>
<td align="left">functionList</td>
<td align="left">a list of all functions of the feature</td>
</tr>
<tr>
<td align="left">usedFor</td>
<td align="left">a list of all feature that uses this feature</td>
</tr>
<tr>
<td align="left">previouslyNeeded</td>
<td align="left">a list of all feature which are used by this feature</td>
</tr>
<tr>
<td align="left">displayColor</td>
<td align="left">color of the feature</td>
</tr>
</table>
</div>

---

### FeatureWrapper
{:.no_toc}

All features are stored as [featureWrapper](https://github.com/OpenIndy/OpenIndy/blob/master/src/featurewrapper.h)  in one list in the [controller class](https://github.com/OpenIndy/OpenIndy/blob/master/controller/controller.h). To avoid type conversions (downcast), the [featureWrapper](https://github.com/OpenIndy/OpenIndy/blob/master/src/featurewrapper.h) has a pointer for each feature type. 

{% highlight c++ %}
void FeatureWrapper::setPoint(Point *p){
    if(p != NULL){
        this->myFeature = p;
        this->myGeometry = p;
        this->myPoint = p;
        this->typeOfFeature = Configuration::ePointFeature;
    }
}

Point* FeatureWrapper::getPoint(){
    return this->myPoint;
}
{% endhighlight %}



## - Geometry feature

geometry



## - Station feature

station



## - Transformation parameter feature

transformation parameter


## - Coordinate system feature

coordinate system

---

## Function

The concept behind OpenIndy envisages that every feature can be solved by one or more functions. Hence you can assign as many functions as you want to a feature. The order of those functions is important, because they will be executed in the same order as they were assigned to a feature. This concept of assigning multiple functions to a feature is illustrated in the following diagram.

![function concept](/documentation/images/dev/functionConcept.png)

The first function that is assigned to a feature solves it and each additional function changes the previously solved feature. Thereby each function can calculate statistical values for the feature using variance propagation. In OpenIndy there are five different types of functions which are described in the following table.  

<div class="CSSTableGenerator" >
<table>
<tr><td>function type </td><td>description</td></tr>
 <tr><td>fit function </td><td>If overdetermination is present, fit functions have the purpose to calculate the adjusted parameters of a geometry by using observations. So for example a fit function can calculate the center of n xyz-observations.</td></tr>
 <tr><td>construct function </td><td>Construct functions are used to calculate geometries by using other geometries. For example the intersection of a line and a plane results in a new point.</td></tr>
 <tr><td>geodetic function </td><td>Geodetic functions are intended to calculate special geodetic tasks like spatial intersection etc..</td></tr>
 <tr><td>object transformation </td><td>In contrast to fit- and construct functions, object transformations do not define a geometry, but change a previously defined one. A point which has been fit before can for instance be moved along a line by a certain amount.</td></tr>
 <tr><td>system transformation </td><td>System transformations are used to calculate the parameters of a transformation from one coordinate system to another one.</td></tr>
</table>
</div>
<br><br>

While object transformations can only be assigned to a feature that was previously solved by another function, the other function types are meant to solve a feature so functions of those types can only be assigned to a feature once and only as the first function. (For example you cannot shift a point that has never been solved.) To better understand this concept let us consider a concrete example:

![concept example](/documentation/images/dev/functionConceptExample.png)

In OpenIndy you can for example create a point feature which you then can measure with a sensor of your choice. This results in n observations and precision values for each of those observations. When you now add the function "Best Fit" to the point feature you get the adjusted coordinates of the point. 
Therefor the function "Best Fit" uses the n observations and their precision values. Besides the adjusted coordinates this function also calculates the covariance matrix &sum;<sub>XX</sub> with the accuracy of the parameters of the point (X,Y,Z) and their correlation. 

Now let us assume that you want to shift the adjusted point along the normal vector of a plane by a specified amount. So you add another function "Translate by Plane" to the point. This function needs a plane and a distance to be executed, so you have to input a plane and a distance which may both have been solved by a chain of functions before. 
In summary the function "Translate by Plane" gets the adjusted point with its covariance matrix, a plane with a covariance matrix and a distance with its accuracy as input. Because this function knows its functional relationship it is able do variance propagation. As the result you get the covariance matrix of the shifted point and, of course, the coordinates of the shifted point itself.  

OpenIndy allows you to implement you own functions. Therefor you have to write a plugin (or extend an existing one). How this works is described [here](/documentation/docu-dev/plugins.html#oiplugintemplate-tutorial).

---

## Sensor

sensor

---

## OpenIndyLib (linear algebra)

"openIndyLib" is a seperate Qt-project where classes for linear algebra are implemented. In OpenIndy and in all plugins the "openIndyLib" is linked against as a dynamic library. In the following diagram you can see the structure of that library.

![linear algebra](/documentation/images/dev/openIndyLib_linearAlgebra.png)

There are two classes [OiVec](https://github.com/OpenIndy/OpenIndy/blob/master/lib/openIndyLib/include/oivec.h) and [OiMat](https://github.com/OpenIndy/OpenIndy/blob/master/lib/openIndyLib/include/oimat.h). This classes have methods to do vector and matrix algebra and to access the elements of a vector and a matrix respectively. It is recommended to use this classes in your plugins for all calculations. Furthermore there is an interface [LinearAlgebra](https://github.com/OpenIndy/OpenIndy/blob/master/lib/openIndyLib/include/linearalgebra.h) where all methods for vector and matrix algebra are defined. 

The implementations of the classes [OiVec](https://github.com/OpenIndy/OpenIndy/blob/master/lib/openIndyLib/include/oivec.h) and [OiMat](https://github.com/OpenIndy/OpenIndy/blob/master/lib/openIndyLib/include/oimat.h) use one implementation of that interface. The interface is defined as follows:
{% highlight c++ %}
class OI_LIB_EXPORT LinearAlgebra
{
public:
    virtual ~LinearAlgebra(){}

    virtual OiVec addIn(OiVec v1, OiVec v2) = 0;
    virtual OiMat addIn(OiMat m1, OiMat m2) = 0;
    virtual OiVec substract(OiVec v1, OiVec v2) = 0;
    virtual OiMat substract(OiMat m1, OiMat m2) = 0;
    virtual OiMat multiply(OiMat m1, OiMat m2) = 0;
    virtual OiVec multiply(OiMat m, OiVec v) = 0;
    virtual OiMat multiply(double s, OiMat m) = 0;
    virtual OiVec multiply(double s, OiVec v) = 0;
    virtual OiMat invert(OiMat m) = 0;
    virtual OiMat transpose(OiMat m) = 0;
    virtual void svd(OiMat &u, OiVec &d, OiMat &v, OiMat x) = 0;
    virtual OiVec cross(OiVec a, OiVec b) = 0;
    virtual double dot(OiVec a, OiVec b) = 0;
};

{% endhighlight %}
This interface may be realized by many different implementations. The great advantage of this is that OpenIndy is not dependent on one special linear algebra library. There is always the possibility to switch the used linear algebra implementation. To switch the current implementation there is the static class [ChooseLALib](https://github.com/OpenIndy/OpenIndy/blob/master/lib/openIndyLib/include/chooselalib.h). Via the method `setLinearAlgebra` the current linear algebra library can be changed. Currently there is only one implementation `LAArmadillo` which uses the open source library [Armadillo](http://arma.sourceforge.net/).

---

## Database (SQLite)

database