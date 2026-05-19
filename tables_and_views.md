# 1. table analytics.orders_data
## Основная таблица с продажами
### Ключевые поля:
1. Ссылка_на_АП: ссылка на адресную программу, которую формируют менеджеры
2. Месяц_размещения
3. Город_размещения
4. side_id
5. Способ_показа: digital, статика, призма, скроллер
6. Формат: 6x3, 12x4 и т.д.
7. Оператор_из_экспорта
8. Отдел
9. Менеджер
10. Количество_спотов: сколько спотов продажи
11. Сумма_размещения_без_НДС: сколько денег получили
12. Тип_размещения: коммерческое, политическое, акционное, бонус и т.д. 
13. Статус: статус продажи (продано, в резерве, свободно)
14. Категория_РС: суперсайт, медиафасад, билборд (6х3) и т.д.
15. Сеть: сеть по РС
16. Бренд
17. Клиент
### Обновление в airflow [dag](https://af.bookingboard.ru/dags/refresh_orders_data_2026_every_2_hours/grid) с использованим процедуры из analytics.функции => refresh_orders_data_by_months
#### За таблицу отвечает Сергей Савельев, тг: @scvscvcv
---
# 2. mv analytics.sides_counting
## Здесь собран весь инвентарь со всеми статусами в конкретный месяц
## Основа как минимум для [[КС] Считалка](https://ss.outplan.ru/superset/dashboard/22/) и [[КС] Динамика показателей](https://ss.outplan.ru/superset/dashboard/71/)
### Ключевые поля:
1. month: месяц статуса
2. status: статус работы (планируется, работает, в ремонте и т.д.)
3. side_id: id стороны
4. cnt: кол-во повернхостей у стороны
5. city: город, где РК стоит
6. department: отдел, в чьей зоне ответственности находится РК, другими словами, кто должен продавать
7. sides_rim_gid: уникальный id РС 
8. rim_gid: уникальный id РП
9. side_type: способ показа РС
10. direction: направление РС
11. structure_category: категория РК
12. operator: юр. лицо, за кем закреплена РС
13. net: сеть, к которой относится РС (привязка идет именно к РС)
14. is_local: сеть, к которой относится оператор (привязка к оператору)
15. grp: рейтинг РС
16. ots: кол-во потенциальных контаков с рекламой
17. show_admetrix_stats: если True, то показывает данные grp, ots из admetrix, иначе показывает рассчитанные нами grp, ots
### Обновление в airflow [dag](https://af.bookingboard.ru/dags/refresh_sides_counting/grid), актуальный запрос на формирование mv: [sides_counting.sql](https://github.com/zherikrim-jpg/sql/blob/main/sides_counting.sql)
---
# 3. table public.planning_sides_data
## Основа для планирования analytics.planning_data_total
### Ключевые поля:
1. year_month: месяц
2. side_id: id стороны
3. region: регион
4. city: город
5. side_type: способ показа
6. side_size: 6x3, 12x4 и т.д.
7. operator: оператор
8. rim_gid: уникальный id РС
9. surfaces_count_at_end: количество РП на конец месяца
10. avg_cost_local: средняя цена за спот у локалов за 6 месяцев с year_month - 7 по year_month - 1. То есть для 2026-01 окно будет с 2025-07 по 2025-12
11. avg_cost_fra: аналогично для федералов
12. sum_cost_local: сумма продаж локалов за такой же период, как выше
13. sum_cost_fra: сумма продаж федералов за аналогичный период
14. sum_spots_local: кол-во проданных спотов локаалами за такой же период
15. sum_spots_fra: кол-во проданных спотов федералами, период такой же
16. locals_can_sell: флаг на то, могут ли РС продавать локалы
17. price: прайс за сторону в указанный месяц
18. placements_stardet_at: дата, когда РС заработало
19. fact_spots_local: кол-во проданных спотов локалами в указанный месяц
20. fact_spots_fra: кол-во проданных спотов федералами в указанный месяц
### Обновление на стороне разработчиков ДЦТ раз в день, ночью
#### Отвечает Артем Дащенко, тг: @ArKaNeMaN
---
# 4. mv analytics.planning_data_total
## Итоговая таблица с планами 
## Основа для дашбордов [[КС] Планирование](https://ss.outplan.ru/superset/dashboard/51/) и [[КС] Производный Аналитика](https://ss.outplan.ru/superset/dashboard/24/)
### Ключевые поля:
1. year_month: месяц
2. side_id: id стороны
3. region: регион
4. city: город
5. who_can_sell: аналог locals_can_sell из planning_sides_data
6. dsp_ssp_availability: есть ли подключение к dsp
7. type_ru: Тип РК (медиафасад, суперсайт, щит 6x3 и т.д.) - аналог категории РК, но чем они отличаются, хз
8. date_of_contract_end: дата окончания разрешения
9. placements_started_at: дата, когда РС заработало
10. working_days: сколько дней РС работала в месяце
11. coeff_working_days: коэффициент, который высчитывается как working_days / days_in_month
12. is_local: сеть, к которой относится оператор (привязка к оператору
13. is_permission_active: активно ли разрешение
14. side_type: способ показа
15. side_size: 6x3, 12x4 и т.д.
16. operator: оператор
17. side_category: категория РК (суперсайт, медиафасад, билборд (6х3) и т.д.)
18. fact_spots_local: кол-во проданных спотов локалами в указанный месяц
19. fact_spots_fra: кол-во проданных спотов федералами в указанный месяц
20. static_target_part_loc: таргетная доля по статике для локалов
21. digital_target_part_loc: таргетная доля по диджиталке для локалов
22. static_target_part_fra: таргетная доля по статике для федералов
23. digital_target_part_fra: таргетная доля по статике для федералов
24. surfaces_count_at_end: количество РП на конец месяца
25. month_coeff_loc: коэффициент сезонности для локалов
26. month_coeff_fra: коэффициент сезонности для федералов
27. new_type: тип сети, который проставляется в зависимости от net и is_local
28. occupancy_rate: процент заполняемости
29. avg_amount_loc: средняя цена за спот у локалов за 6 месяцев с year_month - 7 по year_month - 1. То есть для 2026-01 окно будет с 2025-07 по 2025-12
30. avg_amount_fra: средняя цена за спот у федералов за 6 месяцев с year_month - 7 по year_month - 1. То есть для 2026-01 окно будет с 2025-07 по 2025-12
31. oc_rate_ps_min_loc: минимально возможная заполняемость для локалов на сторонах партнера
32. oc_rate_ps_max_loc: максимально возможная заполняемость для локалов на сторонах партнера
33. oc_rate_ps_min_fra: минимально возможная заполняемость для федералов на сторонах партнера
34. oc_rate_ps_max_fra: максимально возможная заполняемость для федералов на сторонах партнера
35. ac_factor_per: коэффициент разгона для партнерских сторон
36. coef_opacity: плановый коэффициент заполняемости для сторон собственной сети
37. coef_opacity_loc_partner: плановый коэффициент заполняемости локалов для сторон партнера
38. coef_opacity_fra_partner: плановый коэффициент заполняемости федералов для сторон партнера
39. max_revenue_loc: максимальная выручка локалов
40. max_revenue_fra: максимальная выручка фра
41. max_revenue:fra_pb: максимальная выручка фра прямые бренды
42. max_revenue_super_f: максимальная выручка суперформат
43. custom_plan_coeff_lop: корректировачный коэффициент для локалов
44. custom_plan_coeff_fra: корректировачный коэффициент для фра
45. custom_plan_coeff_fra_pb: корректировачный коэффициент для фра прямые бренды
46. custom_plan_coeff_super_f: корректировачный коэффициент для суперформат
47. coeff_plan_fra: коэффициент фра
48. coeff_plan_pb: коэффициент фра прямые бренды
49. coeff_plan_dsp: коэффициент фра dsp
50. plan_loc: план локалов
51. plan_fra: план фра
52. plan_fra_pb: план фра прямые бренды
53. plan_dsp: план фра dsp
54. plan_super_f: план суперформат
### Обновление в airflow [dag](https://af.bookingboard.ru/dags/refresh_planning_data_total_full_pipeline/grid), актуальный запрос для mv [planning.sql](https://github.com/zherikrim-jpg/sql/blob/main/planning.sql)
#### В этом же даге высчитывается кол-во рабочих дней РС для таблицы analytics.working_days_sides_count
---
# 5. mv analytics.lop_inventory
## Основа для дашборда [[КС] Инвентарь лоп СС](https://ss.outplan.ru/superset/dashboard/83/)
### Ключевые поля:
1. year_month: месяц
2. region: регион
3. city: город
4. department_lop: название локального отдела
5. side_type: способ показа
6. format: 6x3, 12x4 и т.д.
7. structure_category: категория РК (медиафасад, суперсайт, билборд (6х3) и т.д.)
8. avg_price: средний прайс
9. surfaces_count_at_end: кол-во спотов на конец месяц
10. avg_amount_loc: средняя цена за спот у локалов
11. sales_amount: сумма продаж
12. spots_comm: кол-во проданных спотов с коммерческим типом размещения
13. spots_pol: кол-во проданных спотов с политическим типом размещения
14. spots_soc: кол-во проданных спотов с типом размещения НЕ за деньги
15. max_rev: максимальная выручка
16. count_inventory: кол-во инвентаря, который доступен к продаже (умножен на 0.9)
17. perc_comm_occ: процент коммерческой заполняемости
18. 18. target_part: таргетная доля
### Обновление в airflow [dag](https://af.bookingboard.ru/dags/refresh_lop_inventory/grid)
---
# 6. mv test_fact_coeff_rk
## Таблица с коэффициентами для расчета выручки с коэффициентом 
## В дашбордах [[КС] Тест факт. ЛОП](https://ss.outplan.ru/superset/dashboard/48/) и [[КС] Тест факт. ФРА](https://ss.outplan.ru/superset/dashboard/62/)
### Ключевые поля
1. side_id: id стороны
2. year_month: месяц
3. type_ru: тип РК (аналог категории РК)
4. coeff_working_months: коэффициент за кол-во рабочих месяцев
5. check_prev3_sales: коэффициент за отсутствие продаж за последние 3 месяца
6. coeff_mf: коэффициент за медиафасад
### Обновление в airflow [dag](https://af.bookingboard.ru/dags/refresh_test_fact_coeff_rk/grid), актуальный запрос на mv [test_fact_coeff_rk.sql](https://github.com/zherikrim-jpg/sql/blob/main/test_fact_coeff_rk.sql)
---
# 7. table analytics.crm_deals
## Данные по сделкам из Битрикс24
## Используется для расчета кол-ва новых клиентов в mv analytics.new_clients_viab24
Ключевые поля:
1. deal_id: id сделки
2. deal_create_date: дата создания сделки
3. company_title: название клиента
4. created_by: кто создал сделку
5. modify_by: кем изменено
6. assigned_by: кому назначено
### Обновление через запуск [deal_to_pg.py](https://github.com/zherikrim-jpg/python/blob/main/deals_to_pg.py), нужен будет id_rsa
---
# 8. mv analytics.new_clients_viab24
## Таблица с метками новых клиентов
## Используется в дашборде [[КС] Тест факт. ЛОП](https://ss.outplan.ru/superset/dashboard/48/)
Ключевые поля:
1. year_month: месяц
2. manager: менеджер
3. client: имя клиента
4. check_source: источник клиента, если есть в битриксе, то 1, если нет, то подставляется клиент из ЕРП
5. deals: ссылки на сделки
6. first_placement_date: первое размещение клиента
7. prev1_date: месяц предыдущего месяца размещения от месяца в строке
8. prev2_date: месяц препредыдущего месяца размещения от месяца в строке
9. prev3_date: месяц препрепредыдущего месяца размещения от месяца в строке
10. is_new_client: является ли клиент новым для текущего месяца year_month
### Обновление в airflow [dag](https://af.bookingboard.ru/dags/refresh_new_clients/grid)
---
# 9. table analytics.unit_economics
## Таблица с расходами на РК
## Используется в дашборде [[КС] Юнит-экономика](https://ss.outplan.ru/superset/dashboard/69/)
Ключевые поля:
1. year_month: месяц
2. status: статус работы
3. side_id: id стороны
4. city: город
5. rim_gid: уникальный id РП
6. side_type: способ показа
7. format: 6x3, 12x4 и т.д.
8. rent_cost: плата за аренду
9. electricity: плата за электричество
10. leasing: плата за лизинг
11. renta_profit: доход от аренды
### Обновление через запуск [unit_econom.py](https://github.com/zherikrim-jpg/python/blob/main/unit_econom.py)
---
# 10. mv analytics.status_resets
## Данные по снятиям
## Основа для дашборда [[КС] База снятий](https://ss.outplan.ru/superset/dashboard/96/)
Ключевые поля:
1. year_month_reset: месяц снятия
2. year_month_placement: месяц размещения
3. city: город
4. side_id: id стороны
5. rim_gid: уникальный id РС
6. side_type: способ показа
7. structure_category: категория РК
8. manager: менеджер
9. new_type: тип сети, который вычисляется из net и is_local
10. first_cost: стоимость первоначального размещения
11. cost: итоговая сумма размещения
12. base_price: базовый прайс
13. status_reset_reason: причина снятия
14. department: отдел
### Обновление в airflow [dag](https://af.bookingboard.ru/dags/refresh_status_resets/grid), актуальный запрос для mv [status_resets.sql](https://github.com/zherikrim-jpg/sql/blob/main/status_resets.sql)
---
