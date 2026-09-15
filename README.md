# Output Praktikum 2 — WebGL Primitive Playground

**Nama:** Mario Napitupulu
**NRP:** 5025241085
**Nama:** Nathanael Oliver
**NRP:** 5025241109

## Deskripsi Aplikasi

WebGL Primitive Playground merupakan aplikasi interaktif berbasis WebGL2 yang digunakan untuk menampilkan dan mengeksplorasi berbagai primitive grafis. Aplikasi menyediakan rendering object statis dan bergerak, pengaturan draw mode, perubahan warna, kontrol kecepatan animasi, interaksi keyboard dan mouse, serta spawning primitive pada posisi klik canvas.

Aplikasi juga dilengkapi HUD untuk menampilkan FPS, jumlah primitive yang sedang dirender, posisi mouse dalam koordinat NDC, dan draw mode yang sedang aktif.

## File

* `index.html` — struktur halaman, canvas, dan kontrol aplikasi;
* `main.js` — inisialisasi WebGL2, shader, buffer, primitive, animasi, input, dan rendering;
* `style.css` — layout, tampilan playground, kontrol, dan HUD.

## Primitive yang Digunakan

Primitive yang digunakan dalam aplikasi meliputi:

* **Triangle** — dibuat menggunakan `TRIANGLES`;
* **Rectangle** — dibentuk dari dua triangle;
* **Line Loop / Line Shape** — digunakan untuk menampilkan bentuk berbasis garis;
* **Line Strip** — digunakan pada beberapa bentuk garis;
* **Points** — digunakan untuk menampilkan vertex sebagai titik;
* **Lines** — digunakan untuk membuat procedural grid.

## Draw Mode

Aplikasi menyediakan beberapa draw mode WebGL:

* `TRIANGLES`
* `LINE_LOOP`
* `LINE_STRIP`
* `LINES`
* `POINTS`

Draw mode dapat dipilih melalui kontrol **Draw Mode** pada halaman playground.

Untuk primitive dengan mode `POINTS`, setiap vertex ditampilkan sebagai titik. Vertex shader juga mengatur `gl_PointSize` sehingga titik dapat terlihat pada canvas.

## Fitur Animasi

Fitur animasi yang diimplementasikan meliputi:

* lima moving triangle dengan posisi awal berbeda;
* setiap object memiliki kecepatan dan arah gerak yang berbeda;
* object bergerak secara kontinu menggunakan `requestAnimationFrame()`;
* object memantul ketika mencapai batas koordinat NDC;
* kecepatan animasi dapat diubah menggunakan kontrol **Speed**;
* animasi dapat di-pause dan di-resume menggunakan tombol `Space`;
* tersedia pilihan pembatas FPS melalui kontrol **FPS Limit**;
* tersedia mode trail/fading untuk memberikan efek jejak pada object.

## Fitur Interaksi

Aplikasi memiliki beberapa bentuk interaksi:

| Kontrol                | Fungsi                                          |
| :--------------------- | :---------------------------------------------- |
| Primitive selector     | Memilih bentuk object yang akan di-spawn        |
| Draw mode              | Mengubah mode rendering WebGL                   |
| Speed                  | Mengatur kecepatan pergerakan object            |
| FPS Limit              | Mengatur batas FPS rendering                    |
| Trail                  | Mengatur efek trail/fading                      |
| Color buttons          | Mengubah warna object aktif                     |
| Random                 | Menghasilkan warna secara acak                  |
| `W A S D` / Arrow Keys | Menggerakkan player secara kontinu              |
| `C`                    | Mengganti warna player                          |
| `R`                    | Mereset posisi player dan object spawn          |
| `Space`                | Pause/resume animasi                            |
| Klik canvas            | Membuat primitive baru pada posisi mouse        |
| Clear Spawned          | Menghapus seluruh primitive hasil spawn         |
| Pattern toggle         | Menampilkan atau menyembunyikan procedural grid |

## Fitur Rendering

Program menggunakan WebGL2 dengan:

* WebGL2 context;
* viewport dan clear color;
* vertex shader dan fragment shader GLSL ES 3.00;
* shader compile dan program linking;
* Vertex Array Object (VAO);
* buffer untuk data vertex;
* attribute `a_position` untuk posisi;
* attribute `a_color` untuk warna;
* `gl.drawArrays()` untuk melakukan draw call;
* `requestAnimationFrame()` sebagai rendering loop.

Setiap vertex menyimpan data posisi `(x, y)` dan warna `(r, g, b, a)`.

## Vertex Color dan Interpolasi

