# Komputer Grafis - Team Exercies 1
**Anggota Kelompok:**
- Muhammad Omar Haqqi
- Muhammad Raka Fadhillah
- Faris Dian Pradipta
- Khaelano Abroor Maulana

## Langkah 1 – Inisialisasi Scene, Kamera, dan Renderer
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Textured Room – Step 1</title>
  <style>
    html, body { margin: 0; height: 100%; overflow: hidden; background: #111; }
    canvas { display: block; }
  </style>
</head>
<body>
<script src="https://cdn.jsdelivr.net/npm/three@0.146.0/build/three.min.js"></script>
<script>
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0x222222);

  const camera = new THREE.PerspectiveCamera(
    60, window.innerWidth / window.innerHeight, 0.1, 100
  );
  camera.position.set(0, 1.5, 3);
  camera.lookAt(0, 1, 0);

  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);

  function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
  }
  animate();

  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });
</script>
</body>
</html>
```
Kode di atas merupakan implementasi dasar untuk menyiapkan lingkungan kerja **Three.js** yang terdiri atas _scene_, _camera_, dan _renderer_. Pada bagian awal, dituliskan struktur dokumen HTML standar dengan pengaturan gaya (CSS) agar tampilan memenuhi seluruh layar dan memiliki latar belakang berwarna gelap. Selanjutnya, pustaka **Three.js** dimuat melalui _Content Delivery Network_ (CDN) untuk memudahkan penggunaan tanpa perlu instalasi lokal. Di dalam tag `<script>`, dibuat sebuah _scene_ menggunakan `THREE.Scene()` sebagai wadah utama yang akan menampung seluruh objek tiga dimensi. Kemudian, kamera bertipe **PerspectiveCamera** diinisialisasi dengan sudut pandang sebesar 60 derajat, rasio aspek yang menyesuaikan ukuran jendela, serta jarak pandang antara 0.1 hingga 100 satuan. Kamera ditempatkan pada posisi (0, 1.5, 3) dan diarahkan ke titik (0, 1, 0) agar sudut pandang menyerupai mata manusia yang menghadap ke tengah ruang.

Selanjutnya, _renderer_ bertipe **WebGLRenderer** dibuat dengan opsi _antialias_ untuk menghasilkan tepi objek yang lebih halus. Ukuran _renderer_ disesuaikan dengan ukuran layar perangkat, dan kanvas hasil _rendering_ kemudian ditambahkan ke elemen `<body>` agar dapat ditampilkan di halaman web. Fungsi `animate()` digunakan sebagai _render loop_ yang terus-menerus memanggil `renderer.render(scene, camera)` setiap frame menggunakan `requestAnimationFrame`, sehingga memungkinkan tampilan tiga dimensi diperbarui secara real-time. Terakhir, ditambahkan _event listener_ untuk menyesuaikan ukuran kamera dan _renderer_ secara otomatis apabila jendela browser diubah ukurannya, sehingga tampilan 3D tetap proporsional. Secara keseluruhan, kode ini menghasilkan sebuah kanvas kosong berwarna abu-abu gelap yang telah siap digunakan untuk menampilkan objek tiga dimensi pada langkah selanjutnya.

## Langkah 2 – Menambahkan OrbitControls (Kamera Interaktif)
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Textured Room – Step 2 (OrbitControls)</title>
  <style>
    html, body { margin: 0; height: 100%; overflow: hidden; background: #111; }
    canvas { display: block; }
    .hint {
      position: absolute; top: 10px; left: 50%; transform: translateX(-50%);
      color: #ddd; font-family: system-ui, sans-serif; font-size: 14px;
      background: rgba(0,0,0,0.35); padding: 6px 10px; border-radius: 8px;
      user-select: none;
    }
  </style>
</head>
<body>
<div class="hint">Drag = rotate · Scroll = zoom · Right-drag = pan · R = reset camera</div>
<script src="https://cdn.jsdelivr.net/npm/three@0.146.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.146.0/examples/js/controls/OrbitControls.js"></script>
<script>
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0x222222);

  const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 100);
  const defaultPos = new THREE.Vector3(0, 1.5, 3);
  camera.position.copy(defaultPos);
  camera.lookAt(0, 1, 0);

  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);

  const controls = new THREE.OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.target.set(0, 1, 0);
  controls.minDistance = 0.5;
  controls.maxDistance = 12;

  function animate() {
    requestAnimationFrame(animate);
    controls.update();
    renderer.render(scene, camera);
  }
  animate();

  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });

  window.addEventListener('keydown', (e) => {
    if (e.key === 'r' || e.key === 'R') {
      camera.position.copy(defaultPos);
      controls.target.set(0, 1, 0);
      camera.lookAt(0, 1, 0);
      controls.update();
    }
  });
</script>
</body>
</html>
```
Kode di atas merupakan pengembangan dari langkah sebelumnya yang menambahkan fitur **OrbitControls** pada lingkungan kerja **Three.js** agar kamera dapat digerakkan secara interaktif menggunakan mouse. Struktur dasar HTML masih sama, dengan pengaturan tampilan layar penuh dan latar belakang berwarna gelap. Selain pustaka utama _Three.js_, kode ini juga memuat berkas **OrbitControls.js** dari CDN, yang berfungsi sebagai pengendali interaktif untuk kamera. Di bagian awal skrip, objek _scene_ dibuat dan diberi warna latar belakang abu-abu gelap. Kemudian, kamera perspektif diinisialisasi dengan sudut pandang 60 derajat serta rasio aspek yang menyesuaikan ukuran jendela. Kamera ditempatkan pada posisi awal `(0, 1.5, 3)` dan diarahkan ke titik `(0, 1, 0)`. Posisi awal ini juga disimpan dalam variabel `defaultPos` untuk keperluan reset kamera nantinya.

