# Password Strength Checker API

## Deskripsi
Password Strength Checker API adalah layanan berbasis Flask yang digunakan untuk mengevaluasi kekuatan password berdasarkan beberapa kriteria keamanan. API ini menyediakan fitur untuk memeriksa apakah password mengandung kata-kata yang berada dalam list kata terlarang (wordlist).

## Fitur
- Memeriksa kekuatan password berdasarkan panjang, kombinasi karakter, dan pola umum yang lemah.
- Mengecek apakah password mengandung kata yang terdapat dalam wordlist.
- Menyimpan log akses untuk analisis lebih lanjut.
- Menambahkan dan menghapus kata dari wordlist yang digunakan dalam pengecekan password.

## Teknologi yang Digunakan
- Python 3
- Library Flask
- Database PostgreSQL
- Docker & Docker Compose

## Instalasi dan Menjalankan API
### 1. Requirement
- Sudah menginstal Docker dan Docker Compose.
- Port `5001` untuk API checkerpass dan `5432` untuk database PostgreSQL

### 2. Menjalankan dengan Docker Compose
Jalankan perintah berikut untuk build dan running container API :

```sh
docker-compose up -d --build
```

API berjalan di `http://localhost:5001`.

## Endpoint API
### 1. Cek Kekuatan Password
**Endpoint:**
```
POST /check_password
```
**Request Body:**
```json
{
    "password": "ExamplePassword12!"
}
```
**Response:**
```json
{
    "check_results": {
        "strength_check": "Password is strong.",
        "wordlist_check": "Password does not contain any restricted words.",
        "is_safe": 1,
        "password": "ExamplePassword12!"
    }
}
```

### 2. Menambahkan Kata ke database Wordlist
**Endpoint:**
```
POST /add_wordlist
```
**Request Body:**
```json
{
    "word": "password"
}
```
**Response:**
```json
{
    "message": "Word 'password' has been added to the wordlist."
}
```

### 3. Menghapus Kata dari database Wordlist
**Endpoint:**
```
DELETE /delete_wordlist
```
**Request Body:**
```json
{
    "word": "password"
}
```
**Response:**
```json
{
    "message": "Word 'password' has been deleted from the wordlist."
}
```

## Struktur Project
```
.
├── Dockerfile
├── docker-compose.yaml
├── requirements.txt
├── app
│   ├── main.py
│   ├── routes.py
│   ├── config.py
│   ├── models.py
```

## DATABASE
API ini menggunakan dua tabel utama dalam PostgreSQL:

### 1. Tabel `AccessLog`
Tabel ini digunakan untuk menyimpan log akses setiap kali seseorang menggunakan API untuk memeriksa password.

**Kolom:**
- `id` (Integer, Primary Key): ID unik untuk setiap log.
- `ip` (String): Alamat IP pengguna.
- `origin` (String): Sumber permintaan (origin header).
- `respon` (Text): Hasil evaluasi password yang diberikan.

### 2. Tabel `Wordlist`
Tabel ini menyimpan daftar kata terlarang yang tidak boleh digunakan dalam password.

**Kolom:**
- `id` (Integer, Primary Key): ID unik untuk setiap kata.
- `word` (String, Unique): Kata yang tidak boleh digunakan dalam password.
- `count` (Integer, Default: 0): Jumlah kali kata tersebut muncul dalam password yang diperiksa yang membantu menganalisis pola habit penggunaan password yang lemah.


