# ADR-0004: Instinct Mode — Fallback เมื่อ Token หมด

## Status
Proposed

## Context
BYOK แปลว่า Agent จะหยุดทำงานเมื่อ Player หมด Token Budget, API ล่ม, หรือ key หมดอายุ ถ้า Character "แข็งค้าง" หรือตายทันทีเมื่อ token หมด เกมจะลงโทษคนงบน้อยรุนแรงเกินไป และทำให้โลก persistent พังเมื่อหลายคน offline พร้อมกัน

## Decision
เมื่อ Agent ใช้งานไม่ได้ Character เข้า **Instinct Mode** — รันด้วย **rule-based fallback** แทน:
กิน (ถ้า Hunger ต่ำ) → นอน/พัก (ถ้า Stamina ต่ำ) → ทำงาน routine ล่าสุดต่อ → หนีเมื่อถูกโจมตี
Character **ไม่ตาย**เพราะ token หมด แต่จะไม่ทำ strategic action ใหม่ๆ

## Consequences
**ดี:** คนงบน้อยเล่นได้จริง (ตัวละครอยู่รอด), โลกไม่พังตอนคน offline, ความได้เปรียบของคนจ่ายมาก = "ตัดสินใจเชิงกลยุทธ์ได้" ไม่ใช่ "มีชีวิตอยู่ได้" → fair และโปร่งใส
**เสีย:** ต้องเขียน rule engine fallback ที่ "พอใช้ได้" สำหรับทุก career path, อาจมีคนเล่นโดยพึ่ง Instinct Mode ล้วนเพื่อประหยัด (ต้องดูว่าเป็นปัญหา balance หรือเป็น playstyle ที่ยอมรับได้ — ดู OPEN-QUESTIONS)
