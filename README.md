#opencv-edge -detectors

A Python-based computer vision project comparing three fundamental edge detection algorithms—**Sobel**, **Prewitt**, and **Canny**—using OpenCV, NumPy, and Matplotlib.

---

## 📋 Features

- **Sobel Operator:** Computes spatial intensity gradients along $X$ and $Y$ axes using Gaussian smoothing and differentiation filters.
- **Prewitt Operator:** Detects horizontal and vertical edges via custom 3x3 directional convolution kernels.
- **Canny Edge Detector:** Applies a multi-stage approach featuring Gaussian noise filtering, gradient magnitude calculation, non-maximum suppression, and hysteresis thresholding.
- **Visualization:** Displays all edge detection masks side-by-side with the original image for easy comparison.

---

## ⚙️ Installation & Prerequisites

Ensure you have Python 3.x installed. Install the required dependencies using `pip`:

```bash
pip install opencv-python numpy matplotlib
```

---

## 🚀 How to Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/YOUR-USERNAME/edge-detection-comparison.git
   cd edge-detection-comparison
   ```

2. **Add an image:** Place your input image (e.g., `image.jpg`) in the project directory (or update the file path in the script).

3. **Run the script:**
   ```bash
   python main.py
   ```

---

## 📜 Source Code

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

def show_comparison(original, sobel, prewitt, canny):
    plt.figure(figsize=(12, 10))

    plt.subplot(2, 2, 1)
    plt.imshow(original, cmap='gray')
    plt.title("Original Grayscale Image")
    plt.axis('off')

    plt.subplot(2, 2, 2)
    plt.imshow(sobel, cmap='gray')
    plt.title("Sobel Edge Detection")
    plt.axis('off')

    plt.subplot(2, 2, 3)
    plt.imshow(prewitt, cmap='gray')
    plt.title("Prewitt Edge Detection")
    plt.axis('off')

    plt.subplot(2, 2, 4)
    plt.imshow(canny, cmap='gray')
    plt.title("Canny Edge Detection")
    plt.axis('off')

    plt.tight_layout()
    plt.show()

# Load image
img = cv2.imread("image.jpg")

if img is None:
    print("Error: Image not found")
else:
    # Convert image to grayscale for accurate edge detection
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    # 1. Sobel Edge Detection
    sobelx = cv2.Sobel(gray, cv2.CV_64F, 1, 0, ksize=3)
    sobely = cv2.Sobel(gray, cv2.CV_64F, 0, 1, ksize=3)
    sobel_combined = cv2.magnitude(sobelx, sobely)

    # 2. Prewitt Edge Detection
    kernelx = np.array([[-1, 0, 1], [-1, 0, 1], [-1, 0, 1]])
    kernely = np.array([[1, 1, 1], [0, 0, 0], [-1, -1, -1]])
    prewittx = cv2.filter2D(gray, -1, kernelx)
    prewitty = cv2.filter2D(gray, -1, kernely)
    prewitt_combined = prewittx + prewitty

    # 3. Canny Edge Detection
    canny_edges = cv2.Canny(gray, 100, 200)

    # Display comparison
    show_comparison(gray, np.uint8(np.absolute(sobel_combined)), prewitt_combined, canny_edges)
```

---

## 📊 Comparison Summary

| Algorithm | Strengths | Weaknesses | Best Used For |
| :--- | :--- | :--- | :--- |
| **Sobel** | Fast, simple, reduces high-frequency noise using Gaussian blur | Thicker edges, sensitive to noise | General-purpose edge estimation |
| **Prewitt** | Simple, computationally efficient horizontal/vertical edge detection | No internal smoothing, noisy results | Simple images with high contrast |
| **Canny** | Superior edge resolution, thin continuous lines, low error rate | More computationally intensive | Fine structural feature extraction |

---

## 📜 License

This project is open-source and released under the [MIT License](LICENSE).
