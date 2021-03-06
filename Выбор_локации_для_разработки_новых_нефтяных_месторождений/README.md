# Выбор локации для скважины

## Постановка задачи
Нам предоставлена информация о пробах нефти в трёх регионах: в каждом 10 000 месторождений, где измерили качество нефти и объём её запасов. Построим модель машинного обучения, которая поможет определить регион, где добыча принесёт наибольшую прибыль. Проанализируем возможную прибыль и риски техникой *Bootstrap.*

Описание данных:
* *id* — уникальный идентификатор скважины;
* *f0*, *f1*, *f2* — три признака точек (неизвестно, что они означают, но сами признаки значимы);
* *product* — объём запасов в скважине (тыс. баррелей).

Детали, из которых исходим:

* Для обучения модели подходит только линейная регрессия (остальные — недостаточно предсказуемые).
* При разведке региона исследуют 500 точек, из которых с помощью машинного обучения выбирают 200 лучших для разработки.
* Бюджет на разработку скважин в регионе — 10 млрд рублей.
* При нынешних ценах один баррель сырья приносит 450 рублей дохода. Доход с каждой единицы продукта составляет 450 тыс. рублей, поскольку объём указан в тысячах баррелей.
* После оценки рисков оставим лишь те регионы, в которых вероятность убытков меньше 2.5%. Среди них выберем регион с наибольшей средней прибылью.

---
## Что сделано
1. Провели предобработку данных: нашли и удалили дубликаты.
2. Изучили распределения признаков. Обнаружили, что некоторые из них имеют экзотический вид, что возможно указывает на то, что нам предоставлены синтетические данные.
3. Построены диаграммы рассеяния и матрицы корреляции. Один из признаков сильно скоррелирован с целевым признаком. Проблем с мультиколлинеарностью не обнаружили.
4. Выделили 1/4 выборок для тестирования качества моделей, обучили линейные регрессии на 3/4 выборок, оценили качество предсказания: в одном регионе (1) более низкий средний запас, но хорошее качество предсказания, в двух других (0 и 2) - предсказанные запасы в полтора раза больше, но крайне низкое качество предсказаний.
5. Оцениваем возможную прибыль и риски убытков, имитирую процесс геологоразведки: случайным образом выбираем подвыборку из 500 скважин, строим прогноз запасов, выбираем 200 лучших скважин и считаем прибыль с учетом затрат на разработку. Повторяем процесс 1000 раз для каждого региона и анализируем полученные распределения прибыли.
6. Вероятность убытков по регионам с плохим качеством предсказаний (0 и 2) оказалась ниже порогового значения в 2.5%, в регионе с более низким средним запасом, но хорошим качеством предсказаний (1), ожидаемая прибыль оказалась наибольшей, а риск убытков оказался приемлемым (0.9%). 
7. Вероятно, использование нелинейных моделей или другого набора признаков для построения предсказаний могло бы улучшить качество прогнозирования запасов и, соответственно, целевые бизнес-метрики.
