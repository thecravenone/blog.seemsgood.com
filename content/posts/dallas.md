+++
author = "Sam Craven"
categories = ["travel"]
date = 2025-01-01T21:13:54Z
description = ""
draft = false
image = "__GHOST_URL__/content/images/2018/02/Xvisionx_Dallas_Stemmons.jpg"
slug = "sams-recommendations-for-a-weekend-in-dallas"
summary = "A friend recently asked me for advice on what to do with her parents on a weekend in Dallas. Here's my recommendations."
tags = ["travel"]
title = "Sam's Recommendations for a Weekend in Dallas"

+++


A friend recently asked me for advice on what to do with her parents on a weekend in Dallas. There was an additional specification that her parents did not like outdoors activities. With that in mind, here is a slightly-cleaned up version of what I sent her.

# Map
(May take a moment to load as it processes the place list)

<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<script type="text/javascript">
    google.charts.load('current', {
      'packages': ['map'],
      // Note: you will need to get a mapsApiKey for your project.
      // See: https://developers.google.com/chart/interactive/docs/basic_load_libs#load-settings
      'mapsApiKey': 'AIzaSyB90h64jf6vmUYrTr1tA331rAyROzA40ks'
    });
    google.charts.setOnLoadCallback(drawMap);
    function drawMap () {
      var data = new google.visualization.DataTable();
      data.addColumn('string', 'Address');
      data.addColumn('string', 'Location');
      data.addRows([
		 ['2201 N Field St, Dallas, TX 75201', 'Perot Museum of Nature and Science'],
        ['1801 N Griffin St, Dallas, TX 75202', 'Dallas World Aquarium'],
        ['2943 SMU Boulevard, Dallas, TX 75205',     'George W Bush Presidential Library and Museum' ],
		['411 Elm St, Dallas, TX 75202', 'The Sixth Floor Museum at Dealey Plaza'],
		['1717 N Harwood St, Dallas, TX 75201', 'Dallas Museum of Art'],
		['1428 Young St, Dallas, TX 75202', 'Pioneer Plaza'],
		['5610 E Mockingbird Ln, Dallas, TX 75206', 'Campisis Egyptian (actually Italian) Restaurant'],
		['3127 Routh St, Dallas, TX 75201', 'Katy Trail Ice House'],
		['6537 E Northwest Hwy, Dallas, TX 75231', 'Kellers Drive In']
      ]);
      var options = {
        mapType: 'styledMap',
        zoomLevel: 12,
        showTooltip: true,
        showInfoWindow: true,
        useMapTypeControl: true,
        maps: {
          // Your custom mapTypeId holding custom map styles.
          styledMap: {
            name: 'Styled Map', // This name will be displayed in the map type control.
            styles: [
              {featureType: 'poi.attraction',
               stylers: [{color: '#fce8b2'}]
              },
              {featureType: 'road.highway',
               stylers: [{hue: '#0277bd'}, {saturation: -50}]
              },
              {featureType: 'road.highway',
               elementType: 'labels.icon',
               stylers: [{hue: '#000'}, {saturation: 100}, {lightness: 50}]
              },
              {featureType: 'landscape',
               stylers: [{hue: '#259b24'}, {saturation: 10}, {lightness: -22}]
              }
        ]}}
      };
      var map = new google.visualization.Map(document.getElementById('map_div'));
      map.draw(data, options);
    }
  </script>
  <div id="map_div" style="height: 500px; width: 900px"></div>
  
# Things to do:
* [**Perot Museum of Nature and Science**](https://www.perotmuseum.org/) - I haven't been here since before it moved but back then, it was a super accessible museum and a common field trip spot for schools.
* [**George W Bush Presidential Library and Museum**](https://www.georgewbushlibrary.smu.edu/) - When I read my Dallas list to my folks, this was the one thing they both said needed to be added. I haven't been yet but it's supposed to be quite good.
* [**Dallas World Aquarium**](https://www.dwazoo.com/) - Unlike Houston, Dallas actually has an aquarium!
* [**Pioneer Park**](https://en.wikipedia.org/wiki/Pioneer_Plaza) - Okay, I know you said no outdoorsy stuff but this one is as little just a few minutes and is close to some other things I've recommended. This park is home to dozens (hundreds?) of individual sculptures depicting a cattle drive. The work proceeds through the main plaza and into several adjacent blocks. Usually cool for non-Texans to see just how Texan Texas can be.
* [**The Sixth Floor Museum at Dealey Plaza**](https://www.jfk.org/) - Already on your list but adding so that you can have just one easy to reference list!
* [**Dallas Museum of Art**](https://dma.org/) - I don't know what to say. It's Dallas' art museum.

# Food/Drinks:
* [**Campisi's**](https://www.campisis.us/) - Old school Italian made by claimed small time mobsters. You'll want to go to the original location on Mockingbird. Search "Campisi's Egyptian" to find it easily on your map app. If there's room, ask to sit in the old section of the restaurant. I strongly recommend a pizza with sausage on it (plus whatever else you like).
* [**Katy Trail Ice House**](http://katyicehouse.com/) - Picture the West Alabama Ice House but on five times as much land and owned by a multimillionaire. They have a significant indoor portion as well as food and liquor.
* [**Keller's Drive In**](https://www.yelp.com/biz/kellers-drive-in-dallas) - Alright, to be honest, this place doesn't make the list of best restaurants in Dallas but it's an old family favorite. This place is just a little bit stuck in time. A cheeseburger is $2.35 and an ice cold Bud Heavy is $2.50, brought straight to your car. Turn on your hazards to let them know you're ready to order. Get the number 5 burger. Roll down the windows (leaving a couple inches for your tray to hang on) and listen to Stevie Ray Vaughan and ZZ Top while you get your nostalgia on. A pic of the menu from last time I was there: 

![0Hyz3Qa-1-](__GHOST_URL__/content/images/2018/02/0Hyz3Qa-1-.jpg)

# Fort Worth:
Fort Worth is a solid half hour drive from downtown Dallas but has some cool stuff if you don't mind venturing a bit further. Without going into a ton of detail:

* **Stockyards** - A portion of downtown left more or less as it was when Ft Worth was "The Gateway to the West," through which all cattle came before heading to the East coast
* **Water Gardens** - Again, outside but brief, and quite pretty. Logan's Run as well as several other movies have featured these.
* **Joe T Garcia's** - Claims to have invented queso.
* **Kimball Art Museum** - Top tier art museum. When big shows come through, it'll be here in Ft Worth rather than in Dallas (I saw Manet/Monet here).
* **Ft Worth Museum of Science and History** - Mostly go to experience one of only a handful of OMNI theaters in the world.
* **The original Flying Saucer location** - duh.

---

Feature image by [Drumguy8800](https://en.wikipedia.org/wiki/User:Drumguy8800) - [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0) - [Link](https://commons.wikimedia.org/w/index.php?curid=3578148)

