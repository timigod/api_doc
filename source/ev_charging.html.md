---
title: Electric Vehicle Charging

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript
  - python

toc_footers:
  - Parts of the EV charging API were
  - inspired by the <a href="https://github.com/PlugShare/slate">PlugShare API</a>.
  - Documentation powered by <a href="https://github.com/lord/slate">Slate</a>.

includes:

search: true
---

# Electric Vehicle Charging Protocol

The communication protocol for electric vehicle charging describes the format of a request for a charging service (need), and the response sent by a charging provider (bid).

> Need

```shell
curl "discovery_endpoint_here" \
  --data "start_at=2017-12-11T15:18:54+03:00" \
  --data "latitude=32.787793" \
  --data "longitude=-79.935005" \
  --data "radius=1000" \
  --data "connector=tesla_supercharger" \
  --data "amenities=3"
```

```javascript
const discoveryEndPoint = "discovery_endpoint_here";

fetch(discoveryEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "start_at": "2017-12-11T15:18:54+03:00",
    "latitude": "32.787793",
    "longitude": "-79.935005",
    "radius": "1000",
    "connector": "tesla_supercharger",
    "amenities": "3",
  })
});
```

```python
import requests
payload = {
    "start_at": "2017-12-11T15:18:54+03:00",
    "latitude": "32.787793",
    "longitude": "-79.935005",
    "radius": "1000",
    "connector": "tesla_supercharger",
    "amenities": "3",
  }
requests.post("discovery_endpoint_here", data=payload)
```

For example, an electric vehicle might search for charging stations within 1 km of a given coordinate that support a Tesla supercharger plug, and also have restrooms.

> Bid

```shell
curl "vehicle_endpoint_here" \
  --data "request_uid=ae7bd8f67f3089c" \
  --data "expires_at=2017-12-11T15:18:59+03:00" \
  --data "price=2300000000000000000" \
  --data "latitude=32.785889" \
  --data "longitude=-79.935569" \
  --data "available_from=2017-12-11T15:18:54+03:00" \
  --data "available_until=2017-12-12T15:18:54+03:00" \
  --data "connectors=tesla_hpwc,tesla_supercharger" \
  --data "levels=2,3" \
  --data "amenities=2,3,4,7,9" \
  --data "address=372 King St, Charleston, SC 29401, USA" \
  --data "manufacturer=Tesla" \
  --data "model=Supercharger"
```

```javascript
const vehicleEndPoint = "vehicle_endpoint_here";

fetch(vehicleEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "request_uid": "ae7bd8f67f3089c",
    "expires_at": "2017-12-11T15:18:59+03:00",
    "price": "2300000000000000000",
    "latitude": "32.785889",
    "longitude": "-79.935569",
    "available_from": "2017-12-11T15:18:54+03:00",
    "available_until": "2017-12-12T15:18:54+03:00",
    "connectors": "tesla_hpwc,tesla_supercharger",
    "levels": "2,3",
    "amenities": "2,3,4,7,9",
    "address": "372 King St, Charleston, SC 29401, USA",
    "manufacturer": "Tesla",
    "model": "Supercharger",
  })
});
```

```python
import requests
payload = {
    "request_uid": "ae7bd8f67f3089c",
    "expires_at": "2017-12-11T15:18:59+03:00",
    "price": "2300000000000000000",
    "latitude": "32.785889",
    "longitude": "-79.935569",
    "available_from": "2017-12-11T15:18:54+03:00",
    "available_until": "2017-12-12T15:18:54+03:00",
    "connectors": "tesla_hpwc,tesla_supercharger",
    "levels": "2,3",
    "amenities": "2,3,4,7,9",
    "address": "372 King St, Charleston, SC 29401, USA",
    "manufacturer": "Tesla",
    "model": "Supercharger",
  }
requests.post("vehicle_endpoint_here", data=payload)
```

In response, a charging station might send back a bid with a price per kWh, and the full details of the services it offers.

# Need

A statement of need for charging stations. Typically this will be sent by an electric vehicle that is looking for a charging station around certain coordinates.

This request is sent to the decentralized discovery engine which responds with status `200` and a unique identifier for this request. The details of this request are then broadcasted to DAV entities that can provide this service. <a href="#bid">Bids</a> are later received as separate calls.

## Arguments

> Post request to a local/remote discovery endpoint

