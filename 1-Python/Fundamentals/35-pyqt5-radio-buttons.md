# PyQt5 - Radio Buttons

## 📚 Table of Contents
1. [QRadioButton Widget](#qradiobutton-widget)
2. [Button Groups](#button-groups)
3. [Exclusive Selection](#exclusive-selection)
4. [Practice Questions](#practice-questions)

---

## 1. QRadioButton Widget

```python
from PyQt5.QtWidgets import QApplication, QWidget, QRadioButton, QVBoxLayout
import sys

class RadioWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Radio Buttons")
        self.setGeometry(100, 100, 300, 200)
        
        layout = QVBoxLayout()
        
        radio1 = QRadioButton("Option 1")
        radio2 = QRadioButton("Option 2")
        radio3 = QRadioButton("Option 3")
        
        radio1.setChecked(True)  # Default selection
        
        layout.addWidget(radio1)
        layout.addWidget(radio2)
        layout.addWidget(radio3)
        
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = RadioWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 2. Button Groups

```python
from PyQt5.QtWidgets import QApplication, QWidget, QRadioButton, QButtonGroup, QVBoxLayout, QLabel
import sys

class RadioGroupWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Radio Button Groups")
        self.setGeometry(100, 100, 300, 300)
        
        self.label = QLabel("Select an option", self)
        
        # Create button group
        self.button_group = QButtonGroup()
        
        self.radio1 = QRadioButton("Python")
        self.radio2 = QRadioButton("JavaScript")
        self.radio3 = QRadioButton("Java")
        
        # Add to group
        self.button_group.addButton(self.radio1, 1)
        self.button_group.addButton(self.radio2, 2)
        self.button_group.addButton(self.radio3, 3)
        
        # Connect signal
        self.button_group.buttonClicked.connect(self.on_radio_clicked)
        
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.radio1)
        layout.addWidget(self.radio2)
        layout.addWidget(self.radio3)
        
        self.setLayout(layout)
    
    def on_radio_clicked(self, button):
        self.label.setText(f"Selected: {button.text()}")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = RadioGroupWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 3. Exclusive Selection

```python
from PyQt5.QtWidgets import QApplication, QWidget, QRadioButton, QVBoxLayout, QLabel, QPushButton
import sys

class ExclusiveRadioWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Exclusive Selection")
        self.setGeometry(100, 100, 300, 300)
        
        self.label = QLabel("Choose your favorite color:", self)
        
        self.red_radio = QRadioButton("Red")
        self.green_radio = QRadioButton("Green")
        self.blue_radio = QRadioButton("Blue")
        
        self.submit_btn = QPushButton("Submit")
        self.submit_btn.clicked.connect(self.submit_choice)
        
        self.result_label = QLabel("", self)
        
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.red_radio)
        layout.addWidget(self.green_radio)
        layout.addWidget(self.blue_radio)
        layout.addWidget(self.submit_btn)
        layout.addWidget(self.result_label)
        
        self.setLayout(layout)
    
    def submit_choice(self):
        if self.red_radio.isChecked():
            self.result_label.setText("You chose: Red")
        elif self.green_radio.isChecked():
            self.result_label.setText("You chose: Green")
        elif self.blue_radio.isChecked():
            self.result_label.setText("You chose: Blue")
        else:
            self.result_label.setText("Please select a color")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = ExclusiveRadioWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 4. Practice Questions

**Question 1**: Create radio buttons for selecting payment methods.
**Question 2**: Build a quiz with radio button options.
**Question 3**: Create a settings panel with radio button groups.

---

## 🎯 Key Takeaways

1. QRadioButton creates radio button widgets
2. Only one radio button can be selected at a time
3. Use QButtonGroup for managing groups
4. isChecked() returns selection state
5. Radio buttons are for exclusive choices

---

**Next Topic**: PyQt5 - Line Edits
**Previous Topic**: PyQt5 - Checkboxes

Happy Coding! 🐍
