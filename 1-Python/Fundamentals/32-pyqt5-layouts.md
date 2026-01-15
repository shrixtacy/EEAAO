# PyQt5 - Layout Managers

## 📚 Table of Contents
1. [QVBoxLayout and QHBoxLayout](#qvboxlayout-and-qhboxlayout)
2. [QGridLayout](#qgridlayout)
3. [QFormLayout](#qformlayout)
4. [Layout Properties](#layout-properties)
5. [Practice Questions](#practice-questions)

---

## 1. QVBoxLayout and QHBoxLayout

```python
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QHBoxLayout, QPushButton
import sys

class LayoutWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Layouts")
        self.setGeometry(100, 100, 400, 300)
        
        # Vertical layout
        vbox = QVBoxLayout()
        vbox.addWidget(QPushButton("Button 1"))
        vbox.addWidget(QPushButton("Button 2"))
        vbox.addWidget(QPushButton("Button 3"))
        
        # Horizontal layout
        hbox = QHBoxLayout()
        hbox.addWidget(QPushButton("Left"))
        hbox.addWidget(QPushButton("Center"))
        hbox.addWidget(QPushButton("Right"))
        
        # Combine layouts
        vbox.addLayout(hbox)
        self.setLayout(vbox)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = LayoutWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 2. QGridLayout

```python
from PyQt5.QtWidgets import QApplication, QWidget, QGridLayout, QPushButton
import sys

class GridWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Grid Layout")
        self.setGeometry(100, 100, 400, 300)
        
        grid = QGridLayout()
        
        # Add widgets to grid (row, column)
        grid.addWidget(QPushButton("1"), 0, 0)
        grid.addWidget(QPushButton("2"), 0, 1)
        grid.addWidget(QPushButton("3"), 0, 2)
        grid.addWidget(QPushButton("4"), 1, 0)
        grid.addWidget(QPushButton("5"), 1, 1)
        grid.addWidget(QPushButton("6"), 1, 2)
        
        self.setLayout(grid)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = GridWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 3. QFormLayout

```python
from PyQt5.QtWidgets import QApplication, QWidget, QFormLayout, QLineEdit, QLabel
import sys

class FormWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Form Layout")
        self.setGeometry(100, 100, 400, 200)
        
        form = QFormLayout()
        
        form.addRow(QLabel("Name:"), QLineEdit())
        form.addRow(QLabel("Email:"), QLineEdit())
        form.addRow(QLabel("Phone:"), QLineEdit())
        
        self.setLayout(form)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = FormWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 4. Layout Properties

```python
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QPushButton
import sys

class StyledLayoutWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Layout Properties")
        self.setGeometry(100, 100, 400, 300)
        
        layout = QVBoxLayout()
        
        # Set spacing
        layout.setSpacing(20)
        
        # Set margins
        layout.setContentsMargins(30, 30, 30, 30)
        
        layout.addWidget(QPushButton("Button 1"))
        layout.addWidget(QPushButton("Button 2"))
        layout.addWidget(QPushButton("Button 3"))
        
        # Add stretch
        layout.addStretch()
        
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = StyledLayoutWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 5. Practice Questions

**Question 1**: Create a calculator layout using QGridLayout.
**Question 2**: Build a form with QFormLayout for user registration.
**Question 3**: Combine multiple layouts to create a complex UI.

---

## 🎯 Key Takeaways

1. QVBoxLayout arranges widgets vertically
2. QHBoxLayout arranges widgets horizontally
3. QGridLayout arranges widgets in a grid
4. QFormLayout creates form-style layouts
5. Use setSpacing() and setContentsMargins() for styling

---

**Next Topic**: PyQt5 - Buttons
**Previous Topic**: PyQt5 - Images

Happy Coding! 🐍