```shell
curl "discovery_endpoint_here" \
  --data "start_at=2017-12-11T15:18:54+03:00" \
  --data "latitude=32.787793" \
  --data "longitude=-79.935005" \
  --data "radius=10000" \
  --data "connector=tesla_supercharger" \
  --data "level=3" \
  --data "amenities=2,3"
```

```javascript
const discoveryEndPoint = "discovery_endpoint_here";

fetch(discoveryEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "start_at": "2017-12-11T15:18:54+03:00",
    "latitude": "32.787793",
    "longitude": "-79.935005",
    "radius": "10000",
    "connector": "tesla_supercharger",
    "level": "3",
    "amenities": "2,3",
  })
});
```

```python
import requests
payload = {
    "start_at": "2017-12-11T15:18:54+03:00",
    "latitude": "32.787793",
    "longitude": "-79.935005",
    "radius": "10000",
    "connector": "tesla_supercharger",
    "level": "3",
    "amenities": "2,3",
  }
requests.post("discovery_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">start_at</code>
      <div class="type required">required</div>
    </td>
    <td>The time at which the requester would like to arrive at charging station. Specified in <a href="https://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <code class="field">latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate around which to search.</td>
  </tr>
  <tr>
    <td>
      <code class="field">longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate around which to search.</td>
  </tr>
  <tr>
    <td>
      <code class="field">radius</code>
      <div class="type required">required</div>
    </td>
    <td>Radius in meters around the search coordinates to search. Specified as an integer.</td>
  </tr>
  <tr>
    <td>
      <code class="field">connector</code>
      <div class="type">optional</div>
    </td>
    <td>The connector type required by the EV. Specified as a connector id. See <a href="#connector-types">Connector Types</a> for available values.</td>
  </tr>
  <tr>
    <td>
      <code class="field">level</code>
      <div class="type">optional</div>
    </td>
    <td>The charging level as defined by SAE standards. Specified as an integer. See <a href="#charging-levels">Charging Levels</a>.</td>
  </tr>
  <tr>
    <td>
      <code class="field">amenities</code>
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
curl "vehicle_endpoint_here" \
  --data "request_uid=ae7bd8f67f3089c" \
  --data "expires_at=2017-12-11T15:18:59+03:00" \
  --data "price=2300000000000000000" \
  --data "latitude=32.785889" \
  --data "longitude=-79.935569" \
  --data "available_from=2017-12-11T15:18:54+03:00" \
  --data "available_until=2017-12-12T15:18:54+03:00" \
  --data "connectors=tesla_hpwc,tesla_supercharger" \
  --data "levels=2,3" \
  --data "amenities=2,3,4,7,9" \
  --data "address=372 King St, Charleston, SC 29401, USA" \
  --data "manufacturer=Tesla" \
  --data "model=Supercharger"
```

```javascript
const vehicleEndPoint = "vehicle_endpoint_here";

fetch(vehicleEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "request_uid": "ae7bd8f67f3089c",
    "expires_at": "2017-12-11T15:18:59+03:00",
    "price": "2300000000000000000",
    "latitude": "32.785889",
    "longitude": "-79.935569",
    "available_from": "2017-12-11T15:18:54+03:00",
    "available_until": "2017-12-12T15:18:54+03:00",
    "connectors": "tesla_hpwc,tesla_supercharger",
    "levels": "2,3",
    "amenities": "2,3,4,7,9",
    "address": "372 King St, Charleston, SC 29401, USA",
    "manufacturer": "Tesla",
    "model": "Supercharger",
  })
});
```

