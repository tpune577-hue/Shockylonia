# ADR-0007: BYOK Fairness Model — Budget เป็น Mechanic ไม่ใช่ปัญหา

## Status
Accepted

## Context
BYOK แปลว่า Player แต่ละคนใช้ AI คนละระดับ คนละงบ ตัวแปรที่ต่างกันได้:
- **Decision frequency** — ตัดสินใจทุก 1 นาที vs ทุก 30 นาที
- **Context depth** — Memory ย้อนหลัง 1,000 events vs 10 events
- **Model tier** — Claude Opus vs Gemini Flash

คำถามคือควร lock บางตัวแปรให้เท่ากันเพื่อ competitive fairness หรือปล่อยให้เป็น player choice ทั้งหมด

## Decision
**ทุกตัวแปรเป็น player-controlled budget mechanic — ไม่ lock เพื่อ fairness**

สิ่งที่ lock เท่ากันมีเพียง: **output format** (structured action enum ตาม ADR-0003) เพื่อป้องกัน prompt injection และรักษา game state integrity ไม่ใช่เพื่อ fairness

Budget ที่สูงกว่า = ได้เปรียบจริง โดยตั้งใจ สอดคล้องกับ design pillar BYOK: "สมองฉลาดแค่ไหนขึ้นกับ Token Budget ที่ Player ยอมจ่าย"

## Consequences
**ดี:**
- Consistent กับ core concept — ไม่ต้องสร้าง matchmaking หรือ tier system เพิ่ม
- Player รู้ว่าลงทุนอะไรได้อะไร โปร่งใสตั้งแต่ต้น
- Instinct Mode เป็น floor ที่ทำให้คนงบน้อยยังเล่นได้ ไม่ใช่ pay-to-survive

**เสีย/ความเสี่ยง:**
- Early meta อาจถูกครอบงำโดย player ที่จ่าย token สูงก่อนที่ Will quality และ strategy จะ matter มากกว่า budget
- ต้องสื่อสาร mechanic นี้อย่างชัดเจนตั้งแต่ onboarding — ไม่ใช่ "เกมฟรี" ในเชิงการแข่งขัน
