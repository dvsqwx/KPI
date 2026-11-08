# DEFENSE — Завдання 5: Компонентна діаграма (Component diagram)

**1. Що я просив у AI**
Згенерувати за розділом 12 spec.md компонентну діаграму в component-diagram.mmd (5 рівнів, інтерфейси, межа системи, SMS Gateway) і файл adr/0004-layered-component-architecture.md. Промпти: ai/lab5/component_diagram_prompt.md.

**2. Топ-3 розбіжності, які я знайшов і виправив** (spec ↔ вихід AI):

| # | Що було не так (проти якого пункту spec) | Мій слід (промпт / фідбек / дод. spec) | Виправлення (коміт) |
| - | ---------------------------------------- | -------------------------------------- | ------------------- |
| 1 | AI пропустив вхід UC0 (REQ-20), переніс validateJwt у контролер замість Booking Service (проти розділів 9 і 11 spec.md) і додав зайвий TransactionManager. | ai/lab5/component_diagram_prompt.md · правка розділу 12 у spec.md | 
https://github.com/dvsqwx/KPI/commit/8327ec69667b11f4f57f6a7915ca52327c341dfe
https://github.com/dvsqwx/KPI/commit/218d361f550c56f620df05f9efc92bf71dda3761
https://github.com/dvsqwx/KPI/commit/b64e4637adfd32b236747d24c72d68437f602bfe |
| 2 | Не було показано зв'язок M:M між DOCTOR і SPECIALTY (проти розділу 3 spec.md і REQ-11) та пропущено UC12a–UC12c у Catalog Service. | ai/lab5/component_diagram_prompt.md · доповнення розділу 12 у spec.md | https://github.com/dvsqwx/KPI/commit/99d532421eef2205382cd313869ee3626b45d4ff |
| 3 | У Billing Service пропущено вимогу REQ-14 (прийоми без рахунка через 24 год, розділ 6 spec.md) і не вказано частину кодів REQ та NFR у сервісах. | ai/lab5/component_diagram_prompt.md · звірка з розділами 6 і 7 spec.md | https://github.com/dvsqwx/KPI/commit/8b2ec136eb4e04618109a28d56561ac5ef504449
https://github.com/dvsqwx/KPI/commit/55c7302772035a95e719be1e3577374bcb22c6d1 |

**3. Ключове рішення:** Розділити бекенд на три сервіси (Booking, Catalog, Billing) і винести спільні репозиторії Data Access перед PostgreSQL — тому що логіка запису, довідників і рахунків змінюється окремо, а спільні ServiceRepository і RoomRepository прибирають дублювання SQL-запитів (adr/0004-layered-component-architecture.md).

**4. Перевірка:**
- Перевірено рендер component-diagram.mmd через Mermaid у VS Code.
- Звірено узгодженість зі spec.md і попередніми діаграмами: 7 репозиторіїв покривають усі 8 сутностей з er-diagram.mmd, сервіси охоплюють усі UC0–UC12c, REQ та NFR, а виклики методів і транзакція BEGIN / COMMIT / ROLLBACK збігаються з sequence-diagram.mmd та activity-diagram.mmd.