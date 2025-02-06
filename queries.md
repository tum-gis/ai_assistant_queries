# 📜 Query Log File

## 📖 Example of Queries <a name="queries"></a>: 
1. [User Request: Show all buildings in Röblingweg](#query-1)
2. [User Request: Find the building with the highest heat demand in January 2024 among the buildings above.](#query-2)
3. [User Request: Show all buildings in Weyprechtstraße with more than 5 apartments](#query-3)
4. [User Request: Find the oldest building in Weyprechtstraße](#query-4)
5. [User Request: What was the electricity consumption of this building today, June 26, 2024?](#query-5)
6. [User Request: Attached is the Energy Certificate of a building. Show me the building.](#query-6)
7. [User Request: Compare the annual heat demand in the Energy certificate with the sum of the monthly head demand of this building.](#query-7)
8. [User Request: Where is the nearest restaurant to this building? And what is the distance between the two?](#query-8)
9. [User Request: Which buildings on Weyprechstraße are connected to the district heating system?](#query-9)
10. [User Request: What are the different usage types of these buildings?](#query-10)
11. [User Request: How many habitants are living in these buildings on average?](#query-11)
12. [User Request: What is the highest building in Schleißheimer Street?](#query-12)
13. [User Request: Which building has the most floors?](#query-13)
14. [User Request: Give me a tabular overview of the different types of use of the buildings for each street and how many buildings fall into each category.](#query-14)
15. [User Request: Create a table in which the number of residential buildings and the respective totals of the number of apartments, number of residents and total usable area are shown for each street.](#query-15)
16. [User Request: Give me a tabular overview of the different types of use of the buildings in Weyprechtstraße and how many buildings fall into each category.](#query-16)
17. [User Request: Can you please do this street by street for all buildings?](#query-17)
18. [User Request: Create a table in which the number of residential buildings and the respective totals of the number of apartments, number of residents and total usable area are shown for each street.](#query-18)

**DISCLAIMER:** The values presented in the Results section are not based on real data. For privacy reasons, the original values have been altered and do not reflect actual data.
---

## 🏢 User Request: Show all buildings in Röblingweg <a name="query-1"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT DISTINCT co.gmlid
 FROM cityobject co
 JOIN building b ON b.id = co.id
 JOIN address_to_building ab ON b.id = ab.building_id
 JOIN address a ON ab.address_id = a.id
 WHERE a.street LIKE 'Röblingweg%';
```

**📊 Result:**
```
[
  '{"gmlid":"DEBY_LOD2_4965683"}',
  '{"gmlid":"DEBY_LOD2_4965796"}',
  '{"gmlid":"DEBY_LOD2_4965797"}',
  '{"gmlid":"DEBY_LOD2_4965798"}',
  '{"gmlid":"DEBY_LOD2_4965799"}',
  '{"gmlid":"DEBY_LOD2_4965800"}',
  '{"gmlid":"DEBY_LOD2_4965801"}',
  '{"gmlid":"DEBY_LOD2_4965802"}',
  '{"gmlid":"DEBY_LOD2_4965854"}',
  '{"gmlid":"DEBY_LOD2_4965855"}',
  '{"gmlid":"DEBY_LOD2_4965856"}',
  '{"gmlid":"DEBY_LOD2_4965857"}',
  '{"gmlid":"DEBY_LOD2_4965858"}'
]
```
**🛠️ Function Called:** `showMeTheResult`

**🔍 Function Parameters:**
```
{
  gmlids: [
  {gmlid:"DEBY_LOD2_4965683"},
  {gmlid:"DEBY_LOD2_4965796"},
  {gmlid:"DEBY_LOD2_4965797"},
  .....
  ]
}
```


[🔝 Back to the list of queries](#queries)

---

## 🔥 User Request: Find the building with the highest heat demand in January 2024 among the buildings above <a name="query-2"></a>

**🛠️ Function Called:** `getObservationsFromFrostServer`

**🔍 Function Parameters:**
```
[
{
  observedProperty: 'Heat Demand',
  buildingID: 'DEBY_LOD2_4965683',
  phenomenonTime: '2024-01-01T00:00:00Z'
},
{
  observedProperty: 'Heat Demand',
  buildingID: 'DEBY_LOD2_4965796',
  phenomenonTime: '2024-01-01T00:00:00Z'
},
....
]
```
This will be repeated for all the buildings.

### 📊 Results:
```
[
  {
    '@iot.selfLink': 'http://frost-server:8080/FROST-Server/v1.1/Observations(3347)',
    '@iot.id': 3347,
    phenomenonTime: '2024-01-01T00:00:00Z',
    resultTime: null,
    result: 16487
  },
  {
    '@iot.selfLink': 'http://frost-server:8080/FROST-Server/v1.1/Observations(2269)',
    '@iot.id': 2269,
    phenomenonTime: '2024-01-01T00:00:00Z',
    resultTime: null,
    result: 9816
  },
  {
    '@iot.selfLink': 'http://frost-server:8080/FROST-Server/v1.1/Observations(2857)',
    '@iot.id': 2857,
    phenomenonTime: '2024-01-01T00:00:00Z',
    resultTime: null,
    result: 9896
  },
....
]
```
**🏆 Conclusion:**
- The building **DEBY_LOD2_4965683** has the highest heat demand (**16,487**).

[🔝 Back to the list of queries](#queries)

---

## 🏠 User Request: Show all buildings in Weyprechtstraße with more than 5 apartments <a name="query-3"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍Function Parameters:**
```sql
SELECT co.gmlid, cga.strval AS number_of_building_units 
FROM cityobject co 
JOIN cityobject_genericattrib cga ON co.id = cga.cityobject_id 
JOIN address_to_building ab ON ab.building_id = co.id 
JOIN address a ON ab.address_id = a.id 
WHERE a.street LIKE 'Weyprechtstraße%' 
AND cga.attrname = 'number_of_building_units' 
AND CAST(cga.strval AS INTEGER) > 5;
```
### 📊 Results:

| Building ID         | number_of_building_units|
|---------------------|-------------------------|
| DEBY_LOD2_4966426   | 12                      |
| DEBY_LOD2_4913265   | 9                       |
| DEBY_LOD2_4913232   | 8                       |
| DEBY_LOD2_4913270   | 6                       |
| DEBY_LOD2_4913228   | 54                      |
| DEBY_LOD2_4913416   | 12                      |
| DEBY_LOD2_4913418   | 12                      |

**🛠️ Function Called:** `showMeTheResult`

**🔍 Function Parameters:**
```
{
  gmlids: [
  { gmlid:: "DEBY_LOD2_4966426"},
  { gmlid:: "DEBY_LOD2_4913265"},
  { gmlid:: "DEBY_LOD2_4913232"},
  { gmlid:: "DEBY_LOD2_4913270"},
  { gmlid:: "DEBY_LOD2_4913228"},
  { gmlid:: "DEBY_LOD2_4913416"},
  { gmlid:: "DEBY_LOD2_4913418"}
  ]
}
```

[🔝 Back to the list of queries](#queries)

---

## 🏛️ User Request: Find the oldest building in Weyprechtstraße <a name="query-4"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT co.gmlid, MIN(b.year_of_construction) as year_of_construction 
FROM cityobject co 
JOIN building b ON b.id = co.id 
JOIN address_to_building ab ON b.id = ab.building_id 
JOIN address a ON ab.address_id = a.id 
JOIN cityobject_genericattrib cga ON co.id = cga.cityobject_id 
WHERE a.street LIKE 'Weyprechtstraße%' 
AND cga.attrname = 'number_of_building_units' 
AND CAST(cga.strval AS INTEGER) > 5 
GROUP BY co.gmlid 
ORDER BY year_of_construction 
LIMIT 1;
```

**📊 Result:**
| GML ID               | Year of Construction |
|----------------------|---------------------|
| DEBY_LOD2_4913414   | 1956                 |

**🛠️ Function Called:** `showMeTheResult`

**🔍 Function Parameters:**
```
{gmlids: [{ gmlid:: "DEBY_LOD2_4913414"}]}
```
[🔝 Back to the list of queries](#queries)

---

## 🏗️ User Request: What was the electricity consumption of this building today, June 26, 2024? <a name="query-5"></a>

**🛠️ Function Called:** `getObservationsFromFrostServer`

**🔍 Function Parameters:**
```
[
{
  observedProperty: 'Electricity Consumption',
  buildingID: 'DEBY_LOD2_4913414',
  phenomenonTime: '2024-06-26T00:00:00Z'
},
{
  observedProperty: 'Electricity Consumption',
  buildingID: 'DEBY_LOD2_4913414',
  phenomenonTime: '2024-06-26T01:00:00Z'
},
{
  observedProperty: 'Electricity Consumption',
  buildingID: 'DEBY_LOD2_4913414',
  phenomenonTime: '2024-06-26T02:00:00Z'
},
...
]
```
**📊 Result:**
| GML ID               | Electricity Consumption of June 26,2024 |
|----------------------|---------------------|
| DEBY_LOD2_4913414   | 58.294               |


[🔝 Back to the list of queries](#queries)

---

## 🏗️ User Request:  Attached is the Energy Certificate of a building. Show me the building. <a name="query-6"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT co.gmlid
FROM cityobject co
JOIN building b ON b.id = co.id
JOIN address_to_building ab ON b.id = ab.building_id
JOIN address a ON ab.address_id = a.id
WHERE a.street = 'Weyprechtstraße'
AND a.house_number = 25
AND a.city = 'München';
```

**📊 Result:**
```
{ gmlid: "DEBY_LOD2_4913221" }
```

**🛠️ Function Called:** `showMeTheResult`

**🔍 Function Parameters:**
```
{gmlids: [{ gmlid:: "DEBY_LOD2_4913221"}]}
```

[🔝 Back to the list of queries](#queries)

---

## 🏗️ User Request: Compare the annual heat demand in the Energy certificate with the sum of the monthly head demand of this building. <a name="query-7"></a>

**🛠️ Function Called:** `getObservationsFromFrostServer`

**🔍 Function Parameters:**
```
{
  observedProperty: 'Heat Demand',
  buildingID: 'DEBY_LOD2_4913221',
  phenomenonTime: '2024-01-01T00:00:00Z'
}
{
  observedProperty: 'Heat Demand',
  buildingID: 'DEBY_LOD2_4913221',
  phenomenonTime: '2024-02-01T00:00:00Z'
}
...
{
  observedProperty: 'Heat Demand',
  buildingID: 'DEBY_LOD2_4913221',
  phenomenonTime: '2024-12-01T00:00:00Z'
}
```

**📊 Result:**
```
[
 {
    tool_call_id: 'call_pL4K7SkSQaFY0CfLZIW5Fxum',
    output: '[{"@iot.selfLink":"http://10.162.246.86:8080/FROST-Server/v1.1/Observations(1541)","@iot.id":1541,"phenomenonTime":"2024-01-01T00:00:00Z","resultTime":null,"result":72697}]'
  },
.....
  {
    tool_call_id: 'call_5zHWZLGmeTRKsrbi6jurVroS',
    output: '[{"@iot.selfLink":"http://10.162.246.86:8080/FROST-Server/v1.1/Observations(1551)","@iot.id":1551,"phenomenonTime":"2024-11-01T00:00:00Z","resultTime":null,"result":51492}]'
  },
  {
    tool_call_id: 'call_mPNc8gQWCYz2p8qzLkObj17l',
    output: '[{"@iot.selfLink":"http://10.162.246.86:8080/FROST-Server/v1.1/Observations(1552)","@iot.id":1552,"phenomenonTime":"2024-12-01T00:00:00Z","resultTime":null,"result":68624}]'
  }
]
```

[🔝 Back to the list of queries](#queries)

---

## 🏗️ User Request:   Where is the nearest restaurant to this building? And what is the distance between the two? <a name="query-8"></a>

**🛠️ Function Called:** `queryOSMGeolocation`

**🔍 Function Parameters:**
```query_text:
'restaurants near 48.20535302220026, 11.566860935563833'
```

**📊 Result:**
```
{
  formattedAddress: 'Hanssam, 456, Schleißheimer Straße, Nordhaide, Am Hart, Milbertshofen-Am Hart, München, Bayern, 80935, Deutschland',
  coordinates: { latitude: 48.2060886, longitude: 11.5644655 }
}
```

**🛠️ Function Called:** `calculateDistance`

**🔍 Function Parameters:**
```
{
  lat1: '48.20535302220026',
  lon1: '11.566860935563833',
  lat2: '48.2060886',
  lon2: '11.5644655'
}
```
**📊 Result:**
The Distance between the building and the closest restaurant is 0.19545495302809132

[🔝 Back to the list of queries](#queries)

---

## 🏗️ User Request:  Which buildings on Weyprechstraße are connected to the district heating system? <a name="query-9"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT DISTINCT co.gmlid 
FROM cityobject co 
JOIN building b ON b.id = co.id 
JOIN address_to_building ab ON b.id = ab.building_id 
JOIN address a ON ab.address_id = a.id
JOIN cityobject_genericattrib cga ON co.id = cga.cityobject_id
WHERE a.street LIKE 'Weyprechtstraße%' AND cga.attrname = 'district_heating' AND cga.strval = 'connected';
```

**📊 Result:**
```
[
  '{"gmlid":"DEBY_LOD2_4913206"}',
  '{"gmlid":"DEBY_LOD2_4913208"}',
  '{"gmlid":"DEBY_LOD2_4913209"}',
  '{"gmlid":"DEBY_LOD2_4913221"}',
  '{"gmlid":"DEBY_LOD2_4913222"}',
  '{"gmlid":"DEBY_LOD2_4913223"}',
  '{"gmlid":"DEBY_LOD2_4913224"}',
  '{"gmlid":"DEBY_LOD2_4913228"}',
  '{"gmlid":"DEBY_LOD2_4913230"}',
  '{"gmlid":"DEBY_LOD2_4913231"}',
  '{"gmlid":"DEBY_LOD2_4913232"}',
  '{"gmlid":"DEBY_LOD2_4913233"}',
  '{"gmlid":"DEBY_LOD2_4913234"}',
  '{"gmlid":"DEBY_LOD2_61557"}'
]
```

[🔝 Back to the list of queries](#queries)

---

## 🏗️ User Request:  What are the different usage types of these buildings? <a name="query-10"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT DISTINCT ga.strval AS usage_type
FROM cityobject_genericattrib ga
JOIN cityobject co ON ga.cityobject_id = co.id
WHERE co.gmlid IN ('DEBY_LOD2_4913206', 'DEBY_LOD2_4913208', 'DEBY_LOD2_4913209', 'DEBY_LOD2_4913221', 'DEBY_LOD2_4913222', 'DEBY_LOD2_4913223', 'DEBY_LOD2_4913224', 'DEBY_LOD2_4913228', 'DEBY_LOD2_4913230', 'DEBY_LOD2_4913231', 'DEBY_LOD2_4913232', 'DEBY_LOD2_4913233', 'DEBY_LOD2_4913234', 'DEBY_LOD2_4913257')
AND ga.attrname = 'building_usage';"
```

**📊 Result:**
```
[
  {"usage_type":"Chruch"},
  {"usage_type":"Residential"},
  {"usage_type":"Residential/Industry"}
]
```

[🔝 Back to the list of queries](#queries)

---
## 🏗️ User Request:  How many habitants are living in these buildings on average? <a name="query-11"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT AVG(CASE WHEN ga.datatype = 1 THEN CAST(ga.strval AS numeric) WHEN ga.datatype = 2 THEN ga.intval END) AS average_inhabitants
FROM cityobject_genericattrib ga
JOIN cityobject co
ON ga.cityobject_id = co.id
WHERE co.gmlid IN  ('DEBY_LOD2_4913206', 'DEBY_LOD2_4913208', 'DEBY_LOD2_4913209', 'DEBY_LOD2_4913221', 'DEBY_LOD2_4913222', 'DEBY_LOD2_4913223', 'DEBY_LOD2_4913224', 'DEBY_LOD2_4913228', 'DEBY_LOD2_4913230', 'DEBY_LOD2_4913231', 'DEBY_LOD2_4913232', 'DEBY_LOD2_4913233', 'DEBY_LOD2_4913234', 'DEBY_LOD2_4913257')
AND ga.attrname = 'num_of_inhabitants';
```

**📊 Result:**
```
[{"average_inhabitants":"22.2888888888888889"}]
```

[🔝 Back to the list of queries](#queries)

---
## 🏗️ User Request: What is the highest building in Schleißheimer Street? <a name="query-12"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT co.gmlid, b.measured_height
FROM cityobject co 
JOIN building b ON b.id = co.id
WHERE co.gmlid IN (
  SELECT DISTINCT co.gmlid
  FROM cityobject co 
  JOIN building b ON b.id = co.id 
  JOIN address_to_building ab ON b.id = ab.building_id
  JOIN address a ON ab.address_id = a.id
  WHERE a.street LIKE 'Schleißheimer%' 
)
ORDER BY b.measured_height DESC 
LIMIT 1;
```

**📊 Result:**
```
{
    gmlid:"DEBY_LOD2_4913221",
    measured_height:26.166
}
```
[🔝 Back to the list of queries](#queries)

---
## 🏗️ User Request: Which building has the most floors? <a name="query-13"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT co.gmlid, b.storeys_above_ground
FROM cityobject co
JOIN building b ON co.id = b.id
WHERE b.storeys_above_ground = (SELECT MAX(storeys_above_ground) FROM building)
```

**📊 Result:**
```
{
    gmlid:"DEBY_LOD2_4914289",
    storeys_above_ground: 11
}
```

[🔝 Back to the list of queries](#queries)

---
## 🏗️ User Request: Give me a tabular overview of the different types of use of the buildings for each street and how many buildings fall into each category <a name="query-14"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT cg.strval AS building_usage, COUNT(DISTINCT co.gmlid) AS Anzahl, a.street AS Straße
FROM cityobject co
JOIN cityobject_genericattrib cg ON co.id = cg.cityobject_id
JOIN address_to_building ab ON co.id = ab.building_id
JOIN address a ON ab.address_id = a.id
WHERE cg.attrname = 'building_usage'
GROUP BY cg.strval, a.street;
```

**📊 Result:**
```
[
  { "building_usage": "Garage", "count": 1, "street": "Rathenaustraße" },
  { "building_usage": "Commercial", "count": 1, "street": "Schleißheimer Straße" },
  { "building_usage": "Youth Center", "count": 1, "street": "Wegenerstraße" },
  { "building_usage": "Church", "count": 5, "street": "Hugo-Wolf-Straße" },
  { "building_usage": "Church", "count": 1, "street": "Weyprechtstraße" },
  { "building_usage": "Kindergarten", "count": 2, "street": "Hugo-Wolf-Straße" },
  { "building_usage": "Kindergarten", "count": 1, "street": "Wegenerstraße" },
  { "building_usage": "School", "count": 1, "street": "Hugo-Wolf-Straße" },
  { "building_usage": "School", "count": 2, "street": "Wegenerstraße" },
  { "building_usage": "Sports Club", "count": 1, "street": "Neuherbergstraße" },
  { "building_usage": "Sports Club", "count": 1, "street": "Trenkleweg" },
  { "building_usage": "Residential", "count": 4, "street": "Dolleschelstraße" },
  { "building_usage": "Residential", "count": 27, "street": "Erwin-von-Steinbach-Weg" },
  { "building_usage": "Residential", "count": 8, "street": "Feuchtwangerstraße" },
  { "building_usage": "Residential", "count": 30, "street": "Hugo-Wolf-Straße" },
  { "building_usage": "Residential", "count": 33, "street": "Karl-Postl-Straße" },
  { "building_usage": "Residential", "count": 1, "street": "Lieberweg" },
  { "building_usage": "Residential", "count": 8, "street": "Max-Liebermann-Straße" },
  { "building_usage": "Residential", "count": 11, "street": "Max-von-Laue-Straße" },
  { "building_usage": "Residential", "count": 6, "street": "Neuherbergstraße" },
  { "building_usage": "Residential", "count": 49, "street": "Parlerstraße" },
  { "building_usage": "Residential", "count": 22, "street": "Rathenaustraße" },
  { "building_usage": "Residential", "count": 13, "street": "Röblingweg" },
  { "building_usage": "Residential", "count": 12, "street": "Schleißheimer Straße" },
  { "building_usage": "Residential", "count": 3, "street": "Wegenerstraße" },
  { "building_usage": "Residential", "count": 41, "street": "Weyprechtstraße" },
  { "building_usage": "Residential/Commercial", "count": 2, "street": "Schleißheimer Straße" },
  { "building_usage": "Residential/Commercial", "count": 5, "street": "Weyprechtstraße" }
]
```

[🔝 Back to the list of queries](#queries)

---
## 🏗️ User Request: Create a table in which the number of residential buildings and the respective totals of the number of apartments, number of residents and total usable area are shown for each street. <a name="query-15"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT a.street AS Straße, COUNT(DISTINCT co.gmlid) AS Wohngebäude, SUM(CAST(cg1.strval AS INTEGER)) AS Anzahl_Wohnungen, SUM(CAST(cg2.strval AS INTEGER)) AS Anzahl_Bewohner, SUM(cg3.intval) AS gesamte_Nutzungsfläche
FROM cityobject co
JOIN cityobject_genericattrib cg1 ON co.id = cg1.cityobject_id AND cg1.attrname = 'number_of_building_units'
JOIN cityobject_genericattrib cg2 ON co.id = cg2.cityobject_id AND cg2.attrname = 'num_of_inhabitants'
JOIN cityobject_genericattrib cg3 ON co.id = cg3.cityobject_id AND cg3.attrname = 'usable_and_living_area'
JOIN address_to_building ab ON co.id = ab.building_id
JOIN address a ON ab.address_id = a.id
WHERE cg1.strval IS NOT NULL AND cg2.strval IS NOT NULL AND cg3.intval IS NOT NULL GROUP BY a.street;
```

**📊 Result:**
```
[
  {
    "street": "Dolleschelstraße",
    "residential_buildings": 3,
    "apartments": 40,
    "residents": 75,
    "total_usable_area": 1800
  },
  {
    "street": "Erwin-von-Steinbach-Weg",
    "residential_buildings": 25,
    "apartments": 290,
    "residents": 540,
    "total_usable_area": 16000
  },
  {
    "street": "Feuchtwangerstraße",
    "residential_buildings": 5,
    "apartments": 95,
    "residents": 210,
    "total_usable_area": 5700
  },
  {
    "street": "Hugo-Wolf-Straße",
    "residential_buildings": 30,
    "apartments": 300,
    "residents": 600,
    "total_usable_area": 16000
  },
  {
    "street": "Karl-Postl-Straße",
    "residential_buildings": 31,
    "apartments": 155,
    "residents": 230,
    "total_usable_area": 5800
  },
  {
    "street": "Max-Liebermann-Straße",
    "residential_buildings": 6,
    "apartments": 30,
    "residents": 70,
    "total_usable_area": 1400
  },
  {
    "street": "Max-von-Laue-Straße",
    "residential_buildings": 10,
    "apartments": 215,
    "residents": 420,
    "total_usable_area": 12200
  },
  {
    "street": "Neuherbergstraße",
    "residential_buildings": 7,
    "apartments": 280,
    "residents": 620,
    "total_usable_area": 23000
  },
  {
    "street": "Parlerstraße",
    "residential_buildings": 48,
    "apartments": 240,
    "residents": 390,
    "total_usable_area": 10300
  },
  {
    "street": "Rathenaustraße",
    "residential_buildings": 22,
    "apartments": 180,
    "residents": 280,
    "total_usable_area": 8000
  },
  {
    "street": "Röblingweg",
    "residential_buildings": 11,
    "apartments": 55,
    "residents": 110,
    "total_usable_area": 2900
  },
  {
    "street": "Schleißheimer Straße",
    "residential_buildings": 13,
    "apartments": 270,
    "residents": 510,
    "total_usable_area": 17200
  },
  {
    "street": "Wegenerstraße",
    "residential_buildings": 4,
    "apartments": 18,
    "residents": 30,
    "total_usable_area": 500
  },
  {
    "street": "Weyprechtstraße",
    "residential_buildings": 44,
    "apartments": 460,
    "residents": 980,
    "total_usable_area": 27500
  }
]

```

[🔝 Back to the list of queries](#queries)

---
## 🏗️ User Request: Give me a tabular overview of the different types of use of the buildings in Weyprechtstraße and how many buildings fall into each category <a name="query-16"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT cga.strval AS building_usage, COUNT(co.gmlid) AS number_of_buildings
FROM cityobject co
JOIN address_to_building ab ON co.id = ab.building_id
JOIN address a ON ab.address_id = a.id
LEFT JOIN cityobject_genericattrib cga ON cga.cityobject_id = co.id AND cga.attrname = 'building_usage'
WHERE a.street LIKE 'Weyprechtstraße%' AND co.objectclass_id = 26
GROUP BY cga.strval;
```

**📊 Result:**
```
[
    {"building_usage":"Chruch","number_of_buildings":"1"}',
    {"building_usage":"Residential","number_of_buildings":"64"}',
    {"building_usage":"Industry","number_of_buildings":"10"}
]

```
[🔝 Back to the list of queries](#queries)

---
## 🏗️ User Request: Can you please do this street by street for all buildings? <a name="query-17"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT a.street, cga.strval AS building_usage, COUNT(co.gmlid) AS number_of_buildings
FROM cityobject co
JOIN address_to_building ab ON co.id = ab.building_id
JOIN address a ON ab.address_id = a.id
LEFT JOIN cityobject_genericattrib cga ON cga.cityobject_id = co.id AND cga.attrname = 'building_usage'
WHERE co.objectclass_id = 26
GROUP BY a.street, cga.strval;
```

**📊 Result:**
```
[
  {"street": "Dolleschelstraße", "building_usage": "Residential", "number_of_buildings": 5},
  {"street": "Erwin-von-Steinbach-Weg", "building_usage": "Residential", "number_of_buildings": 25},
  {"street": "Feuchtwangerstraße", "building_usage": "Residential", "number_of_buildings": 10},
  {"street": "Hugo-Wolf-Straße", "building_usage": "Church", "number_of_buildings": 4},
  {"street": "Hugo-Wolf-Straße", "building_usage": "Kindergarten", "number_of_buildings": 3},
  {"street": "Hugo-Wolf-Straße", "building_usage": "School", "number_of_buildings": 2},
  {"street": "Hugo-Wolf-Straße", "building_usage": "Residential", "number_of_buildings": 28},
  {"street": "Karl-Postl-Straße", "building_usage": "Residential", "number_of_buildings": 30},
  {"street": "Lieberweg", "building_usage": "Residential", "number_of_buildings": 7},
  {"street": "Max-Liebermann-Straße", "building_usage": "Residential", "number_of_buildings": 6},
  {"street": "Max-von-Laue-Straße", "building_usage": "Residential", "number_of_buildings": 10},
  {"street": "Neuherbergstraße", "building_usage": "Sports Club", "number_of_buildings": 2},
  {"street": "Neuherbergstraße", "building_usage": "Residential", "number_of_buildings": 8},
  {"street": "Parlerstraße", "building_usage": "Residential", "number_of_buildings": 46},
  {"street": "Rathenaustraße", "building_usage": "Garage", "number_of_buildings": 2},
  {"street": "Rathenaustraße", "building_usage": "Residential", "number_of_buildings": 20},
  {"street": "Röblingweg", "building_usage": "Residential", "number_of_buildings": 15},
  {"street": "Schleißheimer Straße", "building_usage": "Commercial", "number_of_buildings": 2},
  {"street": "Schleißheimer Straße", "building_usage": "Residential", "number_of_buildings": 10},
  {"street": "Schleißheimer Straße", "building_usage": "Residential/Commercial", "number_of_buildings": 3},
  {"street": "Trenkleweg", "building_usage": "Sports Club", "number_of_buildings": 2},
  {"street": "Wegenerstraße", "building_usage": "Youth Center", "number_of_buildings": 2},
  {"street": "Wegenerstraße", "building_usage": "Kindergarten", "number_of_buildings": 1},
  {"street": "Wegenerstraße", "building_usage": "School", "number_of_buildings": 3},
  {"street": "Wegenerstraße", "building_usage": "Residential", "number_of_buildings": 5},
  {"street": "Weyprechtstraße", "building_usage": "Church", "number_of_buildings": 2},
  {"street": "Weyprechtstraße", "building_usage": "Residential", "number_of_buildings": 39},
  {"street": "Weyprechtstraße", "building_usage": "Residential/Commercial", "number_of_buildings": 6}
]

```
[🔝 Back to the list of queries](#queries)

---
## 🏗️ User Request: Create a table in which the number of residential buildings and the respective totals of the number of apartments, number of residents and total usable area are shown for each street. <a name="query-18"></a>

**🛠️ Function Called:** `runsqlquery`

**🔍 Function Parameters:**
```sql
SELECT a.street, count(co.gmlid) as anzahl_wohngebaude, SUM(cast(cga6.strval as integer)) AS anz_building_units, SUM(cast(cga15.intval as integer)) AS anz_bewohner, SUM(cast(cga14.intval as integer)) AS gesamte_nutzungsflache
FROM cityobject co
JOIN address_to_building ab ON co.id = ab.building_id
JOIN address a ON ab.address_id = a.id
LEFT JOIN cityobject_genericattrib cga13 ON cga13.cityobject_id = co.id AND cga13.attrname = 'building_usage'
LEFT JOIN cityobject_genericattrib cga6 ON cga6.cityobject_id = co.id AND cga6.attrname = 'number_of_building_units'
LEFT JOIN cityobject_genericattrib cga15 ON cga15.cityobject_id = co.id AND cga15.attrname = 'num_of_inhabitants'
LEFT JOIN cityobject_genericattrib cga14 ON cga14.cityobject_id = co.id AND cga14.attrname = 'usable_area'
WHERE cga13.strval LIKE 'Wohnen%' AND co.objectclass_id = 26
GROUP BY a.street;
```

**📊 Result:**
```
[
  {"street": "Dolleschelstraße", "residential_buildings": 5, "building_units": 50, "residents": null, "total_usable_area": 0},
  {"street": "Erwin-von-Steinbach-Weg", "residential_buildings": 25, "building_units": 290, "residents": null, "total_usable_area": 9400},
  {"street": "Feuchtwangerstraße", "residential_buildings": 10, "building_units": 105, "residents": null, "total_usable_area": 6300},
  {"street": "Hugo-Wolf-Straße", "residential_buildings": 28, "building_units": 310, "residents": null, "total_usable_area": 16900},
  {"street": "Karl-Postl-Straße", "residential_buildings": 30, "building_units": 180, "residents": null, "total_usable_area": 10600},
  {"street": "Lieberweg", "residential_buildings": 8, "building_units": 200, "residents": null, "total_usable_area": 38500},
  {"street": "Max-Liebermann-Straße", "residential_buildings": 7, "building_units": 30, "residents": null, "total_usable_area": 1200},
  {"street": "Max-von-Laue-Straße", "residential_buildings": 10, "building_units": 220, "residents": null, "total_usable_area": 6000},
  {"street": "Neuherbergstraße", "residential_buildings": 7, "building_units": 280, "residents": null, "total_usable_area": 8700},
  {"street": "Parlerstraße", "residential_buildings": 46, "building_units": 240, "residents": null, "total_usable_area": 11000},
  {"street": "Rathenaustraße", "residential_buildings": 20, "building_units": 170, "residents": null, "total_usable_area": 3100},
  {"street": "Röblingweg", "residential_buildings": 12, "building_units": 65, "residents": null, "total_usable_area": 2900},
  {"street": "Schleißheimer Straße", "residential_buildings": 12, "building_units": 275, "residents": null, "total_usable_area": 9100},
  {"street": "Wegenerstraße", "residential_buildings": 4, "building_units": 18, "residents": null, "total_usable_area": 580},
  {"street": "Weyprechtstraße", "residential_buildings": 45, "building_units": 460, "residents": null, "total_usable_area": 22100}
]
```
[🔝 Back to the list of queries](#queries)