Selanjutnya, dibuat _renderer_ dengan _antialiasing_ untuk menghasilkan tampilan yang halus, serta disesuaikan ukurannya dengan layar perangkat. Kanvas hasil render ditambahkan ke dalam elemen `<body>`. Setelah itu, objek **OrbitControls** dihubungkan dengan kamera dan kanvas renderer, memungkinkan pengguna untuk memutar, memperbesar (_zoom_), dan menggeser (_pan_) tampilan menggunakan mouse. Fitur `enableDamping` diaktifkan untuk memberikan efek pergerakan yang halus, sedangkan `controls.target` diatur pada titik `(0, 1, 0)` agar kamera berputar mengelilingi pusat ruang. Selain itu, batas jarak pandang kamera diatur melalui `minDistance` dan `maxDistance` agar pengguna tidak terlalu dekat atau terlalu jauh dari objek.

Fungsi `animate()` digunakan sebagai _render loop_ yang memanggil `controls.update()` di setiap frame untuk memastikan pergerakan kamera tetap sinkron sebelum menampilkan hasil render dengan `renderer.render(scene, camera)`. Kode juga menambahkan _event listener_ untuk menyesuaikan ulang rasio kamera dan ukuran renderer ketika jendela diubah ukurannya, sehingga tampilan tetap proporsional. Terakhir, terdapat _event listener_ tambahan untuk menangani tombol **R**, yang berfungsi mengembalikan posisi dan arah pandang kamera ke kondisi awal. Secara keseluruhan, kode ini menghasilkan tampilan 3D kosong yang dapat dieksplorasi secara interaktif menggunakan kontrol kamera berbasis mouse.

