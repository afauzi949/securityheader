# Security Header Scanner API

## Deskripsi
Security Header Scanner API adalah layanan berbasis Flask yang digunakan untuk memeriksa konfigurasi security header pada suatu website. API ini melakukan pemindaian terhadap semua subpath dalam domain target dan memberikan rekomendasi konfigurasi security header berdasarkan jenis web server yang terdeteksi.

## Fitur
- Memeriksa konfigurasi security headers pada website target.
- Mendeteksi jenis web server yang digunakan (Apache, Nginx, OpenResty, Cloudflare).
- Memberikan rekomendasi konfigurasi security header yang belum diterapkan.
- Menyediakan file konfigurasi security header yang dapat diunduh berdasarkan jenis web server.
- Menyimpan log akses untuk analisis lebih lanjut.

## Teknologi yang Digunakan
- Python 3
- Flask
- SQLAlchemy 
- Database Mysql
- BeautifulSoup4
- Requests
- Docker & Docker Compose

## Instalasi dan Menjalankan API
### 1. Requirement
- Sudah menginstal Docker dan Docker Compose.
- Port `5000` untuk API dan `3306` untuk database MySQL.

### 2. Menjalankan dengan Docker Compose
Jalankan perintah berikut untuk build dan running container API:

```sh
docker-compose up -d --build
```

API berjalan di `http://localhost:5000`.

## Endpoint API
### 1. Home
**Endpoint:**
```
GET /
```
**Response:**
```json
{
  "message": "Welcome to Security Header Scanner API"
}
```

### 2. Scan Website
**Endpoint:**
```
POST /scan
```
**Request Body:**
```json
{
  "url": "https://example.com"
}
```
**Response:**
```json
{
  "target_url": "https://example.com",
  "webserver": "Apache",
  "existing_headers": {
    "X-Frame-Options": "SAMEORIGIN",
    "Strict-Transport-Security": "max-age=31536000"
  },
  "missing_headers": [
    "Content-Security-Policy",
    "X-Content-Type-Options"
  ],
  "external_resources": {
    "script-src": ["https://cdn.example.com/script.js"]
  },
  "config_filename": "example_apache.conf",
  "download_url": "/download/example_apache.conf",
  "recommended_config": "server {\n  add_header Content-Security-Policy 'default-src self';\n}"
}
```

### 3. Download Konfigurasi
**Endpoint:**
```
GET /download/<filename>
```
**Deskripsi:** Mengunduh file konfigurasi security header berdasarkan jenis web server.

## Struktur Project
```
.
├── Dockerfile
├── docker-compose.yaml
├── requirements.txt
├── app
│   ├── main.py
│   ├── checkHeader.py
│   ├── checkWebserver.py
│   ├── generateConfiguration.py
│   ├── saveConfig.py
```

## Database
API ini menggunakan satu tabel utama dalam MySQL :

### Tabel `AccessLog`
Digunakan untuk menyimpan log akses setiap kali seseorang menggunakan API.

**Kolom:**
- `id` (Integer, Primary Key): ID unik untuk setiap log.
- `ip_address` (String): IP address client.
- `origin` (String): Sumber request.
- `response` (Text): Hasil scanning security header.
- `created_at` (Datetime): Timestamp akses.

## Konfigurasi security header yang diperiksa
- **Strict-Transport-Security**
- **X-Frame-Options**
- **X-Content-Type-Options**
- **X-XSS-Protection**
- **Referrer-Policy**
- **Permissions-Policy**
- **Content-Security-Policy**

