# ADR-0004: Instinct Mode — Will-guided Fallback เมื่อ Token หมด

## Status
Accepted

## Context
BYOK แปลว่า Agent จะหยุดทำงานเมื่อ Player หมด Token Budget, API ล่ม, หรือ key หมดอายุ ถ้า Character "แข็งค้าง" หรือตายทันทีเมื่อ token หมด เกมจะลงโทษคนงบน้อยรุนแรงเกินไป

## Decision
เมื่อ Agent ใช้งานไม่ได้ Character เข้า **Instinct Mode** — รันด้วย **Will-guided fallback** แทน AI call

Instinct Mode ไม่ใช่ rule-based แบบ hard-code — พฤติกรรมในโหมดนี้ขึ้นกับ **คุณภาพของ Will ที่ Player เขียน**:
- Will ที่ชัดเจน (identity แน่น, values ระบุ, survival hint ครบ) → Instinct Mode ตัดสินใจสอดคล้องกับตัวตน อยู่รอดได้
- Will ที่เขียนห่วย (คลุมเครือ, ขัดแย้งในตัวเอง, ไม่มี survival context) → Instinct Mode ตัดสินใจผิดพลาด → Character อาจตายได้

Character ยังทำ action พื้นฐาน (กิน, นอน, routine) ได้ในโหมดนี้ แต่ความฉลาดและความสอดคล้องกับเป้าหมายระยะยาวขึ้นกับ Will อย่างเดียว

## Consequences
**ดี:**
- Will quality มีความหมายตลอดเวลา ไม่ใช่แค่ตอน Agent รัน
- Instinct Mode เป็น valid playstyle — แต่ Player ที่ไม่ดูแล Will จะเห็นผลด้วย Character ที่ประพฤติไม่สอดคล้องและอาจตายได้
- เป็น mechanic ที่สอดคล้องกับ core concept "เลี้ยงเจตจำนง" — Will คือรากฐานของ Character ไม่ว่าจะมี Agent หรือไม่

**เสีย/ความเสี่ยง:**
- Will ถูก pre-process เป็น **Survival Profile** (structured rules) ครั้งเดียวตอน Player submit/แก้ Will — Instinct Mode อ่าน Survival Profile โดยไม่เรียก AI อีก ทำให้ทำงานได้แม้ provider ล่มหรือ token หมดสนิท
- Player มือใหม่ที่เขียน Will ไม่เป็นอาจไม่รู้ว่า Instinct Mode จะพาตัวละครไปทางไหน — ต้องมี Will writing guide ที่ดีใน onboarding
