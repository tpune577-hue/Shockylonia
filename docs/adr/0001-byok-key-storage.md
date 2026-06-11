# ADR-0001: ตำแหน่งจัดเก็บ AI Key (BYOK)

## Status
Proposed — รอ grill: prototype ใช้ Option A, production เล็งไป Option B

## Context
Player ต้องนำ **AI Key** มาเอง (BYOK) คำถามคือ key ควรอยู่ที่ไหน เพราะมันกระทบทั้ง security liability, ความสามารถรัน Agent ตอน Player offline, และความง่ายในการเริ่มเล่น

3 ทางเลือก:
- **A — Client-side**: key อยู่ใน browser เท่านั้น, browser เรียก AI เอง แล้วส่ง action result มา server
- **B — Server Proxy / Vault**: encrypt key เก็บใน Supabase Vault, World/Decision Tick ดึงไปเรียกแทน Player
- **C — Player-hosted Agent**: Player รัน script เอง เชื่อม WebSocket มา server

## Decision
- **Prototype (Phase 0): Option A** — เร็วสุด ไม่ต้องแบก liability ของ key
- **Production (Phase 1+): Option B** — Agent รันได้แม้ Player offline, ระบบควบคุม tick ได้เต็ม (จำเป็นต่อ persistent world)
- Option C เก็บไว้เป็น power-user feature ภายหลัง

## Consequences
**ดี:** เริ่ม prototype ได้ไว, production รองรับโลกที่เดินตลอดเวลาแม้ไม่มีใคร online
**เสีย/ความเสี่ยง:**
- Option A → Player cheat ส่ง action เองได้ (ยอมรับได้ใน prototype)
- Option B → เราถือ key ของคนอื่น = ต้องมี Privacy Policy + encryption + RLS เข้มงวด + วิธีให้ Player เพิกถอน key
- ต้องเขียน abstraction layer รองรับทั้งสองโหมดตั้งแต่แรก เพื่อไม่ rewrite ตอนย้าย A→B
