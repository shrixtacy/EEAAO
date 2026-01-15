# PyQt5 - Line Edits

## 📚 Table of Contents
1. [QLineEdit Widget](#qlineedit-widget)
2. [Text Input](#text-input)
3. [Input Validation](#input-validation)
4. [Password Fields](#password-fields)
5. [Practice Questions](#practice-questions)

---

## 1. QLineEdit Widget

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLineEdit, QVBoxLayout, QLabel
import sys

class LineEditWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Line Edits")
        self.setGeometry(100, 100, 400, 200)
        
        layout = QVBoxLayout()
        
        label = QLabel("Enter your name:")
        line_edit = QLineEdit()
        line_edit.setPlaceholderText("Type here...")
        
        layout.addWidget(label)
        layout.addWidget(line_edit)
        
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = LineEditWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 2. Text Input

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLineEdit, QVBoxLayout, QLabel, QPushButton
import sys

class TextInputWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Text Input")
        self.setGeometry(100, 100, 400, 250)
        
        self.name_input = QLineEdit()
        self.name_input.setPlaceholderText("Enter your name")
        
        self.email_input = QLineEdit()
        self.email_input.setPlaceholderText("Enter your email")
        
        self.submit_btn = QPushButton("Submit")
        self.submit_btn.clicked.connect(self.submit_form)
        
        self.result_label = QLabel("")
        
        layout = QVBoxLayout()
        layout.addWidget(QLabel("Name:"))
        layout.addWidget(self.name_input)
        layout.addWidget(QLabel("Email:"))
        layout.addWidget(self.email_input)
        layout.addWidget(self.submit_btn)
        layout.addWidget(self.result_label)
        
        self.setLayout(layout)
    
    def submit_form(self):
        name = self.name_input.text()
        email = self.email_input.text()
        self.result_label.setText(f"Name: {name}, Email: {email}")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = TextInputWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 3. Input Validation

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLineEdit, QVBoxLayout, QLabel
from PyQt5.QtGui import QIntValidator, QDoubleValidator
import sys

class ValidationWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Input Validation")
        self.setGeometry(100, 100, 400, 250)
        
        # Integer input
        int_input = QLineEdit()
        int_input.setPlaceholderText("Enter integer (0-100)")
        int_validator = QIntValidator(0, 100)
        int_input.setValidator(int_validator)
        
        # Float input
        float_input = QLineEdit()
        float_input.setPlaceholderText("Enter decimal number")
        float_validator = QDoubleValidator(0.0, 100.0, 2)
        float_input.setValidator(float_validator)
        
        # Max length
        text_input = QLineEdit()
        text_input.setPlaceholderText("Max 10 characters")
        text_input.setMaxLength(10)
        
        layout = QVBoxLayout()
        layout.addWidget(QLabel("Integer (0-100):"))
        layout.addWidget(int_input)
        layout.addWidget(QLabel("Decimal (0.0-100.0):"))
        layout.addWidget(float_input)
        layout.addWidget(QLabel("Text (max 10 chars):"))
        layout.addWidget(text_input)
        
        self.setLayout(layout)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = ValidationWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 4. Password Fields

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLineEdit, QVBoxLayout, QLabel, QPushButton
from PyQt5.QtCore import Qt
import sys

class PasswordWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Password Field")
        self.setGeometry(100, 100, 400, 250)
        
        self.username_input = QLineEdit()
        self.username_input.setPlaceholderText("Username")
        
        self.password_input = QLineEdit()
        self.password_input.setPlaceholderText("Password")
        self.password_input.setEchoMode(QLineEdit.Password)
        
        self.login_btn = QPushButton("Login")
        self.login_btn.clicked.connect(self.login)
        
        self.result_label = QLabel("")
        
        layout = QVBoxLayout()
        layout.addWidget(QLabel("Username:"))
        layout.addWidget(self.username_input)
        layout.addWidget(QLabel("Password:"))
        layout.addWidget(self.password_input)
        layout.addWidget(self.login_btn)
        layout.addWidget(self.result_label)
        
        self.setLayout(layout)
    
    def login(self):
        username = self.username_input.text()
        password = self.password_input.text()
        
        if username and password:
            self.result_label.setText(f"Logging in as {username}...")
        else:
            self.result_label.setText("Please fill all fields")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = PasswordWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 5. Practice Questions

**Question 1**: Create a registration form with name, email, and password fields.
**Question 2**: Build a calculator input with number validation.
**Question 3**: Create a search box that filters results as you type.

---

## 🎯 Key Takeaways

1. QLineEdit creates text input fields
2. text() retrieves input value
3. setText() sets input value
4. setPlaceholderText() shows hint text
5. setEchoMode(Password) hides input
6. Use validators for input validation

---

**Next Topic**: PyQt5 - CSS Styles
**Previous Topic**: PyQt5 - Radio Buttons

Happy Coding! 🐍
