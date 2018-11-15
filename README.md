****Project 2****

**Overview**

For this project, I decided to make a simple set of maps depicting the change in vacant housing in Baltimore between 2000 and 2009. To do this, I used python to select the neighborhoods based off of how much they changed. This seperated them into those neighborhoods that increased the percentage of vacant lots, and those that diminished or maintained their levels of vacancy. This initial map, seperating only those two conditions, was strikingly similar to the "Black Butterfly" in Baltimore, where racial segregation had deligated racial minorities to live.

![](https://user-images.githubusercontent.com/42807663/48531150-05201980-e869-11e8-93f7-ed9ad5fc82b3.jpg)

Next, I symbolized the varied increases and decreases in vacancy by putting them in color gradients, with white symbolizing little to no change in vacancy, while dark red and dark blue symbolize dramatic increases and decreases respectively.

![](https://user-images.githubusercontent.com/42807663/48531148-02bdbf80-e869-11e8-8c34-e3396da9e58b.jpg)

Finally, I created 2 maps, one of 2000, and one of 2009, to quickly show the differences in vacancy that can be represented in a gif format below.

![](https://user-images.githubusercontent.com/42807663/48530405-95f4f600-e865-11e8-990e-1d70c13bec9d.gif)

**Methods and Data**

To create this map, I utilized the 2000-2009 shapefiles, and manipulated the data such that it displayed the areas of the city most heavily affected by an increased number of vacant lots

The Python scripts used in this project are below.
```python

#adding layers. Nothing Fancy here
layer1 = QgsVectorLayer('A:/UMBC/Senior/GES486/Project2/Data/2009.shp','2000','ogr')
layer1.isValid()
True
QgsProject.instance().addMapLayer(layer1)
<qgis._core.QgsVectorLayer object at 0x000001C1EEDE15E8>

#selecting the areas where the difference between 2000 and 2009 were positive (increased vacancy). I ran an Identical operation for the neighborhood that were stagnant or increased.
layer1 = iface.activeLayer()
layer1.selectByExpression("Dif > 0")
QgsVectorFileWriter.writeAsVectorFormat(layer1, r'A:/UMBC/Senior/GES486/Project2/Data/OUTPUT_layer1.gpkg', 'utf-8', layer1.crs(),'GPKG', True)
(0, '')
IncVac = QgsVectorLayer('A:/UMBC/Senior/GES486/Project2/Data/OUTPUT_layer1.gpkg','IncVac','ogr')
QgsProject.instance().addMapLayer(IncVac)
<qgis._core.QgsVectorLayer object at 0x000001BBFB2A38B8>


#My attempts at repainting various features to a gradient. I could get single colors, but couldn't get gradeints from white to red (areas with increased vacancy) and white to blue (areas withdecreased vacancy)
#About half the time I spent doing this project was on this part. I know I wasn't the only one who couldn't figure it out. Would you go over this?

sym = QgsFillSymbol.createSimple({'color_expression':'Dif','color':'blue'})
renderer = layer1.renderer()
renderer.setSymbol(sym)
layer1.triggerRepaint()
sym = QgsFillSymbol.createSimple({'color_expression':'Dif','color':'blues'})
renderer = layer1.renderer()
renderer.setSymbol(sym)
layer1.triggerRepaint()
#I tried several more ways besides these. Each either came up black, or was still a solid color
sym = QgsFillSymbol.createSimple({'color_expression':'Dif','graduated':'blues'})
renderer = layer1.renderer()
renderer.setSymbol(sym)
layer1.triggerRepaint()
sym = QgsFillSymbol.createSimple({'color_expression':'Dif','color':'red'})
renderer = layer2.renderer()
renderer.setSymbol(sym)
layer2.triggerRepaint()
iface.layerTreeView().refreshLayerSymbology(layer1.id())
iface.layerTreeView().refreshLayerSymbology(layer2.id())

```
