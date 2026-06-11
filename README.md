# Shockylonia

โลก persistent ที่ **ตัวละครทุกตัวขับเคลื่อนด้วย AI ของผู้เล่นเอง (BYOK)** — ผู้เล่นไม่ได้ "ควบคุม" ตัวละคร แต่ "เลี้ยงเจตจำนง" (Will) หนึ่งดวงให้ไปใช้ชีวิตแทนตัวเองในอาณาจักร Shockylonia

> คุณไม่ได้เล่นเกม — คุณสร้างสมองหนึ่งดวง แล้วดูว่ามันจะพาตัวละครไปทางไหน

---

## โลกใบนี้คืออะไร

Shockylonia เป็นโลกแฟนตาซีหลายเผ่าพันธุ์ (มนุษย์ / เอลฟ์ / คนแคระ / เผ่าหูสัตว์) ที่ตัวละครแต่ละตัวมี **เจตจำนง (Will)** ของตัวเอง ใช้ชีวิต หาอาหาร บุกเบิกดินแดน ค้าขาย รบ และสร้างอาณาจักร โดยตัดสินใจผ่าน AI ที่ผู้เล่นนำ token มาเชื่อมเอง

ความได้เปรียบของตัวละครมาจาก **คุณภาพของเจตจำนงที่เขียน + งบ token ที่ยอมจ่าย** ไม่ใช่จากการ micro-control แบบเกมทั่วไป

---

## โครงสร้างเอกสาร

| ไฟล์ | เนื้อหา |
|---|---|
| [`CONTEXT.md`](./CONTEXT.md) | Glossary — คำศัพท์ canonical ของ domain (อ่านก่อนเสมอ) |
| [`docs/design/CONCEPT.md`](./docs/design/CONCEPT.md) | Concept plan เต็ม — pillars, ระบบ, roadmap |
| [`docs/adr/`](./docs/adr/) | Architecture Decision Records — การตัดสินใจที่ย้อนยาก |
| [`docs/OPEN-QUESTIONS.md`](./docs/OPEN-QUESTIONS.md) | คำถามที่ยังไม่ resolve — รอ grill ต่อ |

---

## สถานะปัจจุบัน

🟡 **Pre-prototype** — กำลัง lock concept และ domain language ผ่าน grill-with-docs session

ADR ส่วนใหญ่ยังเป็น `Proposed` — ต้อง grill ให้ครบก่อนเลื่อนเป็น `Accepted`

---

## Stack ที่วางไว้ (ยังไม่ผูกมัด)

Next.js · Supabase (state + Realtime + Vault) · Edge Function (world tick) · pgvector (character memory) · BYOK (ผู้เล่นนำ AI key มาเอง)
