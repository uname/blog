title: PyQt4中的右键弹出菜单
date: 2016-03-15 10:36:05
tags: Python PyQt4
---

笔者在用PyQt4写GUI程序时，有时为了获得更好的交互体验，会给一些控件加右键弹出菜单。这里以一段代码为例，记录下给控件添加右键菜单的步骤。
```python
#-*- coding: utf-8 -*-
import sys
from PyQt4 import QtGui
from PyQt4 import QtCore

class TestWidget(QtGui.QWidget):
    
    def __init__(self, parent=None):
        QtGui.QWidget.__init__(self, parent)
        
        self.setGeometry (100, 100, 200, 120)
        
        # STEP 0 create a widget
        self.button = QtGui.QPushButton("Test button", self)
        # STEP 1 setContextMenuPolicy
        self.button.setContextMenuPolicy(QtCore.Qt.CustomContextMenu)
        # STEP 2 connect signal
        self.connect(self.button,
                     QtCore.SIGNAL("customContextMenuRequested(const QPoint&)"),
                     self.onButtonPopMenu)
        # STEP 3 make a menu and add actions
        self.buttonMenu = QtGui.QMenu(self)
        action1 = QtGui.QAction("Test Action", self,
                                 priority=QtGui.QAction.LowPriority,
                                 triggered=self.onAction1)
        # Add icon for menu action if you have icon resource(not necessary)
        #action1.setIcon(QtGui.QIcon(":/app/icons/app/copy.png"))
        self.buttonMenu.addAction(action1)
        # you can create more actions and add them to self.buttonMenu
    
    def onButtonPopMenu(self, point):
        self.buttonMenu.exec_(self.button.mapToGlobal(point))
    
    def onAction1(self):
        print "Hello uname"
        
if __name__ == "__main__":
    app = QtGui.QApplication(sys.argv)
    widget = TestWidget()
    widget.show()
    app.exec_()
    
```
以上代码运行效果如图：
![](/images/pyqt4-popmenu/demo.gif)

