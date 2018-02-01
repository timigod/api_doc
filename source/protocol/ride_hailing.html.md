---
title: Ride Hailing

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript
  - python

toc_footers:
  - Documentation powered by <a href="https://github.com/lord/slate" target="blank">Slate</a>.

search: true
---

<p class="header-image"><img src="/images/ride_hailing/header.png" alt="Ride Hailing"></p>

#  Ride Hailing Protocol

The communication protocol for ride hailing describes the format of a request for a ride (also referred to as `need`) sent by a user, and the response (`bid`) sent by vehicle, driver, or car owner.

For example, a user might search for a ride within 3 km of a given coordinate in a car that can load a large travel suitcase, to a destination 20 km away.

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

In response, an autonomous vehicle might send back a bid with a price for the ride, the vehicle type and model, and the estimated time of arrival.

> Bid

```shell
curl "bidding_endpoint_here" \
  --data "request_uid=ae7bd8f67f3089c" \
  --data "expires_at=2017-12-11T15:18:59+03:00" \
  --data "price=2300000000000000000" \
  --data "latitude=32.785889" \
  --data "longitude=-79.935569" \
  --data "available_from=2017-12-11T15:18:54+03:00" \
  --data "available_until=2017-12-12T15:18:54+03:00" \
  --data "connectors=tesla_hpwc,tesla_supercharger" \
  --data "levels=2,3" \
  --data "amenities=2,3,4,7,9"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
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
  }
requests.post("bidding_endpoint_here", data=payload)
```

# Need

A statement of need for a ride. Typically this will be sent by a user that is looking for a ride to a certain destination.

This request is sent to the decentralized discovery engine which responds with status `200` and a unique identifier for this request. The details of this request are then broadcasted to DAV entities that can provide this service. <a href="#bid">Bids</a> are later received as separate calls.

## Arguments

> Post request to a local/remote discovery endpoint

