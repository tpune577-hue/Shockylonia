# ADR-0003: Agent ตอบเป็น Structured Action Enum เท่านั้น

## Status
Proposed

## Context
Agent ของแต่ละ Player เป็นคนละโมเดล (BYOK) output ของ LLM ไม่ deterministic และเป็น free text ได้ ถ้าปล่อยให้ Agent บรรยาย action อิสระ จะเกิด: state inconsistent, parse ยาก, และช่องโหว่ prompt injection (Player เขียน Will ให้ Agent ตัวเองพยายามสั่งระบบ หรือยัด instruction ใส่ Character อื่นผ่านบทสนทนา)

## Decision
Agent ต้องตอบกลับเป็น **structured action จาก enum ที่กำหนดไว้** เท่านั้น (เช่น Move/Gather/Eat/Rest/Craft/Trade/Talk/Attack/Build/Claim) พร้อม parameter ที่ระบบ validate ได้ — ไม่รับ freeform action
ทุก input ที่เข้า Agent (รวมข้อความจาก Character อื่น) ถือเป็น **untrusted data** ไม่ใช่คำสั่ง

## Consequences
**ดี:** deterministic, validate ได้, กัน prompt injection ระหว่าง Player, simulation reproducible
**เสีย:** ลดความ "สร้างสรรค์" ของ Agent (ทำได้แค่สิ่งที่ระบบรองรับ), การเพิ่มความสามารถใหม่ = ต้องเพิ่ม action เข้า enum + รองรับใน engine, ส่วน "เนื้อเรื่อง/บทพูด" ต้องแยกออกจาก "การกระทำที่มีผลต่อ state"
