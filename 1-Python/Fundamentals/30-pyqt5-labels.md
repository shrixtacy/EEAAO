# PyQt5 - Labels

## 📚 Table of Contents
1. [QLabel Widget](#qlabel-widget)
2. [Text Labels](#text-labels)
3. [Label Properties](#label-properties)
4. [Alignment and Styling](#alignment-and-styling)
5. [Practice Questions](#practice-questions)

---

## 1. QLabel Widget

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel
import sys

class MyWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Labels Example")
        self.setGeometry(100, 100, 400, 300)
        
        # Create label
        label = QLabel("Hello, World!", self)
        label.move(150, 100)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MyWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 2. Text Labels

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel
from PyQt5.QtCore import Qt
import sys

class MyWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Text Labels")
        self.setGeometry(100, 100, 500, 400)
        
        # Simple label
        label1 = QLabel("Simple Label", self)
        label1.move(50, 50)
        
        # Multi-line label
        label2 = QLabel("Line 1\nLine 2\nLine 3", self)
        label2.move(50, 100)
        
        # HTML label
        label3 = QLabel("<h1>HTML Label</h1><p>With <b>bold</b> text</p>", self)
        label3.move(50, 200)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MyWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 3. Label Properties

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel
from PyQt5.QtGui import QFont
import sys

class MyWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Label Properties")
        self.setGeometry(100, 100, 500, 400)
        
        label = QLabel("Styled Label", self)
        label.setGeometry(50, 50, 200, 50)
        
        # Set font
        font = QFont("Arial", 16)
        font.setBold(True)
        label.setFont(font)
        
        # Set style
        label.setStyleSheet("color: blue; background-color: yellow;")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MyWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 4. Alignment and Styling

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel
from PyQt5.QtCore import Qt
import sys

class MyWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Label Alignment")
        self.setGeometry(100, 100, 400, 300)
        
        # Center aligned
        label1 = QLabel("Center Aligned", self)
        label1.setGeometry(50, 50, 300, 50)
        label1.setAlignment(Qt.AlignCenter)
        label1.setStyleSheet("border: 1px solid black;")
        
        # Right aligned
        label2 = QLabel("Right Aligned", self)
        label2.setGeometry(50, 120, 300, 50)
        label2.setAlignment(Qt.AlignRight | Qt.AlignVCenter)
        label2.setStyleSheet("border: 1px solid black;")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MyWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 5. Practice Questions

**Question 1**: Create a label with custom font and color.
**Question 2**: Create multiple labels with different alignments.
**Question 3**: Create a label that displays HTML formatted text.

---

## 🎯 Key Takeaways

1. QLabel displays text or images
2. Use setFont() for custom fonts
3. Use setStyleSheet() for styling
4. Use setAlignment() for text alignment
5. Supports HTML formatting

---

**Next Topic**: PyQt5 - Images
**Previous Topic**: PyQt5 - Introduction

Happy Coding! 🐍
