# Histogram-Equalization
UTS Pengolahan Citra Digital

```python
from PIL import Image
import numpy as np
import matplotlib.pyplot as plt

# Load the image
image = Image.open('your_image.jpg').convert('L')  # Convert to grayscale
width, height = image.size
pixels = np.array(image)

# Initialize histograms
h = np.zeros(256, dtype=int)
h2 = np.zeros(256, dtype=int)

# Calculate the histogram of the image
for i in range(width):
    for j in range(height):
        intensity = pixels[j, i]
        h[intensity] += 1

# Perform histogram equalization
cdf = np.cumsum(h)  # Cumulative distribution function
cdf_min = cdf[np.nonzero(cdf)].min()  # Minimum non-zero cdf value
cdf_normalized = (cdf - cdf_min) * 255 / (width * height - cdf_min)
cdf_normalized = cdf_normalized.astype('uint8')

# Apply the equalized histogram to the image
equalized_image = cdf_normalized[pixels]
for i in range(width):
    for j in range(height):
        intensity = equalized_image[j, i]
        h2[intensity] += 1

# Save and display the result
equalized_image = Image.fromarray(equalized_image)
equalized_image.save('equalized_image.jpg')
equalized_image.show()

# Plot original and equalized histograms
plt.figure(figsize=(12, 6))

plt.subplot(1, 2, 1)
plt.title("Original Histogram")
plt.bar(range(256), h, color='blue')

plt.subplot(1, 2, 2)
plt.title("Equalized Histogram")
plt.bar(range(256), h2, color='green')

plt.show()
```

### Program Python untuk Histogram Equalization

Program ini melakukan **Histogram Equalization** menggunakan pustaka `PIL` (Pillow) untuk manipulasi gambar dan `matplotlib` untuk menampilkan histogram. 

### Langkah-langkah Program

1. **Memuat dan Mengonversi Gambar ke Grayscale**:
   - Program memuat gambar dan mengonversinya menjadi grayscale (hitam-putih) menggunakan `Image.open()` dan `.convert('L')`. Ini diperlukan karena Histogram Equalization biasanya diterapkan pada gambar grayscale.

2. **Menghitung Histogram Awal**:
   - Histogram awal dihitung dengan menghitung frekuensi (jumlah) setiap nilai intensitas piksel dari 0 hingga 255. 
   - Dalam kode, `h[intensity] += 1` menghitung berapa kali setiap nilai intensitas muncul di gambar asli.

3. **Menghitung CDF (Cumulative Distribution Function)**:
   - CDF atau fungsi distribusi kumulatif dihitung menggunakan `np.cumsum(h)`, yang menghasilkan akumulasi dari nilai-nilai histogram.
   - CDF kemudian dinormalisasi sehingga rentangnya sesuai dengan rentang intensitas piksel (0 hingga 255). Ini membantu mengatur ulang intensitas piksel dalam gambar agar lebih merata.

4. **Menerapkan Histogram Equalization pada Gambar**:
   - Setelah CDF dinormalisasi, hasilnya diterapkan ke gambar untuk menghasilkan gambar baru yang telah di-"equalize".
   - `equalized_image = cdf_normalized[pixels]` menerapkan nilai-nilai dari CDF yang dinormalisasi ke setiap piksel, sehingga gambar baru memiliki distribusi intensitas yang lebih merata.

5. **Menyimpan dan Menampilkan Hasil**:
   - Gambar hasil equalization disimpan dan ditampilkan menggunakan `equalized_image.save()` dan `equalized_image.show()`.

6. **Menampilkan Histogram Asli dan Histogram Equalization**:
   - Program menampilkan histogram dari gambar asli dan gambar hasil equalization menggunakan `matplotlib`.
   - Histogram asli akan menunjukkan distribusi intensitas yang tidak rata, sedangkan histogram hasil equalization akan menunjukkan distribusi intensitas yang lebih merata.

### Penjelasan Setiap Bagian Kode

- `pixels = np.array(image)` mengonversi gambar menjadi array numpy, sehingga kita bisa mengakses setiap pikselnya.
- `h = np.zeros(256, dtype=int)` membuat array kosong untuk histogram asli.
- `cdf = np.cumsum(h)` menghitung fungsi distribusi kumulatif dari histogram asli.
- `cdf_normalized = (cdf - cdf_min) * 255 / (width * height - cdf_min)` menormalkan nilai CDF agar berada dalam rentang 0–255.
- `equalized_image = cdf_normalized[pixels]` menghasilkan gambar baru dengan intensitas yang sudah di-"equalize".

Pastikan untuk mengganti `'your_image.jpg'` dengan jalur file gambar yang ingin Anda gunakan.

