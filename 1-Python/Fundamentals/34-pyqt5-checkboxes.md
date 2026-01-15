# PyQt5 - Checkboxes

## 📚 Table of Contents
1. [QCheckBox Widget](#qcheckbox-widget)
2. [Checkbox States](#checkbox-states)
3. [Signals and Slots](#signals-and-slots)
4. [Practice Questions](#practice-questions)

---

## 1. QCheckBox Widget

```python
from PyQt5.QtWidgets import QApplication, QWidget, QCheckBox, QVBoxLayout
import sys

class CheckboxWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Checkboxes")
        self.setGeometry(100, 100, 300, 200)
        
        layout = QVBoxLayout()
        
        checkbox1 = QCheckBox("Option 1")
        checkbox2 = QCheckBox("Option 2")
        checkbox3 = QCheckBox("Option 3")
        
        layout.addWidget(checkbox1)
        layout.addWidget(checkbox2)
        layout.addWidget(checkbox3)
        
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = CheckboxWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 2. Checkbox States

```python
from PyQt5.QtWidgets import QApplication, QWidget, QCheckBox, QLabel, QVBoxLayout
from PyQt5.QtCore import Qt
import sys

class CheckboxStateWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Checkbox States")
        self.setGeometry(100, 100, 300, 200)
        
        self.label = QLabel("Select options", self)
        
        self.checkbox1 = QCheckBox("Python")
        self.checkbox2 = QCheckBox("JavaScript")
        self.checkbox3 = QCheckBox("Java")
        
        self.checkbox1.stateChanged.connect(self.update_label)
        self.checkbox2.stateChanged.connect(self.update_label)
        self.checkbox3.stateChanged.connect(self.update_label)
        
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.checkbox1)
        layout.addWidget(self.checkbox2)
        layout.addWidget(self.checkbox3)
        
        self.setLayout(layout)
    
    def update_label(self):
        selected = []
        if self.checkbox1.isChecked():
            selected.append("Python")
        if self.checkbox2.isChecked():
            selected.append("JavaScript")
        if self.checkbox3.isChecked():
            selected.append("Java")
        
        if selected:
            self.label.setText(f"Selected: {', '.join(selected)}")
        else:
            self.label.setText("No selection")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = CheckboxStateWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 3. Signals and Slots

```python
from PyQt5.QtWidgets import QApplication, QWidget, QCheckBox, QPushButton, QVBoxLayout, QLabel
import sys

class CheckboxSignalsWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Checkbox Signals")
        self.setGeometry(100, 100, 300, 250)
        
        self.label = QLabel("Status: None", self)
        
        self.agree_checkbox = QCheckBox("I agree to terms")
        self.subscribe_checkbox = QCheckBox("Subscribe to newsletter")
        
        self.submit_btn = QPushButton("Submit")
        self.submit_btn.clicked.connect(self.submit)
        
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.agree_checkbox)
        layout.addWidget(self.subscribe_checkbox)
        layout.addWidget(self.submit_btn)
        
        self.setLayout(layout)
    
    def submit(self):
        if self.agree_checkbox.isChecked():
            status = "Submitted"
            if self.subscribe_checkbox.isChecked():
                status += " (Subscribed)"
            self.label.setText(f"Status: {status}")
        else:
            self.label.setText("Status: Please agree to terms")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = CheckboxSignalsWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 4. Practice Questions

**Question 1**: Create checkboxes for selecting pizza toppings.
**Question 2**: Build a settings panel with multiple checkboxes.
**Question 3**: Create a todo list with checkboxes for completion.

---

## 🎯 Key Takeaways

1. QCheckBox creates checkbox widgets
2. isChecked() returns checkbox state
3. stateChanged signal fires on state change
4. Use checkboxes for multiple selections
5. Combine with buttons for form submission

---

**Next Topic**: PyQt5 - Radio Buttons
**Previous Topic**: PyQt5 - Buttons

Happy Coding! 🐍
