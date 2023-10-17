# qgis-playground

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
