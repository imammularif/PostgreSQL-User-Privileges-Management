# PostgreSQL-User-Privileges-Management

Deskripsi Project

Project ini mendokumentasikan proses membuat user PostgreSQL dengan hak akses spesifik, memberikan hak CRUD ke tabel lama dan default privileges untuk tabel baru. Juga membandingkan user CRUD vs superuser.

Tujuan:
- Memahami manajemen user dan role di PostgreSQL
- Praktik GRANT CRUD, ALTER DEFAULT PRIVILEGES
- Portfolio skill PostgreSQL

Tools
- PostgreSQL (versi xx.x)
- DBeaver (versi xx.x) atau psql CLI
- Ubuntu / Windows / macOS

### 1. Membuat User baru

-- User CRUD
CREATE USER imul_crud WITH PASSWORD 'supersecret123';

-- User Admin / Superuser
CREATE USER imul_admin WITH PASSWORD 'admin123';
ALTER USER imul_admin WITH SUPERUSER;

### 2. Memebuat Database

CREATE DATABASE db_test OWNER imul_crud;

atau jika databasenya sudah ada, beri hak akses konek ke database-nya

GRANT CONNECT ON DATABASE db_test TO imul_crud;


### 3. Mmberikan Hak Akses

-- Koneksi ke database
\c db_test

-- Hak CRUD tabel yang sudah ada
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO imul_crud;

-- Hak CRUD untuk tabel baru (default privileges)
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO imul_crud;

### 4. Cek User & Hak Akses (Optional)

-- Daftar semua user / role
\du

-- Hak akses CRUD per user di semua tabel
SELECT grantee AS "User",
       table_schema AS "Schema",
       table_name AS "Table",
       string_agg(privilege_type, ', ') AS "Privileges"
FROM information_schema.role_table_grants
GROUP BY grantee, table_schema, table_name
ORDER BY grantee, table_schema, table_name;




