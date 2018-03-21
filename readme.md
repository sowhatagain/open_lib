# Определение темы книги по обложке, названию.

Источником данных является интеренет-библиотека www.openlibrary.org


# 1) Данные (папка Data)


https://openlibrary.org/data - информация по работе с данными сайта


источники по основным темам, вынесенным в шапку сайта берем данные с 2000 по 2018 гг


Fantasy 5670 
Medicine 4383 
Music 9771 
Mystery and detective stories 2534 
Religion 14901 
Romance 11284 
Recipes 1531 
Science 16848 
Children 5766
Science fiction 4677


Для получения json c данными о названии отправляется запрос вида: http://openlibrary.org/subjects/key.json?published_in=2000-2018&limit=books_number (число книг по тебе в шапке .json)
где в key указывется тематика, а также число запрашеваемых книг.


Для скачивания изображений - cкрипт типа subject.ipynb. Каждый скрипт в своей папке собирает обложки книг указанной темы. Запускалось параллельно несколько, чтобы потоками быстрее скачивать весь объем информации.


# 2) EDA (папка Preprocessing)


Работа велась с заголовками книг, изображениями.


а) Заголовки книг - исследование в data_preprocessing.ipynb Графики см. в папке.

* очистили заголовки от символов
* не брали в расчет заголовки с буквами не латинского алфавита (работаем с англ. языком)
* в попытке сохранить имена собственные, большинство которых зачастую незнакомы лемматайзерам, и word2vec оставили заголовки в которых есть английское слово
* длины заголовков небольшие, медиана обычно составляет 2-3 символа, поэтому удобно использовать предобученный на большом корпусе word2vec
* wordcloud помогли удостовериться, что действительно, у книг одной тематики есть преобладающие и связанные по смыслу слова

Основное затруднение - мультиклассовая задача классификации. Одна и та же книга принадлежит нескольким классам. Также неравновесное распределение классов по темам. Из всех возможных задач по уравниваю весов классов пришла к downsampling-y, число отдельно взятых книг по теме достаточно для классификации типа one-to-all. Рандомно сделать upsampling "связных по смыслу заголовков" не представляется возможным.


б) Изображения - скачанные изображения пакетно улучшили по качеству в фоторедакторе и сжали до размеров, которые удобно передавать в нейросеть ( 224х224х3 px), чтобы не тратить на это время работы модели. img_changes_size_name.ipynb уменьшение, изменение имени файла (нужны при мультиклассовой классификации)


# 3) Предсказания по заголовкам:

Предсказание велось в предположении, что будем предсказывать класс one-to-all для 10-ти классов, получив на выходе веса для 10 лучших моделей. Таким образом, "прогнав" название через 10 предобученных моделей, получим на выходе принадлежность одному или нескольким классам.


train/test разделяла в прпорции 80/20 (из-за сложности данных train увеличила до 80)


Для того, чтобы получить больше признаков, на которых можно обучить нейронную сеть для леммы слова в заголовке взяли эмбеддинги из предобученной модели word2vec от Google.


В результате экспериментов остановились на последовательной нейронной сети со слоями LSTM, которые дают возможность "запоминить" предыдущие состояния и связи, что важно для текста.


35 эпох обучения достаточно, чтобы нейросеть давала хорошие устойчивые результаты, обычно хороший результат достигался к 20-25 эпохам. Окончательный расчет на ноутбуке занял 3 с небольшим часа. Реализация: нейронная сеть с LSTM. Дала хорошие результаты классификации по отдельно взятым классам precision, recall и f1-меры класса для разных классов варьируются в пределах 0,7-0,85.


Попытка решить эту задачу как задачу мультиклассификации не провела пока из-за ограниченности во времени.


# 4) Предсказания по изображениям (папка 4.Cover)


Все изображения свела к единому наименованию по cover-id, что облегчило задачи по обработке данных. (изначально, скачивая, каждому изображению добавила в название имя класса)


а) мультиклассовая реализация Расчеты велись на страционарном компоютере: GPU Nvidia 1080, оперативка граф. процессора 8 Гб.


скрипт: cover-multiclass.ipynb На вхое предобученная сеть ResNet. Слои заморожены. Выход сети передается на один или несколько dense-слоев с 9-ю выходами по кол-ву классов. train/test разделялись в пропорции 90/10. 

Точность классификации невысокая. Найти новые способы нахождения порога. Добавить данные.


Также, как причину, вижу то, что в сеть отдавала несбалансированые по классам данные. 




# 5) Выводы по построенным моделям:

По заголовку книги получается достаточно точная идентификация темы книги. Для изображения обложки сработал метод CNN+LSTM скорее всего, обусловлено качеством данных, передаваемых в модель.



# 6) Дальнейшие изыскания:

Общее - sklearn обладает комплексом процедур, которые могут работать с мультиклассовыми задачами. Обратиться к этим классификаторам и изучить их возможности в классификации этой задачи. (http://scikit-learn.org/stable/modules/multiclass.html) Также обратиться к решениям на деревьях, тк они не зависят от мультиклассовости.

Заголовки:

* возможность обращаться к онлайн-переводчику по API (например, Яндекс), поможет перевести остальную часть названий и включить их в обучение.
* для имен собственных спарсить набор статей (Википедия), дообучить существующий word2vec, что тоже должно поднять результаты классификации.


Изображения:


* применить метод снижения размерностей и посмотреть на результат, может быть в случае неоднородных данных это поможет в классификации.
* image agumentation части изображений также может помочь улучшить качество классификации.
* усложене текущей модели: добавление в обучение весов от других распространенных предобученных на imagenet моделей, усложнение архитектуры и формы передачи данных от модели друг другу всегда качественно положительно влияет на результат. Решение таких задач стоит проводить в облачных ресурсах или на мощных серверах.

Найти подходы к решению мультиклассовой задачи, чтобы не разбивать ее на несколько подзадач.

В идеале, работающий продакт-продукт - это ансамбль, который берет результаты обеих моделей и выдает финальный результат.