```shell
curl "discovery_endpoint_here" \
  --data "start_at=2017-12-11T15:18:54+03:00" \
  --data "latitude=32.787793" \
  --data "longitude=-79.935005" \
  --data "radius=10000" \
  --data "height=200" \
  --data "width=120" \
  --data "length=330" \
  --data "weight=1200" \
  --data "connector=tesla_supercharger" \
  --data "level=3" \
  --data "energy_source=solar" \
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
    "height": "200",
    "width": "120",
    "length": "330",
    "weight": "1200",
    "connector": "tesla_supercharger",
    "level": "3",
    "energy_source": "solar",
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
    "height": "200",
    "width": "120",
    "length": "330",
    "weight": "1200",
    "connector": "tesla_supercharger",
    "level": "3",
    "energy_source": "solar",
    "amenities": "2,3",
  }
requests.post("discovery_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">pickup_at</code>
      <div class="type">optional</div>
    </td>
    <td>The time at which the requester would like to be picked up (if undefined, pick up time will be ASAP). Specified in <a href="https://en.wikipedia.org/wiki/ISO_8601" target="blank">ISO 8601</a> including date, time, and time offset from UTC</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of the pickup location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of the pickup location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_street</code>
      <div class="type">optional</div>
    </td>
    <td>The street name of the pick up location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_house_number</code>
      <div class="type">optional</div>
    </td>
    <td>The house number of the pick up location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_city</code>
      <div class="type">optional</div>
    </td>
    <td>The city of the pick up location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_postal_code</code>
      <div class="type">optional</div>
    </td>
    <td>The postal code of the pick up location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_county</code>
      <div class="type">optional</div>
    </td>
    <td>The county of the pick up location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_state</code>
      <div class="type">optional</div>
    </td>
    <td>The state of the pick up location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_country</code>
      <div class="type">optional</div>
    </td>
    <td>The country of the pick up location</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_location_name</code>
      <div class="type">optional</div>
    </td>
    <td>A human readable name/description of the pick up location (e.g., Bayonne Crossing Shopping Center New Jersey)</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_location_name_lang</code>
      <div class="type">optional</div>
    </td>
    <td>The language used in <code>pickup_location_name</code>. Specified using the 3 letter <a href="https://en.wikipedia.org/wiki/ISO_639-3" target="blank">ISO 639-3</a> language code</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of the dropoff destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of the dropoff destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_street</code>
      <div class="type">optional</div>
    </td>
    <td>The street name of the destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_house_number</code>
      <div class="type">optional</div>
    </td>
    <td>The house number of the destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_city</code>
      <div class="type">optional</div>
    </td>
    <td>The city of the destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_postal_code</code>
      <div class="type">optional</div>
    </td>
    <td>The postal code of the destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_county</code>
      <div class="type">optional</div>
    </td>
    <td>The county of the destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_state</code>
      <div class="type">optional</div>
    </td>
    <td>The state of the destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_country</code>
      <div class="type">optional</div>
    </td>
    <td>The country of the destination</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_location_name</code>
      <div class="type">optional</div>
    </td>
    <td>A human readable name/description of the destination location (e.g., Stadium Plaza Shopping Center New Jersey)</td>
  </tr>
  <tr>
    <td>
      <code class="field">destination_location_name_lang</code>
      <div class="type">optional</div>
    </td>
    <td>The language used in <code>destination_location_name_lang</code>. Specified using the 3 letter <a href="https://en.wikipedia.org/wiki/ISO_639-3" target="blank">ISO 639-3</a> language code</td>
  </tr>
  <tr>
    <td>
      <code class="field">vehicle_type</code>
      <div class="type">optional</div>
    </td>
    <td>The type of vehicle required for the ride. See full list of options <a href="#vehicle-types">here</a></td>
  </tr>
    <tr>
    <td>
      <code class="field">passengers</code>
      <div class="type required">required</div>
    </td>
    <td>The total number of passengers the vehicle should accommodate</td>
  </tr>
</table>

# Bid

A bid to provide a ride service. Typically sent by a car owner with the price for the ride, the distance from the pick up location and the estimated time of arrival.

## Arguments

> Post request to a local/remote endpoint representing the rider

```shell
curl "bidding_endpoint_here" \
  --data "request_uid=ae7bd8f67f3089c" \
  --data "expires_at=2017-12-11T15:18:59+03:00" \
  --data "price=2300000000000000000" \
  --data "latitude=32.785889" \
  --data "longitude=-79.935569" \
  --data "entrance_latitude=32.785878" \
  --data "entrance_longitude=-79.935558" \
  --data "exit_latitude=32.785878" \
  --data "exit_longitude=-79.935558" \
  --data "location_floor=2" \
  --data "location_name=IKEA parking lot B" \
  --data "location_name_lang=eng" \
  --data "location_house_number=372" \
  --data "location_street=King" \
  --data "location_city=Charleston" \
  --data "location_postal_code=29401" \
  --data "location_county=Charleston" \
  --data "location_state=SC" \
  --data "location_country=USA" \
  --data "available_from=2017-12-11T15:18:54+03:00" \
  --data "available_until=2017-12-12T15:18:54+03:00" \
  --data "height=300" \
  --data "width=200" \
  --data "length=580" \
  --data "weight=10000" \
  --data "connectors=tesla_hpwc,tesla_supercharger" \
  --data "levels=2,3" \
  --data "energy_source=solar" \
  --data "amenities=2,3,4,7,9" \
  --data "provider=Tesla" \
  --data "manufacturer=Tesla" \
  --data "model=Supercharger"
```

```javascript
const biddingEndPoint = "bidding_endpoint_here";

