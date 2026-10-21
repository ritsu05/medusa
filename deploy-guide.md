# 🚀 คู่มือ Deploy E-Commerce สู่ Vercel (100% Vercel เท่านั้น)

โปรเจกต์นี้เป็น **Full-stack Next.js E-Commerce** ที่ออกแบบมาสำหรับรันบน **Vercel ได้ 100% โดยตรง**:
- ✅ **ไม่ต้องใช้ Railway หรือ Docker ใดๆ ทั้งสิ้น**
- ✅ **มีระบบ Demo Mode ในตัว** (มีสินค้าตัวอย่าง, ตะกร้าสินค้า, ระบบสั่งซื้อ, และหน้าแอดมินพร้อมใช้งานทันที)
- ✅ **เชื่อมต่อ Custom Domain ที่คุณจดไว้ได้ฟรี** พร้อม SSL HTTPS อัตโนมัติ

---

## ฟีเจอร์ที่พร้อมใช้งานทันที
- 🛍️ **หน้าร้าน (Storefront):** หน้าแรก, รายการสินค้าแยกหมวดหมู่, ค้นหาสินค้า, หน้ารายละเอียดสินค้า
- 🛒 **ตะกร้าสินค้า (Cart):** เพิ่ม/ลดจำนวน, สรุปยอดเงิน, บันทึกอัตโนมัติ
- 💳 **ระบบ Checkout:** กรอกที่อยู่จัดส่ง, เลือกวิธีชำระเงิน (Stripe / PayPal / เก็บเงินปลายทาง)
- 📊 **หน้าแอดมิน (Admin Dashboard):** เข้าผ่าน `/dashboard` จัดการสินค้า เพิ่ม/ลบสินค้า ดูคำสั่งซื้อ ดูลูกค้า
- 👤 **ระบบสมาชิก:** สมัครสมาชิก, ล็อกอิน, ประวัติคำสั่งซื้อ

---

## ขั้นตอนที่ 1: Push โค้ดขึ้น GitHub (ทำครั้งเดียว)

1. ไปที่ https://github.com/new แล้วตั้งชื่อ Repo เช่น `my-ecommerce-store`
2. เลือกว่าเป็น **Public** หรือ **Private** แล้วกด **Create repository**
3. เปิด Terminal ในโฟลเดอร์นี้ แล้วรันคำสั่ง:

```bash
git remote add origin https://github.com/YOUR_USERNAME/my-ecommerce-store.git
git branch -M main
git push -u origin main
```
*(เปลี่ยน `YOUR_USERNAME/my-ecommerce-store` เป็น URL ของคุณ)*

---

## ขั้นตอนที่ 2: Deploy ขึ้น Vercel (ฟรี)

1. ไปที่ https://vercel.com แล้ว Login ด้วย GitHub
2. กดปุ่ม **Add New...** → เลือก **Project**
3. จะเห็น Repository `my-ecommerce-store` ให้กดปุ่ม **Import**
4. การตั้งค่าในหน้า Configure Project:
   - **Framework Preset:** `Next.js` (Vercel เลือกให้อัตโนมัติ)
   - **Root Directory:** `./` (ค่าเริ่มต้น ไม่ต้องเปลี่ยน)
   - **Environment Variables:** **เว้นว่างไว้ได้เลย** (โปรเจกต์มีโหมด Demo สินค้าทำงานได้ทันที)
5. กดปุ่ม **Deploy**
6. รอประมาณ 1-2 นาที คุณจะได้ URL เว็บไซต์จริงทันที! 🎉

---

## ขั้นตอนที่ 3: เชื่อมต่อโดเมนที่คุณจดไว้ (Custom Domain)

1. ในหน้าโปรเจกต์บน Vercel ไปที่เมนู **Settings** → **Domains**
2. พิมพ์โดเมนของคุณลงไป เช่น:
   - `yourdomain.com`
   - `www.yourdomain.com`
   แล้วกด **Add**
3. Vercel จะแสดงค่า DNS ให้คุณนำไปใส่ที่ผู้ให้บริการจดโดเมน (เช่น Cloudflare, GoDaddy, Namecheap, Hostinger, Thaidatahosting):
   - **Type A:** `@` ชี้ไปที่ `76.76.21.21`
   - **CNAME:** `www` ชี้ไปที่ `cname.vercel-dns.com`
4. รอ DNS อัปเดตประมาณ 5-15 นาที เว็บไซต์ของคุณจะเปิดผ่านโดเมนจริงได้ทันที พร้อมเครื่องหมายกุญแจเขียว (SSL) อัตโนมัติ!

---

## การเชื่อมต่อระบบชำระเงินจริงและ Database (ทำทีหลังเมื่อต้องการ)
หากต้องการเปลี่ยนจากโหมด Demo เป็น Database และการชำระเงินจริง:
1. **Supabase (Database ฟรี):** สมัครที่ supabase.com นำค่า `NEXT_PUBLIC_SUPABASE_URL` และ `NEXT_PUBLIC_SUPABASE_ANON_KEY` ไปใส่ในเมนู **Settings → Environment Variables** บน Vercel
2. **Stripe (รับบัตรเครดิต):** ใส่ `STRIPE_SECRET_KEY` ใน Environment Variables
