---
title: Drone Delivery

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript
  - python

toc_footers:
  - Documentation powered by <a href="https://github.com/lord/slate" target="blank">Slate</a>.

search: true
---

<p class="header-image"><img src="/images/ev_charging/header.png" alt="Drone Delivery"></p>

# Drone Delivery Protocol

The following document describes the communication protocol for a package delivery service provided by an autonomous drone. It includes the format for both the request for a delivery service (also referred to as `need`) and the response sent by drones that `bid` on providing the delivery service.

For example, a user is looking for a drone to pick up a small 2kg package from his doorstep and deliver it to a friend's backyard 20km from him.

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

In response, a drone might send back a bid with a price, the estimated time it will arrive at the pickup location, and the estimated time it will arrive at the dropoff location.

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
  --data "amenities=2,3,4,7,9"
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
requests.post("vehicle_endpoint_here", data=payload)
```

# Need

A statement of need for a delivery service. Typically this will be sent by a user that is looking to deliver a package using a drone.

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
      <code class="field">start_at</code>
      <div class="type required">required</div>
    </td>
    <td>
      The time at which the requester would like the package to be picked up (if delivery should be done ASAP, specify the current time). This should be specified in <a href="https://en.wikipedia.org/wiki/ISO_8601" target="blank">ISO 8601</a> including date, time, and time offset from UTC.
    </td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of the pickup location.</td>
  </tr>
  <tr>
    <td>
      <code class="field">pickup_longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of the pickup location.</td>
  </tr>
  <tr>
    <td>
      <code class="field">dropoff_latitude</code>
      <div class="type required">required</div>
    </td>
    <td>The latitude coordinate of the dropoff destination.</td>
  </tr>
  <tr>
    <td>
      <code class="field">dropoff_longitude</code>
      <div class="type required">required</div>
    </td>
    <td>The longitude coordinate of the dropoff destination.</td>
  </tr>
  <tr>
    <td>
      <code class="field">requester_name</code>
      <div class="type">optional</div>
    </td>
    <td>The name of the person that is asking for the delivery service.</td>
  </tr>
  <tr>
    <td>
      <code class="field">requester_phone_number</code>
      <div class="type">optional</div>
    </td>
    <td>The phone number of the person that is asking for the delivery service.</td>
  </tr>
  <tr>
    <td>
      <code class="field">reference_id_number</code>
      <div class="type">optional</div>
    </td>
    <td>An ID number that might be needed for package dispatch.</td>
  </tr>
  <tr>
    <td>
      <code class="field">cargo_type</code>
      <div class="type required">required</div>
    </td>
    <td>The type of cargo to be delivered. See the full list of options <a href="#cargo-types">here</a>.
    </td>
  </tr>
  <tr>
    <td>
      <code class="field">hazardous_goods</code>
      <div class="type">optional</div>
    </td>
    <td>The drone's supported plug types. See the full list of options <a href="#hazardous-goods">here</a>.</td>
  </tr>
  <tr>
    <td>
      <code class="field">ip_protection_level</code>
      <div class="type">optional</div>
    </td>
    <td>Drones may provide a certain level of protection to the delievered package. See full list of options <a href="#ip-protection-level">here</a>.</td>
  </tr>
  <tr>
    <td>
      <code class="field">height</code>
      <div class="type">optional</div>
    </td>
    <td>The height of the package. Specified as an integer representing centimeters.</td>
  </tr>
  <tr>
    <td>
      <code class="field">width</code>
      <div class="type">optional</div>
    </td>
    <td>The width of the package. Specified as an integer representing centimeters.</td>
  </tr>
  <tr>
    <td>
      <code class="field">length</code>
      <div class="type">optional</div>
    </td>
    <td>The length of the package. Specified as an integer representing centimeters.</td>
  </tr>
  <tr>
    <td>
      <code class="field">weight</code>
      <div class="type">optional</div>
    </td>
    <td>The weight of the package. Specified as an integer representing kilograms.</td>
  </tr>
  <tr>
    <td>
      <code class="field">insurance_required</code>
      <div class="type">optional</div>
    </td>
    <td>Some deliveries may require an insurance service. Specified as a boolean TRUE/FALSE (deafult is false)</b>.</td>
  </tr>
  <tr>
    <td>
      <code class="field">insured_value</code>
      <div class="type">optional</div>
    </td>
    <td>The declared value of the package to be insured.</td>
  </tr>
  <tr>
    <td>
      <code class="field">insured_value_currency</code>
      <div class="type">optional</div>
    </td>
    <td>The currency in which the declared value is denoted. This should be specified as a 3 letter <a href="https://en.wikipedia.org/wiki/ISO_4217" target="blank">ISO 4217</a> code or "DAV".</td>
  </tr>
</table>

# Bid

A bid to provide a delivery service. Typically sent from a delivery drone to the user who requested the service.

## Arguments

> Post request to a local/remote endpoint representing the vehicle

```shell
curl "vehicle_endpoint_here" \
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
const vehicleEndPoint = "vehicle_endpoint_here";

