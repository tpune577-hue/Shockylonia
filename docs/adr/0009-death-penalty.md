# ADR-0009: Death Penalty

## Status
Accepted

## Context
Death penalty กำหนด risk appetite ของทั้งโลก — เบาเกิน PvP ไม่มีน้ำหนัก หนักเกิน ทุกคนหลีกเลี่ยงความเสี่ยงจนโลกซบเซา

ที่ pace 1 วันเกม = 10 นาทีจริง ต้องชั่งว่า "ตาย 1 ครั้ง เสียงานไปเท่าไร" รู้สึกได้แต่ไม่ทำลาย months of progress

## Decision
- **Skill loss**: ลด 10-15% ของ level ปัจจุบันทุก skill ที่ใช้งาน
- **Item drop**: ดรอป**ของที่ถืออยู่ทั้งหมด** (equipped + carried) แต่ **ไม่แตะ** ของที่เก็บไว้ใน Settlement inventory
- **Respawn**: ที่ Shrine ที่ Character ผูกไว้ล่าสุด
- **Recovery period**: skill ค่อยๆ คืนสู่ระดับก่อนตายภายใน **1-3 ชม.จริง (6-18 วันเกม)** — ระหว่างนี้ Character ยังทำงานได้แต่ stats ต่ำกว่าปกติ

## Consequences
**ดี:**
- ตาย = เจ็บพอให้ระวัง แต่ settlement ยังเป็น safe harbor สำหรับทรัพย์สมบัติ
- Recovery 6-18 วันเกมสร้าง downtime ที่รู้สึกได้โดยไม่ทำลาย Character ถาวร
- PvP มีน้ำหนัก สงครามมีความหมาย แต่ไม่ทำให้คนกลัวจนไม่กล้าออกจาก Settlement

**เสีย/ความเสี่ยง:**
- ต้องมี Shrine binding mechanic ที่ชัดเจน — Character ผูก Shrine ได้ที่ไหน เปลี่ยนได้บ่อยแค่ไหน
- ถ้า Settlement ที่มี inventory ถูกยึดขณะ Character ตาย — ทรัพย์สมบัติที่อยู่ใน inventory ก็อาจสูญ (ตาม Settlement conquest mechanic ซึ่งยังไม่ได้ออกแบบ)
