---
layout: page
title: OpenIndy Server Interface
excerpt: "Developer documentation for OpenIndy - OI Server Interface"
author: dev
image:
  feature: banner/b_tracker2.jpg
---


<section id="table-of-contents" class="toc">
  <header>
    <h3>Server Interface Overview</h3>
  </header>
<div id="drawer" markdown="1">
* bla
{:toc} 

</div>
</section><!-- /#table-of-contents -->

---

<a href="/documentation/docu-dev.html" class="btn">Overview</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/concept.html" class="btn">Concept and Architecture</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/plugins.html" class="btn">Plugins</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/interface.html" class="btn btn-success">Server Interface</a>&nbsp;&nbsp;<a href="/documentation/docu-dev/gui.html" class="btn">Model View Control</a>



## OpenIndy XML schema

While OpenIndy is running it is possible for a client to connect to OpenIndy via a TCP connection. A client which knows the IP and the port OpenIndy listens to it can send XML based requests to OpenIndy. Then OpenIndy itself will work off the requested tasks and will send a XML based response message back to the client. There are various possible request types and their corresponding XML formats which are presented here.

## Request types
For a list of possible request types look at the following table.

<div class="CSSTableGenerator" >
<table>
    <tr>
      <td style="text-align: left">request type</td>
      <td style="text-align: left">ID</td>
      <td style="text-align: left">description</td>
    </tr>
    <tr>
      <td style="text-align: left">GetProject</td>
      <td style="text-align: left">0</td>
      <td style="text-align: left">Get the whole OpenIndy project</td>
    </tr>
    <tr>
      <td style="text-align: left">SetProject</td>
      <td style="text-align: left">1</td>
      <td style="text-align: left">Restore the given OpenIndy project</td>
    </tr>
    <tr>
      <td style="text-align: left">GetActiveFeature</td>
      <td style="text-align: left">2</td>
      <td style="text-align: left">Get the currently selected feature</td>
    </tr>
    <tr>
      <td style="text-align: left">SetActiveFeature</td>
      <td style="text-align: left">3</td>
      <td style="text-align: left">Set the active feature</td>
    </tr>
    <tr>
      <td style="text-align: left">GetActiveStation</td>
      <td style="text-align: left">4</td>
      <td style="text-align: left">Get the active station</td>
    </tr>
    <tr>
      <td style="text-align: left">SetActiveStation</td>
      <td style="text-align: left">5</td>
      <td style="text-align: left">Set the active station</td>
    </tr>
    <tr>
      <td style="text-align: left">GetActiveCoordinateSystem</td>
      <td style="text-align: left">6</td>
      <td style="text-align: left">Get the currently selected display coordinate system</td>
    </tr>
    <tr>
      <td style="text-align: left">SetActiveCoordinateSystem</td>
      <td style="text-align: left">7</td>
      <td style="text-align: left">Set the display coordinate system</td>
    </tr>
    <tr>
      <td style="text-align: left">Aim</td>
      <td style="text-align: left">8</td>
      <td style="text-align: left">Aim the active feature with the active sensor</td>
    </tr>
    <tr>
      <td style="text-align: left">Move</td>
      <td style="text-align: left">9</td>
      <td style="text-align: left">Move the active sensor by the given amount</td>
    </tr>
    <tr>
      <td style="text-align: left">Measure</td>
      <td style="text-align: left">10</td>
      <td style="text-align: left">Measure the active feature with the active sensor</td>
    </tr>
    <tr>
      <td style="text-align: left">StartWatchWindow</td>
      <td style="text-align: left">11</td>
      <td style="text-align: left">Start a watch window stream</td>
    </tr>
    <tr>
      <td style="text-align: left">StopWatchWindow</td>
      <td style="text-align: left">12</td>
      <td style="text-align: left">Stop the watch window stream</td>
    </tr>
    <tr>
      <td style="text-align: left">StartStakeOut</td>
      <td style="text-align: left">13</td>
      <td style="text-align: left">Start the stake out mode</td>
    </tr>
    <tr>
      <td style="text-align: left">StopStakeOut</td>
      <td style="text-align: left">14</td>
      <td style="text-align: left">Stop the stake out mode</td>
    </tr>
    <tr>
      <td style="text-align: left">GetNextGeometry</td>
      <td style="text-align: left">15</td>
      <td style="text-align: left">In stake out mode get the next geometry to measure</td>
    </tr>
</table>
</div>

