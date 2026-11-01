### DEFENSE — Завдання 4: Бізнес-процеси (Activity Diagram)

**1. Що я просив у AI**
Зробити діаграму активності (`Mermaid flowchart`) у файлі `activity-diagram.mmd` для процесу запису пацієнта на прийом (`UC1` разом із входом `UC0` та SMS у `UC10`) за розділом 11 `spec.md`, а також написати `ADR-0003` про вибір нотації. Основні вимоги: 5 доріжок для ролей, цикл вибору послуг із підрахунком `total_duration`, перевірки `if/else` (вільні слоти, JWT-токен, конфлікт лікаря чи кабінету в транзакції БД) і паралельні гілки `fork / join` після збереження запису. Промпти: https://github.com/dvsqwx/KPI/blob/main/ai/lab4/activity_diagram_prompt.md.

**2. Топ-3 розбіжності, які я знайшов і виправив** (spec ↔ вихід AI):

| # | Що було не так (проти якого пункту spec) | Мій слід (промпт / фідбек / дод. spec) | Виправлення (коміт) |
| - | ---------------------------------------- | -------------------------------------- | ------------------- |
| 1 | Під час транзакції AI поставив ромб перевірки конфліктів `D6{"Doctor or room conflict?"}` у доріжку `Database`, ніби база сама вирішує робити `ROLLBACK` чи зберігати запис, а крок `B6` (пошук пацієнта й послуг) взагалі не звертався до бази (проти розділів 9 і 11 у `spec.md`). | Дописав у `spec.md` (розділ 11, п. 4 і критерії прийняття), що `Database` виконує тільки запити і команди транзакції (`BEGIN`, `COMMIT`, `ROLLBACK`), виніс читання даних у доріжку БД і переніс ромб перевірки конфлікту в `Booking Service` (після генерації за `ai/lab4/activity_diagram_prompt.md`). | https://github.com/dvsqwx/KPI/commit/ed23e2264f972d960d417676e1e87f7133ca2c41 |
| 2 | Після помилки `409 Conflict` (`W10`) стрілка повертала пацієнта назад на вибір дати (`P4`) замість вибору іншого часу зі списку (`P5`), а при помилці `401 Unauthorized` на сторінці входу (`UC0`) не було виходу до `End`, якщо користувач передумав входити (виходив нескінченний цикл). | Уточнив у `spec.md` (розділ 11, п. 3), що при відмові від входу процес завершується, змінив блок входу на перевірку `P6{"Sign in (UC0)?"}` зі стрілкою `cancel -> End` і додав перехід із `W10` одразу на вибір іншого слота (`P5`). | https://github.com/dvsqwx/KPI/commit/97c4730593e10bfce01a31edde1e3ffa256b32bf |
| 3 | У паралельній гілці `UC10` після `Fork` при статусі `SENT` (`B10`) стрілка одразу йшла в `Join` усередині `Booking Service` і не показувала повідомлення про SMS у `Web App` (проти `REQ-09` і Сценарію 1 у `sequence-diagram.mmd`), а кінець процесу `End` стояв у доріжці `Booking Service` замість `Patient`. | Доповнив `spec.md` (розділ 11, п. 4 і 5) про показ повідомлення в `Web App` та порядок `BEGIN`, додав у доріжку `Web App` блок `W12["Show message: SMS confirmation sent"]` перед `Join` і переніс `End` у доріжку `Patient`. | https://github.com/dvsqwx/KPI/commit/0ab06d34fd51df49d3896b48ea9e30c40c65ee9d |

**3. Ключове рішення:**
Обрав для опису процесу Activity-діаграму з доріжками ролей (`Mermaid flowchart` із `subgraph`), а не машину станів (`stateDiagram-v2`) чи звичайну схему без ролей (`adr/0003-activity-flowchart-process-modeling.md`). У записі беруть участь 5 учасників (`Patient`, `Web App`, `Booking Service`, `Database`, `SMS Gateway`), тому на одній схемі зручніше показати і послідовність кроків із циклами та паралельними гілками `fork / join`, і те, яка саме роль відповідає за кожну дію.

**4. Перевірка:**
- Звірив кроки, назви методів (`getAvailableSlots`, `findAvailableRoom`, `findConflictingAppointments`, `saveAppointment`, `sendSms` тощо) і полів (`total_duration`, `duration_minutes`, `actual_price`, `status = scheduled`) у `activity-diagram.mmd` зі `spec.md`, `er-diagram.mmd`, `use-case-diagram.mmd` (`UC0`, `UC1`, `UC10`) та 4 сценаріями у `sequence-diagram.mmd`.
- Перевірив саму схему: до кожного блоку можна дійти від початку `Start`, обидві гілки після `Fork` сходяться в `Join`, немає тупикових кроків без виходу, а код відкривається в Mermaid без помилок.

**Здача:**
(а) посилання на GitHub PR: https://github.com/dvsqwx/KPI
(б) цей заповнений DEFENSE.