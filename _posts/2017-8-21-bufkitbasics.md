---
layout: post
title: How to use BUFKIT
---

BUFKIT is an analysis tool kit, and it’s used by forecasters to better understand what is happening with the atmosphere by looking at it vertically. It’s a widely used tool, from forecasters in the National Weather Service to hobbyists, and looks great on resumes for those meteorology students who wish to go into an operational forecasting position.

## DISCLAIMER
This is for BUFKIT 2017. This article may not be updated for any future versions.

![bufkit]({{ site.baseurl }}/images/2017-8-21-bufkitbasics/BUFKIT.png)

## Downloading  
If you haven’t download BUFKIT, you can download it [here](https://training.weather.gov/wdtd/tools/BUFKIT/bufkit19.zip) (direct download) and go through the installation process. Once you have it installed, open it up.

![uright]({{ site.baseurl }}/images/2017-8-21-bufkitbasics/BUFKIT-2Blistings.png)

## Interface  
In the top left, there is a “Done” button, along some text. The text in green are your models, while the text all the way on the right are the station identifiers. These are your stations that you have data imported for (we will discuss importing data later). Here, I have KLNS (Lancaster, PA), HGR (Hagerstown, MD), and a few others. You can have as many as you’d like here. The “Done” button is the equivalent of clicking the fancy red “x” in the upper right corner.

![uleft]({{ site.baseurl }}/images/2017-8-21-bufkitbasics/BUFKIT-2Bget-2Bdata.png)

Now, let’s discuss what’s going on in the upper right corner.  
__Get Data:__ This button allows you to get the BUFKIT profiles you want. We will discuss this in detail a bit later.  
__Set Up:__ If you click this, a pop up window will appear with a few settings. The only thing you should modify for the time being is the timezone. Once you do that, hit “save”, and the window will automatically close.  
__Loop:__ This button loops the skew-t. The settings for this can be found under the “controls” button (which is explained later).  
__Training:__ Utilize this. The training button opens up a new window and gives you a thorough run-through of what BUFKIT is and provides a few forecasting techniques that go hand-in hand with using the software.  
__Overview:__ I won’t get into detail with what this is here (future blog TBD), but you’re looking at how a parameter or variable progresses with time and height (such as temperature, dewpoint), with the current time being highlighted with a vertical white line. Go ahead and play around with this and see what you discover with it.  
__dProg/dT:__ If you have past model data archived, you can view the data and compare it to the current model run using dProg/dT.

## Getting the Data
Getting the data is very easy and somewhat straight forward. Go ahead and click on the “Get Data” tab. A new window will open (Note: this is called BufGet). If you go most of the way down in the window, you’ll see an area called “Load Profile List”. Go ahead and select the letter C. You can type in the black area, so go ahead and highlight everything in the black area and delete it. Go to one of these websites (or both if you wish):  
1. [Penn State Bufkit](http://www.meteo.psu.edu/bufkit/CONUS_GFS_12.html)
2 .[Iowa State Bufkit](http://www.meteor.iastate.edu/~ckarsten/bufkit/data/)  
Before continuing, I want to make mention that not every point on either site has model data available. You may need to pick and choose a different site depending upon what model you want to use.
__PENN STATE LINK:__ On the left side, you will see all the models .buf files they provide, along with each run time. The current model run is highlighted in green (this is what you want to choose). Go ahead and select the latest GFS model run. A new map will quickly load. Click on any station you’d like to. You’ll find yourself on a page with what seems like nonsense, which is what you want. Take note of the URL (be sure it ends in .buf).
__IOWA STATE LINK:__ Scroll down a little bit until you see a map. This map is interactive, unlike the Penn State map. Click on any dot you’d like to get a window to pop open. From there, go ahead and click on the most GFS run (if it’s available – if not, find another station). Now, you should be on a page with what looks like nonsense. Take note of the URL (be sure it ends in .buf)  
Go up to the URL and copy it and then open BufGet. Now, let’s archive our data (because it’s always a great idea to archive it) by typing in “Archive on” (without the quotes). Hit enter twice. Now, paste that URL you copied not too long ago. Do note that “Archive on” should be highlighted in red. Yours should look similar to the picture below:
