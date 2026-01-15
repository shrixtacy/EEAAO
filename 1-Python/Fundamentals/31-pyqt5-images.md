# PyQt5 - Images

## 📚 Table of Contents
1. [QPixmap](#qpixmap)
2. [Loading and Displaying Images](#loading-and-displaying-images)
3. [Image Scaling](#image-scaling)
4. [Practice Questions](#practice-questions)

---

## 1. QPixmap

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel
from PyQt5.QtGui import QPixmap
import sys

class MyWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Image Display")
        self.setGeometry(100, 100, 500, 500)
        
        label = QLabel(self)
        pixmap = QPixmap('image.png')
        label.setPixmap(pixmap)
        label.setGeometry(50, 50, pixmap.width(), pixmap.height())

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MyWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 2. Loading and Displaying Images

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel
from PyQt5.QtGui import QPixmap
from PyQt5.QtCore import Qt
import sys

class ImageWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Image Viewer")
        self.setGeometry(100, 100, 600, 600)
        
        # Create label for image
        self.image_label = QLabel(self)
        self.image_label.setGeometry(50, 50, 500, 500)
        
        # Load and display image
        self.load_image('photo.jpg')
    
    def load_image(self, image_path):
        pixmap = QPixmap(image_path)
        if not pixmap.isNull():
            self.image_label.setPixmap(pixmap)
            self.image_label.setScaledContents(True)
        else:
            self.image_label.setText("Image not found")

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = ImageWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 3. Image Scaling

```python
from PyQt5.QtWidgets import QApplication, QWidget, QLabel
from PyQt5.QtGui import QPixmap
from PyQt5.QtCore import Qt
import sys

class ScaledImageWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Scaled Image")
        self.setGeometry(100, 100, 400, 400)
        
        label = QLabel(self)
        label.setGeometry(50, 50, 300, 300)
        
        # Load image
        pixmap = QPixmap('image.png')
        
        # Scale image
        scaled_pixmap = pixmap.scaled(300, 300, Qt.KeepAspectRatio, Qt.SmoothTransformation)
        
        label.setPixmap(scaled_pixmap)
        label.setAlignment(Qt.AlignCenter)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = ScaledImageWindow()
    window.show()
    sys.exit(app.exec_())
```

---

## 4. Practice Questions

**Question 1**: Create a window that displays an image.
**Question 2**: Load and scale an image to fit a specific size.
**Question 3**: Create an image viewer with multiple images.

---

## 🎯 Key Takeaways

1. Use QPixmap to load images
2. Use QLabel to display images
3. scaled() method for resizing
4. Qt.KeepAspectRatio maintains proportions
5. setScaledContents() for automatic scaling

---

**Next Topic**: PyQt5 - Layout Managers
**Previous Topic**: PyQt5 - Labels

Happy Coding! 🐍
