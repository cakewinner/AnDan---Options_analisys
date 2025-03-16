# Проект по проверке теоретической стоимости опционов

## Команда проекта
* Бахметьев Кирилл
* Кириенко Тимур
* Пономарева Анна

## Источники данных
https://www.cboe.com/us/options/market_statistics/flex_trade_reports/ \
https://ru.tradingview.com/

## Структура файлов
В файле options мы работаем с данными по опционам \
В файле indexes мы работаем с данными по индексам \
В следующей итерации мы смерджим обе обработанные таблички и будем работать с прогнозированием

## План
Шаг 1. Сбор и мердж данных \
Шаг 2. Предварительная обработка \
Шаг 3. Визуализация и создание новых признаков \
Шаг 4. Проверка гипотез о расхождении теоретической и рыночной стоимости опционов \
Шаг 5. Машинное обучение? \
Шаг 6. Выводы

## Предпологаемые гипотезы
* Расхождение между теоретической стоимостью опционов (по формуле Блэка-Шоулза) и рыночной в момент покупки <1%
* Используя данные по опционам возможно спрогнозировать значение индекса на момент покупки опциона
* Данные по экспирации опционов помогут в прогнозировании временных рядов индексов

## Что мы уже сдалали?

1. Мы сформировали датафрейм из данных по динамике цен опционов по
минутам за февраль 2025 года
2. Провели EDA для полученного датафрейма
3. Выделили 3 ключевых опциона на индексасы с наибольшими объемами торгов, по которым мы хотим сделать более глубокий анализ и сравнение с акциями
4. Сформировали датафрейм с ценами на индексы по минутам за февраль 2025
5. Вычислили логарифмическую доходность по дням для каждого индекса
6. Вычислили минутную волатильность цен для каждого индекса
7. Рассчитали время до даты эксперации (в дальнейшем это поможет нам при проверке гипотез)
8. Рассчитали объемы сделок по опционам
9. В целом разобрались, как устроены наши данные


## Приложение
### Формула Блэка-Шоулза для оценки опциона:

$$
C(S, t) = S \cdot N(d_1) - K \cdot e^{-r(T - t)} \cdot N(d_2)
$$

$$
P(S, t) = K \cdot e^{-r(T - t)} \cdot N(-d_2) - S \cdot N(-d_1)
$$

Где:

$$
d_1 = \frac{\ln\left(\frac{S}{K}\right) + \left(r + \frac{\sigma^2}{2}\right)(T - t)}{\sigma \sqrt{T - t}}
$$

$$
d_2 = d_1 - \sigma \sqrt{T - t}
$$

| «Грек» | Опцион call | Опцион put |
|--------|---------------|--------------|
| ДЕЛЬТА | $\(N(d_1)\)$ | $\(-N(-d_1) = N(d_1) - 1\)$ |
| ГАММА | $\(\frac{N'(d_1)}{S \sigma \sqrt{T}}\)$  | $\(\frac{N'(d_1)}{S \sigma \sqrt{T}}\)$ |
| ВЕГА | $\(S N'(d_1) \sqrt{T}\)$  | $\(S N'(d_1) \sqrt{T}\)$ |
| ТЕТА | $\(-\frac{S N'(d_1) \sigma}{2 \sqrt{T}} - rKe^{-r(T)} N(d_2)\)$ | $\(-\frac{S N'(d_1) \sigma}{2 \sqrt{T}} + rKe^{-r(T)} N(-d_2)\)$ |

Обозначения:
- $C(S, t)$ — цена европейского опциона колл;
- $P(S, t)$ — цена европейского опциона пут;
- $S$ — текущая цена базового актива;
- $K$ или $X$ — страйк-цена опциона;
- $T$ — время до экспирации опциона;
- $t$ — текущее время;
- $r$ — безрисковая процентная ставка;
- $\sigma$ — волатильность базового актива;
- $N(\cdot)$ — функция стандартного нормального распределения.


