# Open Questions — รอ grill ให้ resolve

คำถามที่ยังไม่ตัดสินใจ เรียงตามผลกระทบ ตัวไหน resolve แล้วให้ย้ายไปเป็น ADR หรืออัปเดต CONTEXT.md

---

## 🔴 กระทบสถาปัตยกรรม (ตอบก่อน)

### Q1. Pace ของโลก
real-time ช้า (1 วันเกม = 1 ชม.จริง) หรือเร็วกว่า?
→ กระทบ Token cost ของ Player โดยตรง และกระทบ World Tick frequency (ADR-0002)
💡 *แนะนำ: เริ่มช้า ให้ Decision Tick ไม่ถี่ จะคุมต้นทุน Player ได้ง่ายตอน prototype*

### Q2. Persistent ตลอดกาล vs Season Reset
reset ทุก ~3 เดือน (แบบ Rust) หรือโลกเดียวถาวร?
→ กระทบ data model, การออกแบบ Wallboard, และ retention loop
💡 *แนะนำ: Season-based — สนุกกว่าสำหรับเกมแข่งขัน, ลด snowball ของคนมาก่อน, สร้าง "จุดเริ่มใหม่" ให้คนใหม่*

### Q3. Standalone product vs ใต้ Sandory Box
แชร์ AI/infra กับ Sandory Box ได้เยอะ แต่ positioning ต่างกัน
→ กระทบ branding, billing, codebase strategy
💡 *แนะนำ: แยก repo (อันนี้) แต่ reuse infra pattern — ตัดสินใจ branding ทีหลังได้*

---

## 🟡 กระทบ Game Design

### Q4. Fairness ของ BYOK ให้ลึกแค่ไหน
lock context size/response window เท่ากันพอไหม? หรือต้อง lock model tier ด้วย? หรือใช้ ELO matchmaking จับคู่ Agent ระดับใกล้กัน?
→ ถ้าปล่อยเสรี โมเดลแพงสุดชนะทุกเกม

### Q5. หนึ่ง Player มีกี่ Character
เริ่มที่ 1 ตัว แต่เพิ่มได้ไหม? ถ้าได้ — กระทบ Token Budget, Wallboard, และ balance อำนาจ

### Q6. Will marketplace
ให้ขาย/แชร์ Will template กันได้ไหม (Phase 2)? เป็น creator economy ที่ดี แต่ทำให้ meta แข็งตัวเร็ว

### Q7. Instinct Mode abuse
มีคนเล่นพึ่ง Instinct Mode ล้วนเพื่อประหยัด token — เป็น playstyle ที่ยอมรับได้ หรือเป็นปัญหา balance? (เกี่ยวกับ ADR-0004)

---

## 🟢 รายละเอียด (ตอบทีหลังได้)

### Q8. Memory model
Character จำอะไรได้บ้าง / นานแค่ไหน / pgvector เก็บ event แบบไหน → กระทบ context depth = token cost

### Q9. Death penalty หนักแค่ไหน
skill ลดเท่าไร, ดรอปของทั้งหมดหรือบางส่วน, respawn ที่ไหน

### Q10. หน่วยเวลาในเกม
"วัน" ในเกมยาวเท่าไรจริง, season กี่วันเกม, อายุ Character (Elf อายุยืน) มีผลกลไกอย่างไร
