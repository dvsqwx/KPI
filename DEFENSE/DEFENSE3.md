DEFENSE — Завдання 3: Взаємодія (Sequence)

Намір і критерії (1–2 речення):


Топ-3 розбіжності (spec ↔ артефакт) + коміт-виправлення:
•У варіанті AI прописав вибір room_id у форму бронювання пацієнта (Select doctor, date and time slot, room_id). В реальнiй ситуацiï такого не буває, так як клiєнт обирає тiльки стоматолога. Прибрав room_id із клієнтськоï частини та переніс визначення кабінету на рівень сервера через метод findAssignedRoom(doctor_id, date_time) перед перевіркою конфліктів. 
комiт: remove room_id from client side and match with sequence diagram
remove room_id from client side
https://github.com/dvsqwx/KPI/commit/a68f979c99a288805e4dcffc192c0b9db7c2cf3d
https://github.com/dvsqwx/KPI/commit/dd52ca12f56ac292bee5490716cea5f740d0b73c
•помилка порушення послідовності вибору слота. У версії ai пацієнт міг обрати часовий слот до того, як обрав послуги. Це нелогічно для стоматологічної клініки, оскільки тривалість візиту залежить від набору процедур, і система не знає, яке часове вікно потрібне. Переніс вибір слота після блоку вибору послуг і додав запит доступних вікон під отриману тривалість.
комiт: change booking flow + calculate duration before slot selection
hange booking flow and fix Gherkin US-01 (випадково пропустив c в словi 'change')
https://github.com/dvsqwx/KPI/commit/3d628be6ba2dbd8383f66bad46a663e40b581248
https://github.com/dvsqwx/KPI/commit/b8be1b4cb8c5d86bdc576fcef540a15d3e6453e6
• 

Ключове рішення — які альтернативи зважив і чому обрав цю (ADR):


Перевірка — узгодженість із попередньою моделлю (як перевірив):


Здача: https://github.com/dvsqwx/KPI
