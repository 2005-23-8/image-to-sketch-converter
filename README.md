# image-to-sketch-converter
import tkinter as tk
from tkinter import filedialog
import cv2
from PIL import Image, ImageTk

def upload_image():
    file_path = filedialog.askopenfilename()
    if file_path:
        image = cv2.imread(file_path)
        sketch = convert_to_sketch(image)
        display_sketch(sketch)

def convert_to_sketch(image):
    gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    inverted_image = cv2.bitwise_not(gray_image)
    blurred_image = cv2.GaussianBlur(inverted_image, (21, 21), 0)
    sketch = cv2.divide(gray_image, blurred_image, scale=256.0)
    return sketch

def display_sketch(sketch):
    sketch = cv2.cvtColor(sketch, cv2.COLOR_BGR2RGB)
    sketch = Image.fromarray(sketch)
    sketch = ImageTk.PhotoImage(sketch)
    sketch_label.config(image=sketch)
    sketch_label.image = sketch

# Create the main window
root = tk.Tk()
root.title("Image to Sketch Converter")

# Button to upload image
upload_button = tk.Button(root, text="Upload Image", command=upload_image)
upload_button.pack(pady=10)

# Label to display sketch
sketch_label = tk.Label(root)
sketch_label.pack()

# Run the main event loop
root.mainloop()