## Langkah 3 – Tekstur & Konstruksi “Textured Room”
``` javascript
// ========= 3) Lights =========
const ambient = new THREE.AmbientLight(0xffffff, 0.4);
const point   = new THREE.PointLight(0xffffff, 1.0, 100);
point.position.set(0, 2.4, 0);
scene.add(ambient, point);

// ========= 4) Texture loader + fallback =========
const loader = new THREE.TextureLoader();
function makeFallback(label) {
  const c = document.createElement('canvas'); c.width = c.height = 256;
  const ctx = c.getContext('2d');
  ctx.fillStyle = '#2a2a2a'; ctx.fillRect(0,0,256,256);
  ctx.fillStyle = '#999'; ctx.font = '16px system-ui';
  ctx.fillText(label, 20, 130);
  return new THREE.CanvasTexture(c);
}
function safeLoad(path, label) {
  let tex = loader.load(
    path,
    () => { tex.needsUpdate = true; },
    undefined,
    () => { tex = makeFallback(label); }
  );
  tex.wrapS = tex.wrapT = THREE.RepeatWrapping;
  tex.minFilter = THREE.LinearMipmapLinearFilter;
  tex.magFilter = THREE.LinearFilter;
  tex.anisotropy = renderer.capabilities.getMaxAnisotropy();
  return tex;
}
const floorTex   = safeLoad('floor.jpg',   'floor.jpg?');
const wallTex    = safeLoad('wall.jpg',    'wall.jpg?');
const ceilingTex = safeLoad('ceiling.jpg', 'ceiling.jpg?');

floorTex.repeat.set(2, 2);
wallTex.repeat.set(1.5, 1);
ceilingTex.repeat.set(1, 1);

// ========= 5) Materials =========
const matFloor   = new THREE.MeshStandardMaterial({ map: floorTex, side: THREE.FrontSide });
const matWall    = new THREE.MeshStandardMaterial({ map: wallTex, side: THREE.FrontSide });
const matCeiling = new THREE.MeshStandardMaterial({ map: ceilingTex, side: THREE.FrontSide });

// ========= 6) Room Geometry =========
const W = 4, H = 3, D = 6;

// LANTAI
const floor = new THREE.Mesh(new THREE.PlaneGeometry(W, D), matFloor);
floor.rotation.x = -Math.PI / 2;
scene.add(floor);

// ATAP
const ceiling = new THREE.Mesh(new THREE.PlaneGeometry(W, D), matCeiling);
ceiling.rotation.x = Math.PI / 2;
ceiling.position.set(0, H, 0);
scene.add(ceiling);

// DINDING
const backWall = new THREE.Mesh(new THREE.PlaneGeometry(W, H), matWall);
backWall.position.set(0, H/2, -D/2);
scene.add(backWall);

const frontWall = new THREE.Mesh(new THREE.PlaneGeometry(W, H), matWall);
frontWall.rotation.y = Math.PI;
frontWall.position.set(0, H/2, D/2);
scene.add(frontWall);

const leftWall = new THREE.Mesh(new THREE.PlaneGeometry(D, H), matWall);
leftWall.rotation.y = -Math.PI/2;
leftWall.position.set(-W/2, H/2, 0);
scene.add(leftWall);

const rightWall = new THREE.Mesh(new THREE.PlaneGeometry(D, H), matWall);
rightWall.rotation.y = Math.PI/2;
rightWall.position.set(W/2, H/2, 0);
scene.add(rightWall);
```
Kode di atas berfungsi untuk menambahkan pencahayaan, tekstur, material, serta konstruksi geometris ruangan tiga dimensi pada _scene_ yang telah dibuat dengan **Three.js**. Pada bagian awal, dua jenis sumber cahaya diinisialisasi, yaitu **AmbientLight** dan **PointLight**. Cahaya ambient (`AmbientLight`) memberikan penerangan merata ke seluruh permukaan objek dengan intensitas rendah sebesar 0.4, sedangkan cahaya titik (`PointLight`) berfungsi sebagai sumber cahaya utama yang dipancarkan dari satu titik dengan intensitas 1.0 dan jangkauan 100 satuan. Posisi lampu titik ditempatkan pada koordinat `(0, 2.4, 0)`, yaitu di tengah ruangan mendekati atap, kemudian kedua sumber cahaya tersebut ditambahkan ke dalam _scene_.

Selanjutnya, dibuat sebuah _texture loader_ menggunakan `THREE.TextureLoader()` untuk memuat citra permukaan yang akan digunakan sebagai tekstur pada objek 3D. Agar program tetap dapat berjalan meskipun berkas gambar tidak ditemukan, disediakan fungsi `makeFallback()` yang membuat tekstur pengganti berupa kanvas abu-abu dengan label nama file. Fungsi `safeLoad()` digunakan untuk memuat setiap tekstur (lantai, dinding, dan atap) secara aman dengan mekanisme _fallback_ apabila terjadi kesalahan. Setelah tekstur berhasil dimuat, beberapa properti diatur, seperti `wrapS` dan `wrapT` dengan nilai `THREE.RepeatWrapping` agar pola tekstur dapat berulang, serta pengaturan _filtering_ dan _anisotropy_ untuk meningkatkan ketajaman tampilan. Setiap tekstur juga diberikan nilai pengulangan berbeda (`repeat.set`) agar pola terlihat proporsional terhadap ukuran bidang.

