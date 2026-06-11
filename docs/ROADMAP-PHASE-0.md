# Phase 0 Roadmap — Shockylonia Prototype

> สถานะ: **Planned** · เป้าหมาย: ทดสอบ loop หลักกับเพื่อน 5-10 คนภายใน ~7 สัปดาห์
> ปรับ scope จาก CONCEPT.md §11 หลัง grill session 2026-06-11

---

## 1. เป้าหมาย Phase 0

ทดสอบ loop หลักของเกม:
**Will → Survival Profile → Agent (BYOK ผ่าน AI Gateway) → Structured Action → World State → Memory → next Decision Tick**

ถ้า loop นี้รู้สึก "ถูก" และ Will quality สร้าง differentiation จริงระหว่าง Character — เกมพร้อมเข้า Phase 1

## 2. Scope

### สร้าง
- 1 race: **Human** เท่านั้น
- **10 actions** with simplified semantics (ดู §5)
- Grid โลก: starting **30×30**, ขยายตาม exploration (ไม่มี hard bounds)
- Survival ครบ (Hunger / Stamina / HP / Mood)
- **BYOK Server-side via Supabase Vault + Vercel AI Gateway** (ADR-0001 Option B, ตัดสินใจใหม่จาก Option A)
- Will + Survival Profile generator
- Memory 3 ชั้น (Working / Relationship / World Knowledge)
- Instinct Mode (Survival Profile-driven)
- Public Wallboard เบื้องต้น

### ตัดออกจาก Phase 0
- Settlement levels (Camp/Village/Town/Kingdom — เป็น Phase 1)
- Currency, market — Phase 0 ใช้ barter เท่านั้น
- 3 races อื่น (Elf / Dwarf / Beastkin)
- Will marketplace
- War declarations & diplomacy
- AI-generated Chronicle (Phase 0 ใช้ template-based)
- Shrine binding flow เต็ม (Phase 0 ใช้ spawn point เดียว)

## 3. Architecture

```
[Player Browser]                          [Vercel + Supabase]
  Onboarding UI                            ┌──────────────────────────┐
   ส่ง provider key  ─────────────────→    │ Supabase Vault           │
                                           │ (encrypted key storage)  │
  Will Editor                              └────────────┬─────────────┘
  Wallboard / Character page                            │
  Realtime subscription                                 ↓
                                           ┌──────────────────────────┐
                              ┌─────────→  │ Vercel Cron (1 min)      │
                              │            │  ├─ World Tick           │
                              │            │  │   (rule engine ล้วน)   │
                              │            │  │   - Game clock +0.1d  │
                              │            │  │   - Hunger decay      │
                              │            │  │   - Stamina regen     │
                              │            │  │   - HP / Mood update  │
                              │            │  │                       │
                              │            │  └─ Decision Tick fanout │
                              │            │     - SELECT characters  │
                              │            │       WHERE next_dec<=now│
                              │            │     - per-char worker    │
                              │            └──────────┬───────────────┘
                              │                       ↓
                              │            ┌──────────────────────────┐
                              │            │ Decision Worker          │
                              │            │  1. โหลด Will + Survival │
                              │            │     Profile + Memory     │
                              │            │  2. AI Gateway call      │
                              │            │     (key จาก Vault)      │
                              │            │  3. validate Structured  │
                              │            │     Action (Zod)         │
                              │            │  4. apply → world state  │
                              │            │  5. log → actions table  │
                              │            └──────────┬───────────────┘
                              │                       ↓
                              │            ┌──────────────────────────┐
                              │            │ Supabase Postgres        │
                              │            │  + pgvector (Memory)     │
                              │            │  + Realtime channels     │
                              └─── push ───┤  → Wallboard updates     │
                                           └──────────────────────────┘
```

## 4. Data Model (ตารางหลัก)

