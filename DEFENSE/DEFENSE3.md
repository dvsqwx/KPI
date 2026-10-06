DEFENSE — Завдання 3: Взаємодія (Sequence)

Намір і критерії (1–2 речення):


Топ-3 розбіжності (spec ↔ артефакт) + коміт-виправлення:
•У варіанті AI прописав вибір room_id у форму бронювання пацієнта (Select doctor, date and time slot, room_id). В реальнiй ситуацiï такого не буває, так як клiєнт обирає тiльки стоматолога. Прибрав room_id із клієнтськоï частини та переніс визначення кабінету на рівень сервера через метод findAssignedRoom(doctor_id, date_time) перед перевіркою конфліктів. 
https://github.com/dvsqwx/KPI/commit/a68f979c99a288805e4dcffc192c0b9db7c2cf3d
https://github.com/dvsqwx/KPI/commit/dd52ca12f56ac292bee5490716cea5f740d0b73c
•
•

Ключове рішення — які альтернативи зважив і чому обрав цю (ADR):


Перевірка — узгодженість із попередньою моделлю (як перевірив):


Здача: https://github.com/dvsqwx/KPI
