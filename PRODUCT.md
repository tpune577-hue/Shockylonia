# Product

## Register

product

## Users

ผู้เล่นเกมแนวออกแบบกลยุทธ์ระยะยาว (Crusader Kings / Dwarf Fortress / Rimworld players) ที่ชอบ "ดูเรื่องราวเกิดขึ้น" มากกว่า "ควบคุมทุกการกระทำ" Player ส่วนใหญ่จะเข้ามาดู Character ของตัวเองวันละหลายครั้ง ครั้งละ 5-15 นาที — ไม่ใช่ session ยาวต่อเนื่อง

Context: Player อาจอยู่บน mobile ตอนพักงาน หรือ desktop ตอนพักยาว ทั้งสองต้อง first-class — แต่ mobile-first เพราะ "ดูแว้บ" คือ pattern หลัก

Job to be done:
- เลี้ยง Character ที่ใช้ชีวิตแทนตัวเองในโลก persistent
- เขียน/แก้ Will ให้สะท้อนตัวตน Character
- ดูว่า Character ทำอะไร เจอใคร ไปไหน
- ตัดสินใจ strategic เป็นครั้งคราว (ปรับ Will, ตอบสนอง event)

## Product Purpose

Shocky Topia ให้ Player สร้าง "เจตจำนง" หนึ่งดวงและปล่อยให้ AI Agent (ที่ Player นำ key มาเอง) พา Character ของตัวเองไปใช้ชีวิตในโลก persistent ที่มี Character อื่นอีกหลายคน เกมรัน 24/7 ไม่มี reset แม้ Player ไม่อยู่ Character ยังเดิน

Success: Player กลับมาแล้วรู้สึก "อยากอ่านต่อ" — Chronicle ของ Character เขาเองและของคนอื่นน่าติดตามพอที่จะทำให้กลับมาดูทุกวัน ไม่ใช่ "อยากกดเล่นต่อ" แบบเกม session-based

## Brand Personality

**Strategic · Living · Persistent**

- **Strategic** — UI สนับสนุนการคิดและเขียน ไม่ใช่การกดทันที Will Editor คือ centerpiece, action log คือสิ่งที่ดูทบทวน
- **Living** — โลกหายใจอยู่เสมอ มี real-time pulse แม้ Player ไม่ได้สั่งอะไร แสดงให้เห็นว่า Character ของเขาไม่ใช่ตัวเลขใน DB แต่เป็นชีวิตที่กำลังเกิดขึ้น
- **Persistent** — ทุกอย่างเดินต่อ ไม่มี reset ไม่มี loading state ที่บอกว่า "เริ่มใหม่" Chronicle สะสมเรื่อยๆ ตามเวลาจริง

Voice: เหมือนอ่านวรรณกรรมแฟนตาซีฉบับวิจารณ์ — ใช้ภาษาแน่น ไม่ตื่นเต้นเกินจำเป็น แต่ละคำมีน้ำหนัก สั่งงานด้วย verb + object ("Pen a will", "Sever the binding", "Inscribe trade") ไม่ใช่ "Click here" / "OK"

## Anti-references

**ไม่ควรเป็น:**
- **AAA fantasy HUD** (Skyrim, MMO, Genshin) — ไม่มี hotbar, compass, persistent health bar overlay, quest markers Player ไม่ได้ "ควบคุม" Character → ไม่ต้องการ HUD แบบนั้น
- **Cream/parchment AI default** — fantasy ส่วนใหญ่ใช้ beige paper background นี่คือ AI generation reflex สำคัญที่สุดที่ต้องเลี่ยง Shocky Topia ใช้ committed dark surface
- **SaaS dashboard cards** — ไม่ใช่ "Stripe-but-for-fantasy" ไม่มี KPI tiles, ไม่มี card grid generic
- **Generic gacha/anime UI** — ไม่มี rarity tiers, sparkle effects, banner

## Design Principles

1. **Reading is the primary action** — Player ส่วนใหญ่อ่าน (Chronicle, Will, action log) มากกว่าคลิก typography hierarchy + reading comfort สำคัญกว่า dashboard density

2. **The world doesn't wait** — UI ต้องสื่อว่าเวลาเดินจริง ไม่ใช่ pull-to-refresh model live tick / real-time updates / subtle motion สำคัญ แต่ไม่ใช่ flashy

3. **One soul, one screen** — ไม่ใช่ multi-character management UI Player มี Character เดียว ทุกหน้าจอควรเคารพข้อเท็จจริงนั้น ไม่ทำให้รู้สึกเหมือนเล่น multi-account

4. **Decisions, not commands** — UI button ทุกตัวต้องสื่อความ "deliberate choice" ไม่ใช่ "quick action" การแก้ Will มี cooldown และ feel ของการเขียน manifesto ไม่ใช่ submit form

5. **The chronicle is the product** — Wallboard + Chronicle Feed คือสิ่งที่ดึงคนกลับมา ออกแบบให้ดูสวย น่าเล่า น่า screenshot ลง social ได้

## Accessibility & Inclusion

- WCAG AA สำหรับ body text (≥4.5:1), AAA สำหรับ long-form reading surfaces (Chronicle, Will) (≥7:1)
- รองรับ Thai + English UI สลับได้ (Player คนไทยเป็นกลุ่มแรก แต่ design ต้องรองรับ scale ภาษาอื่น)
- `prefers-reduced-motion`: เปลี่ยน real-time pulse จาก animation → static indicator
- Color is never the only signal — Survival state ใช้ color + label + position
- Touch target ≥44×44px บน mobile
- Reading mode: respect `prefers-color-scheme` แต่ default = dark (Editorial chronicle ที่ commit กับ dark surface)
