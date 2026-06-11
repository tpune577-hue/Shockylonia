# CONTEXT.md — Shockylonia Domain Glossary

> Canonical terms ของ project นี้ ใช้คำเหล่านี้ให้ตรงกันทั้ง code, doc, และ UI
> ห้าม couple กับ implementation detail — ใส่เฉพาะสิ่งที่ domain expert เข้าใจได้

---

## Player
มนุษย์ที่นั่งหน้าจอ เป็นเจ้าของ **Character** และเป็นผู้เขียน **Will** + ผูก **AI Key** ตัวเอง
Player ไม่ได้สั่ง action ทีละ step — Player ออกแบบเจตจำนง แล้วปล่อยให้ Agent ตัดสินใจ
หนึ่ง Player มีได้เพียง **หนึ่ง Character เสมอ** — เป็นหลักการของเกม ไม่ใช่ข้อจำกัดชั่วคราว

## Character
ตัวละครหนึ่งตัวในโลก Shockylonia มี **Race**, **Stats**, **Survival** state และ **Memory** ของตัวเอง
หนึ่ง Player เริ่มที่หนึ่ง Character (จำนวนต่อ Player ในอนาคต = open question)

## Will (เจตจำนง)
เอกสารที่ Player เขียนเพื่อนิยาม "ตัวตน" ของ Character — identity, goal, values, fears
เป็น **คำบรรยายตัวตน ไม่ใช่ชุดคำสั่ง** — Agent ตีความเอาเองตามสถานการณ์
Will เป็น**รากฐานของพฤติกรรมทั้งหมด** — ทั้งตอน Agent รัน และตอนอยู่ใน Instinct Mode
Will ที่ดี = Character ที่อยู่รอดและพัฒนาได้แม้ไม่มี token / Will ที่ห่วย = Character ประพฤติไม่สอดคล้อง อาจตายได้
แก้ได้แต่มี cooldown เพื่อรักษา identity และกัน micro-control

## Agent
"สมอง" ของ Character — โมเดล AI ที่ Player นำมาเชื่อม (ผ่าน BYOK) ทำหน้าที่อ่าน world state + Will + Memory แล้วตัดสินใจออกมาเป็น **Action**
Agent ของแต่ละ Player เป็นคนละโมเดล/คนละงบกันได้

## BYOK (Bring Your Own Key)
หลักการที่ Player ต้องนำ **AI Key** ของตัวเองมาใช้ — เจ้าของเกมไม่แจก token ให้
ต้นทุน AI ทั้งหมดอยู่กับ Player และกลายเป็น game mechanic (จ่ายมาก = สมองฉลาด/ตัดสินใจถี่กว่า)

## AI Key
credential (api key + endpoint + model) ที่ Player ผูกไว้กับ Character เพื่อให้ Agent เรียกใช้
ตำแหน่งจัดเก็บ key เป็นการตัดสินใจสำคัญ — ดู ADR-0001

## Token Budget
เพดานการใช้ token ที่ Player กำหนดเอง (เช่น cap ต่อวัน + ความถี่การตัดสินใจ + ความลึกของ context)
หมดงบ → Character เข้า **Instinct Mode**

## Instinct Mode
สถานะที่ Character รันด้วย **Survival Profile** แทน Agent เมื่อ token หมด / API ล่ม
พฤติกรรมขึ้นกับคุณภาพของ Will ที่เขียน — Will ดี = อยู่รอดและประพฤติสอดคล้อง / Will ห่วย = ประพฤติผิดพลาด อาจตายได้
ไม่เรียก AI เลยในโหมดนี้ — ดู ADR-0004

## Memory
สิ่งที่ Character "รู้" และส่งเป็น context ให้ Agent ทุก Decision Tick มีสามชั้น:
- **Working Memory** — เหตุการณ์ล่าสุด ~20-50 events (sliding window, context size คงที่)
- **Relationship Memory** — ความสัมพันธ์กับ Character/Settlement ที่รู้จัก (decay เมื่อไม่เจอกันนาน)
- **World Knowledge** — ข้อมูลภูมิศาสตร์และการเมืองที่ Character เคยพบเห็น (stale เมื่อไม่ update)

## Survival Profile
Structured rules ที่ถูก generate จาก Will ครั้งเดียวตอน Player submit หรือแก้ Will
เป็นสิ่งที่ Instinct Mode ใช้ตัดสินใจโดยไม่ต้องเรียก AI — คุณภาพขึ้นกับความชัดเจนของ Will ต้นทาง

## Action
ผลลัพธ์การตัดสินใจของ Agent ในหนึ่ง Decision Tick เลือกจาก **enum ที่กำหนดไว้เท่านั้น** (ไม่ใช่ freeform) — ดู ADR-0003
ตัวอย่าง action พื้นฐาน: Move / Gather / Eat / Rest / Craft / Trade / Talk / Attack / Build / Claim