Setiap vertex memiliki warna masing-masing. Vertex shader meneruskan warna melalui `v_color` menuju fragment shader.

GPU kemudian melakukan interpolasi warna antar-vertex sehingga triangle dan primitive lainnya dapat menghasilkan perpaduan warna secara otomatis tanpa menghitung warna setiap pixel secara manual.

## Multiple Primitive dan Draw Mode

Rectangle direpresentasikan menggunakan dua triangle karena triangle merupakan primitive dasar yang digunakan dalam proses rendering.

Bentuk berbasis garis menggunakan `LINE_LOOP` atau `LINE_STRIP`, sedangkan procedural grid menggunakan `LINES`.

Object yang di-spawn dapat menggunakan draw mode yang dipilih melalui selector. Untuk mode `POINTS`, object dirender sebagai kumpulan titik.

## Dynamic Buffer dan Animasi

Posisi object yang bergerak diperbarui setiap frame. Setelah posisi berubah, data vertex terbaru dikirim kembali ke GPU menggunakan dynamic buffer dengan:

```
gl.bufferData(..., gl.DYNAMIC_DRAW)
```

Dengan pendekatan tersebut, posisi object dapat berubah secara real-time tanpa menggunakan transformation matrix.

## Keyboard Input

Input keyboard menggunakan event `keydown` dan `keyup`.

State tombol disimpan sehingga tombol `W`, `A`, `S`, `D`, dan arrow keys dapat digunakan untuk menggerakkan player secara kontinu.

Sementara itu, tombol `R`, `C`, dan `Space` digunakan untuk aksi berbasis event seperti reset, pergantian warna, dan pause/resume.

## Mouse dan NDC

Posisi mouse pada canvas dikonversi dari koordinat pixel menjadi Normalized Device Coordinates (NDC).

Konversinya menggunakan konsep:

```
ndcX = (pixelX / canvasWidth) * 2 - 1
ndcY = 1 - (pixelY / canvasHeight) * 2
```

Nilai NDC berada pada rentang `-1` sampai `1`.

Posisi tersebut digunakan untuk menentukan lokasi primitive baru ketika canvas diklik. Posisi mouse juga ditampilkan pada HUD.

## HUD

HUD aplikasi menampilkan beberapa informasi secara real-time:

* **FPS** — jumlah frame per second;
* **Primitive** — jumlah primitive yang sedang dirender;
* **Mouse NDC** — posisi mouse dalam koordinat NDC;
* **Spawn Mode** — draw mode yang sedang aktif;
* **RUNNING / PAUSED** — status animasi.

## Challenge

Challenge yang dikerjakan:

* **Challenge A — Primitive Selector:** menyediakan selector untuk memilih primitive yang akan di-spawn;
* **Challenge B — Color Control:** menyediakan kontrol warna merah, hijau, biru, cyan, dan random;
* **Challenge C — Mouse Spawn:** membuat primitive baru pada posisi canvas yang diklik;
* **Challenge D — Moving Objects:** membuat beberapa object bergerak dengan posisi, kecepatan, dan arah berbeda serta memantul pada batas NDC;
* **Challenge E — Procedural Pattern:** membuat procedural grid menggunakan perulangan JavaScript;
* **Challenge F — HUD:** menampilkan FPS, jumlah primitive, draw mode, dan posisi mouse dalam NDC.

## Cara Menjalankan Project

1. Pastikan browser mendukung **WebGL2**.
2. Simpan file berikut dalam satu folder project:

   * `index.html`
   * `main.js`
   * `style.css`
3. Buka project menggunakan local server. Contohnya menggunakan **VS Code Live Server**.
4. Jalankan `index.html` melalui local server.
5. Pastikan canvas WebGL tampil dan tidak terdapat error pada browser console.
6. Gunakan kontrol yang tersedia untuk mencoba primitive, draw mode, animasi, keyboard, mouse, dan fitur challenge.

### Contoh menggunakan Live Server

Jika menggunakan Visual Studio Code:

1. Install extension **Live Server**.
2. Buka folder project.
3. Klik kanan `index.html`.
4. Pilih **Open with Live Server**.
5. Browser akan membuka halaman Graphics Playground.

## Kesimpulan

WebGL Primitive Playground mengimplementasikan dasar rendering WebGL2 melalui shader, buffer, attribute, primitive, draw mode, animasi, keyboard input, mouse input, serta interaksi spawning object. Fitur tambahan berupa color control, procedural grid, FPS control, trail, dan HUD membuat aplikasi dapat digunakan sebagai playground untuk mengeksplorasi primitive graphics secara interaktif.
