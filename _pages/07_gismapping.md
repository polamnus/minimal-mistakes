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

The Meshtastic application includes a range test module which can be used to generate data sets, plotting the distance, location, and signal strength between any two nodes. These may then be plotted on various mapping services, the most ubiquitous being Google, so we'll use that to focus on.

 We will be making use of two nodes configured with a single base station and a single mobile node. We put the base station into a "Lighthouse" mode where it beacons out every so many seconds, our mobile node will be receiver and its location will be used as the source of our mapped GPS coordinates.

 1. Connect to Stationary Node, enable range test mode. If walking, set 30s, if driving, set 120s...or similar enough for your needs.
 2. Connect to Mobile Node, optionally clear debug log to start a fresh data file (all existing logs will be lost!).
 3. Enable Range Test mode, do not set a send time, we will only be receiving.
 4. Soon, you should see messages appear on your mobile node, like seq, with incrementing numbers.
 5. Start walking! Or driving, or hiking, or biking, or driving. Keep watching your mobile client for missed pings. Keep moving until you're sure you've missed a number of pings due to range, line of sight, etc.
 6. Continue to "Map" the periphery of your connectivity with the base station. Consider whether you're losing signal due to range related, or line of sight related issues...line of sight may clear up in a few meters...range usually gradually fades.
 7. When satisfied with your data set, return to home base. Disable Range Test mode in BOTH the mobile base station and client nodes.

> Ensure you disable Range Test mode when not actively in-use. Not only
> is it a drain on the battery of the participating nodes, even
> though the broadcasts have a zero hop kill-switch, and are ignored by
> non-participating nodes, the packets are still processed by the mesh
> and therefore have the capacity to be detrimental to the health of the
> mesh.

## Saving the Data
Now you need to connect to the mobile radio node and export/save the `rangetest.csv` file. This will save to the local storage of your mobile device. Now you need to trasfer the `rangetest.csv` file off the phone and onto a laptop...presuming you don't want to work with it on a phone's UI...I don't want to work with it on a phone's UI.

Use something like a messenger app, as the CSV files are relatively small, especially if you clear the debug log prior to starting your range test. Send it to yourself through whatever means necessary, and save the file on your laptop of choice.

## Google Sheets: Ingesting the Data Set
Now we create a new sheet in [Google Sheets](https://sheets.google.com), don't bother giving it a name, this is only temporary.
<img src="/media/sheets.jpg">

 1. From the file menu, chose Import and browse your local storage to the `rangetest.csv` file.
 2. Accept defaults and Google Sheets will import the file.
 3. Click on the payload column, chose Create Filter.
    <img src="/media/CreateFilter.png">
 5. From the filter menu, clear the selected items.
 6. Enter: `seq` in the search field and then click 'select all.'
    <img src="/media/filter_selectall.png">
 7. Your spreadsheet will now only display rows with the seq prefix in the payload column. These are your range test pings. Note you should see Rx SNR, Rx Lat, & RX Long for each packet received.
 8. Ctrl+A (Cmd+A on Mac) to select all, then Ctrl+C (Cmd+C on Mac) to copy the whole sheet to clipboard.
 9. Click the plus icon at the bottom to add a new sheet, enter the new sheet and Ctrl+V (Cmd+V on Mac) to paste the range test rows to the new sheet.
    <img src="/media/sheet_new.png">
 10. Now delete the original sheet, leaving just the filtered range test packets and nothing else.
    <img src="/media/filter_results.png">

## Google's My Maps
 1. Start a new map in [Google's My Maps](https://www.google.com/mymaps), or open an existing map to add data to it.
 2. Click to import data into the initial layer, browse your Google drive for the spreadsheet we worked on in the previous section.
     <img src="/media/MapImportLayer.png">
 4. Google Maps will ask you which columns are to be used to represent latitude and longitude. Fortunately, this is easy as the columns are RxLat and RxLon.
     <img src="/media/latlong.png">
 5. Google Maps next asks which column to use to name the mapped points. For this, chose Rx SNR.
 6. Your mapped range test points are now displayed, but we are not done yet!
 7. Click the paint roller icon and change Group Places By and Set Labels both to Rx SNR.
 8. Click the radio button next to Ranges and set a reasonable number here, I use 8.
 9. Set a color scheme of your choice.
     <img src="/media/ImportMap.png">
    
Now you will have a color coded set of plots, each representing a single ping packet sent from your base station to your mobile node during the range test. The color gradient represents the SNR as calculated by the mobile receiver node.  

> You may now repeat this same test any number of times.
> By changing a variable each time and comparing to your
> previous data-sets, you can make informed,
> objective opinions about the efficiacy of any changes.

## Back to Baseline
<img src="/media/baseline.png">

Before you can determine whether anything you do on the mesh is a good or bad idea, you must first establish a baseline. You can't know where you're going if you don't know where you are, so the first thing you ought to do is establish a baseline against which to measure any future updates or changes. With an adequate baseline, you may make objective determinations about whether any changes are good or bad.

## Google Earth
<img src="/media/GEarch_Pano.png">
From within the Google My Maps interface, you may open the same map inside of the web version of Google Earth or even export your map in a number of formats. Using Google Earth, you can do some really interesting analysis and visualizations in 3D virtual space.

Got any interesting maps worth sharing? Send me a copy, and I'll be glad to feature interesting maps here!
