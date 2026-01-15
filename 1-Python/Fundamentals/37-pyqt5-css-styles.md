# PyQt5 - CSS Styles

## 📚 Table of Contents
1. [StyleSheets in PyQt5](#stylesheets-in-pyqt5)
2. [CSS Syntax for Qt](#css-syntax-for-qt)
3. [Styling Widgets](#styling-widgets)
4. [Themes](#themes)
5. [Practice Questions](#practice-questions)

---

## 1. StyleSheets in PyQt5

```python
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QVBoxLayout
import sys

class StyledWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("CSS Styles")
        self.setGeometry(100, 100, 400, 300)
        
        # Apply stylesheet to window
        self.setStyleSheet("""
            QWidget {
                background-color: #f0f0f0;
            }
        """)
        
        button = QPushButton("Styled Button")
        button.setStyleSheet("""
            QPushButton {
                background-color: #4CAF50;
                color: white;
                border: none;
                padding: 10px;
                font-size: 16px;
                border-radius: 5px;
            }
            QPushButton:hover {
                background-color: #45a049;
            }
            QPushButton:pressed {
                background-color: #3d8b40;
            }
        """)
        
        layout = QVBoxLayout()
        layout.addWidget(button)
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = StyledWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 2. CSS Syntax for Qt

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel, QPushButton, QVBoxLayout
import sys

class CSSWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("CSS Syntax")
        self.setGeometry(100, 100, 500, 400)
        
        # Global stylesheet
        self.setStyleSheet("""
            QWidget {
                background-color: #2c3e50;
                font-family: Arial;
            }
            QLabel {
                color: white;
                font-size: 18px;
                padding: 10px;
            }
            QPushButton {
                background-color: #3498db;
                color: white;
                border: 2px solid #2980b9;
                padding: 10px 20px;
                font-size: 14px;
                border-radius: 5px;
                min-width: 100px;
            }
            QPushButton:hover {
                background-color: #2980b9;
            }
        """)
        
        label = QLabel("Welcome to PyQt5 Styling!")
        button1 = QPushButton("Button 1")
        button2 = QPushButton("Button 2")
        
        layout = QVBoxLayout()
        layout.addWidget(label)
        layout.addWidget(button1)
        layout.addWidget(button2)
        
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = CSSWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 3. Styling Widgets

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel, QPushButton, QLineEdit, QVBoxLayout
import sys

class WidgetStylesWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Widget Styles")
        self.setGeometry(100, 100, 500, 400)
        
        # Styled label
        label = QLabel("Styled Label")
        label.setStyleSheet("""
            QLabel {
                background-color: #e74c3c;
                color: white;
                font-size: 20px;
                font-weight: bold;
                padding: 15px;
                border-radius: 10px;
            }
        """)
        
        # Styled input
        line_edit = QLineEdit()
        line_edit.setPlaceholderText("Enter text...")
        line_edit.setStyleSheet("""
            QLineEdit {
                padding: 10px;
                font-size: 14px;
                border: 2px solid #3498db;
                border-radius: 5px;
                background-color: white;
            }
            QLineEdit:focus {
                border: 2px solid #2ecc71;
            }
        """)
        
        # Styled button
        button = QPushButton("Submit")
        button.setStyleSheet("""
            QPushButton {
                background-color: #2ecc71;
                color: white;
                border: none;
                padding: 12px 30px;
                font-size: 16px;
                font-weight: bold;
                border-radius: 5px;
            }
            QPushButton:hover {
                background-color: #27ae60;
            }
            QPushButton:pressed {
                background-color: #229954;
            }
        """)
        
        layout = QVBoxLayout()
        layout.addWidget(label)
        layout.addWidget(line_edit)
        layout.addWidget(button)
        layout.setSpacing(20)
        
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = WidgetStylesWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 4. Themes

```python
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QVBoxLayout, QLabel
import sys

class ThemedWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Themes")
        self.setGeometry(100, 100, 500, 400)
        
        self.label = QLabel("Current Theme: Dark")
        
        dark_btn = QPushButton("Dark Theme")
        dark_btn.clicked.connect(self.apply_dark_theme)
        
        light_btn = QPushButton("Light Theme")
        light_btn.clicked.connect(self.apply_light_theme)
        
        blue_btn = QPushButton("Blue Theme")
        blue_btn.clicked.connect(self.apply_blue_theme)
        
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(dark_btn)
        layout.addWidget(light_btn)
        layout.addWidget(blue_btn)
        
        self.setLayout(layout)
        self.apply_dark_theme()
    
    def apply_dark_theme(self):
        self.setStyleSheet("""
            QWidget {
                background-color: #2c3e50;
                color: white;
            }
            QPushButton {
                background-color: #34495e;
                color: white;
                border: none;
                padding: 10px;
                font-size: 14px;
                border-radius: 5px;
            }
            QPushButton:hover {
                background-color: #4a5f7f;
            }
        """)
        self.label.setText("Current Theme: Dark")
    
    def apply_light_theme(self):
        self.setStyleSheet("""
            QWidget {
                background-color: #ecf0f1;
                color: #2c3e50;
            }
            QPushButton {
                background-color: #bdc3c7;
                color: #2c3e50;
                border: none;
                padding: 10px;
                font-size: 14px;
                border-radius: 5px;
            }
            QPushButton:hover {
                background-color: #95a5a6;
            }
        """)
        self.label.setText("Current Theme: Light")
    
    def apply_blue_theme(self):
        self.setStyleSheet("""
            QWidget {
                background-color: #3498db;
                color: white;
            }
            QPushButton {
                background-color: #2980b9;
                color: white;
                border: none;
                padding: 10px;
                font-size: 14px;
                border-radius: 5px;
            }
            QPushButton:hover {
                background-color: #21618c;
            }
        """)
        self.label.setText("Current Theme: Blue")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = ThemedWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 5. Practice Questions

**Question 1**: Create a custom theme for a login form.
**Question 2**: Style a calculator app with modern CSS.
**Question 3**: Build a theme switcher with multiple color schemes.

---

## 🎯 Key Takeaways

1. Use setStyleSheet() to apply CSS
2. Qt supports most CSS properties
3. Pseudo-states: :hover, :pressed, :focus
4. Apply styles globally or per widget
5. Create themes by switching stylesheets

---

## 📚 Common CSS Properties

- **background-color**: Widget background
- **color**: Text color
- **border**: Border style
- **padding**: Internal spacing
- **margin**: External spacing
- **font-size**: Text size
- **font-weight**: Text weight
- **border-radius**: Rounded corners

---

**Next Topic**: API Management
**Previous Topic**: PyQt5 - Line Edits

Happy Coding! 🐍