Bagian berikutnya membuat tiga material berbasis **MeshStandardMaterial**, masing-masing untuk lantai, dinding, dan atap, dengan peta tekstur (_map_) yang telah dimuat sebelumnya. Parameter `side: THREE.FrontSide` memastikan bahwa permukaan hanya terlihat dari sisi dalam ruangan. Setelah itu, dibentuk enam bidang geometris yang merepresentasikan lantai, atap, dan empat dinding ruangan menggunakan `THREE.PlaneGeometry()`. Ukuran ruangan ditentukan oleh variabel `W` (lebar = 4), `H` (tinggi = 3), dan `D` (panjang = 6). Lantai ditempatkan secara horizontal di bawah dengan rotasi −90° terhadap sumbu X, sedangkan atap diposisikan di bagian atas dengan rotasi +90°. Keempat dinding ditempatkan mengelilingi ruangan dengan rotasi yang disesuaikan agar tekstur menghadap ke arah dalam. Masing-masing bidang kemudian ditambahkan ke _scene_ menggunakan `scene.add()`.

## Perbaikan 1 – Perbaikan Loading Tekstur
``` javascript
...
const floorTex = safeLoad('textured_room/floor.jpg', 'floor.jpg?');
const wallTex = safeLoad('textured_room/wall.jpg', 'wall.jpg?');
const ceilingTex = safeLoad('textured_room/ceiling.jpg', 'ceiling.jpg?');
...
```
Perubahan pertama pada kode tersebut dilakukan untuk menyesuaikan **jalur (path)** pemanggilan berkas tekstur yang disimpan di dalam folder khusus bernama _textured_room_. Pada kode asli, pemanggilan fungsi `safeLoad()` menggunakan nama berkas secara langsung, seperti `'floor.jpg'`, `'wall.jpg'`, dan `'ceiling.jpg'`, yang mengasumsikan bahwa seluruh berkas gambar berada pada direktori yang sama dengan berkas `index.html`. Namun, setelah struktur proyek diatur agar file tekstur ditempatkan di dalam subfolder _textured_room_, jalur akses harus diperbarui agar fungsi pemuat tekstur dapat menemukan file yang dimaksud. Oleh karena itu, setiap argumen jalur file diubah menjadi `'textured_room/floor.jpg'`, `'textured_room/wall.jpg'`, dan `'textured_room/ceiling.jpg'`. Dengan modifikasi ini, **Three.js** akan mencari file gambar di dalam folder _textured_room_ relatif terhadap lokasi file `index.html`, sehingga proses pemuatan tekstur dapat berjalan dengan benar tanpa menimbulkan kesalahan _file not found_ saat program dijalankan melalui server lokal.

## Perbaikan 2 – Rotasi Dinding Kiri dan Kanan agar Teksturnya Menghadap ke Dalam
``` javascript
...
leftWall.rotation.y = Math.PI/2;
...

...
rightWall.rotation.y = -Math.PI/2;
...
```
Perubahan pada bagian kode tersebut dilakukan untuk memperbaiki orientasi dinding kiri dan kanan agar menghadap ke arah dalam ruangan, sehingga tampilan interior terlihat dengan benar dari sudut pandang kamera. Pada kode awal, rotasi sumbu-Y untuk dinding kiri (`leftWall`) dan dinding kanan (`rightWall`) masing-masing menggunakan nilai `-Math.PI/2` dan `Math.PI/2`. Pengaturan tersebut menyebabkan arah normal bidang menghadap ke luar ruangan, sehingga permukaan tekstur tidak terlihat ketika dilihat dari dalam. Untuk mengatasi hal tersebut, nilai rotasi kedua dinding dibalik: dinding kiri diberi rotasi `Math.PI/2`, sedangkan dinding kanan menggunakan `-Math.PI/2`. Dengan demikian, orientasi bidang berputar 180 derajat dari posisi sebelumnya, membuat sisi depan (_front face_) dari material `matWall` kini menghadap ke arah dalam ruang. Perubahan ini memastikan seluruh dinding ruangan, termasuk kiri dan kanan, tampil konsisten dan dapat menampilkan tekstur dengan benar pada bagian interior saat scene dirender.