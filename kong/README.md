# Kong API Gateway dengan Docker Compose

## Deskripsi

Konfigurasi ini menyediakan lingkungan Kong API Gateway menggunakan Docker Compose dengan PostgreSQL sebagai database backend.

## Fitur

- Kong API Gateway untuk manajemen API.
- PostgreSQL sebagai penyimpanan konfigurasi Kong.

## Persyaratan

- Docker dan Docker Compose harus terinstal.
- Pastikan port berikut tidak digunakan:
  - `8000` (HTTP Proxy)
  - `8443` (HTTPS Proxy)
  - `8001` (Admin API HTTP)
  - `8444` (Admin API HTTPS)
  - `8002` (Admin GUI HTTP KONG MANAGER)
  - `5432` (PostgreSQL Database) EXPOSE PORT 5433

## Instalasi dan Menjalankan Kong

### Jalankan Kong API Gateway dan Database PostgreSQL

```sh
docker compose up -d


## Struktur Docker Compose

```
.
├── docker-compose.yml
├── POSTGRES_PASSWORD (file secret berisi password akses database)
├── config/ (folder untuk konfigurasi Kong YAML)
```
## Konfigurasi Kong

- **Kong menggunakan PostgreSQL** sebagai database.
- **Secrets** digunakan untuk menyimpan password akses database.
- **Volumenya tersimpan dalam `kong_data`** untuk menjaga data persisten.
- **Admin API tersedia di `8001`**, GUI di `8002`.

## Menambahkan Gateway Service dan Routes

Dalam konfigurasi Kong, terdapat dua service yang terdaftar:

1. **checkerpass**
   - Host: `checkerpassAPI`
   - Port: `5001`
   - Digunakan untuk layanan pemeriksaan kekuatan password.
   
2. **securityheader**
   - Host: `securityheaderAPI`
   - Port: `5000`
   - Digunakan untuk pemeriksaan securityheader.

### Menambahkan Route

Setiap service memiliki route yang mengarah ke API terkait:

- **checker-password-service**:
  - Path: `/checkpass`
  - Mendukung HTTP dan HTTPS.
  
- **securityheader**:
  - Path: `/sechead`
  - Mendukung HTTP dan HTTPS.

Kong akan meneruskan request ke service yang sesuai berdasarkan path yang diberikan.