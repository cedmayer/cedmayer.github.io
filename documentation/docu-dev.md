---
layout: page
title: OpenIndy Developer Documentation
excerpt: "Developer documentation for OpenIndy."
author: dev
image:
  feature: banner_oi.jpg
---


<section id="table-of-contents" class="toc">
  <header>
    <h3>Developer Documentation Overview</h3>
  </header>
<div id="drawer" markdown="1">
* bla
{:toc} 

</div>
</section><!-- /#table-of-contents -->

---

##  Concept
The following section explains the concept and special features of OpenIndy.

> "Metrology is the science of measurement. Metrology includes all theoretical and practical aspects of measurement. The word comes from Greek μέτρον (metron), "measure" + "λόγος" (logos), amongst others meaning "speech, oration, discourse, quote, study, calculation, reason". In Ancient Greek the term μετρολογία (metrologia) meant "theory of ratios"." 
[(wikipedia)](http://en.wikipedia.org/wiki/Metrology){:target="blank"}

Requirements for OpenIndy:

* Controlling of 3D - Measurement Systems (laser tracker, tachymeter, etc) [(plugins)](/documentation/plugins.html)
* Open data exchange format (openindyXML.xml)
* Various algorithms to fit, construct and manipulate geometries [(plugins)](/documentation/plugins.html)
* Algorithms for the measurement analysis are easy to add or change [(plugins)](/documentation/plugins.html)
* Easy to use development framework for students 
* Easy to use GUI, which provides a deep insight into the measurement data.

### OpenIndy architecture
{:.no_toc}

![OpenIndy Architecture](/images/openIndyArchitecture.png) <br><br>


OpenIndy is a typical desktop application. All functionality and sensor integration will be realized by [plugins](/documentation/plugins.html).
That way you can configure OpenIndy for your custom task pane. The backend is a SQLite database that stores the user-defined configuration. The collected and processed data are stored in a XML format ([openIndyXML](#openindy-xml-schema)).

![OpenIndy class diagram](/images/classDiagramSimple.png)

OpenIndy has three major object groups:

 * [Sensor](#sensor)
 * [Feature](#feature)
 * [Function](#function)

A sensor generates readings. A reading is an original measurement value of the sensor. For example a polar reading is described by two angles and a distance. If possible the reading will be transformed to an observation. An observation is a 3D coordinate that knows the reading from which it was generated. Each observation can belong to one or more geometries and can be changed by system tranformations.

A feature is anything which can be manipulated by functions. OpenIndy uses features to describe all elements in an abstract way that you need to solve your task. For example you want to measure a pipeline. So you will need the feature cylinder ([geometry](#feature-geometry)) to reconstruct the pipeline. In addition to the feature, you need a function(fit or construct) that solves the cylinder.


### Feature and function
{:.no_toc}
![feature and function](/images/functionConcept.png) <br><br>

OpenIndy is [feature](#feature) based. Every feature can be changed by [functions](#function). For example, you can solve a geometry with a fit function, and then rotate the geometry using a transformation function. Each function generates a statistics object with information about the accuracy and passes it to the next function. Thus, a consistent error propagation is possible.

---

## Plugins


<div markdown="0"><a href="/documentation/plugins.html" class="btn btn-success">Start the OiPluginTemplate Tutorial</a></div>

---

## Feature

![feature types](/images/featureTypes.png)

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

---

## - Geometry feature

geometry

---

## - Station feature

station

---

## - Transformation parameter feature

transformation parameter

---

## - Coordinate system feature

coordinate system

---

## Function

The concept behind OpenIndy envisages that every feature can be solved by one or more functions. Hence you can assign as many functions as you want to a feature. The order of those functions is important, because they will be executed in the same order as they were assigned to a feature. This concept of assigning multiple functions to a feature is illustrated in the following diagram.

![function concept](/images/functionConcept.png)

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

![concept example](/images/functionConceptExample.png)

In OpenIndy you can for example create a point feature which you then can measure with a sensor of your choice. This results in n observations and precision values for each of those observations. When you now add the function "Best Fit" to the point feature you get the adjusted coordinates of the point. 
Therefor the function "Best Fit" uses the n observations and their precision values. Besides the adjusted coordinates this function also calculates the covariance matrix &sum;<sub>XX</sub> with the accuracy of the parameters of the point (X,Y,Z) and their correlation. 

Now let us assume that you want to shift the adjusted point along the normal vector of a plane by a specified amount. So you add another function "Translate by Plane" to the point. This function needs a plane and a distance to be executed, so you have to input a plane and a distance which may both have been solved by a chain of functions before. 
In summary the function "Translate by Plane" gets the adjusted point with its covariance matrix, a plane with a covariance matrix and a distance with its accuracy as input. Because this function knows its functional relationship it is able do variance propagation. As the result you get the covariance matrix of the shifted point and, of course, the coordinates of the shifted point itself.  

OpenIndy allows you to implement you own functions. Therefor you have to write a plugin (or extend an existing one). How this works is described [here](/documentation/plugins.html).

---

## Sensor

sensor

---

## OpenIndyLib (linear algebra)

"openIndyLib" is a seperate Qt-project where classes for linear algebra are implemented. In OpenIndy and in all plugins the "openIndyLib" is linked against as a dynamic library. In the following diagram you can see the structure of that library.

![linear algebra](/images/openIndyLib_linearAlgebra.png)

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

## OpenIndy XML schema

OpenIndy XML schema

---

## Model view control (GUI)

openIndy uses the model-view-control principle to handle multi different views on the same data. The data (geometries, stations, functions etc. ) are stored in the model classes and the different views show extracts of the data and its attributes. The controller class handles the interactions between the view and the model. If some changes are made to the view (e.g. some data is edited) the controller calls all needed functions and manages the changes in the model. 
This allows us to have our own internal data logic that manages our data all needed dependencies and does not depend on any view.
The multiple views give us different views on the data for a better support during the measurement task. Each view can have some benefits depending on the task, like the graphical representation of the data.
<br><br>
Views in OpenIndy:

* [tableviews](#tableview)
* [graphic view](#graphic-view)
* [console](#console)

### Tableview
{:.no_toc}
the tableview is a tabular representation of the features and their attributes. Each row represents one feature (e.g. one point) and the columns represent the metadata, attributes and values, number of observations for this geometry, measurement configuration and the functions of this feature.
<figure>
	<a href="/images/tableview.png"><img src="/images/tableview.png"></a>
</figure>


###Graphic view
{:.no_toc}
The 3D graphic view, using OpenGL, gives the opportunity for a graphical representation of the features and their dependencies. The graphic view also contains a small tree view that contains all features and their attributes.
<figure>
	<a href="/images/graphicView.png"><img src="/images/graphicView.png"></a>
</figure>

###Console
{:.no_toc}
The console actually is used as a output device for information, warnings and errors of currently executed functions and actions/ interactions.
<figure>
	<a href="/images/console.png"><img src="/images/console.png"></a>
</figure>

###Implementation
{:.no_toc}
Our model class [`tablemodel`](https://github.com/OpenIndy/OpenIndy/blob/master/ui/tablemodel.h) is derived from `QAbstractTableModel` and therefore has to implement three functions.
{% highlight c++ %}
int rowCount(const QModelIndex &parent = QModelIndex()) const;
int columnCount(const QModelIndex &parent = QModelIndex()) const;
QVariant data(const QModelIndex &index, int role = Qt::DisplayRole) const;
{% endhighlight %}
In addition the optional function `headerdata` is also implemented to specify the column description. If you do not implement this function the column descriptions will be their index.
So all in all our model looks like this:
{% highlight c++ %}
class TableModel : public QAbstractTableModel
{
    Q_OBJECT
public:
    explicit TableModel(QList<FeatureWrapper*> &features, Station *myStation,FeatureWrapper *myFeature,QObject *parent = 0);
    int rowCount(const QModelIndex &parent = QModelIndex()) const;
    int columnCount(const QModelIndex &parent = QModelIndex()) const;
    QVariant data(const QModelIndex &index, int role = Qt::DisplayRole) const;
    virtual QVariant headerData(int section, Qt::Orientation orientation, int role = Qt::DisplayRole) const;

    QList<FeatureWrapper*> &features;

    FeatureWrapper *activeFeature;
    Station *activeStation;

signals:
    
public slots:
    void updateModel(FeatureWrapper *fW, Station *sT);
    
};
{% endhighlight %}
Now the different views on the model are implemented as `QSortFilterProxyModel`.
As an example here is the view for the main tableview [`featureoverviewproxymodel.h`](https://github.com/OpenIndy/OpenIndy/blob/master/ui/featureoverviewproxymodel.h) including the geometries and their metadata, attributes, etc.
{% highlight c++ %}
class FeatureOvserviewProxyModel : public QSortFilterProxyModel
{
    Q_OBJECT
public:

    QList<FeatureWrapper*> &features;

    explicit FeatureOvserviewProxyModel(QList<FeatureWrapper*> &features,QObject *parent = 0);
    
protected:
    bool filterAcceptsColumn ( int source_column, const QModelIndex & source_parent ) const;
    bool filterAcceptsRow ( int source_row, const QModelIndex & source_parent ) const;
    
};
{% endhighlight %}
Here you only have to implement this two functions with your own logic, to filter the columns and rows displayed in the view.
As a last step you need to set the filter on the source model and then show the filtered data in the view.
{% highlight c++ %}
this->featureOverviewModel = new FeatureOvserviewProxyModel(this->features);
this->featureOverviewModel->setSourceModel(this->tblModel);
{% endhighlight %}
{% highlight c++ %}
ui->tableView_data->setModel(control.featureOverviewModel);
{% endhighlight %}

The graphic view is an [`GLWidget`](https://github.com/OpenIndy/OpenIndy/blob/master/ui/glwidget.h) rendered with OpenGL. The draw function of the widget handles the different draw methods of each feature type.
{% highlight c++ %}
void GLWidget::draw(){

    glMatrixMode(GL_MODELVIEW);
    glPushMatrix();
    glLoadIdentity();
    //TODO sicht auf schwerpunkt zentrieren
    glTranslatef(translateX, translateY, translateZ);
    glRotatef(rotationX, 1.0, 0.0 ,0.0);
    glRotatef(rotationY, 0.0, 1.0 ,0.0);
    glRotatef(rotationZ, 0.0, 0.0 ,1.0);



    qglColor(Qt::red);
    if(features->size() > 0){

        for(int i =0; i< features->size(); i++){
            OiGraphix::drawFeature(features->at(i));
        }

{% endhighlight %}
To do this it calls the draw function of [`OiGraphix`](https://github.com/OpenIndy/OpenIndy/blob/master/ui/oiGraphixFactory/oigraphix.h) that determines the correct feature type (e.g. plane) and the specific draw function.
{% highlight c++ %}
void OiGraphix::drawFeature(FeatureWrapper* feature){

    switch(feature->getTypeOfFeature()){
    case Configuration::eCoordinateSystemFeature:{
        break;
    }
    case Configuration::eTrafoParamFeature:{
        break;
    }
    case Configuration::ePlaneFeature:
        OiGraphix::drawPlane(feature->getPlane());
        break;
{% endhighlight %}
The specific drawing classes of the features need to derive from the [`OiGraphixGeomtry class`](https://github.com/OpenIndy/OpenIndy/blob/master/ui/oiGraphixFactory/oigraphix_geometry.h) and implement the draw function. The X, Y and the Z attributes are parameters of the draw function. If more attributes are needed (e.g. ijk vector) they have to be given in the constructor.
In case of a [`plane`](https://github.com/OpenIndy/OpenIndy/blob/master/ui/oiGraphixFactory/oigraphix_plane.h) it looks as follows:
{% highlight c++ %}
class OiGraphixPlane: public OiGraphixGeometry
{
public:
    OiGraphixPlane(GLfloat nx,GLfloat ny, GLfloat nz);

    GLfloat rx;
    GLfloat ry;
    GLfloat rz;

    void draw(GLfloat x, GLfloat y, GLfloat z);
};
{% endhighlight %}

{% highlight c++ %}
OiGraphixPlane::OiGraphixPlane(GLfloat nx,GLfloat ny, GLfloat nz)
{

    rx = nx;
    ry = ny;
    rz = nz;

}

void OiGraphixPlane::draw(GLfloat x, GLfloat y, GLfloat z){

    glMatrixMode(GL_MODELVIEW);
    glPushMatrix();
    glTranslatef(x,y,z);

    glRotatef(acos(rx) * 180.0/PI,1,0,0);
    glRotatef(acos(rz) * 180.0/PI,0,1,0);
    glRotatef(acos(ry) * 180.0/PI,0,0,1);

    glBegin(GL_LINES);
    for (GLfloat i = -2.5; i <= 2.5; i += 0.25) {
      glVertex3f(i, 0, 2.5); glVertex3f(i, 0, -2.5);
      glVertex3f(2.5, 0, i); glVertex3f(-2.5, 0, i);
    }
    glEnd();

    glPopMatrix();

}
{% endhighlight %}

---

## Database (SQLite)

database

---

## 3D view (oiGraphixFactory)

OpenGL 3D View

---

## Plugin loader

plugin loader

---

## Data import (oiDataExchanger)

data import 