## World Tick
รอบอัปเดตโลกแบบ rule engine ล้วน (hunger ลด, stamina ฟื้น, พืชโต, unit ขยับ) — **ไม่เรียก AI** — ดู ADR-0002

## Decision Tick
รอบที่ระบบเรียก **Agent** ของ Character ให้ตัดสินใจ ความถี่ขึ้นกับ Token Budget ของ Player
trigger ได้ทั้งแบบ "ถึงรอบ" หรือ "มี event สำคัญ" (ถูกโจมตี / มีคนมาคุยด้วย)

## Survival
ชั้นความอยู่รอดของ Character วัดเป็นค่าชัดเจน: **Stamina, Hunger, HP, Mood**
ห่วงโซ่: Hunger ต่ำ → Stamina regen ช้า → HP ลด → ตายได้จริง

## Stats
ค่าความสามารถที่ **พัฒนาและถดถอยตามการใช้งานจริง** (ใช้บ่อย=ขึ้น, ไม่ใช้นาน=decay)
แกนหลัก: STR / AGI / INT / CHA / VIT + Skills (Farming, Mining, Combat, Trading, Crafting, Magic, Diplomacy)

## Race
เผ่าพันธุ์ของ Character — ต่างกันที่ **Passive bonus** ไม่ใช่แค่ skin
Human (เรียนรู้เร็ว) / Elf (มานาสูง อิ่มนาน) / Dwarf (คราฟต์-ขุดเก่ง) / Beastkin (stamina regen ไว)

## Settlement
หน่วยการรวมกลุ่มของ Character บนแผนที่ ไล่ระดับ: **Camp → Village → Town → Kingdom**
ระดับสูงขึ้นปลดล็อกตลาด, กฎหมาย, กองทัพ, การทูต

## Kingdom
Settlement ระดับสูงสุด มีผู้ปกครอง เก็บภาษี ออกกฎ ทำสงคราม/การทูตกับ Kingdom อื่นได้
สงครามระดับ Kingdom ต้อง **declare ล่วงหน้า**

## Infamy
ค่าชื่อเสียงด้านลบที่เกิดจากการก่อความรุนแรง (เช่น ฆ่าใน safe zone)
Infamy สูง → NPC guard ไล่ล่า, เมืองไม่ต้อนรับ — กลไกสร้าง friction ให้ PvP

## Wallboard
ชั้น spectator สาธารณะ — แผนที่โลกสด, leaderboards, และ **Chronicle**
เป็นทั้งหน้าจอติดตามเกมและช่องทาง viral

## Chronicle
พงศาวดารอัตโนมัติ — ระบบเขียนเหตุการณ์สำคัญของโลกเป็นเรื่องเล่า ("วันที่ 47: ราชอาณาจักรเหล็กประกาศสงคราม...")

## The Blessing (พรแห่ง Shockylonia)
พรที่ผู้คนทุกคนในแผ่นดิน Shockylonia ได้รับ — ทำให้มีอายุยืนยาวและไม่ตายตามอายุขัยตามธรรมชาติ
ผลกลไก: Character ไม่ตายจาก aging แต่เปลี่ยนแปลงตาม **Age Phase** ตลอดชีวิต

## Age Phase
ช่วงชีวิตของ Character ที่กำหนด stat modifier: **Young** (XP gain สูง) → **Prime** (peak) → **Elder** (INT/CHA ขึ้น, STR/AGI ลง) → **Ancient** (ร่างกายอ่อนแอ แต่ Relationship Memory depth พิเศษ, NPC เคารพสูง)
Race ที่อายุยืนกว่าอยู่ใน Prime phase นานกว่า — ดู ADR-0010

## Season (ฤดูกาล)
หน่วยเวลาย่อยของปี — 1 Season = 30 วันเกม = 5 ชม.จริง
4 Seasons: Spring / Summer / Autumn / Winter

## Year (ปีเกม)
หน่วยเวลาหลักที่ใช้ใน Chronicle — 1 Year = 4 Seasons = 120 วันเกม = 20 ชม.จริง

## Undiscovered Territory
พื้นที่บนแผนที่ที่ยังไม่มี Character ไปถึง — ไม่มีข้อมูลอยู่ใน world state จนกว่าจะถูก explore
การค้นพบ Undiscovered Territory เป็น event ที่บันทึกใน Chronicle

## Frontier
ดินแดนที่เพิ่งถูก discover แต่ยังไม่มีใครครอบครอง — พื้นที่สำหรับผู้เล่นใหม่ที่ไม่ต้องการอยู่ภายใต้ Kingdom เดิม
