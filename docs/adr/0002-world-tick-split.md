# ADR-0002: แยก World Tick ออกจาก Decision Tick

## Status
Proposed

## Context
โลกเป็น persistent และมี Character จำนวนมาก ถ้าเรียก Agent (AI) ทุกตัวทุกรอบการอัปเดตโลก ต้นทุน token จะระเบิดและ scale ไม่ได้ อีกทั้ง Player แต่ละคนมี Token Budget ไม่เท่ากัน

## Decision
แยกเป็นสองจังหวะ:
- **World Tick** (~ทุก 1 นาที): rule engine ล้วน — hunger ลด, stamina ฟื้น, พืชโต, unit ที่กำลังเดินขยับต่อ — **ไม่เรียก AI เลย**
- **Decision Tick**: เรียก **Agent** เฉพาะเมื่อ (a) ถึงรอบตาม Token Budget ของ Player หรือ (b) มี event สำคัญ (ถูกโจมตี / มีคนมาคุยด้วย)

## Consequences
**ดี:** ต้นทุน AI ผูกกับงบ Player แต่ละคนโดยตรง, โลกยัง "มีชีวิต" แม้ Agent ไม่ได้คิดทุกวินาที, scale ตามจำนวนคนได้
**เสีย:** logic แตกเป็นสองชั้น ต้องระวัง state race ระหว่าง World Tick กับ Decision Tick, Character ที่งบสูงจะ react ไวกว่า (ตั้งใจให้เป็น mechanic แต่ต้องคุม fairness — ดู OPEN-QUESTIONS)
