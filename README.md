# Running live @ 

<a href="#" target="_blank" rel="[noopener noreferrer](https://jeanyuhs.com/routetool/)">Jeanyuhs Rout Tool</a>
Download location CSV to test https://jeanyuhs.com/routetool/noodles-co.csv

# Self-Routing-Tool

Territory Route Helper (Local, Offline, Minimalist Version)
Overview

Territory Route Helper is a lightweight, secure, single-file route-planning tool for field sales managers.
It allows you to:

Upload a CSV of store locations (kept only in your browser)

Generate an optimized visit route (nearest-neighbor algorithm)

Filter by categories (VIP, New, Under Performer, etc.)

Add full timing estimates:

Leave-home time

Meeting duration

Average driving speed

Extra drive time to/from home

Produce a daily schedule with:

Arrival time at each stop

Meeting end time

Miles between stores

“Back home by” time

Open the full route in Google Maps for navigation

No backend. No installs. No data leaves your device.
Everything runs in the browser from a single .html file.

1. How We Collected Store Addresses + Geocode Data

For the prototype (Colorado Noodles & Company locations), we used OpenStreetMap’s Overpass Turbo — a web-based data extraction tool requiring no installation.

This method works for any brand, any category, and any region.

Steps to collect all store locations

Go to:
https://overpass-turbo.eu

Paste this query:

[out:csv(
  name,
  "addr:city",
  "addr:state",
  "addr:postcode",
  ::lat,
  ::lon;
  true;
  ","
)][timeout:60];

area["ISO3166-2"="US-CO"][admin_level=4]->.searchArea;

(
  node["name"="Noodles & Company"](area.searchArea);
  way["name"="Noodles & Company"](area.searchArea);
  relation["name"="Noodles & Company"](area.searchArea);
);

out center;


Click Run

Export → Raw data (Overpass API)

Save as noodles_raw.csv

Fixing the headers

Overpass exports fields like this:

name,addr:city,addr:state,addr:postcode,lat,lon


Change that line to this:

name,city,state,zip,lat,lng


Now the CSV is compatible with the route helper.

2. CSV Format Required by the Route Helper

You must include these fields:

name,city,state,zip,lat,lng


Optional (but fully supported):

address,category,last_visit_date


Example:

name,city,state,zip,lat,lng
Noodles & Company,Denver,CO,80222,39.1234,-104.5678
Noodles & Company,Boulder,CO,80305,39.9876,-105.1234

3. Using the Territory Route Helper
Files you need

route-helper.html

Your CSV file

README.md

Open the HTML file directly in your browser.
No server needed.

Step-by-step
1. Upload Your CSV

Click Choose CSV File → pick your file.
Data stays entirely in memory.

2. Select Your Route Settings

Anchor store (must-visit)

Max additional stops

Category filters

3. Configure timing

You enter:

Leave home time

Meeting duration (minutes)

Average driving speed (mph)

Extra time:

Home → first stop

Last stop → home

4. Generate route

The tool outputs:

Stop order

Arrival time

Meeting end time

Distances

Estimated time back home

5. Launch Google Maps

Tap Open in Google Maps to navigate via your real route.

4. How Routing & Timing Work
Routing

Uses nearest-neighbor:

Start at anchor store

Pick closest remaining store

Repeat until limit reached

Timing engine

Distance = Haversine formula (straight-line)

Drive time = distance ÷ speed

Meetings add fixed duration

Extra home→first and last→home times included

All converted to HH:MM timestamps

5. Privacy & Security

Data never uploaded

Runs offline

No tracking

No backend

Safe for sensitive sales territory files.

6. Deployment

Since it’s a single HTML file, you can host it anywhere:

GitHub Pages

Netlify / Vercel

Regular website

Local machine

Mobile Files app

USB

7. Future Enhancements (Optional)

Export schedule to CSV/PDF

"Visited today" tracking

Map visualization using Leaflet.js

True TSP optimization

Multi-day routing

PWA mobile app wrapper
