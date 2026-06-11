# ADR-0008: Memory Model — Three-layer Architecture

## Status
Accepted

## Context
Character ต้องมี Memory เพื่อให้ Agent ตัดสินใจได้อย่างมีบริบท แต่ถ้าส่ง event ทั้งหมดตลอดอายุ Character เป็น context จะเกิดสองปัญหา:
1. Token cost พุ่งไม่หยุดตามระยะเวลาที่เล่น
2. Character เก่าได้เปรียบ Character ใหม่เพราะ context ลึกกว่าโดย default ไม่ใช่เพราะ Will ดีกว่า

## Decision
Memory แบ่งเป็นสามชั้น ทำให้ context size per Decision Tick คงที่ไม่ว่า Character จะเล่นมานานแค่ไหน:

| ชั้น | เนื้อหา | Lifecycle |
|---|---|---|
| **Working Memory** | เหตุการณ์ล่าสุด ~20-50 events | Sliding window — event เก่าสุดถูกดันออกเมื่อเกิน limit |
| **Relationship Memory** | ความสัมพันธ์กับ Character/Settlement ที่รู้จัก (ระดับความไว้ใจ, ประวัติการติดต่อ) | Persistent แต่ decay ถ้าไม่ได้ interact กันนาน |
| **World Knowledge** | ข้อมูลภูมิศาสตร์และการเมืองที่ Character พบเห็นโดยตรง | Persistent แต่ mark stale เมื่อข้อมูลไม่ได้รับการยืนยันนาน |

Player ที่ลง context depth สูงกว่าจะได้ชั้น Relationship Memory และ World Knowledge ที่ครบกว่า ไม่ใช่ Working Memory ที่ยาวกว่า

## Consequences
**ดี:**
- Token cost per tick คาดการณ์ได้และ cap ได้ตาม Player budget
- Character ใหม่และเก่า start Decision Tick ด้วย context size ใกล้เคียงกัน
- Relationship decay สร้าง dynamic ที่ Character ต้องรักษาความสัมพันธ์ผ่าน interaction จริง

**เสีย/ความเสี่ยง:**
- Working Memory ขนาด ~20-50 อาจต้องปรับ — ต้องทดสอบใน prototype ว่า Agent มีบริบทเพียงพอสำหรับการตัดสินใจที่ดี
- World Knowledge stale mechanism ต้องออกแบบอย่างระวัง — ถ้า stale เร็วเกินไปจะทำให้ Agent ตัดสินใจโดยไม่มีข้อมูลแผนที่
