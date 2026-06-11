# ADR-0010: ปฏิทิน, Aging และ The Blessing

## Status
Accepted

## Context
ต้องกำหนดหน่วยเวลาในเกมให้ Chronicle อ่านได้ดี และตัดสินใจว่า aging ของ Race ต่างๆ มีผลกลไกอย่างไร โดยไม่ลงโทษ Player ที่ลงทุน BYOK มาแล้ว

## Decision

### ปฏิทิน
```
1 ฤดู  = 30 วันเกม  = 5 ชม.จริง
1 ปี   = 4 ฤดู      = 120 วันเกม = 20 ชม.จริง
```
4 ฤดู: Spring / Summer / Autumn / Winter
Chronicle เขียนในรูป "Year X, Summer: ..." — ใน 1 เดือนจริงโลกจะมีประวัติศาสตร์ ~36 ปีเกม

### Aging — The Blessing
ทุก Character ที่อาศัยอยู่ใน Shockylonia ได้รับ **The Blessing** — พรที่ทำให้ผู้คนในแผ่นดินนี้มีอายุยืนยาว Character จึงไม่ตายตามอายุขัย แต่ร่างกายเปลี่ยนแปลงตาม **Age Phase**:

| Phase | ช่วงอายุ (% ของ lifespan ดั้งเดิมของ Race) | ผลกลไก |
|---|---|---|
| Young | 0-20% | +XP gain, Stamina regen ไว |
| Prime | 20-70% | Peak performance |
| Elder | 70-90% | STR/AGI ลด, INT/CHA ขึ้น |
| Ancient | 90%+ | ร่างกายอ่อนแอ แต่ Relationship Memory depth พิเศษ, NPC เคารพสูง |

Race ที่อายุยืนกว่า (Elf) จะอยู่ใน Prime phase นานกว่ามาก Beastkin จะผ่าน phase ต่างๆ เร็วกว่า — ไม่ใช่ข้อเสีย แต่เป็น playstyle ที่ต่างกัน

## Consequences
**ดี:**
- The Blessing เป็น lore ที่อธิบายโลกได้ในประโยคเดียว — ไม่ต้องอธิบาย edge case aging
- Age Phase สร้าง lifecycle ให้ Character รู้สึกได้โดยไม่ทำลาย BYOK investment
- Beastkin เหมาะกับ playstyle เร่งรีบ Elf เหมาะกับ long-game — race choice มีความหมายจริง

**เสีย/ความเสี่ยง:**
- Ancient phase อาจ underpowered จน player หลีกเลี่ยง — ต้องให้ bonus ที่ meaningful พอ (Relationship Memory depth เป็นตัวเลือกที่ดี)
- ต้องกำหนด lifespan ดั้งเดิมของแต่ละ Race เป็นตัวเลขปีเกมเพื่อคำนวณ phase threshold