fetch(vehicleEndPoint, {
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
    <td>This bid will expire at this time. Specified in <a href="https://en.wikipedia.org/wiki/ISO_8601" target="blank">ISO 8601</a> including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <code class="field">price</code>
      <div class="type required">required</div>
    </td>
    <td>The offered price for the delivery (including any additional fees, insurance or taxes). Specified as an integer representing DAV tokens without the decimal point padded to 18 decimals (1 DAV is 1000000000000000000).</td>
  </tr>
  <tr>
    <td>
      <code class="field">time_to_pickup</code>
      <div class="type required">required</div>
    </td>
    <td>The estimate time of arrival at the pickup location. Specified in <a href="https://en.wikipedia.org/wiki/ISO_8601" target="blank">ISO 8601</a> including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <code class="field">time_to_dropoff</code>
      <div class="type required">required</div>
    </td>
    <td>The estimate time of arrival at the dropoff location. Specified in <a href="https://en.wikipedia.org/wiki/ISO_8601" target="blank">ISO 8601</a> including date, time, and time offset from UTC.</td>
  </tr>
  <tr>
    <td>
      <code class="field">drone_contact</code>
      <div class="type">optional</div>
    </td>
    <td>Human readable information regarding the drone (e.g Megadronix Deliveries LTD. +31-338-594332).</td>
  </tr>
  <tr>
    <td>
      <code class="field">drone_manufacturer</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the manufacturer of this drone.</td>
  </tr>
  <tr>
    <td>
      <code class="field">drone_model</code>
      <div class="type">optional</div>
    </td>
    <td>Name of the model of this drone.</td>
  </tr>
</table>


# Cargo Types

The following table describes the different types of cargos according to international delivery standards.

<table class="reference">
  <tr>
    <th>Type</th>
  </tr>
  <tr>
    <td><code>class_1_Satchel 500g</code></td>
  </tr>
  <tr>
    <td><code>class_2_Satchel 1kg</code></td>
  </tr>
  <tr>
    <td><code>class_3_Satchel 3kg</code></td>
  </tr>
  <tr>
    <td><code>class_4_Satchel 5kg</code></td>
  </tr>
  <tr>
    <td><code>class_5_Unpackaged</code></td>
  </tr>
    <tr>
    <td><code>class_6_Other/Misc</code></td>
  </tr>
  <tr>
    <td><code>class_7_Envelope</code></td>
  </tr>
  <tr>
    <td><code>class_8_Bag</code></td>
  </tr>
  <tr>
    <td><code>class_9_Satchel</code></td>
  </tr>
  <tr>
    <td><code>class_10_Crate</code></td>
  </tr>
    <tr>
    <td><code>class_11_Tube</code></td>
  </tr>
  <tr>
    <td><code>class_12_Pallet</code></td>
  </tr>
  <tr>
    <td><code>class_13_Skid</code></td>
  </tr>
  <tr>
    <td><code>class_14_Heavy Carton</code></td>
  </tr>
      <tr>
    <td><code>class_15_Carton containing glass</code></td>
  </tr>
  <tr>
    <td><code>class_16_Carton containing liquids</code></td>
  </tr>
  <tr>
    <td><code>class_17_wine/beer/liquor carton</code></td>
  </tr>
  <tr>
    <td><code>class_18_Carton</code></td>
  </tr>
</table>

# Hazardous Goods

The following table describes the different types of hazardous goods according to international delivery standards.

<table class="reference">
  <tr>
    <th>Type</th>
  </tr>
  <tr>
    <td><code>class_1_explosives</code></td>
  </tr>
  <tr>
    <td><code>class_2_gases</code></td>
  </tr>
  <tr>
    <td><code>class_3_flammable and combustible liquids</code></td>
  </tr>
  <tr>
    <td><code>class_4_flammable solids</code></td>
  </tr>
  <tr>
    <td><code>class_5_oxidizing sibstances, organic peroxides</code></td>
  </tr>
    <tr>
    <td><code>class_6_toxic and infectious substances</code></td>
  </tr>
  <tr>
    <td><code>class_7_radioacive materials</code></td>
  </tr>
  <tr>
    <td><code>class_8_corrosives</code></td>
  </tr>
  <tr>
    <td><code>class_9_misc Hazardous</code></td>
  </tr>
</table>

# IP Protection Level

A drone may provide a certain level of protection from solids and/or liquids (mainly water and dust). The following table describes the different levels of protection based on International Protection Rating.

<table class="reference">
  <tr>
    <th>IP Rating</th>
    <th>First Digit - SOLIDS</th>
    <th>Second Digit - LIQUIDS</th>
  </tr>
  <tr>
    <td><code>IP51</code></td>
    <td>Protected from limited dust ingress</td>
    <td>Protected from dripping water (vertically falling drops)</td>
  </tr>
  <tr>
    <td><code>IP54</code></td>
    <td>Protected from limited dust ingress</td>
    <td>Protected from splashing water (from any direction) against the enclosure</td>
  </tr>
    <tr>
    <td><code>IP65</code></td>
    <td>Total protection from dust ingress</td>
    <td>Protected from projected water/water jets (from any direction) against the enclosure</td>
  </tr>
    <tr>
    <td><code>IP68</code></td>
    <td>Total protection from dust ingress</td>
    <td>Total protection from water ingress <b>or</b> water can enter but only in such a manner that it produces no harmful effects</td>
  </tr>
</table>