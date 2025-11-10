---
title: Разбор api wildberries
draft: false
tags:
date:
Ссылка:
---
# Обзор отчетов и методов апи

Разбедем показания трех отчетов в интерфейсе. Найдем соответствующие каждому методы api. Проследим как данные «бъются друг с другом», чему можно верить, а чему нельзя. Рассмотрим отчеты:

- Аналитика продавца —> Воронка продаж
- Аналитика поиска —> Поисковые запросы: ваши товары
- Аналитика —> Портрет покупателя

Как найти в интерфейсе:
![[Pasted image 20251110142625.png]]
![[Pasted image 20251110142656.png]]
![[Pasted image 20251110143003.png]]
# Методы api
## Аналитика продавца —> Воронка продаж
Возвращает статистику трафика на карточке товара в общем. Не только из поиска, но и из рекомендаций и других мест.

### Статистика карточек товаров за период
https://seller-analytics-api.wildberries.ru/api/analytics/v3/sales-funnel/products
Возвращает данные в целом. Все, кроме показов.
![[Pasted image 20251110153059.png]]
[Пример кода](https://colab.research.google.com/drive/1Q9I0VnubJMs2V29tQYwDutnkylx9ZT7b#scrollTo=5ri3a7Jytslf&line=2&uniqifier=1)
### Статистика карточек товаров по дням
https://seller-analytics-api.wildberries.ru/api/analytics/v3/sales-funnel/products/history
Возвращает детализацию по дням, тоже кроме показов.
![[Pasted image 20251110153141.png]]
[Пример запроса](https://colab.research.google.com/drive/1Q9I0VnubJMs2V29tQYwDutnkylx9ZT7b#scrollTo=pRoB4VnkuPa-&line=1&uniqifier=1)
## Аналитика поиска —> Поисковые запросы: ваши товары
### Поисковая статистика по товару в общем
https://seller-analytics-api.wildberries.ru/api/v2/search-report/report
Возвращает статистику трафика на карточке товара, пришедшего из поиска.

![[Pasted image 20251110150747.png]]
[Пример запроса в Колабе](https://seller-analytics-api.wildberries.ru/api/v2/search-report/report)
### Кластеры поисковых запросов по товару
https://seller-analytics-api.wildberries.ru/api/v2/search-report/product/search-texts. Возвращает статистику по каждому поисковому запросу (обведено красным ниже). Это подмножество данных из предыдущего запроса.
![[Pasted image 20251110150502.png]][Пример запроса в Колабе](https://colab.research.google.com/drive/1Q9I0VnubJMs2V29tQYwDutnkylx9ZT7b#scrollTo=68z63anGkg8F)

# Аналитика —> Портрет покупателя
В отчете Аналитика — Портрет покупателя показаны ПОЛЬЗОВАТЕЛИ, а не переходы.

![[Pasted image 20251110154513.png]]

## Портрет покупателя — Точки входа

Данные совпадают с отчетом по поисковым запросам. Но в тексте опять ошибка. В отчете Портрет покупателя — Точки входа показаны КЛИКИ а не пользователи.

![[Pasted image 20251110155205.png]]

# Склеивание статистики
Мы хотим построить полную воронку по каждому кластеру ключевых запросов в рекламной компании, а так же определить нашу «долю рынка» в каждом запросе.

## Просмотры по кластеру ключевых фраз в рекламной компании
Аналитика продавца и Аналитика поиска НЕ возвращают просмотры по ключевой фразе через api, хотя в интерфейсе это указано. Получить просмотры можно методом
https://advert-api.wildberries.ru/adv/v0/normquery/stats

![[Pasted image 20251110162712.png]]

Возвращает:
- 'avg_pos'
- 'views'
- 'clicks'
- 'ctr'
- 'cpc'
- 'cpm'
- 'atbs' — добавлений в корзину
- 'orders'

## Аналитика поисковых запросов по товару
https://seller-analytics-api.wildberries.ru/api/v2/search-report/product/search-texts
В отличие от аналитики кластеров в компании показывает позицию и видимость.

Возвращает:
- 'frequency' — Ключевое. Мы считаем это «рынком» запроса
- 'weekFrequency'
- 'medianPosition'
- 'avgPosition'
- 'openCard'
- 'addToCart'
- 'openToCart'
- 'orders'
- 'cartToOrder'
- 'visibility' — Процент видимости товара в результатах поиска