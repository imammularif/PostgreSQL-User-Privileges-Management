## User Privileges Management in PostgreSQL (CRUD access control)

Di pekerjaan sebelumnya, saya mempelajari pentingnya pengaturan hak akses user di database. Dari situ saya mulai & berinisiatif mendalami bagaimana cara membuat, mengatur, dan memberikan hak akses CRUD di PostgreSQL secara aman dan efisien. Mini-Project ini menjadi bagian dari proses saya memahami manajemen keamanan data serta memperkuat dasar pengetahuan saya di bidang database administration dan data governance.

## What This Project Does
- Membuat user baru dengan hak CRUD di database / tabel publik  
- Mengatur default privileges agar setiap tabel baru otomatis memberi hak CRUD ke user  
- Memberi hak akses `CREATE` agar user bisa membuat tabel sendiri  
- Mengecek dan menampilkan hak akses tiap user lewat query di `information_schema`

### 1. Membuat User baru

--masuk ke administrator postgresql-nya
```bash 
sudo -i -u postgres
```

```bash 
psql
```

-- User CRUD

```bash 
CREATE USER imul_crud WITH PASSWORD 'supersecret123';
```

-- User Admin / Superuser

```bash 
CREATE USER imul_admin WITH PASSWORD 'admin123';
```
```bash 
ALTER USER imul_admin WITH SUPERUSER;
```

### 2. Memebuat Database

```bash 
CREATE DATABASE db_test OWNER imul_crud;
```
atau jika databasenya sudah ada, beri hak akses konek ke database-nya

```bash 
GRANT CONNECT ON DATABASE db_test TO imul_crud;
```

### 3. Memberikan Hak Akses

-- Koneksi ke database

```bash 
\c db_test
```

-- Hak CRUD tabel yang sudah ada

```bash 
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO imul_crud;
```
```bash 
GRANT CREATE ON SCHEMA public TO imul_crud;
```

-- Hak CRUD untuk tabel baru (default privileges)

```bash 
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO imul_crud;
```

### 4. Cek User & Hak Akses (Optional)

-- Daftar semua user / role

```bash 
\du
```

-- Hak akses CRUD per user di semua tabel

```bash 
SELECT grantee AS "User",
       table_schema AS "Schema",
       table_name AS "Table",
       string_agg(privilege_type, ', ') AS "Privileges"
FROM information_schema.role_table_grants
GROUP BY grantee, table_schema, table_name
ORDER BY grantee, table_schema, table_name;
```

![cek hak akses users](https://github.com/imammularif/PostgreSQL-User-Privileges-Management/blob/main/Chapture/Screenshot%202025-10-09%20204010.png)

NOTE : Project ini saya buat sebagai latihan dan dokumentasi pribadi untuk memperdalam pemahaman tentang pengelolaan hak akses user di PostgreSQL secara praktis.




