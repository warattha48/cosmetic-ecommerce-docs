# เอกสารข้อกำหนดระบบ (System Requirement Specification)

**โครงการ:** ระบบร้านค้าออนไลน์เครื่องสำอาง (Cosmetics E-Commerce Platform)

## สารบัญ
1. ภาพรวมโครงการ
2. เป้าหมายทางธุรกิจและขอบเขต
3. สถาปัตยกรรมระบบ
4. ความต้องการด้านฟังก์ชันการทำงาน
5. ความต้องการด้านที่ไม่ใช่ฟังก์ชัน (Non-functional)
6. โมเดลข้อมูลเบื้องต้น
7. แนวทางการทำงานแบบ Collaborative Development
8. DevOps Workflow เบื้องต้น
9. แผนการพัฒนาและกำหนดส่ง
10. ความเสี่ยงและแผนรองรับ
ภาคผนวก

---

## 1. ภาพรวมโครงการ
แพลตฟอร์มดิจิทัลสำหรับจำหน่ายเครื่องสำอางและสกินแคร์ ที่รองรับการเลือกชมสินค้า ค้นหาตามสภาพผิวหรือหมวดหมู่ จัดการตะกร้าสินค้า และระบบชำระเงินออนไลน์ พร้อมทั้งระบบหลังบ้าน (Admin Dashboard) สำหรับผู้ดูแลเพื่อจัดการสต็อกสินค้าและคำสั่งซื้ออย่างมีประสิทธิภาพ

## 2. เป้าหมายทางธุรกิจและขอบเขต
*   **เป้าหมายทางธุรกิจ:** เพิ่มช่องทางการจัดจำหน่ายออนไลน์ ยกระดับประสบการณ์การซื้อเครื่องสำอางของลูกค้า และเพิ่มยอดขายรวมของธุรกิจ
*   **ขอบเขตระบบ:** ครอบคลุมการทำงาน 2 ส่วนหลัก ได้แก่ ระบบหน้าเว็บสำหรับลูกค้า (Customer Frontend) และ ระบบจัดการหลังบ้านสำหรับผู้ดูแล (Admin Dashboard)

## 3. สถาปัตยกรรมระบบ
ออกแบบโดยประยุกต์ใช้หลักการ Separation of Concerns และแบ่งการทำงานแบบ Microservices เพื่อให้ระบบมีความยืดหยุ่น โดยใช้เทคโนโลยี Nuxt สำหรับพัฒนาส่วน Frontend และใช้ Express.js ร่วมกับ Prisma ORM ในส่วนของ Backend

```mermaid
graph TD
    User[ลูกค้า] -->|HTTP| Frontend[Customer Frontend: Nuxt]
    Admin[แอดมิน] -->|HTTP| AdminUI[Admin Dashboard: Nuxt]
    
    Frontend --> Gateway[API Gateway]
    AdminUI --> Gateway
    
    subgraph Backend Services [Backend Services: Express.js]
        Gateway --> AuthSvc[User/Auth Service]
        Gateway --> ProductSvc[Product Service]
        Gateway --> OrderSvc[Order Service]
    end
    
    AuthSvc --> ORM[Prisma ORM]
    ProductSvc --> ORM
    OrderSvc --> ORM
    
    ORM --> DB[(Database: MySQL)]