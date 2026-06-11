# Shocky Topia — Concept & Design

สถานะ: **Draft / Pre-prototype** · ภาษา domain ดู [`../../CONTEXT.md`](../../CONTEXT.md)

---

## 1. Core Concept

> โลก persistent ที่ Character ทุกตัวขับเคลื่อนด้วย Agent (AI) ของ Player เอง — Player เขียน **Will** (เจตจำนง) ไม่ใช่คำสั่ง แล้วดูว่าตัวละครจะใช้ชีวิตไปทางไหน

Player คือ "ผู้สร้าง" ไม่ใช่ "ผู้ควบคุม"

---

## 2. Design Pillars (เสาหลัก 4 ต้น)

1. **Will** — Player กำหนดตัวตน ไม่ใช่ macro คำสั่ง → Agent ตีความเอง
2. **Consequence** — ทุก action มีผลถาวร ตาย/จน/รุ่งเรืองได้จริง
3. **Coexistence** — โลกเดียวกัน Character เจอกัน ค้าขาย ร่วมมือ หักหลัง
4. **BYOK** — สมองฉลาดแค่ไหนขึ้นกับ Token Budget ที่ Player ยอมจ่าย

---

## 3. Races

| Race | Passive | Playstyle |
|---|---|---|
| Human | +XP gain, เรียนรู้เร็ว | ปรับตัว / การเมือง |
| Elf | Mana สูง, อายุยืน, Hunger ช้า | เวทมนตร์ / ระยะยาว |
| Dwarf | Craft/Mine bonus, ทนทาน | ช่างฝีมือ / เศรษฐกิจ |
| Beastkin | Stamina regen ไว, sense สูง | สำรวจ / ล่าสัตว์ |

---

## 4. Character & Status

### Stats (use-based growth + decay)
- แกน: **STR / AGI / INT / CHA / VIT**
- Skills: Farming, Mining, Combat, Trading, Crafting, Magic, Diplomacy
- ใช้บ่อย = ขึ้น, ไม่ใช้นาน = ค่อยๆ ลด (แบบ Runescape ไม่ใช่แจก point)

### Survival (ชัดเจน วัดได้)
```
Stamina  0-100  → ทุก action กิน stamina / พัก-นอนเพื่อฟื้น
Hunger   0-100  → ลดตามเวลา / กินเพื่อเพิ่ม
HP       0-100  → ตายได้จริง (ดู §8)
Mood     0-100  → กระทบ efficiency ทุก action
```
ห่วงโซ่: **Hunger ต่ำ → Stamina regen ช้า → HP ลด** → ทำให้ "อาหาร" เป็นเศรษฐกิจพื้นฐานของโลก

---

## 5. Will System (หัวใจของเกม)

Player เขียน Will Document ตอนสร้าง Character:

```yaml
identity: "พ่อค้าคนแคระผู้รักสันติ แต่จดจำทุกคนที่โกงเขา"
goal: "สร้างเครือข่ายการค้าใหญ่ที่สุดใน Shocky Topia"
values:
  - ไม่เริ่มสงครามก่อน แต่ตอบโต้เสมอ
  - ขายเมื่อกำไร 20%+ เท่านั้น
fears: "กลัวความจนมากกว่ากลัวตาย"
budget_hint: "ประหยัด token ถ้าไม่มีอะไรสำคัญ"
```

ทุก Decision Tick → ส่ง Will + world state + Memory → Agent → ได้ Action กลับ
แก้ Will ได้แต่มี cooldown (เช่น 24 ชม.)

---

## 6. BYOK + Token Budget

Player กำหนดเอง:
- **Provider + Model** (Gemini Flash ถูก ↔ Claude แพงแต่ฉลาด)
- **Decision frequency** (ทุก 10 นาที ถูก ↔ ทุก 1 นาที react ไว แต่แพง)
- **Context depth** (Memory ย้อนหลังมาก = ฉลาด = แพง)
- **Daily token cap** → หมดงบเข้า **Instinct Mode** (ดู ADR-0004)

Fairness: lock ให้ทุก Agent ได้ context size / response window เท่ากันต่อ tick → ความต่างมาจาก strategy ไม่ใช่ pay-to-win ดิบ (ดู OPEN-QUESTIONS)

---

## 7. Career Paths (ทุกทางต้อง viable — ไม่บังคับสู้)

- **ผู้บุกเบิก** — explore, claim ที่ดิน, ตั้งถิ่นฐาน
- **เกษตรกร/นายพราน** — ผลิตอาหาร (demand ถาวรจาก Hunger)
- **ช่างฝีมือ** — craft เครื่องมือ/อาวุธ/ยา
- **พ่อค้า** — arbitrage ระหว่างเมือง, ตั้งร้าน
- **นักรบ/ทหารรับจ้าง** — PvE + PvP (มี friction, ดู §8)
- **ผู้ปกครอง** — รวมคนเป็น Settlement → Kingdom, เก็บภาษี, ออกกฎ

### Settlement Progression
```
Camp (1) → Village (5+) → Town (15+, มีตลาด) → Kingdom (30+, กฎหมาย+กองทัพ+การทูต)
```

---

## 8. Conflict & Death

- **PvP มี friction**: safe zone ตีไม่ได้ / ฆ่าคน → **Infamy** → guard ไล่ล่า, เมืองไม่ต้อนรับ
- **ตาย ≠ ลบ Character**: respawn ที่ shrine, ดรอปของที่ถือ, skill ลดเล็กน้อย, ขึ้น death log บน Wallboard
- สงคราม Kingdom ต้อง **declare ล่วงหน้า** (ให้ฝั่งตรงข้ามเตรียม + คนดูสนุก)

---

## 9. Wallboard (Spectator / Marketing engine)

- **World Map สด** — territory, settlement, สงครามที่กำลังเกิด
- **Leaderboards** — รวยสุด / อาณาเขตใหญ่สุด / อยู่รอดนานสุด / ค้าขายมากสุด
- **Chronicle Feed** — พงศาวดารอัตโนมัติ
- **Character Page** — ประวัติ, ความสัมพันธ์, journal ที่ Agent เขียนเอง

→ แชร์ลง social ได้ = viral loop ฟรี

---

## 10. Tech Architecture

```
Next.js          → Wallboard + Character management UI
Supabase         → World state, RLS, Realtime broadcast
Supabase Vault   → เก็บ AI Key ของ Player (ถ้าเลือก Option B — ADR-0001)
Edge Function    → World Tick engine (cron)
pgvector         → Character Memory
```

### Tick Design
- **World Tick** (ทุก ~1 นาที): rule engine ล้วน ไม่เรียก AI (ADR-0002)
- **Decision Tick** (ตาม Token Budget): เรียก Agent เฉพาะถึงรอบ หรือมี event สำคัญ
- Agent ตอบเป็น **structured Action enum** เท่านั้น (ADR-0003)

---

## 11. Roadmap

**Phase 0 — Prototype (3-4 สัปดาห์)**
โลก grid เล็ก, 1 เผ่า, Survival ครบ, BYOK client-side, 6 actions, Wallboard ง่ายๆ → ทดสอบกับเพื่อน 5-10 คน

**Phase 1 — Closed Alpha (~2 เดือน)**
4 เผ่า, skill system, ตลาด, Settlement, Vault-based key, Instinct Mode, Chronicle

**Phase 2 — Open Beta**
Kingdom, สงคราม, การทูต, Character/Will marketplace, season/reset cycle
