# Warehouse Inventory Management

Warehouse Inventory Management adalah aplikasi berbasis web untuk mengelola stok barang dalam sebuah gudang. Aplikasi ini terdiri dari **Frontend** menggunakan Vue.js dan **Backend** menggunakan Golang.

---

##  Tech Stack

### Frontend:
- Vue.js 3 (Composition API)
- Bootstrap 
- Axios
- Vite

### Backend:
- Golang 
- Mux
- MySQL (Database)

---

### Link Demo
- **DEMO**: [https://drive.google.com/file/d/1mUHY0qL2pg96FW-oNyKY0OsTYrJFwD0e/view?usp=sharing](https://drive.google.com/file/d/1mUHY0qL2pg96FW-oNyKY0OsTYrJFwD0e/view?usp=sharing)

## 📌 Cara Setup Backend (Golang)

### 1. Clone Repository Backend
```bash
git clone https://github.com/BilyHakim/fullstack-test
cd Backend
```

### 2. Install Dependencies
```bash
go mod tidy
```

### 3. Setup Database MySQL
1. Buat database baru di MySQL:
   ```sql
   CREATE DATABASE warehouse_db;
   ```
2. Buat tabelnya:
   ```sql
   CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    sku VARCHAR(50) NOT NULL,
    quantity INT NOT NULL DEFAULT 0,
    location VARCHAR(255),
    status VARCHAR(50)
    );
   ```

### 4. Jalankan Backend
```bash
go run main.go
```
Backend akan berjalan di `http://localhost:8080`

---

## 📌 Cara Setup Frontend (Vue.js)

### 1. Clone Repository Frontend
```bash
git clone https://github.com/BilyHakim/fullstack-test
cd warehouse-frontend
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Konfigurasi API Endpoint
Pastikan di dalam file `src/api.js` atau di tempat yang sesuai dalam kode, backend mengarah ke:
```javascript
const API_URL = "http://localhost:8080/products";
```

### 4. Jalankan Frontend
```bash
npm run dev
```
Akses aplikasi di `http://localhost:5173` atau sesuai dengan konfigurasi Vite.

---

## 🔥 Fitur Utama
- Menambahkan produk baru
- Mengedit produk yang sudah ada
- Menghapus produk dari daftar
- Menampilkan daftar produk secara real-time

---
