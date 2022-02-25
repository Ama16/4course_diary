Цель: обзор методов прогнозирования коротких рядов (рядов с короткой историей (от 14 до 30 точек))

Статьи:
* https://www.researchgate.net/publication/5055578_Bayesian_Forecasting_Methods_for_Short_Time_Series
* https://journals.sagepub.com/doi/10.1177/0047287506291594
* http://siba-ese.unisalento.it/index.php/ejasa/article/view/16825/16315
* https://www.researchgate.net/publication/234838605_A_Comparative_Analysis_of_Short_Time_Series_Processing_Methods
* https://arxiv.org/pdf/1810.07066.pdf

| Подход | Идея |
| ----------- | ----------- |
| fuzzy time series (библиотека pyFTS) | Ищет минимальные и максимальные значения в ряде, разбивает на [Umin, Umax] на интервалы-состояния, смотрит на все переходы из состояний. При прогнозе смотрит состояние последнего известного и смотрит на все состояния, в которые можно было перейти из данного. Усредняет по ним |
| Moving Average | |
| Exponential smoothing | |
| [grey theory](https://ieeexplore.ieee.org/document/725894) | Строит дифференциальные уравнения, ищет псевдорешения |
| conditional mean | взвешенные лаги, коэффициенты - ядерная функция от удаленности |
| Holt and Holt Winters procedure| |
| Nearest Neighbor Regression | нужно разбить ряды на окна, мерять расстояния, усреднять наиближайшие	|
