## Learning PySide6 by code and projects.

### 1. PySide6 introduction
>Qt for Python (PySide6) offers the official Python bindings for Qt, so that you can use Qt6 APIs in your Python applications.

### 2. PySide6 website
[PySide6 Website: https://doc.qt.io/qtforpython/](https://doc.qt.io/qtforpython/)

### 3. A simple window source code

```Python
# coding=utf-8

import sys
import random

from PySide6 import QtCore, QtWidgets, QtGui

class MyWidget(QtWidgets.QWidget):
  def __init__(self):
    super().__init__()

    self.button = QtWidgets.QPushButton("Click me!")
    self.text = QtWidgets.QLabel("Hello world", alignment=QtCore.Qt.AlignCenter)
    
    self.layout = QtWidgets.QVBoxLayout(self)
    self.layout.addWidget(self.text)
    self.layout.addWidget(self.button)

    self.hello = ["hello A", "hello B", 'hello C']
    self.button.clicked.connect(self.magic)

  @QtCore.Slot()
  def magic(self):
    self.text.setText(random.choice(self.hello))

if __name__=="__main__":
  app = QtWidgets.QApplication([])

  widget = MyWidget()
  widget.resize(720, 540)
  widget.show()

  sys.exit(app.exec())
```

### 4. Porting from PySide2 (Like PyQt5) to PySide6 (Like PyQt6)
```Python
# coding=utf-8

# Question: Stats people with salary < 20000 and people with salary > 20000?
# name salary age
# AAA 10000 30
# BBB 20000 32
# CCC 15000 24
# DDD 25000 34

# PySide 2to6 diff-1
# from PySide2.QtWidgets import QApplication, QMainWindow, QMessageBox, QPushButton, QPlainTextEdit
from PySide6.QtWidgets import QApplication, QMainWindow, QMessageBox, QPushButton, QPlainTextEdit

class Stats():
  def __init__(self):
    self.window = QMainWindow()
    self.window.resize(720, 540)
    self.window.move(300, 300)
    self.window.setWindowTitle("Statistics")

    self.textEdit = QPlainTextEdit(self.window)
    self.textEdit.setPlaceholderText("Please input your data table")
    self.textEdit.move(10, 10)
    self.textEdit.resize(520, 520)

    self.button = QPushButton('Stats', self.window)
    self.button.move(540, 10)
    self.button.clicked.connect(self.clickStats)

  def clickStats(self):
    info = self.textEdit.toPlainText()

    salary_above_20k = ''
    salary_below_20k = ''
    for line in info.splitlines():
      if not line.strip():
        continue
      parts = line.split(' ')
      parts = [p for p in parts if p]
      name, salary, age = parts
      if int(salary) >= 20000:
        salary_above_20k += name + "\n"
      else:
        salary_below_20k += name + "\n"

    QMessageBox.about(self.window, 'Stats results', 
      f'''Results of above 20k: \n{salary_above_20k} \nResults of below 20k: \n{salary_below_20k}''')

if __name__=="__main__":
  app = QApplication([])

  stats = Stats()
  stats.window.show()

  app.exec()
```