fetch(biddingEndPoint, {
  method: "POST",
  body: JSON.stringify({
    "request_uid": "ae7bd8f67f3089c",
    "expires_at": "2017-12-11T15:18:59+03:00",
    "price": "2300000000000000000",
    "latitude": "32.785889",
    "longitude": "-79.935569",
    "entrance_latitude": "32.785878",
    "entrance_longitude": "-79.935558",
    "exit_latitude": "32.785878",
    "exit_longitude": "-79.935558",
    "location_floor": "2",
    "location_name": "IKEA parking lot B",
    "location_name_lang": "eng",
    "location_house_number": "372",
    "location_street": "King",
    "location_city": "Charleston",
    "location_postal_code": "29401",
    "location_county": "Charleston",
    "location_state": "SC",
    "location_country": "USA",
    "available_from": "2017-12-11T15:18:54+03:00",
    "available_until": "2017-12-12T15:18:54+03:00",
    "height": "300",
    "width": "200",
    "length": "580",
    "weight": "10000",
    "connectors": "tesla_hpwc,tesla_supercharger",
    "levels": "2,3",
    "energy_source": "solar",
    "amenities": "2,3,4,7,9",
    "provider": "Tesla",
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
    "entrance_latitude": "32.785878",
    "entrance_longitude": "-79.935558",
    "exit_latitude": "32.785878",
    "exit_longitude": "-79.935558",
    "location_floor": "2",
    "location_name": "IKEA parking lot B",
    "location_name_lang": "eng",
    "location_house_number": "372",
    "location_street": "King",
    "location_city": "Charleston",
    "location_postal_code": "29401",
    "location_county": "Charleston",
    "location_state": "SC",
    "location_country": "USA",
    "available_from": "2017-12-11T15:18:54+03:00",
    "available_until": "2017-12-12T15:18:54+03:00",
    "height": "300",
    "width": "200",
    "length": "580",
    "weight": "10000",
    "connectors": "tesla_hpwc,tesla_supercharger",
    "levels": "2,3",
    "energy_source": "solar",
    "amenities": "2,3,4,7,9",
    "provider": "Tesla",
    "manufacturer": "Tesla",
    "model": "Supercharger",
  }
requests.post("bidding_endpoint_here", data=payload)
```

<table class="arguments">
  <tr>
    <td>
      <code class="field">request_uid</code>
      <div class="type required">required</div>
    </td>
    <td>The UID of the request. This arrives as part of the request</td>
  </tr>
  <tr>
    <td>
      <code class="field">expires_at</code>
      <div class="type required">required</div>
    </td>
    <td>This bid will expire at this time. Specified in <a href="https://en.wikipedia.org/wiki/ISO_8601" target="blank">ISO 8601</a> including date, time, and time offset from UTC</td>
  </tr>
  <tr>
    <td>
      <code class="field">price</code>
      <div class="type required">required</div>
    </td>
    <td>The total price for the ride. Specified as an integer representing DAV tokens without the decimal point padded to 18 decimals (1 DAV is 1000000000000000000)</td>
  </tr>
  <tr>
    <td>
      <code class="field">current_latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of of where the vehicle is currently located</td>
  </tr>
  <tr>
    <td>
      <code class="field">current_longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of where the vehicle is currently located</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_at</code>
      <div class="type required">required</div>
    </td>
    <td>The estimated time of arrival at the pick up location. Specified in <a href="https://en.wikipedia.org/wiki/ISO_8601" target="blank">ISO 8601</a> including date, time, and time offset from UTC</td>
  </tr>
  <tr>
    <td>
      <code class="field">vehicle_type</code>
      <div class="type required">required</div>
    </td>
    <td>The type of vehicle used for the ride. See full list of options <a href="#vehicle-types">here</a></td>
  </tr>
  <tr>
  <tr>
    <td>
      <code class="field">vehicle_manufacturer</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the manufacturer of the vehicle</td>
  </tr>
  <tr>
    <td>
      <code class="field">vehicle_model</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the model of the vehicle</td>
  </tr>
  <tr>
    <td>
      <code class="field">vehicle_color</code>
      <div class="type">optional</div>
    </td>
    <td>The color of the vehicle</td>
  </tr>
  <tr>
    <td>
      <code class="field">vehicle_license_number</code>
      <div class="type">optional</div>
    </td>
    <td>The license plate number of the vehicle</td>
  </tr>
  <tr>
    <td>
      <code class="field">vehicle_contact</code>
      <div class="type">optional</div>
    </td>
    <td>Human readable information regarding the vehicle (e.g <code>James McGill, mobile: 555-338-5943</code>)</td>
  </tr>
</table>

# Vehicle Types

The type of vehicles and their unique identifier.

<table class="reference vehicles">
  <tr>
    <th>Vehicle Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>sedan</code></td>
    <td>Standard private car</td>
  </tr>
  <tr>
    <td><code>station</code></td>
    <td>Extended Sedan with extra room for luggage</td>
  </tr>
  <tr>
    <td><code>suv</code></td>
    <td>A kind of station wagon with off-road vehicle features such as raised ground clearance and ruggedness</td>
  </tr>
  <tr>
    <td><code>luxury</code></td>
    <td>A vehicle with higher quality equipment, better performance and enhanced comfort</td>
  </tr>
</table>
