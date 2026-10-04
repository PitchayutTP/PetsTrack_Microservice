# PetsTrack Microservice

ระบบจัดการและติดตามตำแหน่งสัตว์เลี้ยงแบบ Microservices
แยกหน้าที่ของแต่ละบริการ พร้อมระบบส่งเหตุการณ์และแจ้งเตือนผ่าน WebSocket

## Services
- API Gateway: จัดการเส้นทาง API และตรวจสอบ JWT
- Authentication: จัดการการยืนยันตัวตน
- Pet Management: จัดการข้อมูลสัตว์เลี้ยง
- Tracking: รับและบันทึกพิกัดแยกตามสัตว์เลี้ยง
- Notification: รับเหตุการณ์จาก RabbitMQ และส่งแจ้งเตือนผ่าน WebSocket

## Highlights
- ใช้ PostgreSQL แยกฐานข้อมูลตามบริการหลัก
- ส่งเหตุการณ์อัปเดตตำแหน่งและการแจ้งเตือนผ่าน RabbitMQ
- ใช้ OpenTelemetry และ Jaeger สำหรับ distributed tracing
- มี circuit breaker ที่ API Gateway เพื่อรับมือบริการปลายทางขัดข้อง
- จัดการบริการและเครื่องมือประกอบด้วย Docker Compose

## Tech Stack
- Frontend: React, Tailwind CSS, Leaflet
- Backend: Go, Gin, JWT
- Database: PostgreSQL
- Messaging: RabbitMQ, WebSocket
- Observability: OpenTelemetry, Jaeger
- Infrastructure: Docker Compose, ngrok

## Project Status
เป็นระบบต้นแบบ โดยกฎแจ้งเตือนตำแหน่งยังใช้เงื่อนไขละติจูดแบบคงที่
และต้องปรับการยืนยันตัวตนของ tracker และ WebSocket ก่อนใช้งานจริง

ENV
# Database
DB_USER=postgres

DB_PASSWORD=postgres

DB_PORT=5432

# pgAdmin
PGADMIN_EMAIL=admin@admin.com

PGADMIN_PASSWORD=admin

# JWT Token
SECRET=my_super_secret_key_123

# Port
GATEWAY_PORT=8080

AUTH_PORT=8082

PET_PORT=8083

TRACKING_PORT=8081

NOTI_PORT=8084

# Database ของแต่ละ Service
AUTH_DB_DSN=host=postgres port=5432 user=postgres password=postgres dbname=auth_db sslmode=disable

PET_DB_DSN=host=postgres port=5432 user=postgres password=postgres dbname=pet_db sslmode=disable

TRACKING_DB_DSN=host=postgres port=5432 user=postgres password=postgres dbname=tracking_db sslmode=disable

# Ngrok 
NGROK_AUTHTOKEN=xxx (ลบkeyแล้ว เนื่องจากความปลอดภัยด้านsecurity)


การสร้าง Authtoken ของ ngrok นั้นทำได้ง่ายๆ ผ่านหน้าเว็บไซต์หลัก โดยมีขั้นตอนดังนี้ครับ
1. สมัครสมาชิกหรือ Log in
เข้าไปที่เว็บไซต์ ngrok.com หากยังไม่มีบัญชีให้สมัครใช้งานก่อน (ฟรี) หรือถ้ามีแล้วให้ทำการ Log in เข้าสู่ระบบ
2. ไปที่เมนู Your Authtoken
เมื่อเข้ามาที่หน้า Dashboard แล้ว ให้ดูที่แถบเมนูด้านซ้ายมือ:
ไปที่หัวข้อ Getting Started
คลิกที่เมนู Your Authtoken
3. คัดลอก Token
ในหน้านี้คุณจะเห็นรหัสตัวอักษรยาวๆ ในช่อง Your Authtoken ให้กดปุ่ม Copy เพื่อคัดลอกรหัสนั้นไว้
4. นำมาใส่ใน ENV (NGROK_AUTHTOKEN)
