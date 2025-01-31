# 📜 Query Log File

## 📖 Example of Queries <a name="queries"></a>: 
1. [User Request: Show all buildings in Röblingweg](#query-1)
2. [User Request: Find the building with the highest heat demand among the buildings above.](#query-2)
3. [User Request: Show all buildings in Weyprechtstraße with more than 5 apartments](#query-3)
4. [User Request: Find the oldest building in Weyprechtstraße](#query-4)
5. [User Request: What was the electricity consumption of this building today, June 26, 2024?](#query-5)
6. [User Request: Attached is the Energy Certificate of a building. Show me the building.](#query-6)
7. [User Request: Where is the nearest restaurant to this building? And what is the distance between the two?](#query-7)
8. [User Request: Which buildings on Weyprechstraße are connected to the district heating system?](#query-8)
9. [User Request: What are the different usage types of these buildings?](#query-9)
10. [User Request: How many habitants are living in these buildings on average?](#query-10)

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

## 🔥 User Request: Find the building with the highest heat demand among the buildings above <a name="query-2"></a>

**🛠️ Function Called:** `getObservationsFromFrostServer`

**🔍 Function Parameters:**
```
{
  observedProperty: 'Heat Demand',
  buildingID: 'DEBY_LOD2_4965683',
  phenomenonTime: '2024-01-01T00:00:00Z'
}
```
This will be repeated for all the buildings.

### 📊 Results:

| Building ID           | Heat Demand (January 2024) |
|----------------------|--------------------------|
| DEBY_LOD2_4965683   | **16,487**               |
| DEBY_LOD2_4965796   | 9,816                    |
| DEBY_LOD2_4965797   | 9,896                    |
| DEBY_LOD2_4965798   | 9,945                    |
| DEBY_LOD2_4965799   | 8,771                    |
| DEBY_LOD2_4965800   | 9,320                    |

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
```{
  observedProperty: 'Electricity Consumption',
  buildingID: 'DEBY_LOD2_4913414',
  phenomenonTime: '2024-06-26T00:00:00Z'
}
```
**📊 Result:**
| GML ID               | Electricity Consumption of June 26,2024 |
|----------------------|---------------------|
| DEBY_LOD2_4913414   | 58.294                |


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
| GML ID              |
|---------------------|
| DEBY_LOD2_4913221   | 

**🛠️ Function Called:** `showMeTheResult`

**🔍 Function Parameters:**
```
{gmlids: [{ gmlid:: "DEBY_LOD2_4913221"}]}
```

[🔝 Back to the list of queries](#queries)

---

## 🏗️ User Request:   Where is the nearest restaurant to this building? And what is the distance between the two? <a name="query-7"></a>

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