```
players                  auth + provider preference + daily token cap
characters               1:1 player, stats, survival, position, age
character_skills         skill levels (use-based)
wills                    identity / goal / values / fears / budget_hint
survival_profiles        generated rules จาก Will (Claude Haiku)
world_tiles              x/y, type, resource, discovered_at, discovered_by
memory_working           sliding window event log
memory_relationships     trust + history per known target
memory_world_knowledge   tile-level facts seen by character
actions                  decision log: agent / instinct, params, outcome
game_clock               singleton: current_year, season, day
vault_keys               encrypted BYOK secret + provider metadata
```

## 5. Action Set — 10 actions, simplified semantics

| Action | Phase 0 semantics |
|---|---|
| **Move** | tile→tile, ใช้ Stamina |
| **Gather** | resource tile → inventory |
| **Eat** | inventory → Hunger+ |
| **Rest** | นอน → Stamina regen เร็วขึ้น |
| **Talk** | broadcast ข้อความให้ Character ใน adjacent tiles (ไม่ใช่ 1-on-1 channel) |
| **Attack** | direct HP damage, สร้าง Infamy |
| **Craft** | recipe predefined เท่านั้น (เช่น 3 wood → 1 shelter material) |
| **Trade** | 1-on-1 บน tile เดียวกัน, item-for-item, ไม่มีเงิน |
| **Build** | สร้าง Shelter 1 ช่อง (Rest bonus) จาก material |
| **Claim** | mark tile เป็นของ Character (กัน Gather, ไม่กันเดินผ่าน) |

ยังไม่ทำใน Phase 0: Settlement progression, currency, recipe tree, structure decay, complex social channels

## 6. Decisions ที่ตอบแล้ว (D1-D6)

| ID | Decision | ค่า |
|---|---|---|
| D1 | Actions scope | All 10 actions, simplified semantics (§5) |
| D2 | Grid size | Starting 30×30, ขยายตาม exploration (no hard bounds) |
| D3 | Survival Profile generator | Claude Haiku 4.5, เกมเป็นคนจ่าย, Will cooldown 24 ชม. |
| D4 | Decision Tick scheduling | Vercel Cron 1 นาที (เท่ากับ World Tick) + worker fan-out per character |
| D5 | BYOK provider scope | Hybrid UX — recommended 3 (Anthropic/Gemini/OpenAI) ด้านบน + expandable "All Providers" ผ่าน AI Gateway |
| D6 | Realtime framework | Supabase Realtime channels |

## 7. Milestones

> รวมประมาณ 6-7 สัปดาห์ full-time (เพิ่มจาก plan เดิม 4-5 สัปดาห์ เพราะข้ามไป Option B ตั้งแต่ Phase 0)

### M0 — Bootstrap *(2-3 วัน)*
Next.js 15 + Tailwind / Supabase project / pgvector / Vault extension เปิดใช้ / Vercel deploy / Vercel Cron config / AI Gateway env / Auth (magic link) / empty schema migrated
**Done when:** signed-in user landing on empty page, DB + Vault + AI Gateway พร้อม

### M1 — World Engine *(3-4 วัน)* — ADR-0002, ADR-0005
World tile generator 30×30, `game_clock` advance, World Tick cron (1 min = 0.1 game day), Hunger decay (~1-1.4%/tick), Stamina regen, HP decay, Mood update, hardcoded Character ทดสอบ Survival loop
**Done when:** Character อยู่ในโลก, ถ้าไม่ทำอะไรค่อยๆ ตาย

### M2 — Character + Will + Survival Profile *(3-4 วัน)* — ADR-0004, ADR-0010
Character creation (Human only), Will editor (identity/goal/values/fears/budget_hint), submit → Claude Haiku 4.5 → Survival Profile structured rules, Will cooldown 24 ชม., Age Phase calculator
**Done when:** Player สร้าง Character + Will → generate Survival Profile สำเร็จ