```python
import requests
payload = {
    "request_uid": "ae7bd8f67f3089c",
    "expires_at": "2017-12-11T15:18:59+03:00",
    "price": "2300000000000000000",
    "latitude": "32.785889",
    "longitude": "-79.935569",
    "available_from": "2017-12-11T15:18:54+03:00",
    "available_until": "2017-12-12T15:18:54+03:00",
    "connectors": "tesla_hpwc,tesla_supercharger",
    "levels": "2,3",
    "amenities": "2,3,4,7,9",
    "address": "372 King St, Charleston, SC 29401, USA",
    "manufacturer": "Tesla",
    "model": "Supercharger",
  }
requests.post("vehicle_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">request_uid</code>
      <div class="type required">required</div>
    </td>
    <td>The UID of the request. This arrives as part of the request.</td>
  </tr>
  <tr>
    <td>
      <code class="field">expires_at</code>
      <div class="type required">required</div>
    </td>
    <td>This bid will expire at this time. Specified in <a href="https://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <code class="field">price</code>
      <div class="type required">required</div>
    </td>
    <td>The price per kWh. Specified as an integer representing DAV tokens without the decimal point padded to 18 decimals (1 DAV is 1000000000000000000).</td>
  </tr>
  <tr>
    <td>
      <code class="field">latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of the charging station.</td>
  </tr>
  <tr>
    <td>
      <code class="field">longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of the charging station.</td>
  </tr>
  <tr>
    <td>
      <code class="field">available_from</code>
      <div class="type required">required</div>
    </td>
    <td>The time from which the charging station can be made available. Specified in ISO 8601 including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <code class="field">available_until</code>
      <div class="type">optional</div>
    </td>
    <td>The time until which the charging station can be made available. Specified in <a href="https://en.wikipedia.org/wiki/ISO_8601">ISO 8601</a> including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <code class="field">connectors</code>
      <div class="type">required</div>
    </td>
    <td>A list of connector types available at this charging station. Specified as a comma separated list of connector ids. See <a href="#connector-types">Connector Types</a> for available values.</td>
  </tr>
  <tr>
    <td>
      <code class="field">levels</code>
      <div class="type">optional</div>
    </td>
    <td>A list of charging levels as defined by SAE standards available at this charging station. Specified as a comma separated list of integers. See <a href="#charging-levels">Charging Levels</a>.</td>
  </tr>
  <tr>
    <td>
      <code class="field">amenities</code>
      <div class="type">optional</div>
    </td>
    <td>A list of amenities that are present at this charging station. Specified as a comma separated list of amenity ids. See <a href="#amenities">Amenities</a>.</td>
  </tr>
  <tr>
    <td>
      <code class="field">address</code>
      <div class="type required">required</div>
    </td>
    <td>A street address or description of the charging station location.</td>
  </tr>
  <tr>
    <td>
      <code class="field">manufacturer</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the manufacturer of this station.</td>
  </tr>
  <tr>
    <td>
      <code class="field">model</code>
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
    <td><code>nema_5_15</code></td>
    <td>NEMA 5-15 Wall Outlet</td>
    <td>1</td>
  </tr>
  <tr>
    <td><code>nema_5_20</code></td>
    <td>NEMA 5-20 Wall Outlet</td>
    <td>1</td>
  </tr>
  <tr>
    <td><code>j1772</code></td>
    <td>SAE J1772</td>
    <td>2</td>
  </tr>
  <tr>
    <td><code>mennekes</code></td>
    <td>SAE J3068 / IEC 62196 (Mennekes)</td>
    <td>2</td>
  </tr>
  <tr>
    <td><code>nema_14_50</code></td>
    <td>NEMA 14-50 (RV plug)	</td>
    <td>2</td>
  </tr>
  <tr>
    <td><code>chademo</code></td>
    <td>CHAdeMO</td>
    <td>3</td>
  </tr>
  <tr>
    <td><code>ccs</code></td>
    <td>SAE Combined Charging System (CCS)</td>
    <td>3</td>
  </tr>
  <tr>
    <td><code>tesla_hpwc</code></td>
    <td>Tesla HPWC</td>
    <td>2</td>
  </tr>
  <tr>
    <td><code>tesla_supercharger</code></td>
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
    <td><code>1</code></td>
    <td>Standard house outlet</td>
  </tr>
  <tr>
    <td><code>2</code></td>
    <td>240 volt AC charging</td>
  </tr>
  <tr>
    <td><code>3</code></td>
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
    <td><code>1</code></td>
    <td>Lodging</td>
  </tr>
  <tr>
    <td><code>2</code></td>
    <td>Dining</td>
  </tr>
  <tr>
    <td><code>3</code></td>
    <td>Restrooms</td>
  </tr>
  <tr>
    <td><code>4</code></td>
    <td>EV Parking</td>
  </tr>
  <tr>
    <td><code>5</code></td>
    <td>Valet Parking</td>
  </tr>
  <tr>
    <td><code>6</code></td>
    <td>Park</td>
  </tr>
  <tr>
    <td><code>7</code></td>
    <td>WiFi</td>
  </tr>
  <tr>
    <td><code>8</code></td>
    <td>Shopping</td>
  </tr>
  <tr>
    <td><code>9</code></td>
    <td>Grocery</td>
  </tr>
</table>

