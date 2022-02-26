<h3>Какую задачу решают в статье</h3>
Авторы решают проблему построения предсказательных интервалов для RNN-моделей в случае multi-horizon предсказания временных рядов. Предполагается, что имеется много временных рядов похожей природы.

В статье удается при определенных условиях на датасет создать процедуру определения предсказательных интервалов, которая

* Не требует модификации архитектуры уже существующей RNN-модели
* Относительно проста для вычисления
* Имеет теоретическое обоснование
<h3>Архитектура модели</h3>
Авторы адаптируют уже существующий ICP (inductive conformal prediction) подход для построения интервалов. Применить его как есть не получится, так как он предполагает взаимозаменяемость всех точек датасета, а в случае со временными рядами, очевидно, точки из разных временных отрезков не взаимозаменяемы.

Предлагается использовать взаимозаменяемость точек одного горизонта из разных временных рядов:

![image](https://user-images.githubusercontent.com/48079881/155844276-2fd208b6-94b8-432e-87e4-444d1d3e8b43.png)


Таким образом, мы явно предполагаем, что временные ряды взаимозаменяемы для точного теоретического обоснования подхода.

Прежде чем описать итоговый алгоритм стоит заметить, что при выполнении прогноза на горизонт H используется не авторегрессионный подход. Из эмбеддинга сразу генерируется вектора размера H. Как авторы обосновывают свой выбор:

* Используемая процедура более устойчива к накоплению ошибок.
* Условная независимость предсказаний при условии состояния модели. Это требуется для теоретического обоснования корректности процедуры.


Перед применением алгоритма часть рядов отводится на обучение, а другая часть на калибровку. Можно также заметить, что значения для построения интервалов – это просто константы для каждого значения горизонта.

Опишем итоговый алгоритм:

![image](https://user-images.githubusercontent.com/48079881/155844304-8c4af7f9-fd83-418b-99ff-88c617b4b689.png)


Общая схема:
![image](https://user-images.githubusercontent.com/48079881/155844310-c3fe17e5-710d-409f-b4ff-5c0c61f1c1ed.png)


<h3>На каких датасетах валидируют</h3>
Авторы рассматривают два типа данных:

* Синтетические данные: авторегрессия с шумом
  - шум имеет фиксированную дисперсию
  - шум имеет дисперсию, линейно растущую с номером точки
* Реальные данные
  - MIMIC-III: Medical Information Mart for Intensive Care
  - EEG: electroencephalography dataset
  - COVID-19: daily number of cases within United Kingdom local authority districts


В синтетических данных использовался горизонт H=5, длина обучающей части T=15.

Характеристики реальных датасетов:

![image](https://user-images.githubusercontent.com/48079881/155844379-7733ca54-7146-479d-838e-21fc1767653c.png)


<h3>Какие метрики используют, какое качество получают</h3> 
При оценке качества авторы смотрели на две характеристики:

* Покрытие интервала при alpha=0.1 (90%-ный интервал). Так как предсказание идет на горизонт H, то считается, что траектория попала в предсказательную ленту, если каждая точка попала в соответствующий ей интервал.
* Ширина интервала. Очевидно, что мы не хотим строить слишком широкие интервалы если удовлетворяем условию на покрытие.


Результаты на синтетических данных в сравнении с другими методами

![image](https://user-images.githubusercontent.com/48079881/155844392-4c9253df-b2f2-4c2a-9dc7-8ad5030d4d0e.png)
![image](https://user-images.githubusercontent.com/48079881/155844394-8948642f-1be6-4cec-a25b-a9812be9746b.png)




Результаты на реальных данных в сравнении с другими методами

![image](https://user-images.githubusercontent.com/48079881/155844396-7a8697b9-632d-45f1-a073-8c54a08f0d99.png)

Как видим, интервалы оказались достаточно точными.
