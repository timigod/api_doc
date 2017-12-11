---
title: Electric Vehicle Charging

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:

search: true
---

# Introduction

The communication protocol for Electric Vehicle charging describes the format of a request for a charging service, and the response sent by a charging provider.

# Request

A statement of need for charging stations, typically sent by an electric vehicle.

## Arguments

> Post request to a local/remote discovery endpoint

```shell
curl "discovery_endpoint_here"
  --data "start_at=2017-12-11T15:18:54+03:00"
  --data "latitude=45.518805"
  --data "longitude=-122.707975"
  --data "range=10000"
  --data "connector=tesla_supercharger"
  --data "level=3"
  --data "amenities=2,3"
```

```javascript
const discoveryEndPoint = "discovery_endpoint_here";

fetch(discoveryEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "start_at": "2017-12-11T15:18:54+03:00",
    "latitude": "45.518805",
    "longitude": "-122.707975",
    "range": "10000",
    "connector": "tesla_supercharger",
    "level": "3",
    "amenities": "2,3",
  })
});
```

<table class="arguments">
  <tr>
    <td>
      <div class="field">start_at</div>
      <div class="type required">required</div>
    </td>
    <td>The time at which the requester would like to arrive at charging station. Specified in ISO 8601 including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <div class="field">latitude</div>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate around which to search.</td>
  </tr>
  <tr>
    <td>
      <div class="field">longitude</div>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate around which to search.</td>
  </tr>
  <tr>
    <td>
      <div class="field">range</div>
      <div class="type required">required</div>
    </td>
    <td>Range around the search coordinate to search. Specified as an integer representing meters.</td>
  </tr>
  <tr>
    <td>
      <div class="field">connector</div>
      <div class="type">optional</div>
    </td>
    <td>The connector type required by the EV. Specified as a connector id. See <a href="#connector-types">Connector Types</a> for available values.</td>
  </tr>
  <tr>
    <td>
      <div class="field">level</div>
      <div class="type">optional</div>
    </td>
    <td>The charging level as defined by SAE standards. Specified as an integer. See <a href="#charging-levels">Charging Levels</a>.</td>
  </tr>
  <tr>
    <td>
      <div class="field">amenities</div>
      <div class="type">optional</div>
    </td>
    <td>A list of amenities that need to be present at charging station. Specified as a comma separated list of amenity ids. See <a href="#amenities">Amenities</a>.</td>
  </tr>
</table>

# Connector Types

<table class="reference connectors">
  <tr>
    <th>Connector ID</th>
    <th>Name</th>
    <th>Power Level</th>
  </tr>
  <tr>
    <td>nema_5_15</td>
    <td>NEMA 5-15 Wall Outlet</td>
    <td>1</td>
  </tr>
  <tr>
    <td>nema_5_20</td>
    <td>NEMA 5-20 Wall Outlet</td>
    <td>1</td>
  </tr>
  <tr>
    <td>j1772</td>
    <td>SAE J1772</td>
    <td>2</td>
  </tr>
  <tr>
    <td>mennekes</td>
    <td>SAE J3068 / IEC 62196 (Mennekes)</td>
    <td>2</td>
  </tr>
  <tr>
    <td>nema_14_50</td>
    <td>NEMA 14-50 (RV plug)	</td>
    <td>2</td>
  </tr>
  <tr>
    <td>chademo</td>
    <td>CHAdeMO</td>
    <td>3</td>
  </tr>
  <tr>
    <td>ccs</td>
    <td>SAE Combined Charging System (CCS)</td>
    <td>3</td>
  </tr>
  <tr>
    <td>tesla_hpwc</td>
    <td>Tesla HPWC</td>
    <td>2</td>
  </tr>
  <tr>
    <td>tesla_supercharger</td>
    <td>Tesla supercharger</td>
    <td>3</td>
  </tr>
</table>

# Charging Levels

<table class="reference levels">
  <tr>
    <th>Level</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>1</td>
    <td>Standard house outlet</td>
  </tr>
  <tr>
    <td>2</td>
    <td>240 volt AC charging</td>
  </tr>
  <tr>
    <td>3</td>
    <td>DC fast charging</td>
  </tr>
</table>

# Amenities

A list of amentities can be included in both requests and responses.

<table class="reference">
  <tr>
    <th>ID</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>1</td>
    <td>Lodging</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Dining</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Restrooms</td>
  </tr>
  <tr>
    <td>4</td>
    <td>EV Parking</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Valet Parking</td>
  </tr>
  <tr>
    <td>6</td>
    <td>Park</td>
  </tr>
  <tr>
    <td>7</td>
    <td>WiFi</td>
  </tr>
  <tr>
    <td>8</td>
    <td>Shopping</td>
  </tr>
  <tr>
    <td>9</td>
    <td>Grocery</td>
  </tr>
</table>

