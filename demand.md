Задача: обзор методов предсказания редких рядов (бОльшая часть значений ряда - нули). Пример: транзакции пользователя в рублях в час.

Статьи:
* https://www.sciencedirect.com/science/article/pii/S1877050920303951
* https://jsdajournal.springeropen.com/articles/10.1186/s40488-021-00121-4
* https://medium.com/analytics-vidhya/santander-customer-transaction-prediction-an-end-to-end-machine-learning-project-2cb763172f8a
* https://www.ijcai.org/proceedings/2020/0603.pdf
* https://digscholarship.unco.edu/dissertations/464/
* https://www.lancaster.ac.uk/pg/waller/pdfs/Intermittent_Demand_Forecasting.pdf
* https://www.sciencedirect.com/science/article/abs/pii/S0925527313000273
* https://smartgrid.ucla.edu/pubs/IEEE_SSnties.pdf
* https://www.inovex.de/de/blog/deep-learning-time-series/
* https://www.nuffield.ox.ac.uk/economics/papers/2017/CensorAus5d_Submit.pdf

| Подход      | Идея |
| ----------- | ----------- |
| [kNN, Lazy Learning](https://smartgrid.ucla.edu/pubs/IEEE_SSnties.pdf)      | kNN на лагах рядов; Lazy Learning - модификация kNN, когда количество соседей выбирается автоматически в процессе для каждого ряда      |
| Croston’s method   | Оценивается размер спроса и интервалы между ненулевыми измерениями. Оценки обновляются на каждом измерении с ненулевым спросом      |
| [Модификации Кростона](https://www.lancaster.ac.uk/pg/waller/pdfs/Intermittent_Demand_Forecasting.pdf) | Отличается тем, что оценивает вероятность ненулевого спроса (а не размер интервала) и тем, что оценки обновляются каждый период, а не только тогда, когда возникает спрос.|
| [ARMA, DARMA, INARMA models	INARMA (Integer-valued ARMA)](https://www.lancaster.ac.uk/pg/waller/pdfs/Intermittent_Demand_Forecasting.pdf)| берет авторегрессионные члены и скользящие средние, «прореживают» их и добавляют некоторое случайное неотрицательное целое число|
| [ADIDA](https://www.lancaster.ac.uk/pg/waller/pdfs/Intermittent_Demand_Forecasting.pdf)	|Aggregate-Disaggregate Intermittent Demand Approach, ряд разделяется на блоки (пересекающиеся или нет - на выбор), в каждом блоке значение агрегируются в одно (как именно - на усмотрение) , на полученных значениях обучается любая модель (так как ряд не будет содержать уже так много нулей), прогнозируемое значение разбивается обратно в блок (можно, к примеру, использовать веса, основанные на соотношениях из предыдущих блоков, или же просто равные веса) |
| [Zero-Inflated model](https://jsdajournal.springeropen.com/track/pdf/10.1186/s40488-021-00121-4.pdf)| Сместь распределения Пуассона (или отрицательного биномиального) и дельта функции в нуле|
| [hurdle model](https://jsdajournal.springeropen.com/track/pdf/10.1186/s40488-021-00121-4.pdf) | Смесь усечeнного в нуле распределения Пуассона (или отрицательного биномиального) и дельта функции в нуле. Разница с Zero-Inflated model в том, что тут 0 приходит с одного распределения, тогда как там из смеси (2 разных по происхождению нуля)|
| [Deep Hurdle Network *](https://www.ijcai.org/proceedings/2020/0603.pdf)|Состоит из 3 частей: общий энкодер признаков, MVP (multivariate probit model) для получения ненулевых индексов, MLND (multivariate log-normal distribution) для получения прогнозов для ненулевых индексов |
| [NN-Dual **](https://www.sciencedirect.com/science/article/abs/pii/S0925527313000273)| Это и следующая - своего рода  улучшения Кростона. На вход подаются ненулевой спрос и интервалы с лагами, на выходе ищем следующий интервал и спрос (ответ получаем таким же образом как и в кростоне - делением)|
| [NN-Rate **](https://www.sciencedirect.com/science/article/abs/pii/S0925527313000273)| Идея: не хотим получать 2 значения и делить одно на другое. На вход подаются ненулевой спрос и интервалы с лагами (так же как и в NN-Dual), на выходе одно значение|

'* Архитектура: ![image](https://user-images.githubusercontent.com/48079881/155746354-af4e8933-f2a3-42b5-99c3-bc169c6f6375.png)
<br />
** NN-Dual: ![image](https://user-images.githubusercontent.com/48079881/155746554-a636e082-5415-4da6-9800-0e7defa65860.png)
<br />
** NN-Rate: ![image](https://user-images.githubusercontent.com/48079881/155746596-9cdbd4c3-fd04-4cea-b598-e3c2c63e6987.png)
