---
layout: post
title: Generating Maps using Basemap
---
I’ve had a few people ask me the question “what do you use to generate your maps?” I presume a lot of people think I use GIS, but in fact I don’t – I use python instead.

I have a few reasons as to why I would prefer Python to create maps over GIS. To keep it simple, I’m more comfortable with code (go figure) than I am with software I haven’t learned. Plus, I haven’t had a formal class in GIS.

I’m going to walk you through how I generate my hurricane outlook maps (sample at the top of the post) using the data the National Hurricane Center provides. I have a “skeleton” script at the very bottom of this post, if you wish to skip straight to that. Just know that it is very bare and can be built upon. The goal of this tutorial (if that’s what you wish to call it) is for you to be able to understand how the maps are created, and hopefully inspire you to create your own!

## The Walkthrough Without Python Code
There are only 5 steps if you boil it down to the basics:

1. Declare any lists, etc. and import any packages needed;
2. Retrieve the data from the National Hurricane Center, download, and save it;
3. Input your latitude and longitude bounds and create the “backbone” of the map;
4. Read the data, save the necessary information, and plot;
5. Style as you see fit and show/save!

## The Walkthrough With Python Code

##### Step 1: Get the data.
The National Hurricane Center has [shapefiles](http://www.nhc.noaa.gov/gis/) freely available for each outlook and system. However, we should keep it simple: [download this data](https://docs.google.com/uc?export=download&id=0B8q3KWnlpanAR1VFY29mU3ZBeUU)* and save this to your desktop. These files are the files associated with the graphic to the left.

Note: the directory that all of these files are in is called sampleFile. You will need to know this for the second step.

*The reason why I’m pointing towards this specific data set is because the code provided here works with this data set. If you wish to download the most recent data, you have to edit the code as necessary to be able to read in the data correctly (which is not covered in this blog post).

##### Step 2: Import any packages needed
The way this is setup, we don’t need to declare any lists, dictionaries, etc. We just need to import a few packages and tell the interpeter to close any existing plots.

```python
from mpl_toolkits.basemap import Basemap
import matplotlib.pyplot as plt

plt.close('all') #be sure to always include this; closes any existing plots
```

##### Step 3: Plot the map.
Now, we need to plot up the map using Basemap:

```python
m = Basemap(projection = 'merc', llcrnrlat = 2, urcrnrlat = 32, 
    llcrnrlon = -142, urcrnrlon= -87,resolution='l')
m.drawcoastlines()
m.drawcountries()
m.drawstates()
```

##### Step 4: Read in the data and plot.
When I first started working with shapefiles, I had no idea off-hand how they were structured and how to read in the information properly to work with it. Hopefully I save you a few google searches with this:

```python
outlook_name = os.join(os.getcwd() + 'gtwo_areas_201708171153')
outlook_info = m.readshapefile(outlook_name,  'outlook')
for info, shape in zip(m.outlook_info, m.outlook):        
    print info
```

__Discussion__: We need to tell the script which file to get, then read it in. We do this by declaring `outlook_info`. We don’t have to declare `outlook_name`, but readability is key. The “for loop” might be a bit unusual, as there is a `zip()` function instead of a `range()` function.

When `info` is printed, it will give you a dictionary with (what could be) a lot of information. Do note that this is how you access any .shp file – this is not exclusive to this script.

Suppose you were to type in `print shape` instead of `print info`; this will print out a lot of coordinates, which intentifies the shape outline. Let’s go ahead and save the probability of 5 day development and then plot it.

```python
outlook_name = os.join(os.getcwd() + 'gtwo_areas_201708171153')
outlook_info = m.readshapefile(outlook_name,  'outlook')
for info, shape in zip(m.outlook_info, m.outlook):        
    x, y = zip(*shape)
    probFiveDay = info['PROB5DAY']
    m.plot(x, y, color = 'orange')
```

This will get you the outline, but not the point. The point is very similar: we just need to go in and replace ‘gtwo_areas_2017…’ with ‘gtwo_points_2017…’, as such:

```python
outlook_name = os.join(os.getcwd() + 'gtwo_areas_201708171153')
outlook_info = m.readshapefile(outlook_name,  'outlook')
for info, shape in zip(m.outlook_info, m.outlook):        
    x, y = zip(*shape)
    probFiveDay = info['PROB5DAY']
    m.plot(x, y, c = 'orange', marker = 'X', ms = 9., ls = None, mew = 1, mec = 'k')
```

#### Step 5: Style and show/save
I’m not going to explain how to style, since from here you’re following the same arguments as you would with a matplotlib graph. Saving and showing is the same thing as well, but I’ll provide the code:

```python
plt.savefig('latestoutlook.png')
plt.show()
```

## The Code: The Entire Thing
```python
from mpl_toolkits.basemap import Basemap
import matplotlib.pyplot as plt
plt.close('all')

m = Basemap(projection = 'merc', llcrnrlat = 2, urcrnrlat = 32, llcrnrlon = -142,
urcrnrlon= -87,resolution='l')
m.drawcoastlines(linewidth=0.5)
m.drawcountries(linewidth=0.5)
m.drawstates(linewidth=0.5)

outlook_path = os.path.join(os.getcwd(), 'gtwo_areas_201708171153')
outlook_info = m.readshapefile(outlook_path, 'outlook')
for info, shape in zip(m.outlook_info, m.outlook):
try:
  x, y = zip(shape)
except ValueError:
  x, y = zip(*shape)
  probFiveDay = info['PROB5DAY']
  m.plot(x, y, color = 'orange')

points_path = path + 'gtwo_points_201708171153'
points_info = m.readshapefile(points_path, 'points')
for info, shape in zip(m.points_info, m.points):
try:
  x, y = zip(shape)
except ValueError:
  x, y = zip(*shape)
  m.plot(x, y, c = 'orange', marker = 'X', ms = 9., ls = None, mew = 1, mec = 'k')

plt.savefig('latestoutlook.png')
plt.show()
```

And that’s it! Enjoy playing around with the code! Hopefully you got something out of this and was inspired in someway. If you have any questions or comments, feel free to send me a DM/Tweet: @WxBDM. I’d love to see what you created using this tutorial!
