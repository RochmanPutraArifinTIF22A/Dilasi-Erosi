# Dilasi-Erosi
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Fungsi untuk morfologi: 9 strel, erosi dan dilasi
def morfologi_9_strel(gambar_path):
    # Baca gambar grayscale
    img_gray = cv2.imread(gambar_path, cv2.IMREAD_GRAYSCALE)
    if img_gray is None:
        print("Gagal membaca gambar.")
        return

  # Threshold ke citra biner (putih tulisan, hitam latar)
  _, binary = cv2.threshold(img_gray, 127, 255, cv2.THRESH_BINARY_INV)

  # 9 structuring elements
  strels = [
      ("Rect 3x3", cv2.getStructuringElement(cv2.MORPH_RECT, (3, 3))),
        ("Rect 5x5", cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5))),
        ("Rect 7x7", cv2.getStructuringElement(cv2.MORPH_RECT, (7, 7))),
        ("Ellipse 3x3", cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (3, 3))),
        ("Ellipse 5x5", cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5, 5))),
        ("Cross 3x3", cv2.getStructuringElement(cv2.MORPH_CROSS, (3, 3))),
        ("Cross 5x5", cv2.getStructuringElement(cv2.MORPH_CROSS, (5, 5))),
        ("Line Hor 5x1", cv2.getStructuringElement(cv2.MORPH_RECT, (5, 1))),
        ("Line Vert 1x5", cv2.getStructuringElement(cv2.MORPH_RECT, (1, 5))),
    ]

  # Tampilkan hasil
  plt.figure(figsize=(20, 21))
  plt.subplot(7, 3, 1)
  plt.imshow(binary, cmap='gray')
  plt.title("Gambar Biner (Negatif    + Tanpa BG)")
    plt.axis("off")

  for i, (nama, strel) in enumerate(strels):
        eroded = cv2.erode(binary, strel)
        dilated = cv2.dilate(binary, strel)

  plt.subplot(7, 3, i * 2 + 2)
  plt.imshow(eroded, cmap='gray')
  plt.title(f"Dilasi - {nama}")
  plt.axis("off")

  plt.subplot(7, 3, i * 2 + 3)
  plt.imshow(dilated, cmap='gray')
        plt.title(f"Erosi - {nama}")
        plt.axis("off")

    

# Main
if __name__ == "__main__":
    path = input("Masukkan path ke file gambar (contoh: karakter.png): ")
    morfologi_9_strel(path)