## Request/Response format
All request and response messages are XML based. They all have the following format:
{% highlight XML %}
<OiRequest id="">
</OiRequest>
{% endhighlight %}
{% highlight XML %}
<OiResponse ref="" errorCode="">
</OiResponse>
{% endhighlight %}
Each request comes with an id that defines the request type (see the table above). The attribute "ref" of the response tag equals the id of the corresponding request. If the "errorCode" is "0" then no error occured. The XML formats for the request and response messages to the tasks of the table above are shown below.

###GetProject
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="0"/>
{% endhighlight %}

#### Response
{:.no_toc}

The OpenIndy XML schema is descriped [here](https://github.com/OpenIndy/OpenIndy/blob/master/schema/openIndySchema.xsd)

### GetActiveFeature
{:.no_toc}


#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="2"/>
{% endhighlight %}

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="2" errorCode="0">
    <activeFeature ref=""/>
</OiResponse>
{% endhighlight %}
The attribute "ref" of the "activeFeature" tag represents the id of the active feature.

### SetActiveFeature
{:.no_toc}


#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="3">
    <activeFeature ref=""/>
</OiRequest>
{% endhighlight %}
The attribute "ref" of the "activeFeature" tag represents the id of the feature that shall be activated.

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="3" errorCode="0">
    <activeFeature ref=""/>
</OiResponse>
{% endhighlight %}
The attribute "ref" of the "activeFeature" tag represents the id of the active feature.

### GetActiveStation
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="4"/>
{% endhighlight %}

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="4" errorCode="0">
    <activeStation ref=""/>
</OiResponse>
{% endhighlight %}
The attribute "ref" of the "activeStation" tag represents the id of the active station.

### SetActiveStation
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="5">
    <activeStation ref=""/>
</OiRequest>
{% endhighlight %}
The attribute "ref" of the "activeStation" tag represents the id of the station that shall be activated.

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="5" errorCode="0">
    <activeStation ref=""/>
</OiResponse>
{% endhighlight %}
The attribute "ref" of the "activeStation" tag represents the id of the active station.

### GetActiveCoordinateSystem
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="6"/>
{% endhighlight %}

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="6" errorCode="0">
    <activeCoordinateSystem ref=""/>
</OiResponse>
{% endhighlight %}
The attribute "ref" of the "activeCoordinateSystem" tag represents the id of the active coordinate system.

### SetActiveCoordinateSystem
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="7">
    <activeCoordinateSystem ref=""/>
</OiRequest>
{% endhighlight %}
The attribute "ref" of the "activeCoordinateSystem" tag represents the id of the coordinate system that shall be activated.

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="7" errorCode="0">
    <activeCoordinateSystem ref=""/>
</OiResponse>
{% endhighlight %}
The attribute "ref" of the "activeCoordinateSystem" tag represents the id of the active coordinate system.

### Measure

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="10"/>
{% endhighlight %}
Cann be called to measure the active feature. Previously you have to set the active feature to the geometry you want to be measured.

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="10" errorCode="0"/>
{% endhighlight %}

### StartWatchWindow
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="11">
    <readingType type=""/> <!-- value between 0 and 5 -->
</OiRequest>
{% endhighlight %}
The attribute "type" of the "readingType" tag represents the type of reading (cartesian, polar etc.) that shall be returned. See the table below for a list of possible values for the attribute "type".

<div class="CSSTableGenerator" >
<table>
    <tr>
      <td style="text-align: left">reading type</td>
      <td style="text-align: left">description</td>
    </tr>
    <tr>
      <td style="text-align: left">0</td>
      <td style="text-align: left">Returns a distance value.</td>
    </tr>
    <tr>
      <td style="text-align: left">1</td>
      <td style="text-align: left">Returns cartesian coordinates (x, y, z).</td>
    </tr>
    <tr>
      <td style="text-align: left">2</td>
      <td style="text-align: left">Returns polar coordinates (azimuth, zenith, distance).</td>
    </tr>
    <tr>
      <td style="text-align: left">3</td>
      <td style="text-align: left">Returns azimuth and zenith.</td>
    </tr>
    <tr>
      <td style="text-align: left">4</td>
      <td style="text-align: left">Returns a temperature.</td>
    </tr>
    <tr>
      <td style="text-align: left">5</td>
      <td style="text-align: left">Returns a level measurement (RX, RY, RZ).</td>
    </tr>
</table>
</div>

#### Response
{:.no_toc}

The response message to a "StartWatchWindow" task is shown below.
{% highlight XML %}
<OiResponse ref="11" errorCode="0"/>
{% endhighlight %}
Until you call the "StopWatchWindow" task in regular intervals OpenIndy sends a message of the below format. Depending on the reading type that was requested one of the shown sub tags ("cartesian", "polar", etc.) will be included.
{% highlight XML %}
<OiResponse ref="11" errorCode="0">
    <cartesian x="" y="" z=""/>
    <polar azimuth="" zenith="" d=""/>
    <distance d=""/>
    <direction r=""/>
    <temperature t=""/>
    <level RX="" RY="" RZ=""/>
</OiResponse>
{% endhighlight %}

### StopWatchWindow
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="12"/>
{% endhighlight %}

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="12" errorCode="0"/>
{% endhighlight %}

### StartStakeOut
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="13">
    <mode value=""/>
    <allGeometries value=""/>
    <groups>
        <group name=""/>
    </groups>
    <geometries>
        <geometry ref=""/>
    </geometries>
</OiRequest>
{% endhighlight %}

Starts a stake out task. Possible values for the attribute "value" of the tag "mode" are "sequence" and nearest", where only "nearest" is implemented in OpenIndy at the moment. Set the attribute "value" of the "allGeometries" tag to "1" if you want to include all geometries in the stake out, or "0" if only geometries of special groups or selected geometries shall be included. The tags "groups" and "geometries" are both optional. If only geometries of special feature groups shall be included in the stake out then you have to add their name.

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="13" errorCode="0">
    <stakeOutId id=""/>
</OiResponse>
{% endhighlight %}
The response to a "StartStakeOut" task is a unique id that identifies your stake out.

### StopStakeOut
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="14">
    <stakeOutId id=""/>
</OiRequest>
{% endhighlight %}

To stop a stake out you have to supply the corresponding stake out id.

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="14" errorCode="0"/>
{% endhighlight %}

### GetNextGeometry
{:.no_toc}

#### Request
{:.no_toc}

{% highlight XML %}
<OiRequest id="15">
    <stakeOutId id=""/>
</OiRequest>
{% endhighlight %}

This task delivers the next geometry (which is the spatially closest one to the previous geometry)

#### Response
{:.no_toc}

{% highlight XML %}
<OiResponse ref="15" errorCode="0">
    <geometry ref=""/>
</OiResponse>
{% endhighlight %}

The value "ref" of the "geometry" tag represents the id of the next geometry.

## Error codes

<div class="CSSTableGenerator" >
<table>
    <tr>
      <td style="text-align: left">error code</td>
      <td style="text-align: left">description</td>
    </tr>
    <tr>
      <td style="text-align: left">0</td>
      <td style="text-align: left">No error occured.</td>
    </tr>
    <tr>
      <td style="text-align: left">1</td>
      <td style="text-align: left">No, or wrong, xml format.</td>
    </tr>
    <tr>
      <td style="text-align: left">2</td>
      <td style="text-align: left">The request id does not exist.</td>
    </tr>
    <tr>
      <td style="text-align: left">3</td>
      <td style="text-align: left">There is no active feature. (e.g. measure)</td>
    </tr>
    <tr>
      <td style="text-align: left">4</td>
      <td style="text-align: left">There is no active station.</td>
    </tr>
    <tr>
      <td style="text-align: left">5</td>
      <td style="text-align: left">There is no active coordinate system.</td>
    </tr>
    <tr>
      <td style="text-align: left">6</td>
      <td style="text-align: left">The requested feature id does not exist.</td>
    </tr>
    <tr>
      <td style="text-align: left">7</td>
      <td style="text-align: left">No transformation parameters are available. (e.g. aim)</td>
    </tr>
    <tr>
      <td style="text-align: left">8</td>
      <td style="text-align: left">The task is already in process. (e.g. watch window)</td>
    </tr>
    <tr>
      <td style="text-align: left">9</td>
      <td style="text-align: left">There is no task that could be stopped. (e.g. watch window)</td>
    </tr>
    <tr>
      <td style="text-align: left">10</td>
      <td style="text-align: left">No sensor is available. (e.g. watch window)</td>
    </tr>
    <tr>
      <td style="text-align: left">11</td>
      <td style="text-align: left">The task id does not exist. (e.g. stake out)</td>
    </tr>
    <tr>
      <td style="text-align: left">12</td>
      <td style="text-align: left">Stake out is finished.</td>
    </tr>
    <tr>
      <td style="text-align: left">13</td>
      <td style="text-align: left">You cannot measure a nominal geometry.</td>
    </tr>
</table>
</div>
<br>

---

## Data import (oiDataExchanger)

data import 