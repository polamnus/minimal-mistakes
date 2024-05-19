---
layout: single
author_profile: true
read_time: false
comments: false
share: true
related: true
permalink: gismapping.html
---
# Mapping with Meshtastic
<img src="/media/G_earth_3d_overview.png">

This page summarizes one method to create geographic plots of your Meshtastic node range tests. Using this method you can create heat maps which help visualize the strength and signal between any two participating nodes.

## Range Test Module
<img src="/media/explorer_binocs.png" width="80%">

The Meshtastic application includes a range test module which can be used to generate data sets, plotting the distance, location, and signal strength between any two nodes. These may then be plotted on various mapping services, the most ubiquitous being Google, which is what we'll use here.

 We will be making use of two nodes configured with a single base station and a single mobile node. We put the base station into a "Lighthouse" mode where it beacons out every so many seconds. Meanwhiel, our mobile node will be our receiver and its location will be used as the source of our mapped GPS coordinates.

 1. Enable sharing phone GPS to mesh. I recommend this for range testing, even if you have on-board GPS.
 2. Connect to Stationary Node, enable range test mode. If walking, set 30s, if driving, set 120s.
 3. Connect to Mobile Node, optionally clear debug log to start a fresh data file (all existing logs will be lost!).
 4. Enable Range Test mode, do not set a send time, we will only be receiving.
 5. Soon, you should see messages appear on your mobile node, like `seq00`, incrementing with each ping.
 6. Start walking! Or driving, or hiking, or biking, or flying! Keep watching your mobile client for missed pings. Continue moving until you're sure you've missed a number of pings and have truly lost link with the base station.
 7. Continue to map the periphery of your connectivity with the base station. Make your path a route you don't mind repeating, since you may wish to run successive tests to compare performance under different conditions. Make it a reasonable effort not annoying to have to repeat.
 8. Consider whether you're losing signal due to range, or line of sight. Line of sight may clear up if you keep moving, but range fades over distance.
 9. When satisfied with your data set, return to home base. Disable Range Test mode in **BOTH** the mobile base station and client nodes.
 10. Disable sharing your phone's GPS to mesh.

> Ensure you disable Range Test mode when not actively in-use. Not only
> is it a drain on the battery of the participating nodes, even
> though the broadcasts have a zero hop kill-switch, and are ignored by
> non-participating nodes, the packets are still processed by the mesh
> and therefore have the capacity to spam the mesh. It's absolutely fine
> to use responsibly, when and as needed, just remember to turn it off when done!

## Saving the Data
Now you need to connect to the mobile radio node and export/save the `rangetest.csv` file. This will save to the local storage of your mobile device. Next, trasfer the `rangetest.csv` file off the phone and onto a device of your choice...presuming you don't want to work with it on a phone's UI. *I don't want to work with it on a phone's UI.*

Use something like a messenger app, or even email. The CSV files are relatively small, especially if you clear the debug log prior to starting your range test. Send it to yourself through whatever means necessary, and save the file on the device of your choice.

## Google Sheets: Ingesting the Data Set
Now we create a new sheet in [Google Sheets](https://sheets.google.com). This is only temporary, so name it anything you like.
<img src="/media/sheets.jpg">

 1. From the file menu, chose Import and browse your local storage to the `rangetest.csv` file.
 2. Accept defaults and Google Sheets will import the file.
 3. Click on the payload column, chose Create Filter.
    <img src="/media/CreateFilter.png">
 5. From the filter menu, clear all selected items.
 6. Enter: `seq` in the search field and then click **Select All**.'
    <img src="/media/filter_selectall.png">
 7. Your spreadsheet will now only display rows with the seq prefix in the payload column. These are your range test pings.
     Note you should see **Rx SNR**, **Rx Lat**, & **RX Long** for each packet received.
 9. **Ctrl+A** (**Cmd+A** on Mac) to select all, then **Ctrl+C** (**Cmd+C** on Mac) to copy the whole sheet to clipboard.
 10. Click the plus icon at the bottom to add a new sheet, enter the new sheet and **Ctrl+V** (**Cmd+V** on Mac) to paste the range test rows to the new sheet.
    <img src="/media/sheet_new.png">
 11. Now delete the original sheet, leaving just the sheet containing the filtered range test packets and nothing else.
    <img src="/media/filter_results.png">

## Google's My Maps
 1. Start a new map in [Google's My Maps](https://www.google.com/mymaps), or open an existing map to add data to it.
 2. Import data into the layer, selecting the Sheet we just created in Google Drive.
     <img src="/media/MapImportLayer.png">
 4. Google Maps will ask you which columns are to be used to represent latitude and longitude. Fortunately, this is easy as the columns are *RxLat* and *RxLon*.
     <img src="/media/latlong.png">
 5. Google Maps next asks which column to use to name the mapped points. For this, chose *Rx SNR*.
 6. Your mapped range test points are now displayed, but we are not done yet!
 7. Click the paint roller icon and change *Group Places By* and *Set Labels* both to *Rx SNR*.
 8. Click the radio button next to *Ranges* and set a sensible number. I use 8, but experiment and see what works.
 9. Set a color scheme of your choice.
     <img src="/media/ImportMap.png">
    
Now you will have a color coded set of plots, each representing a single ping packet sent from your base station to your mobile node during the range test. The color gradient represents the SNR as calculated by the mobile receiver node.  

> You may now repeat this same test any number of times. By changing a variable
> each time and comparing to your previous data-sets, you can make informed,
> objective decisions as to whether any changes have been beneficial or not.

## Back to Baseline
<img src="/media/baseline.png">

Before you can determine whether any changes you make to your mesh are worthwhile, you must first establish a baseline. You can't know where you're going if you don't know where you are. Fortunately, this method is an easy way to establish that baseline. With an adequate baseline, you may make objective determinations about whether any changes are good or bad. 

> Was that fancy new antenna actually worth it or not?
>  Would it help to move that base station up another three feet?(Yes...the answer is yes)

Now you can answer that question definitively and objectively, using empirical data.

## Google Earth
<img src="/media/GEarch_Pano.png">
From within the Google My Maps interface, you may open the same map inside of the web version of [Google Earth](https://earth.google.com) or even export your map in a number of formats. Using Google Earth, you can do some really interesting analysis and visualizations in 3D virtual space.

Got any interesting maps worth sharing? Send me a copy, and I'll be glad to feature interesting maps here!
Happy Mapping!
