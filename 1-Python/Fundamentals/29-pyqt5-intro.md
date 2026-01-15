# PyQt5 GUI - Introduction

## 📚 Table of Contents
1. [PyQt5 Installation](#pyqt5-installation)
2. [First GUI Application](#first-gui-application)
3. [QApplication and QWidget](#qapplication-and-qwidget)
4. [Window Properties](#window-properties)
5. [Practice Questions](#practice-questions)

---

## 1. PyQt5 Installation

PyQt5 is a comprehensive set of Python bindings for Qt v5, one of the most powerful and popular cross-platform GUI frameworks. Qt (pronounced "cute") was originally developed by Trolltech and is now maintained by The Qt Company. PyQt5 brings all of Qt's capabilities to Python, allowing you to create professional, native-looking desktop applications with Python code.

Before you can start building GUI applications with PyQt5, you need to install it using Python's package manager, pip. PyQt5 is a large library that includes not just the GUI components, but also modules for networking, multimedia, web browsing, database integration, and much more. The installation process downloads all necessary components and makes them available to your Python environment.

One of the great advantages of PyQt5 is that it's truly cross-platform - applications you build will run on Windows, macOS, and Linux with the same codebase. The widgets (GUI components) automatically adapt to look native on each platform, so your application will feel at home whether it's running on Windows 11 or Ubuntu Linux.

PyQt5 is widely used in professional software development because of its maturity, extensive documentation, and rich feature set. Many commercial applications use Qt/PyQt5, including popular software like VLC Media Player, Autodesk Maya, and even parts of Google Earth. Learning PyQt5 opens doors to creating sophisticated desktop applications that can compete with applications written in C++ or other compiled languages.

```bash
# Install PyQt5
pip install PyQt5

# Verify installation
python -c "import PyQt5; print(PyQt5.__version__)"
```

---

## 2. First GUI Application

Creating your first GUI application with PyQt5 is an exciting milestone in your Python journey. Unlike command-line programs that run in a terminal, GUI applications provide a visual interface with windows, buttons, and other interactive elements that users can click and interact with using their mouse and keyboard.

Every PyQt5 application follows a similar structure. First, you create a QApplication object, which manages the application's control flow and main settings. This object is essential - it handles the event loop, which is the heart of any GUI application. The event loop continuously checks for user interactions (like mouse clicks or key presses) and responds to them appropriately.

Next, you create your main window or widget. In PyQt5, everything you see on screen is a widget - buttons, text boxes, labels, and even the window itself. The QWidget class is the base class for all user interface objects. You can customize your window by setting its title, size, and position on the screen.

Finally, you start the application's event loop by calling `app.exec_()`. This method starts the event loop and keeps your application running until the user closes it. The underscore after exec is necessary because "exec" is a Python keyword, so PyQt5 uses "exec_" instead. When the user closes the window, the event loop ends, and `sys.exit()` ensures the application terminates cleanly, returning control to the operating system.

```python
import sys
from PyQt5.QtWidgets import QApplication, QWidget

# Create application
app = QApplication(sys.argv)

# Create window
window = QWidget()
window.setWindowTitle("My First GUI")
window.setGeometry(100, 100, 400, 300)  # x, y, width, height
window.show()

# Run application
sys.exit(app.exec_())
```

---

## 3. QApplication and QWidget

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel
import sys

class MyWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()
    
    def initUI(self):
        self.setWindowTitle("My Window")
        self.setGeometry(100, 100, 500, 400)
        
        label = QLabel("Hello, PyQt5!", self)
        label.move(200, 150)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MyWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 4. Window Properties

```python
from PyQt5.QtWidgets import QApplication, QWidget
from PyQt5.QtGui import QIcon
import sys

class MyWindow(QWidget):
    def __init__(self):
        super().__init__()
        
        # Window title
        self.setWindowTitle("My Application")
        
        # Window size and position
        self.setGeometry(100, 100, 600, 400)
        
        # Window icon
        # self.setWindowIcon(QIcon('icon.png'))
        
        # Fixed size
        # self.setFixedSize(600, 400)
        
        # Minimum size
        self.setMinimumSize(400, 300)
        
        # Maximum size
        self.setMaximumSize(800, 600)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MyWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 5. Practice Questions

**Question 1**: Create a basic PyQt5 window with a custom title.
**Question 2**: Set window size to 800x600 and position at (200, 200).
**Question 3**: Create a window with fixed size that cannot be resized.

---

## 🎯 Key Takeaways

1. Install PyQt5 with pip
2. QApplication manages the application
3. QWidget is the base class for UI elements
4. Use setGeometry() for size and position
5. Always call app.exec_() to start event loop

---

**Next Topic**: PyQt5 - Labels
**Previous Topic**: Request API Data

Happy Coding! 🐍
