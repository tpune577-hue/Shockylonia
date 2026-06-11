# Open Questions — รอ grill ให้ resolve

คำถามที่ยังไม่ตัดสินใจ เรียงตามผลกระทบ ตัวไหน resolve แล้วให้ย้ายไปเป็น ADR หรืออัปเดต CONTEXT.md

---

## 🔴 กระทบสถาปัตยกรรม (ตอบก่อน)

### ~~Q1. Pace ของโลก~~ ✅ Resolved
**1 วันเกม = 10 นาทีจริง** — Hunger/Survival เป็น background pressure (deplete ~7-10 วันเกม) ไม่ใช่ daily urgency — ดู ADR-0005

### ~~Q2. Persistent ตลอดกาล vs Season Reset~~ ✅ Resolved
**Persistent ตลอดกาล — ไม่ reset** Natural brake = Will diversity ของ Character ในอาณาจักร + Geography ขยายด้วย Exploration (Option B) — ดู ADR-0006

### ~~Q3. Standalone product vs ใต้ Sandory Box~~ ✅ Resolved
**Standalone** — แยก repo, แยก Supabase project, แยก auth, reuse infra pattern ได้แต่ไม่ share DB

---

## 🟡 กระทบ Game Design

### ~~Q4. Fairness ของ BYOK ให้ลึกแค่ไหน~~ ✅ Resolved
**Intentional mechanic** — frequency, context depth, model tier เป็น player-controlled budget choice ทั้งหมด Output format ถูก lock โดย ADR-0003 เท่านั้น — ดู ADR-0007

### ~~Q5. หนึ่ง Player มีกี่ Character~~ ✅ Resolved
**1 Character ต่อ Player เสมอ** — เป็นหลักการของเกม ไม่ใช่ข้อจำกัดชั่วคราว

### Q6. Will marketplace
ให้ขาย/แชร์ Will template กันได้ไหม (Phase 2)? เป็น creator economy ที่ดี แต่ทำให้ meta แข็งตัวเร็ว

### ~~Q7. Instinct Mode abuse~~ ✅ Resolved
**Valid playstyle แต่ bounded โดย Will quality** — Will ที่เขียนดี = Instinct Mode ฉลาด = อยู่รอด / Will ที่ห่วย = Character อาจตายได้ — ดู ADR-0004 (updated)

---

## 🟢 รายละเอียด (ตอบทีหลังได้)

### ~~Q8. Memory model~~ ✅ Resolved
**3 ชั้น**: Working Memory (sliding window ~20-50 events), Relationship Memory (decay เมื่อไม่เจอ), World Knowledge (stale เมื่อไม่ update) — context size คงที่ต่อ tick — ดู ADR-0008

### ~~Q9. Death penalty หนักแค่ไหน~~ ✅ Resolved
**Option B**: skill ลด 10-15%, ดรอปของที่ถืออยู่ทั้งหมด (ไม่แตะ settlement inventory), respawn ที่ shrine ที่ผูกไว้, skill recovery 1-3 ชม.จริง (6-18 วันเกม) — ดู ADR-0009

### ~~Q10. หน่วยเวลาในเกม~~ ✅ Resolved
**ปฏิทิน**: 1 ปี = 4 ฤดู × 30 วัน = 120 วันเกม = 20 ชม.จริง / **Aging**: stat change ตาม Age Phase ไม่ตายตามอายุ เพราะ The Blessing — ดู ADR-0010
