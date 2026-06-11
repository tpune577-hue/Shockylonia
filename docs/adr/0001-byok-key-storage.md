# ADR-0001: ตำแหน่งจัดเก็บ AI Key (BYOK)

## Status
Accepted — Option B ตั้งแต่ Phase 0 (revised จาก initial proposed plan)

## Context
Player ต้องนำ **AI Key** มาเอง (BYOK) คำถามคือ key ควรอยู่ที่ไหน เพราะมันกระทบทั้ง security liability, ความสามารถรัน Agent ตอน Player offline, และความง่ายในการเริ่มเล่น

3 ทางเลือก:
- **A — Client-side**: key อยู่ใน browser เท่านั้น, browser เรียก AI เอง แล้วส่ง action result มา server
- **B — Server Proxy / Vault**: encrypt key เก็บใน Supabase Vault, World/Decision Tick ดึงไปเรียกแทน Player (ผ่าน AI Gateway)
- **C — Player-hosted Agent**: Player รัน script เอง เชื่อม WebSocket มา server

### ทำไม revise จากแผนเดิม
แผนเดิมจะใช้ Option A ใน Phase 0 เพื่อความเร็ว แต่ Option A หมายความว่า Character หยุดทำงานเมื่อ Player ปิด tab ซึ่งขัดกับ core concept "AI Colony วิ่งตลอดเวลา" — Friend Test ที่บังคับ Player เปิด tab ค้างไว้จะไม่ validate concept จริงของเกม

## Decision
**Option B ตั้งแต่ Phase 0** — Supabase Vault เก็บ key encrypted, server-side Decision Tick worker เรียก AI ผ่าน Vercel AI Gateway แทน Player

Player ส่ง provider key ตอน onboarding → encrypt เก็บใน Vault → worker ดึงมาเรียกผ่าน AI Gateway → routing ไป provider ตามที่ Player เลือก

Option C เก็บไว้เป็น power-user feature ในอนาคต (ถ้ามีคนต้องการ self-host Agent)

## Consequences
**ดี:**
- Character รัน 24/7 จริง สอดคล้องกับ core concept "เลี้ยงเจตจำนงที่ใช้ชีวิตแทนตัวเรา"
- AI Gateway = 1 integration รองรับหลาย provider (Anthropic / Gemini / OpenAI) ไม่ต้องเขียน SDK แยก
- Server เป็นคน schedule Decision Tick ทำให้ ADR-0002 (World vs Decision Tick split) ทำงานได้จริง
- Friend Test ที่ทำกับ Player จริงสะท้อนพฤติกรรมจริงของเกม ไม่ใช่ artifact ของการเปิด tab

**เสีย/ความเสี่ยง:**
- เราถือ key ของ Player = **ต้องมี Privacy Policy + ToS + key liability disclosure** ตั้งแต่ Phase 0
- ต้อง encrypt at rest ผ่าน Supabase Vault, RLS เข้มงวด, audit log
- ต้องมี key revocation UI ให้ Player ลบ key ได้ทันที
- เพิ่ม scope Phase 0 +2 สัปดาห์ (Vault + worker + AI Gateway + security review)
- Player ที่ส่ง key invalid / token หมด → Character เข้า Instinct Mode (ADR-0004 ยังทำงานเหมือนเดิม)
