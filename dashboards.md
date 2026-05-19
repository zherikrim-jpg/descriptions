# 1. [[КС] Динамика показателей](https://ss.outplan.ru/superset/dashboard/71/).
### Описание:
Аналитика в разрезе инвентаря год к году. 
### Таблицы в дашборде:
1. analytics.orders_data
2. analytics.sides_counting
3. analytics.lop_cities
---
# 2. [[КС] Производный аналитика](https://ss.outplan.ru/superset/dashboard/24/).
### Описание:
Аналитика по выручке и выполнению планов по отделам.
### Таблицы в дашборде:
1. analytics.orders_data
2. analytics.planning_data_total
3. analytics.lop_cities
4. analytics.lop_sales
---
# 3. [[КС] Планирование](https://ss.outplan.ru/superset/dashboard/51/).
### Описание:
Данные по планам в различных разрезах
### Таблицы в дашборде:
1. analytics.planning_data_total
---
# 4. [[КС] Планирование 4090](https://ss.outplan.ru/superset/dashboard/93/).
### Описание:
То же самое планирование, только в разных вариантах заполняемости: от 40 до 90 и на 27-ой год.
Скорее всего одноразовый дашборд.
### Таблицы в дашборде:
1. analytics.planning_data_total_X, где X от 40 до 90 с шагом в 5.
---
# 5. [[КС] Тест факт. ЛОП](https://ss.outplan.ru/superset/dashboard/48/)
### Описание:
Аналитика выручки, выполнения планов отделами / менеджерами, аналитика по выручке новых/старых клиентов и сроку их жизни + косяки в заполнении сделок в АП и сделок в Б24.
### Таблицы в дашборде:
1. analytics.orders_data
2. analytics.lop_cities
3. analytics.crm_deals
4. analytics.new_clients_viab24
5. analytics.test_fact_coeff_rk
6. analytics.hr_employees
7. analytics.sales_manager_plans
8. analytics.koef_motivation
9. analytics.dep_plans
10. analytics.planning_data_total
11. analytics.deals_errors_erp
12. analytics.deals_errors_b24
---
# 6. [[КС] Тест факт. ФРА](https://ss.outplan.ru/superset/dashboard/62/)
### Описание:
Аналитика выручки, выполнения планов отделами, менеджерами, аналитика по выручке клиентов.
### Таблицы в дашборде:
1. analytics.orders_data
2. analytics.lop_cities
3. analytics.crm_deals
4. analytics.new_clients_viab24
5. analytics.test_fact_coeff_rk
6. analytics.hr_employees
7. analytics.sales_manager_plans
8. analytics.koef_motivation
9. analytics.dep_plans
10. analytics.planning_data_total
---
# 7. [[КС] Считалка](https://ss.outplan.ru/superset/dashboard/22/).
### Описание:
Данные по рабочему / планируемому рекламному инвентарю, доли по сети и по стране.
### Таблицы в дашборде:
1. analytics.sides_counting
2. analytics.admetrix_data
---
# 8. [[КС] PR по рекламным агентствам](https://ss.outplan.ru/superset/dashboard/89/).
### Описание:
Данные по количеству инвентаря в городах глобально, сравнение того, какую долю РА занимают в городе с тем, какую долю они занимают у нас.
### Таблицы в запросе:
1. analytics.pr
---
# 9. [[КС] Инвернтарь ЛОП СС](https://ss.outplan.ru/superset/dashboard/83/).
### Описание:
Похож на динамику показателей, только без YoY и с потерянной прибылью
### Таблицы в запросе:
1. analytics.lop_inventory
---
# 10. [[КС] Юнит-экономика](https://ss.outplan.ru/superset/dashboard/69/).
### Описание:
Данные по эффективности РК и РС в отдельности
### Таблицы в запросе:
1. analytics.orders_data
2. sides_counting
3. unit_economics
