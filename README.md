# Codingan dan Penjelasan

        import cv2 as ame
        import numpy as me
        import matplotlib.pyplot as you

        pict = ame.imread('buah.jpg')

        pict1 = ame.cvtColor(pict, ame.COLOR_BGR2GRAY)
        edges = ame.Canny(pict, 100, 150)


        def color_segmentation(image_path):
            pict2 = ame.imread(image_path)
            pict2 = ame.cvtColor(pict2, ame.COLOR_BGR2RGB)

            hsv_image = ame.cvtColor(pict2, ame.COLOR_RGB2HSV)

            lower_green = me.array([32, 45, 45], dtype=me.uint8)
            upper_green = me.array([89, 247, 247], dtype=me.uint8)

            lower_red = me.array([0, 67, 67], dtype=me.uint8)
            upper_red = me.array([10, 255, 255], dtype=me.uint8)

            lower_yellow = me.array([20, 98, 98], dtype=me.uint8)
            upper_yellow = me.array([30, 247, 247], dtype=me.uint8)

            mask_red = ame.inRange(hsv_image, lower_red, upper_red)
            mask_green = ame.inRange(hsv_image, lower_green, upper_green)
            mask_yellow = ame.inRange(hsv_image, lower_yellow, upper_yellow)

            result_red = ame.bitwise_and(pict2, pict2, mask=mask_red)
            result_green = ame.bitwise_and(pict2, pict2, mask=mask_green)
            result_yellow = ame.bitwise_and(pict2, pict2, mask=mask_yellow)

            fig, axs = you.subplots(1, 4, figsize=(15, 5))

            axs = axs.ravel()

            axs[0].imshow(pict2)
            axs[0].set_title('Original Image')
            axs[0].axis('on')
            axs[0].text(10, 10, f"({pict2.shape[1]}, {pict2.shape[0]})", color='white')

            axs[1].imshow(result_red)
            axs[1].contour(mask_red)
            axs[1].set_title('Red Fruits')
            axs[1].axis('on')
            axs[1].text(10, 10, f"({result_red.shape[1]}, {result_red.shape[0]})", color='white')

            axs[2].imshow(result_green)
            axs[2].contour(mask_green)
            axs[2].set_title('Green Fruits')
            axs[2].axis('on')
            axs[2].text(10, 10, f"({result_green.shape[1]}, {result_green.shape[0]})", color='white')

            axs[3].imshow(result_yellow)
            axs[3].contour(mask_yellow)
            axs[3].set_title('Yellow Fruits')
            axs[3].axis('on')
            axs[3].text(10, 10, f"({result_yellow.shape[1]}, {result_yellow.shape[0]})", color='white')

            # Rotate the images
            for ax in axs[1:]:
                ax.set_adjustable('box')
                ax.set_aspect('equal')
                ax.get_xaxis().set_visible(True)
                ax.get_yaxis().set_visible(True)
                ax.set_frame_on(True)
                ax.yaxis.set_label_coords(0.5, -0.1)
                ax.set_xticks([])
                ax.set_yticks([])
                ax.spines['top'].set_visible(True)
                ax.spines['right'].set_visible(True)
                ax.spines['bottom'].set_visible(True)
                ax.spines['left'].set_visible(True)
                ax.yaxis.set_ticklabels([])
                ax.xaxis.set_ticklabels([])
                ax.text(10, 10, f"({ax.get_xlim()[1]}, {ax.get_ylim()[1]})", color='white')

                ax.set_aspect('auto')

            you.tight_layout()

            you.show()

            # Save segmented images
            ame.imwrite('result_red.jpg', result_red)
            ame.imwrite('result_green.jpg', result_green)
            ame.imwrite('result_yellow.jpg', result_yellow)

        image_path = "buah.jpg"

        color_segmentation(image_path)
        ##AMELIA-NUR-HALIZAH-202131112

Penjelasan penyelesaian codingan di atas adalah sebagai berikut:

1. Library yang digunakan:

   - `cv2` dengan alias `ame`: Digunakan untuk membaca gambar, melakukan segmentasi warna, dan menyimpan gambar.
   - `numpy` dengan alias `me`: Digunakan untuk manipulasi array.
   - `matplotlib.pyplot` dengan alias `you`: Digunakan untuk menampilkan gambar.

2. Membaca gambar input:

   - Gambar dengan nama 'buah.jpg' dibaca menggunakan fungsi `ame.imread` dan disimpan dalam variabel `pict`.

3. Konversi gambar ke grayscale:

   - Gambar dalam variabel `pict` dikonversi menjadi grayscale menggunakan `ame.cvtColor` dengan parameter `ame.COLOR_BGR2GRAY`.
   - Hasil konversi disimpan dalam variabel `pict1`.

4. Deteksi tepi menggunakan algoritma Canny:

   - Menggunakan `ame.Canny`, tepi gambar dalam variabel `pict` dideteksi dengan threshold 100 dan 150.
   - Hasil deteksi tepi disimpan dalam variabel `edges`.

5. Fungsi `color_segmentation(image_path)`:

   - Fungsi ini melakukan segmentasi warna pada gambar yang diberikan sebagai input melalui `image_path`.

6. Membaca gambar input dan konversi ke RGB:

   - Gambar dengan path yang diberikan di `image_path` dibaca menggunakan `ame.imread`.
   - Gambar tersebut dikonversi ke format RGB menggunakan `ame.cvtColor` dengan parameter `ame.COLOR_BGR2RGB`.

