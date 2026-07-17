# Lab 3 — TDD บนระบบ Password Validation

ปฏิบัติการ 2 ชั่วโมง · เขียน test runner ด้วยมือ **ไม่ใช้เฟรมเวิร์ก**

---

## เป้าหมายของ Lab

ฝึกกระบวนการ Test-Driven Development เต็มวงจร กับฟังก์ชันตรวจสอบรหัสผ่าน โดย
**ออกแบบ test case เอง** ด้วยเทคนิคที่เรียนในคาบ แล้วเขียน test runner ด้วยมือ

---

## SPEC ของ `validate(String pw)`

คืนค่า `true` ก็ต่อเมื่อรหัสผ่านผ่าน **ทุกเงื่อนไข** ต่อไปนี้:

| # | เงื่อนไข | ถ้าไม่ผ่าน |
|----|-----------|-----------|
| R1 | `pw` ต้องไม่เป็น `null` | **throw** `IllegalArgumentException` |
| R2 | ความยาว `>= 8` และ `<= 20` (inclusive) | คืน `false` |
| R3 | มีตัวพิมพ์ใหญ่ `A-Z` อย่างน้อย 1 ตัว | คืน `false` |
| R4 | มีตัวพิมพ์เล็ก `a-z` อย่างน้อย 1 ตัว | คืน `false` |
| R5 | มีตัวเลข `0-9` อย่างน้อย 1 ตัว | คืน `false` |
| R6 | ต้องไม่มีช่องว่าง (space) เลย | คืน `false` |

> R1 เป็น input ภายนอก → ตรวจด้วย **exception** เสมอ
> R2–R6 ถ้าผิดข้อใดข้อหนึ่ง → คืน `false` (ไม่ throw)

---

## สิ่งที่ต้องทำ

### 1. ออกแบบ test case 

`src/TestRunner.java` เตรียม helper `check()` และโครง `main()` ไว้ให้แล้ว
พร้อมตัวอย่าง 2 ตัว (assertion ปกติ 1 + แพตเทิร์น try/catch สำหรับ "ต้อง throw" 1)

**หน้าที่ของคุณคือเติม `check(...)` ให้ครบทุกกรณี** โดยใช้:

- **Equivalence partitioning** — แบ่ง input เป็นกลุ่มที่ควรให้ผลเหมือนกัน
  (เช่น valid / สั้นไป / ยาวไป / ไม่มีตัวใหญ่ / ไม่มีตัวเล็ก / ไม่มีเลข / มีช่องว่าง)
  แล้วทดสอบ **ตัวแทนกลุ่มละ 1 ตัว**
- **Boundary value analysis** — ทดสอบค่าตรงรอยต่อของช่วงความยาว `8..20`
  (ขอบล่าง, ขอบล่าง−1, ขอบบน, ขอบบน+1)

> เกณฑ์: test ที่คุณออกแบบต้องครอบ **ทุก branch** ของ implementation (branch coverage ≥ 80%)

### 2. เขียน test ก่อน → รัน (RED)

รัน `TestRunner` โดยที่ `validate()` ยังว่าง → ต้องเห็น `[FAIL]`
นี่คือการพิสูจน์ว่า test ทำงานจริง ไม่ได้ผ่านฟรีๆ

### 3. Implement ให้ผ่าน (GREEN)

เขียนโค้ดใน `starter/PasswordValidator.java` ตาม spec จน test ทุกตัวขึ้น `[PASS]`

### 4. วัด coverage + Refactor

ทำ branch checklist ด้วยมือ (แต่ละ `if` = 2 ทาง) ให้ ≥ 80%
แล้ว refactor โค้ดให้สะอาดขึ้น โดยรัน `TestRunner` ก่อน–หลังทุกครั้ง ต้องได้ผลเท่าเดิม

---

## วิธี compile และรัน

**ตั้งค่าใน VSCode** ให้เปิด `-ea` อัตโนมัติ — เพิ่มใน `.vscode/settings.json`:

```json
{
  "java.debug.settings.vmArgs": "-ea"
}
```

---

## หน้าตาผลลัพธ์เมื่อรัน

```
== Password Validation ==
  [PASS] 'Abcdef12' valid
  [PASS] null -> throws IllegalArgumentException
  ...
==================================
PASS 12 / FAIL 0
==================================
```

เห็น `[FAIL]` ที่ไหน → ไปแก้ฟังก์ชันนั้น แล้วรันใหม่จน `[PASS]` ทั้งหมด (loop กลับไป Green)

---

