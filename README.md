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
| [`docs/adr/`](./docs/adr/) | Architecture Decision Records (ADR-0001 ถึง 0010) |
| [`docs/ROADMAP-PHASE-0.md`](./docs/ROADMAP-PHASE-0.md) | Phase 0 prototype roadmap — milestones, scope, timeline |
| [`docs/OPEN-QUESTIONS.md`](./docs/OPEN-QUESTIONS.md) | Log ของคำถามที่ resolve แล้ว (Q1-Q10 ครบ) |

---

## สถานะปัจจุบัน

🟢 **Ready for Phase 0 prototype** — concept locked, ADR 0001-0010 accepted, roadmap วางแล้ว

เริ่ม M0 — Bootstrap project

---

## Stack

Next.js 15 · Supabase (Postgres + RLS + Realtime + Vault) · Vercel Cron (World/Decision Tick) · Vercel AI Gateway (BYOK routing) · pgvector (Memory) · BYOK server-side (Phase 0 ใช้ ADR-0001 Option B ตั้งแต่ต้น)
