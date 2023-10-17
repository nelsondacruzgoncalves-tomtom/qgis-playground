# qgis-playground

## UI customization
```python
mb = QMessageBox()
mb.setText("Result: " + str(iface.browserModel().initialized()))
mb.exec()
iface.mainWindow().setWindowTitle('Kankun')

iface.pluginToolBar().setVisible(False)

iface.browserModel().refresh()
iface.mainWindow().findChildren(QWidget, 'Browser')[0].show()#.close()
iface.mainWindow().findChildren(QWidget, 'Layers')[0].show()#.close()
# print(dir(QgsBrowserModel))
# disable pan & zoom
iface.mapCanvas().mapTool().deactivate()


vector_menu = iface.vectorMenu()
raster_menu = iface.rasterMenu()
menubar = vector_menu.parentWidget()
menubar.removeAction(vector_menu.menuAction())#.addAction(...)
menubar.removeAction(raster_menu.menuAction())
```
## Http requests

```python
import requests

url = "http://localhost:3000/api/areaCreation/claim"
body = "admin@nelson.com"
headers = {
    'Accept': 'application/json',
    'Content-Type': 'application/json'
}
mb = QMessageBox()
#mb.setText(url)
#mb.exec()

response = requests.post(url=url, headers=headers, data=body)
mb.setText(str(response.status_code))
mb.exec()
mb.setText(str(response.json()["wkt"]))
mb.exec()
```
## Layers and features

```python
vlayer = QgsVectorLayer('Polygon', 'extent', 'memory')
QgsProject.instance().addMapLayer(vlayer)

f = QgsFeature()
geometry = QgsGeometry.fromRect(iface.mapCanvas().extent())
f.setGeometry(geometry)
print(geometry.asWkt())

vlayer.dataProvider().addFeature(f)

```
```python
vlayer = QgsVectorLayer('Point', 'extent', 'memory')
QgsProject.instance().addMapLayer(vlayer)

f = QgsFeature()
geometry = QgsGeometry.fromWkt("POINT(3 4)")
f.setGeometry(geometry)

vlayer.dataProvider().addFeature(f)
```
