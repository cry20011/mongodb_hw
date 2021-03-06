# Домашнее задание 1 MongoDB
Установил MongoDB и MongoDB Compass локально
Заполнил данными о пассажирах на Титанике (но там всего ~900 записей, поэтому раздул данные копиями).
# Запросы
Добавим двух Виталиков, попробуем заново, посмотрим как работает insertOne с таким же запросом, удалим весь этот страшный объект, попробуем с {ordered:false} вставить снова первого и третьего Виталика
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen1.png)
и, да, Виталика оказалось три.
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen2.png)
Найдем всех не выживших женщин младше 20 лет, у которых есть хотя бы один родственник на корабле (и возьмем первых трех).
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen3.png)
Теперь, выживших мужчин старше 30, у которых хотя бы один родственник на корабле, отсортируем по возрасту (по возрастанию) и возьмем первых трех.
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen4.png)
Оказывается, что все, кто старше 20 лет и у которых ровно один родственник на корабле, выжили.
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen5.png)
# Индексы
Сделаем простой запрос: выбрем всех 20-летних пассажиров
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen6.png)
Зафиксировали 1200 миллисекунд, теперь создадим индекс по возрасту по возрастанию.
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen7.png)
60 миллисекунд.
Теперь всех пассажиров младше 20 лет без индекса
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen8.png)
С индексом
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen9.png)
Немного лучше. Попробуем наоборот, старше 20. Без индекса:
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen10.png)
С индексом:
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen11.png)
Вдвое дольше, тут выбралось в три раза больше данных, чем в предыдущем запросе, получается для большой выборки B-дерево замедляет запрос, но для точечного запроса (как было в первом, {"Age": 20}) очень большой прирост в скорости.

Попробуем запрос посложнее:
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen12.png)
1 секунда
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen13.png)
500 миллисекунд, прирост скорости в два раза.

Попробуем теперь в обратную сторону:
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen15.png)
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen16.png)
Та же история, что и в прошлый раз, на бОльших данных индекс замедляет.

Создадим ещё один индекс по возрасту и количеству родственников на корабле.
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen17.png)
Тот запрос, который выполнялся одну секунду, теперь выполняется за четверть секунды.

## Теперь проверим операции вставки и изменения данных
Вставим 100000 Виталиков в базу.

Без индексов:
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen18.png)
Удалим Виталиков и повторим код с индексами по возрасту и по количеству родственников:
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen19.png)
Индексы замедляют вставку, потому что нужно перестраивать дерево.

Попробуем update, пусть всем Виталикам, которые старше 40 лет, оказалось 40 лет.

Без индексов:
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen20.png)
C индексами по возрасту и по количеству родственников:
![](https://github.com/cry20011/mongodb_hw/raw/main/screens/screen21.png)
Тут индексы дали значительный прирост в скорости, видимо потому что не нужно проходить по всем данным, а уже по отсортированным данным быстро нашли всех Виталиков которым больше 40 лет.
