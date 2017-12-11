---
title: Electric Vehicle Charging

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - Parts of this API were inspired by
  - the <a href="https://github.com/PlugShare/slate">PlugShare API</a>
  - Documentation powered by <a href="https://github.com/lord/slate">Slate</a>

includes:

search: true
---

# Introduction

The communication protocol for Electric Vehicle charging describes the format of a request for a charging service, and the response sent by a charging provider.

# Need

A statement of need for charging stations. Typically this will be sent by an electric vehicle that is looking for a charging station around certain coordinates.

This request is sent to the decentralized discovery engine which responds with status 200 and a unique identifier for this request. The details of this request are then broadcasted to DAV entities that can provide this service. <a href="#bid">Bids</a> are later received as separate calls.

## Arguments

> Post request to a local/remote discovery endpoint

```shell
curl "discovery_endpoint_here"
  --data "start_at=2017-12-11T15:18:54+03:00"
  --data "latitude=45.518805"
  --data "longitude=-122.707975"
  --data "radius=10000"
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
    "radius": "10000",
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
      <div class="field">radius</div>
      <div class="type required">required</div>
    </td>
    <td>Radius in meters around the search coordinates to search. Specified as an integer.</td>
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

# Bid

A bid to provide a charging service. Typically sent from a charging station to an electric vehicle.

## Arguments

> Post request to a local/remote endpoint representing the vehicle

```shell
curl "vehicle_endpoint_here"
  --data "request_uid=ae7bd8f67f3089c"
  --data "price=2300000000000000000"
  --data "latitude=45.521361"
  --data "longitude=-122.690619"
  --data "available_from=2017-12-11T15:18:54+03:00"
  --data "available_until=2017-12-12T15:18:54+03:00"
  --data "connectors=tesla_hpwc,tesla_supercharger"
  --data "levels=2,3"
  --data "amenities=2,3,4,7,9"
  --data "address=Kings Hill/SW Salmon St MAX Station, Portland, OR 97205, USA"
  --data "manufacturer=Tesla"
  --data "model=Supercharger"
```

```javascript
const vehicleEndPoint = "vehicle_endpoint_here";

fetch(vehicleEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "request_uid": "ae7bd8f67f3089c",
    "price": "2300000000000000000",
    "latitude": "45.521361",
    "longitude": "-122.690619",
    "available_from": "2017-12-11T15:18:54+03:00",
    "available_until": "2017-12-12T15:18:54+03:00",
    "connectors": "tesla_hpwc,tesla_supercharger",
    "levels": "2,3",
    "amenities": "2,3,4,7,9",
    "address": "Kings Hill/SW Salmon St MAX Station, Portland, OR 97205, USA",
    "manufacturer": "Tesla",
    "model": "Supercharger",
  })
});
```

<table class="arguments">
  <tr>
    <td>
      <div class="field">request_uid</div>
      <div class="type required">required</div>
    </td>
    <td>The UID of the request. This arrives as part of the request.</td>
  </tr>
  <tr>
    <td>
      <div class="field">price</div>
      <div class="type required">required</div>
    </td>
    <td>The price per kWh. Specified as an integer representing DAV tokens without the decimal point padded to 18 decimals (1 DAV is 1000000000000000000).</td>
  </tr>
  <tr>
    <td>
      <div class="field">latitude</div>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of the charging station.</td>
  </tr>
  <tr>
    <td>
      <div class="field">longitude</div>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of the charging station.</td>
  </tr>
  <tr>
    <td>
      <div class="field">available_from</div>
      <div class="type required">required</div>
    </td>
    <td>The time from which the charging station can be made available. Specified in ISO 8601 including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <div class="field">available_until</div>
      <div class="type">optional</div>
    </td>
    <td>The time until which the charging station can be made available. Specified in ISO 8601 including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <div class="field">connectors</div>
      <div class="type">required</div>
    </td>
    <td>A list of connector types available at this charging station. Specified as a comma separated list of connector ids. See <a href="#connector-types">Connector Types</a> for available values.</td>
  </tr>
  <tr>
    <td>
      <div class="field">levels</div>
      <div class="type">optional</div>
    </td>
    <td>A list of charging levels as defined by SAE standards available at this charging station. Specified as a comma separated list of integers. See <a href="#charging-levels">Charging Levels</a>.</td>
  </tr>
  <tr>
    <td>
      <div class="field">amenities</div>
      <div class="type">optional</div>
    </td>
    <td>A list of amenities that are present at this charging station. Specified as a comma separated list of amenity ids. See <a href="#amenities">Amenities</a>.</td>
  </tr>
  <tr>
    <td>
      <div class="field">address</div>
      <div class="type required">required</div>
    </td>
    <td>A street address or description of the charging station location.</td>
  </tr>
  <tr>
    <td>
      <div class="field">manufacturer</div>
      <div class="type">optional</div>
    </td>
    <td>Name of the manufacturer of this station.</td>
  </tr>
  <tr>
    <td>
      <div class="field">model</div>
      <div class="type">optional</div>
    </td>
    <td>Name of the model of this station.</td>
  </tr>
</table>

# Connector Types

Connector types and their unique identifier.

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

Available charging levels.

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

