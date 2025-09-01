
1. Konsistensi Lingkungan

- Lingkungan development dan production identik
- Menghindari masalah "works on my machine"
- Semua dependencies (PHP, MySQL, Nginx) terisolasi dalam container

2. Mudah Di-deploy

- Cukup install Docker dan Docker Compose di VPS
- Clone repository
- Jalankan docker-compose up -d
- Semua service akan berjalan otomatis

3. Mudah Di-scale

- Bisa menambah container untuk load balancing
- Bisa memisahkan database ke server terpisah

4. Manajemen Yang Baik

- Data database tersimpan di folder dbdata
- Konfigurasi PHP dan Nginx terpisah di folder docker
- Source code Laravel di folder src

Untuk deploy ke VPS, langkah-langkahnya akan seperti ini:

1. Install Docker dan Docker Compose di VPS:
   curl -fsSL https://get.docker.com -o get-docker.sh
   sh get-docker.sh

2. Install Docker Compose:
   curl -SL https://github.com/docker/compose/releases/download/v2.24.5/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
   chmod +x /usr/local/bin/docker-compose

3. Clone project:
   git clone [your-repo] /var/www/my-app
   cd /var/www/my-app

4. Setup environment:
   cp src/.env.example src/.env

# Edit .env sesuai kebutuhan production

5. Build dan jalankan:
   docker-compose up -d --build

Beberapa penyesuaian yang mungkin diperlukan untuk production:

1. Tambahkan file .dockerignore untuk mengabaikan file yang tidak perlu:
2. Buat docker-compose.prod.yml untuk konfigurasi production

Perbedaan utama di file production:

1. Tidak ada phpMyAdmin (untuk keamanan)
2. Port 80 dan 443 untuk HTTPS
3. Menggunakan environment variables untuk kredensial database
4. Volume untuk SSL certificates
5. Restart policy unless-stopped untuk semua container

Untuk menjalankan di production:
docker-compose -f docker-compose.prod.yml up -d --build

Tips tambahan untuk production:

1. Selalu gunakan HTTPS
2. Backup folder dbdata secara regular
3. Setup monitoring untuk container
4. Gunakan Docker secrets untuk menyimpan kredensial
5. Pertimbangkan menggunakan Docker Swarm atau Kubernetes untuk scaling

Dengan konfigurasi ini, Anda mendapatkan:

1. Service Node.js yang terpisah untuk asset compilation
2. Hot Module Replacement (HMR) di port 5173
3. NPM dependencies diinstal otomatis
4. Vite development server berjalan otomatis

Cara menggunakan:

1. Install dependencies Node.js:
   docker exec laravel-node npm install

2. Menjalankan Vite dalam mode development:
   docker exec laravel-node npm run dev

3. Build untuk production:
   docker exec laravel-node npm run build

4. Install package NPM baru:
   docker exec laravel-node npm install <package-name>

Pastikan dalam file layout Anda sudah ada:
@vite(['resources/css/app.css', 'resources/js/app.js'])