### M3 — Server-side Agent Loop *(5-6 วัน)* — ADR-0001, ADR-0003, ADR-0007
- BYOK onboarding (Hybrid UX จาก D5) → key เข้า Supabase Vault
- AI Gateway integration (Vercel AI SDK)
- 10 actions enum + Zod validator
- Decision Tick worker: load context → AI Gateway call → validate action → apply state → log
- Server cron query characters พร้อม decision → fan-out to worker
- UI: live action log + state changes
- Key revocation UI
- Privacy Policy + ToS + liability disclosure
**Done when:** Player ผูก key, ปิด browser ไป — Character ยังเดินต่อ ตัดสินใจตาม Will

### M4 — Memory 3 ชั้น *(3 วัน)* — ADR-0008
Working Memory: sliding window ~30 events / Relationship Memory: trust + history / World Knowledge: discovered tiles + facts / Memory aggregator inject เข้า Decision Tick prompt / pgvector setup สำหรับ similarity search
**Done when:** Agent อ้างถึงเหตุการณ์อดีตและจดจำคนที่เจอได้

### M5 — Instinct Mode *(2-3 วัน)* — ADR-0004
Detect: key invalid / token cap หมด / API error 3 ครั้งติด → switch to Instinct Mode / Survival Profile interpreter (deterministic rules) / action enum เดียวกัน / UI indicator แสดงโหมดปัจจุบัน / re-attempt Agent หลัง backoff
**Done when:** key หมดอายุ → Character ยังกินข้าว/นอน/หนี ตาม Will ที่เขียนไว้

### M6 — Multi-Player + Wallboard *(3-4 วัน)* — ADR-0006
5-10 Player ในโลกเดียวกัน / Supabase Realtime: tile + action + character updates / Public Wallboard: live map, character list, action feed, template Chronicle / RLS policies (Will + Vault เข้มงวด) / Exploration-driven tile reveal
**Done when:** Player A เห็น Player B เคลื่อนไหวบนแผนที่ real-time, Wallboard เปิดสาธารณะได้

### M7 — Death + Recovery — minimal *(1-2 วัน)* — ADR-0009
HP=0 → drop carried items, skill -10%, respawn ที่ spawn point ของโลก (Phase 0 ยังไม่มี Shrine binding) / Recovery 1 ชม.จริง stat ค่อยๆ คืน
**Done when:** Character ตาย → กลับมาที่ spawn → 1 ชม. stat กลับ

### M8 — Friend Test *(1 สัปดาห์)*
Invite 5-10 คน / observe: Will quality vs Character behavior, token cost feel, decision frequency feel, server load / collect feedback / decide Phase 1 priorities

## 8. Open Implementation Questions (เก็บไว้ตอบใน Phase 1)

- Settlement progression (Camp → Village → Town → Kingdom) — ต้อง grill ก่อน Phase 1
- Currency design (มี gold? barter-only?) — ขึ้นกับ Trade volume ที่เห็นใน Phase 0
- Shrine binding mechanic (Player ผูก Shrine ที่ไหน เปลี่ยนได้บ่อยแค่ไหน)
- Recipe tree (Phase 0 ใช้ predefined ~5 recipes พอ)
- War declaration UX
- Race lifespan ตัวเลขจริง (Phase 0 ใช้ Human เท่านั้น)
- Will marketplace (Phase 2)

## 9. Acceptance Criteria สำหรับ Phase 0 → Phase 1 transition

1. ≥ 5 Player สามารถสร้าง + เลี้ยง Character ในโลกเดียวกัน
2. Character รัน 24/7 จริงโดย Player ไม่ต้องเปิด tab
3. มี case ที่ Will quality สร้างผลลัพธ์ต่างกันชัดเจน (Will ดี = อยู่รอด, Will ห่วย = ตาย/พังเร็ว)
4. Instinct Mode trigger ได้จริงและ Character ยังประพฤติสอดคล้องกับ Will
5. Token cost ต่อ Player/วัน อยู่ในระดับที่ Player ยินดีจ่าย (validated ผ่าน feedback)
6. World pace 1 วัน = 10 นาทีจริงรู้สึก "ใช่" (ไม่เร็วเกินไป ไม่ช้าเกินไป)

ถ้าผ่านทั้ง 6 ข้อ → ลงทุน Phase 1 (Closed Alpha)
