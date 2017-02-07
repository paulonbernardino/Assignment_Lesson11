###### WUR Geo-scripting course
### Paulo Bernardino
### February 7th 2017
### Assignment 11

#Set WD
os.chdir("/home/ubuntu/Lesson11/Exercise11")

#Import modules
from osgeo import ogr,osr
import os

#Create shapefile
driverName = "ESRI Shapefile"
drv = ogr.GetDriverByName( driverName )
fn="points.shp"
layername="Wag Buildings"
ds = drv.CreateDataSource(fn)

# Set spatial reference
spatialReference = osr.SpatialReference()
spatialReference.ImportFromProj4('+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs')

## Create Layer
layer=ds.CreateLayer(layername, spatialReference, ogr.wkbPoint)

## Create the points
point1 = ogr.Geometry(ogr.wkbPoint)
point2 = ogr.Geometry(ogr.wkbPoint)

#Set points
point1.SetPoint(0,5.666002,51.987556) 
point2.SetPoint(0,5.663640,51.985283)

#Create feature
layerDefinition = layer.GetLayerDefn()
feature1 = ogr.Feature(layerDefinition)
feature2 = ogr.Feature(layerDefinition)
#Add the points to the feature
feature1.SetGeometry(point1)
feature2.SetGeometry(point2)
#Store the feature in a layer
layer.CreateFeature(feature1)
layer.CreateFeature(feature2)

#Export to KML and geojson
bashCommand = "ogr2ogr -f kml -t_srs crs:84 points.kml points.shp"
bashCommand2 = "ogr2ogr -f kml -t_srs crs:84 points.geojson points.kml"
os.system(bashCommand)
os.system(bashCommand2)