# AI Assistant for Urban Digital Twins

This repository is part of the research conducted for the paper:

**Advanced User Interaction with Urban Digital Twins using Large Language Models**  
Authors: Khaoula Kanna, Thomas H. Kolbe  
Link: will be available after publication.

Published at: [GSW 2025 Conference](https://gsw2025.ae/)

---

## 📖 **Overview**
This repository demonstrates the implementation of a framework integrating Large Language Models (LLMs) with Urban Digital Twins (UDTs). The framework supports intuitive interaction with semantic 3D City Models (CityGML) and time-series data (SensorThings API). The implementation highlights:
- Querying geospatial and dynamic data via natural language.
- Mapping user queries into SQL for CityGML data in 3DCityDB.
- Visualization of results.

The repository is structured to show case researchers, city planners, and citizens the potential of using LLMs for urban decision-making and understanding.

![Screenshot 2025-02-06 112440](https://github.com/user-attachments/assets/97dc4d82-13b7-4a04-aef2-f591db0d07d9)
---

## 📂 **Repository Structure**

## 📄 **Queries**
A list of example queries can be found in [queries.md](queries.md).

---

## 📚 **Key Data Sources**
- **CityGML**: The [CityGML](https://www.ogc.org/de/publications/standard/citygml/) LOD2 model was stored in the [3DCityDB](https://github.com/3dcitydb/3dcitydb). The 3DCityDB Schema given to the LLM can be found here: [database_schema.json](database_schema.json)
- **SensorThings API**: [SensorThings API GitHub Repository](https://github.com/opengeospatial/sensorthings)

---
## **🌟 Results**
![image](https://github.com/user-attachments/assets/e1b007e9-5d82-47dd-bab2-0b6e5de5b5b5)

---

## 📝 **Acknowledgments**
This work is supported by the [ASCEND](https://www.ascend-project.eu/) EU-project, in collaboration with the City of Munich.

---

## 📧 **Contact**
For further inquiries, feel free to reach out to:
- Khaoula Kanna: [khaoula.kanna@tum.de](mailto:khaoula.kanna@tum.de)
- Prof. Dr. Thomas H. Kolbe: [thomas.kolbe@tum.de](mailto:thomas.kolbe@tum.de)
