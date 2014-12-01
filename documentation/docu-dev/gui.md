---
layout: page
title: Model View Control
excerpt: "Developer documentation for OpenIndy - Model View Control"
author: dev
image:
  feature: banner/b_tracker2.jpg
---


<section id="table-of-contents" class="toc">
  <header>
    <h3>Model View Control Overview</h3>
  </header>
<div id="drawer" markdown="1">
* bla
{:toc} 

</div>
</section><!-- /#table-of-contents -->

---

<a href="/documentation/docu-dev.html" class="btn">Overview</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/concept.html" class="btn">Concept and Architecture</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/plugins.html" class="btn">Plugins</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/interface.html" class="btn">Server Interface</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/gui.html" class="btn btn-success">Model View Control</a>

## Model view control (GUI)

OpenIndy uses the model-view-control principle to handle multi different views on the same data. The data (geometries, stations, functions etc. ) are stored in the model classes and the different views show extracts of the data and its attributes. The controller class handles the interactions between the view and the model. If some changes are made to the view (e.g. some data is edited) the controller calls all needed functions and manages the changes in the model. 
This allows us to have our own internal data logic that manages our data all needed dependencies and does not depend on any view.
The multiple views give us different views on the data for a better support during the measurement task. Each view can have some benefits depending on the task, like the graphical representation of the data.
<br><br>
Views in OpenIndy:

* [tableviews](#tableview)
* [graphic view](#graphic-view)
* [console](#console)

---

### - Tableview

The tableview is a tabular representation of the features and their attributes. Each row represents one feature (e.g. one point) and the columns represent the metadata, attributes and values, number of observations for this geometry, measurement configuration and the functions of this feature.
<figure>
    <a href="/documentation/images/dev/tableview.png"><img src="/documentation/images/dev/tableview.png"></a>
    <p align="middle"><i>The table view</i></p>
</figure>

---

### - Graphic view

The 3D graphic view, using OpenGL, gives the opportunity for a graphical representation of the features and their dependencies. The graphic view also contains a small tree view that contains all features and their attributes.
<figure>
    <a href="/documentation/images/dev/graphicView.png"><img src="/documentation/images/dev/graphicView.png"></a>
    <p align="middle"><i>The graphic view</i></p>
</figure>

---

### - Console

The console actually is used as a output device for information, warnings and errors of currently executed functions and actions as well as interactions.
<figure>
    <a href="/documentation/images/dev/console.png"><img src="/documentation/images/dev/console.png"></a>
    <p align="middle"><i>The console</i></p>
</figure>

---

##Implementation

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

## 3D view (oiGraphixFactory)

OpenGL 3D View
