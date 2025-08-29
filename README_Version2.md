# 🌾 Spatio-Temporal Drought Assessment Using MODIS & ERA5 in Google Earth Engine

## 📄 Overview
This project applies remote sensing and geospatial analysis to monitor and assess drought conditions over the **WMH District** for **2023**. By integrating MODIS NDVI, MODIS LST, CHIRPS precipitation, and ERA5 soil moisture data, we compute vegetation, temperature, precipitation, and soil moisture indices. The workflow produces the Vegetation Condition Index (VCI), Temperature Condition Index (TCI), Soil Moisture Condition Index (SMCI), Drought Index (NDDI), and the integrated Vegetation Health Index (VHI), enabling identification of drought hotspots.

---

## 📊 Project Specifications

| Attribute          | Details                                                          |
|--------------------|------------------------------------------------------------------|
| **Title**          | Spatio-Temporal Drought Assessment Using MODIS & ERA5            |
| **Study Area**     | WMH District                                                     |
| **Period**         | 2023 (Jan–Dec)                                                   |
| **Data Sources**   | MODIS (NDVI, LST), CHIRPS, ERA5 Soil Moisture                    |
| **Spatial Res.**   | MODIS NDVI 250 m, MODIS LST 1 km, CHIRPS 0.05°, ERA5 ~11 km      |
| **Indices**        | VCI, TCI, SMCI, PCI, NDDI, VHI                                   |
| **Platform**       | Google Earth Engine (GEE)                                        |
| **Output Formats** | Raster (GeoTIFF), Map Layers                                     |

---

## ⚙️ Dependencies

| Library                | Purpose                                         |
|------------------------|-------------------------------------------------|
| Google Earth Engine    | Data access, processing, visualization          |
| ee.ImageCollection     | Handling multi-temporal remote sensing data     |
| Map / Layers           | Visualization of indices & hotspots             |


---

## 🚀 Workflow

```mermaid
graph TD
    A[🎯 Define Objective] --> B[🛰️ Load Remote Sensing Data]
    B --> C[📊 Preprocess Data: Clip, Scale, Convert Units]
    C --> D[🌱 Compute NDVI, LST, Rainfall, Soil Moisture Indices]
    D --> E[🧮 Normalize Indices: VCI, TCI, SMCI, PCI]
    E --> F[📈 Integrate Indices: Compute VHI & NDDI]
    F --> G[📍 Identify Drought Hotspots (VHI < Threshold)]
    G --> H[🗺️ Visualize & Map Results in GEE]
    H --> I[📤 Optional Export for Further GIS Analysis]
```

---

## 📌 Key Parameters

| Index  | Threshold / Scale | Description                             |
|--------|-------------------|-----------------------------------------|
| VCI    | 0–100             | Vegetation Condition Index              |
| TCI    | 0–100             | Temperature Condition Index             |
| SMCI   | 0–100             | Soil Moisture Condition Index           |
| VHI    | 0–5               | Vegetation Health Index (VCI + TCI)/2   |
| NDDI   | -1 to 1           | Drought severity indicator              |

---

## 📈 Analysis & Insights

- **VCI** identifies stressed vegetation zones. 🌱
- **TCI** highlights high-temperature affected areas. 🌞
- **SMCI** pinpoints soil moisture deficits. 💧
- **VHI** integrates VCI & TCI to reveal drought severity. 🔥
- **NDDI** indicates dryness vs wetness trends. 🌾
- Drought hotspots (**VHI < 40**) mapped for risk assessment. 📍

---

## 📦 Final Outputs

| Output Type        | Format       | Description                              |
|--------------------|-------------|------------------------------------------|
| VHI Raster         | GeoTIFF      | Vegetation Health Index across AOI       |
| Drought Hotspots   | Raster/Map   | Areas with severe drought (VHI < 40)     |
| Visualization      | GEE Map      | Color-coded map layers for indices       |

---

## ⚡ Highlights

- **Multi-source integration** → MODIS + CHIRPS + ERA5
- **Dynamic, temporal assessment** for 2023
- **Scalable workflow** for long-term monitoring
- Supports drought management, early warning, and agricultural planning 🌾

---

## ✍️ Author

Tejas Chavan  
* GIS Expert at Government Of Maharashtra Revenue & Forest Department  
* tejaskchavan22@gmail.com  
* +91 7028338510  