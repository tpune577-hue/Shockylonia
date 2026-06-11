# ADR-0005: World Pace — 1 วันเกม = 10 นาทีจริง

## Status
Accepted

## Context
ต้องเลือก ratio ระหว่าง real time กับ game time เพราะมันกระทบทุกอย่าง — World Tick frequency, Decision Tick density, Survival parameter, และ "ความรู้สึก" ของโลก

สิ่งที่ต้องการ: โลกที่เหตุการณ์ scale ใหญ่ (อาณาจักรผุดขึ้น, ปฏิวัติ, การสำรวจดินแดนใหม่) เกิดขึ้นได้ในระยะเวลาที่ Player เล่นจริงๆ ไม่ใช่รอเดือน

ทางเลือกที่พิจารณา:
- **1 วันเกม = 1 ชม.จริง**: ปลอดภัยสำหรับ Survival แต่ epic event ต้องรอนาน (1 ปีเกม = 365 ชม.จริง)
- **1 วันเกม = 10 นาทีจริง**: 1 ปีเกม = ~60 ชม.จริง — kingdom สร้างได้ใน session ไม่กี่วัน
- เร็วกว่านี้: Survival กลายเป็น emergency ทุกนาที, Instinct Mode รับภาระไม่ไหว

## Decision
**1 วันเกม = 10 นาทีจริง**

Survival (Hunger) deplete ช้า — **7-10 วันเกม** (~70-100 นาทีจริง) จาก 100 ถึง 0 เพื่อให้ Survival เป็น background pressure ไม่ใช่ emergency ทุกชั่วโมง Character ที่อยู่ใน Instinct Mode สามารถอยู่รอดข้ามคืนได้ถ้า rule engine "กิน" ทำงาน

## Consequences
**ดี:**
- Epic event (สร้าง kingdom, ปฏิวัติ, สงคราม) เกิดขึ้นได้ใน real-world session ไม่กี่วัน
- 1 season (ถ้าใช้ระบบ season) ที่มีความยาว 90 วันเกม = 15 ชม.จริง — หรือถ้า season = 6 เดือนเกม ≈ 3 วันจริง ซึ่งอาจสั้นเกินไป ต้องกำหนด season length ใน Q10
- ให้ออกแบบ Survival parameters ที่รู้สึกได้ระยะยาว ไม่ใช่ micro-manage ทุก session

**เสีย/ความเสี่ยง:**
- Player ที่ offline 8 ชม. = 48 วันเกมผ่านไป — Instinct Mode ต้องทำงานได้จริง
- World Tick ที่ run ทุก 1 นาทีจริง = 0.1 วันเกมต่อ tick — ต้องเขียน Hunger decay rate ต่อ World Tick ให้ถูก (≈ 1-1.4% ต่อ tick ถ้า Hunger หมดใน 7-10 วันเกม = 70-100 tick)
- Season length ต้องนับในหน่วยวันเกมอย่างระวัง — ดู Q10
