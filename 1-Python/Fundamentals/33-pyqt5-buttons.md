# PyQt5 - Buttons

## 📚 Table of Contents
1. [QPushButton](#qpushbutton)
2. [Button Signals and Slots](#button-signals-and-slots)
3. [Button Styling](#button-styling)
4. [Practice Questions](#practice-questions)

---

## 1. QPushButton

```python
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QVBoxLayout
import sys

class ButtonWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Buttons")
        self.setGeometry(100, 100, 300, 200)
        
        layout = QVBoxLayout()
        
        button1 = QPushButton("Click Me")
        button2 = QPushButton("Press Me")
        button3 = QPushButton("Push Me")
        
        layout.addWidget(button1)
        layout.addWidget(button2)
        layout.addWidget(button3)
        
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = ButtonWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 2. Button Signals and Slots

```python
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QLabel, QVBoxLayout
import sys

class InteractiveWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Button Signals")
        self.setGeometry(100, 100, 300, 200)
        
        self.label = QLabel("Click a button", self)
        self.counter = 0
        
        button1 = QPushButton("Increment")
        button1.clicked.connect(self.increment)
        
        button2 = QPushButton("Reset")
        button2.clicked.connect(self.reset)
        
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(button1)
        layout.addWidget(button2)
        
        self.setLayout(layout)
    
    def increment(self):
        self.counter += 1
        self.label.setText(f"Count: {self.counter}")
    
    def reset(self):
        self.counter = 0
        self.label.setText("Count: 0")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = InteractiveWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 3. Button Styling

```python
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QVBoxLayout
import sys

class StyledButtonWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Styled Buttons")
        self.setGeometry(100, 100, 300, 250)
        
        layout = QVBoxLayout()
        
        # Blue button
        blue_btn = QPushButton("Blue Button")
        blue_btn.setStyleSheet("background-color: blue; color: white; font-size: 16px;")
        
        # Green button
        green_btn = QPushButton("Green Button")
        green_btn.setStyleSheet("background-color: green; color: white; font-size: 16px;")
        
        # Red button
        red_btn = QPushButton("Red Button")
        red_btn.setStyleSheet("background-color: red; color: white; font-size: 16px;")
        
        layout.addWidget(blue_btn)
        layout.addWidget(green_btn)
        layout.addWidget(red_btn)
        
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = StyledButtonWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 4. Practice Questions

**Question 1**: Create a button that changes its text when clicked.
**Question 2**: Build a simple calculator with number buttons.
**Question 3**: Create buttons with custom colors and hover effects.

---

## 🎯 Key Takeaways

1. QPushButton creates clickable buttons
2. Use clicked.connect() to handle button clicks
3. Signals and slots enable event handling
4. setStyleSheet() for custom styling
5. Buttons can trigger any function

---

**Next Topic**: PyQt5 - Checkboxes
**Previous Topic**: PyQt5 - Layout Managers

Happy Coding! 🐍