7. Konversi gambar ke format HSV:

   - Gambar dalam format RGB diubah menjadi format HSV menggunakan `ame.cvtColor` dengan parameter `ame.COLOR_RGB2HSV`.
   - Hasil konversi disimpan dalam variabel `hsv_image`.

8. Segmentasi warna:

   - Menentukan range nilai untuk segmentasi warna pada setiap warna (hijau, merah, kuning) menggunakan `me.array`.
   - Membuat mask untuk masing-masing range warna menggunakan `ame.inRange`.
   - Menggunakan `ame.bitwise_and`, hasil segmentasi warna diterapkan pada gambar asli untuk mendapatkan gambar hasil segmentasi (misal: `result_red`, `result_green`, `result_yellow`).

9. Menampilkan hasil segmentasi:

   - Membuat plot dengan `you.subplots` dengan 1 baris dan 4 kolom untuk menampilkan gambar-gambar.
   - Setiap gambar ditampilkan menggunakan `axs[i].imshow`.
   - Menggunakan `axs[i].contour`, contour dari mask segmentasi ditampilkan pada gambar hasil segmentasi.
   - Menambahkan judul, teks ukuran gambar, dan teks koordinat pada setiap gambar menggunakan metode seperti `axs[i].set_title`, `axs[i].text`.
   - Mengatur penampilan plot seperti pengaturan sumbu, label, batas, dan tampilan gambar menggunakan metode seperti `axs[i].set_adjustable`, `axs[i].set_aspect`, `axs[i].spines`, dll.

10. Menampilkan plot:

    - Menggunakan `you.tight_layout` untuk memastikan tata letak yang rapi.
    - Menampilkan plot menggunakan `you.show`.

11. Menyimpan gambar hasil segmentasi:
    - Menggunakan `ame.imwrite` untuk menyimpan gambar hasil segmentasi dalam format file

, misalnya 'result_red.jpg', 'result_green.jpg', 'result_yellow.jpg'.

## KESIMPULAN

pada segementasi tersebut kita harus mengconvert warna rgb ke hsv agar biner warna dapat di segementasi dan dapat dikenali

## teori segementasi buah menggunakan warna

Segmentasi buah menggunakan warna adalah salah satu metode dalam pengolahan citra yang bertujuan untuk memisahkan buah-buah dalam sebuah gambar berdasarkan warna. Teori ini didasarkan pada asumsi bahwa buah-buah memiliki ciri khas warna yang berbeda-beda, sehingga dapat dibedakan dari latar belakang atau buah lainnya dengan menggunakan analisis warna.

Berikut adalah langkah-langkah umum yang dilakukan dalam segmentasi buah menggunakan warna:

1. Konversi Ruang Warna: Gambar awal dikonversi ke ruang warna yang lebih sesuai untuk analisis warna, seperti ruang warna RGB, HSV, atau LAB. Pilihan ruang warna tergantung pada karakteristik buah dan kemudahan dalam mengekstraksi informasi warna yang dibutuhkan.

2. Penentuan Rentang Warna: Rentang warna untuk setiap buah yang ingin dipisahkan ditentukan. Rentang ini dapat ditentukan secara manual dengan mengamati sampel gambar buah yang representatif, atau menggunakan pendekatan otomatis seperti algoritma k-means untuk mengelompokkan piksel-piksel warna dalam gambar.

3. Pembuatan Masking: Berdasarkan rentang warna yang ditentukan, sebuah mask atau filter warna dibuat untuk mengidentifikasi piksel-piksel dalam gambar yang termasuk dalam rentang warna buah. Masking ini dapat dilakukan dengan menggunakan operasi bitwise pada citra atau menggunakan metode deteksi batas seperti Thresholding.

4. Post-processing: Hasil segmentasi awal dapat mengandung noise atau piksel-piksel yang tidak diinginkan. Oleh karena itu, tahap post-processing dilakukan untuk membersihkan dan memperbaiki hasil segmentasi. Hal ini bisa dilakukan dengan menggunakan teknik seperti operasi morfologi (erosi, dilasi), penghapusan area kecil, atau pemfilteran spasial untuk meningkatkan kualitas segmentasi.

5. Analisis Lanjutan: Setelah berhasil memisahkan buah-buah dalam gambar, langkah selanjutnya adalah melakukan analisis lanjutan, seperti menghitung jumlah buah, mengukur ukuran buah, menghitung luas atau volume buah, atau mengklasifikasikan jenis buah berdasarkan warna atau fitur-fitur lainnya.

Segmentasi buah menggunakan warna memiliki kelebihan yaitu metode yang relatif sederhana dan dapat memberikan hasil yang baik jika buah-buah memiliki warna yang jelas dan berbeda dari latar belakang. Namun, metode ini dapat menjadi kurang akurat jika terdapat variasi warna yang besar antara buah-buah atau jika terdapat gangguan dalam pencahayaan atau latar belakang yang kompleks.

Dalam prakteknya, segmentasi buah menggunakan warna sering digunakan dalam aplikasi pengolahan citra seperti deteksi buah-buahan dalam sistem sortir otomatis, penghitungan jumlah buah dalam pertanian, atau identifikasi buah dalam robot berbasis visi.
