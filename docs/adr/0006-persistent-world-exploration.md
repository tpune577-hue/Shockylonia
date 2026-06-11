# ADR-0006: Persistent World + Exploration-driven Geography Expansion

## Status
Accepted

## Context
ต้องตัดสินใจว่าโลกจะ reset เป็น season (เหมือน Rust) หรือ persistent ตลอดกาล และถ้า persistent จะป้องกัน snowball ของ Kingdom เดิมอย่างไรให้ผู้เล่นใหม่มีที่ยืน

ทางเลือกที่พิจารณา:
- **Season Reset (~3 เดือน)**: ป้องกัน snowball ชัดเจน แต่ Chronicle/history ที่สร้างมาหายตาม
- **Persistent + mechanics penalty**: Kingdom ใหญ่เสีย efficiency เพิ่ม เช่น governance overhead, corruption
- **Persistent + Will diversity + Exploration**: ใช้ธรรมชาติของระบบ Will เป็น natural brake แทน mechanics เพิ่ม

## Decision
**Persistent world ไม่มี reset** โดยใช้สองกลไกเป็น natural check:

1. **Will Diversity** — Kingdom ขนาดใหญ่ประกอบด้วย Character ที่มี Will หลากหลาย Character แต่ละตัวตัดสินใจตาม Will ของตัวเอง ไม่ได้ follow คำสั่ง leader ตาบอด → ยิ่งใหญ่ยิ่งแตกง่าย (internal revolt, defection, secession เกิดได้ organic จาก gameplay)

2. **Exploration-driven Geography** — แผนที่ขยายเฉพาะเมื่อมี Character เดินทางไปถึงขอบ ดินแดนที่ยังไม่ถูก discover เป็น Undiscovered Territory ที่ไม่มีใครครอบครองได้ ผู้เล่นใหม่สามารถมุ่งหน้าสู่ frontier แทนที่จะต้องสู้กับ Kingdom เดิมในดินแดนที่มีคนแล้ว

## Consequences
**ดี:**
- Chronicle สะสมประวัติศาสตร์จริงของโลก ไม่หายตาม reset — กลายเป็น asset ของเกม
- Revolt, secession, การก่อตั้งอาณาจักรใหม่เกิดจาก Will ของ Character เอง ไม่ใช่ mechanics บังคับ
- ผู้เล่นใหม่มีทางเลือก: เข้าร่วม Kingdom เดิม หรือสำรวจ frontier แล้วตั้งตัว
- Exploration มีคุณค่าจริง — Beastkin มี edge, Chronicle บันทึก "วันที่ Mira ค้นพบดินแดน Ashveil"

**เสีย/ความเสี่ยง:**
- ต้องออกแบบ map schema รองรับ Undiscovered Territory (tile ที่ยังไม่ exist ใน DB จนกว่าจะถูก discover)
- Chronicle และ world state จะโตเรื่อยๆ ต้องวางแผน data retention
- Will diversity เป็น brake ที่ soft ไม่ใช่ hard cap — ถ้า player กลุ่มหนึ่ง coordinate กันข้าม Will ยังอาจ snowball ได้ในระยะแรก
