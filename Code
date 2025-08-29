//===========================//
//       1. Study Area       //
//===========================//
var aoi = ee.FeatureCollection('projects/gee-trial2/assets/Shapfile/WMH_Distric');
Map.centerObject(aoi, 5);

//===========================//
//   2. Date Range & Params  //
//===========================//
var start = '2023-01-01';
var end = '2023-12-31';

//===========================//
//    3. Load Remote Sensing //
//===========================//

// MODIS NDVI
var ndvi = ee.ImageCollection("MODIS/006/MOD13Q1")
             .select("NDVI")
             .filterDate(start, end)
             .map(function(img) {
               return img.clip(aoi)
                         .multiply(0.0001)
                         .copyProperties(img, ['system:time_start']);
             });

// MODIS LST
var lst = ee.ImageCollection("MODIS/006/MOD11A2")
             .select("LST_Day_1km")
             .filterDate(start, end)
             .map(function(img) {
               return img.clip(aoi)
                         .multiply(0.02)
                         .subtract(273.15)  // Kelvin to Celsius
                         .copyProperties(img, ['system:time_start']);
             });

// CHIRPS Rainfall
var chirps = ee.ImageCollection("UCSB-CHG/CHIRPS/DAILY")
                .filterDate(start, end)
                .map(function(img) {
                  return img.clip(aoi)
                            .copyProperties(img, ['system:time_start']);
                });

// Soil Moisture (ERA5)
var sm = ee.ImageCollection("ECMWF/ERA5_LAND/DAILY")
            .select('volumetric_soil_water_layer_1')
            .filterDate(start, end)
            .map(function(img) {
              return img.clip(aoi)
                        .copyProperties(img, ['system:time_start']);
            });

//===========================//
//      4. Compute Indices    //
//===========================//

// VCI
var ndviMin = ndvi.min();
var ndviMax = ndvi.max();
var vci = ndvi.map(function(img) {
  return img.subtract(ndviMin)
            .divide(ndviMax.subtract(ndviMin))
            .multiply(100)
            .rename('VCI')
            .copyProperties(img, ['system:time_start']);
});

// TCI
var lstMin = lst.min();
var lstMax = lst.max();
var tci = lst.map(function(img) {
  return lstMax.subtract(img)
               .divide(lstMax.subtract(lstMin))
               .multiply(100)
               .rename('TCI')
               .copyProperties(img, ['system:time_start']);
});

// PCI
var chirpsMin = chirps.min();
var chirpsMax = chirps.max();
var pci = chirps.map(function(img) {
  return img.subtract(chirpsMin)
            .divide(chirpsMax.subtract(chirpsMin))
            .multiply(100)
            .rename('PCI')
            .copyProperties(img, ['system:time_start']);
});

// SMCI
var smMin = sm.min();
var smMax = sm.max();
var smci = sm.map(function(img) {
  return img.subtract(smMin)
            .divide(smMax.subtract(smMin))
            .multiply(100)
            .rename('SMCI')
            .copyProperties(img, ['system:time_start']);
});

//===========================//
//   5. Integrated Indices    //
//===========================//

// VHI = (VCI + TCI)/2
var vhi = vci.map(function(vciImg) {
  var date = ee.Date(vciImg.get('system:time_start'));
  var tciMatch = tci.filterDate(date.advance(-8, 'day'), date.advance(8, 'day')).mean();
  var tciBand = ee.Algorithms.If(
    tciMatch.bandNames().contains('TCI'),
    tciMatch.select('TCI'),
    ee.Image(ee.Number(0)).rename('TCI')
  );
  return vciImg.expression('0.5 * (VCI + TCI)', {
    'VCI': vciImg.select('VCI'),
    'TCI': tciBand
  }).rename('VHI')
    .copyProperties(vciImg, ['system:time_start']);
});

// NDDI = (NDVI - NDWI)/(NDVI + NDWI)
var ndwi = ee.ImageCollection("MODIS/006/MOD13Q1")
             .select("250m_16_days_NDWI")
             .filterDate(start, end)
             .map(function(img) {
               return img.clip(aoi)
                         .multiply(0.0001)
                         .copyProperties(img, ['system:time_start']);
             });

var nddi = ndvi.map(function(ndviImg) {
  var date = ee.Date(ndviImg.get('system:time_start'));
  var ndwiImg = ndwi.filterDate(date.advance(-8, 'day'), date.advance(8, 'day')).mean();
  return ndviImg.expression('(NDVI - NDWI)/(NDVI + NDWI)', {
    'NDVI': ndviImg.select('NDVI'),
    'NDWI': ndwiImg.select('250m_16_days_NDWI')
  }).rename('NDDI')
    .copyProperties(ndviImg, ['system:time_start']);
});

//===========================//
//   6. Visualization & Hotspot //
//===========================//

// Latest VHI for hotspot analysis
var lastVHI = ee.Image(vhi.sort('system:time_start', false).first());
Map.addLayer(lastVHI, {min: 0, max: 5, palette: ['red', 'yellow', 'green']}, "Latest VHI");

// Drought hotspots (VHI < 40)
var droughtHotspot = lastVHI.lt(40).selfMask();
Map.addLayer(droughtHotspot, {palette: ['red']}, "Drought Hotspots (<40)");
