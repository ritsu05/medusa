# 🚀 คู่มือ Deploy: Medusa.js (1 Repo Monorepo) บน Railway + Vercel

ตอนนี้โปรเจกต์ของคุณจัดโครงสร้างเป็น **1 Repository (Monorepo)** เรียบร้อยแล้ว:
```
my-store/ (Repo เดียวบน GitHub)
├── backend/          ← Medusa API v2 + Admin Dashboard (Deploy ไป Railway)
├── storefront/       ← Next.js Storefront (Deploy ไป Vercel)
├── package.json      ← Monorepo Workspace configuration
└── .gitignore
```

---

## STEP 1 — สร้าง GitHub Repo เดียว แล้ว Push ขึ้นไป

### 1.1 สร้าง Repository บน GitHub
1. ไปที่ https://github.com/new
2. ตั้งชื่อ เช่น `my-ecommerce-store`
3. ตั้งค่าเป็น **Public** หรือ **Private** แล้วกด **Create repository**

### 1.2 ผูก Remote และ Push ขึ้น GitHub (รันใน Terminal โฟลเดอร์โปรเจกต์)
```bash
git remote add origin https://github.com/YOUR_USERNAME/my-ecommerce-store.git
git branch -M main
git push -u origin main
```
*(แทนที่ `YOUR_USERNAME/my-ecommerce-store` ด้วย URL ของคุณ)*

---

## STEP 2 — Deploy Backend + Database บน Railway (ฟรี)

> 🔗 ไปที่ https://railway.app แล้ว Login ด้วย GitHub

### 2.1 สร้าง Project และเพิ่ม PostgreSQL
1. กดปุ่ม **+ New Project**
2. เลือก **Provision PostgreSQL** (Railway จะสร้าง Database ให้ทันที พร้อมตัวแปร `DATABASE_URL`)

### 2.2 เชื่อมต่อ Repo (Backend)
1. ในหน้า Project เดิม กด **+ Add Service** หรือ **+ New**
2. เลือก **GitHub Repo** → เลือก `my-ecommerce-store`
3. เข้าไปที่เมนู **Settings** ของ Service นั้น:
   - **Root Directory:** ให้ใส่ `backend` *(สำคัญมาก! เพื่อให้ Railway รันเฉพาะโฟลเดอร์ backend)*
   - **Build Command:** `npm run build`
   - **Start Command:** `npx medusa db:migrate && npx medusa start`

### 2.3 ตั้งค่า Environment Variables ใน Railway
ไปที่แท็บ **Variables** ของ backend service แล้วเพิ่มค่าเหล่านี้:

| Variable | Value ที่แนะนำ |
|---|---|
| `DATABASE_URL` | `${{Postgres.DATABASE_URL}}` *(Railway ลิงก์ให้อัตโนมัติ)* |
| `STORE_CORS` | `https://yourdomain.com,http://localhost:8000` *(ใส่โดเมนหน้าร้านของคุณ)* |
| `ADMIN_CORS` | `https://*.railway.app,https://yourdomain.com` |
| `AUTH_CORS` | `https://*.railway.app,https://yourdomain.com` |
| `JWT_SECRET` | สร้างสตริงยาวๆ สุ่ม เช่น `super_secret_jwt_random_key_123` |
| `COOKIE_SECRET` | สร้างสตริงยาวๆ สุ่ม เช่น `super_secret_cookie_random_key_123` |
| `PORT` | `9000` |

### 2.4 สร้าง Domain ให้ Backend
1. ในแท็บ **Settings** → เลื่อนลงมาที่หัวข้อ **Networking**
2. กด **Generate Domain** (จะได้ URL เช่น `https://backend-production-xxxx.up.railway.app`)
3. หรือกด **Custom Domain** เพื่อต่อกับ Subdomain เช่น `api.yourdomain.com`

---

## STEP 3 — ดึง Publishable API Key จาก Medusa Admin

เมื่อ Railway deploy backend เสร็จ:
1. เข้าไปที่ Admin URL: `https://YOUR_BACKEND_URL/app`
2. สร้างหรือ Login ด้วยบัญชี Admin
3. ไปที่เมนู **Settings** (รูปเฟือง) → **Publishable API Keys**
4. กด **Create API Key** แล้วตั้งชื่อ
5. คัดลอก Key ที่ได้ (จะขึ้นต้นด้วย `pk_...`) เอาไว้ใช้ใน Step 4

*(หมายเหตุ: หากต้องการ seed ข้อมูลสินค้าทดสอบ สามารถเปิดหน้า Terminal ใน Railway ของ service backend แล้วรัน `npm run seed` ได้เลย)*

---

## STEP 4 — Deploy Storefront บน Vercel (ฟรี)

> 🔗 ไปที่ https://vercel.com แล้ว Login ด้วย GitHub

### 4.1 Import Repository
1. กด **Add New...** → **Project**
2. เลือก Repo `my-ecommerce-store` แล้วกด **Import**

### 4.2 กำหนด Root Directory สำหรับ Vercel
1. ในหัวข้อ **Root Directory** ให้กด **Edit**
2. เลือกโฟลเดอร์ `storefront` แล้วกด **Continue**
3. Framework Preset จะขึ้นเป็น **Next.js** อัตโนมัติ

### 4.3 ใส่ Environment Variables บน Vercel
ขยายหัวข้อ **Environment Variables** แล้วใส่:

| Name | Value |
|---|---|
| `MEDUSA_BACKEND_URL` | URL ของ Railway Backend จาก Step 2 (เช่น `https://backend-production-xxxx.up.railway.app` หรือ `https://api.yourdomain.com`) |
| `NEXT_PUBLIC_MEDUSA_PUBLISHABLE_KEY` | Key `pk_...` ที่ก๊อปปี้มาจาก Step 3 |
| `NEXT_PUBLIC_BASE_URL` | URL ของหน้าร้านคุณ (เช่น `https://yourdomain.com`) |
| `NEXT_PUBLIC_DEFAULT_REGION` | `us` หรือ `th` |
| `REVALIDATE_SECRET` | สตริงสุ่มเดียวกับที่คุณตั้ง |

4. กดปุ่ม **Deploy**

---

## STEP 5 — ผูก Custom Domain ของคุณ

### 5.1 ผูกโดเมนหน้าร้าน (Vercel)
1. ในหน้าโปรเจกต์ Vercel → ไปที่ **Settings** → **Domains**
2. พิมพ์โดเมนของคุณ เช่น `yourdomain.com` และ `www.yourdomain.com`
3. Vercel จะแสดงค่า DNS ให้ไปกรอกที่ผู้ให้บริการจดโดเมน (Cloudflare, Namecheap, GoDaddy เป็นต้น):
   - **Type A:** `@` ชี้ไปที่ `76.76.21.21`
   - **CNAME:** `www` ชี้ไปที่ `cname.vercel-dns.com`

### 5.2 ผูก Subdomain ให้ API (Railway - ถ้าต้องการ)
1. ใน Railway Service Backend → **Settings** → **Networking** → **Custom Domain**
2. ใส่ `api.yourdomain.com`
3. ไปตั้งค่า DNS ที่ผู้ให้บริการโดเมน:
   - **CNAME:** `api` ชี้ไปที่ Domain ที่ Railway กำหนดให้
