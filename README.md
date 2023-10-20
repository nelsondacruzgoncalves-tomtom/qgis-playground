# qgis-playground

* https://docs.qgis.org/3.28/en/docs/pyqgis_developer_cookbook/index.html
* https://courses.spatialthoughts.com/pyqgis-in-a-day.html
* https://landscapearchaeology.org/2018/installing-python-packages-in-qgis-3-for-windows/
* https://pypi.org/project/ms-active-directory/
* https://qtibia.com/how-to-debug-a-qgis-plug-in-from-pycharm/

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
```python
from datetime import datetime

def show_time():
    now = datetime.now()
    current_time = now.strftime("%H:%M:%S")
    mb = QMessageBox()
    mb.setText(current_time)
    mb.exec()
    
show_time_action = QAction('Show time')
show_time_action.triggered.connect(show_time)
iface.helpMenu().addSeparator()
iface.helpMenu().addAction(show_time_action)
iface.addToolBarIcon(show_time_action)
```
```python
w = QDockWidget("My stuff", iface.mainWindow())
w.setObjectName("My widget")

frame = QFrame()
w.setWidget(frame)

grid = QGridLayout()
frame.setLayout(grid)

label = QLabel('Hello there')
claim = QPushButton('Claim')
resolve = QPushButton('Resolve')

grid.addWidget(label)
grid.addWidget(claim)
grid.addWidget(resolve)

iface.addDockWidget(Qt.RightDockWidgetArea, w)
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
## Event tracking
```python
def extentsChanged():
    extent = iface.mapCanvas().extent()
    print(str(extent))

iface.mapCanvas().extentsChanged.connect(extentsChanged)
#iface.mapCanvas().extentsChanged.disconnect()

extent = iface.mapCanvas().extent()

mb = QMessageBox()
mb.setText(str(extent))
mb.exec()
```
## Installing python packages

### From OSGeo4W Shell
```shell
python -m pip install ms-active-directory
```

### From Qgis
```python
import pip
pip.main(['install', 'ms-active-directory'])

from ms_active_directory import ADDomain
domain = ADDomain('example.com')

session = domain.create_session_as_user('username@example.com', 'password')
```
```log
ERROR: Could not install packages due to an OSError: [WinError 5] Access is denied: 'C:\\PROGRA~1\\QGIS32~1.9\\apps\\Python39\\Lib\\site-packages\\Crypto'
Consider using the `--user` option or check the permissions.
```
Installation will mark `site-packages` as read only. Need to change that manually.
```python
import subprocess
res = subprocess.check_call(['python', '-m', 'pip', 'install', 'ms-active-directory'])
print(res) # should be 0
```
