# 02

1) git clone https://github.com/UrcaDeLima/linkShortener.git
2) cd linkShortener
3) npm install
4) npm run dev
5) Postman
  a) Скачать Postman(если отсутствует).
  b) Поставить POST запрос url - http://localhost:3000/new/contractor.
  c) Перейти во вкладку body и поставить JSON формат данных.
  d) Создание укороченной ссылки: В body вписать данные в требуемом формате(Пример: {"name": "Adam", "url": "https://www.google.com"}) - после создаётся запись в mongodb и возвращается сокращённая ссылка.
  e) Можно перейти по полученной(укороченой) ссылке(как в браузере, так и в Postman(предварительно указав GET запрос)), после чего произайдёт перенаправление на длинную, counter сделает свою работу и в бд обновятся данные, относящиеся к данной ссылке.
  f) Просмотр списка своих ссылок производится по запросу типа: http://localhost:3000/myUrl/Adam, где Adam идентификатор пользователя(впринципе, можно было сделать полноценную авторизацию, что бы не было казусов с дубляжом, но для показа, надеюсь, и этого хватит) - ответом будет JSON массив со всеми ссылками(как длинными, так и укорочеными) и статистикой перехода по каждой из них.
  g) Просмотреть статистику переходов по каждой ссылке можно указав url типа: http://localhost:3000/counter/c, где c - это сгенерированный shortUrl.

Изначально, когда продумывал архитектуру этой api(в частности алгоритма шифрования для однозначной идентификации shortUrl) отталкивался от знаний о бд, с которой раньше работал(имею ввиду MySQL и PostgreSQL), но сейчас решил делать на Mongodb, собственно, к чему это я, проблема возникла с идентификатором, ибо в mongo он составной, а расчёт был на тип int, поэтому пришлось самостоятельно сделать инкрементор, однако он очень ненадёжный, в случаях, когда человек будет менять идентификатор в бд вручную, будет вероятность дублирования shortUrl(ибо данные шифруются исходя из подаваемой цифры, шифруется до 6 символов, которые будут указываться в shortUrl(http://localhost:3000/abcd, abcd - зашифрованная int переменная)), что, разумеется недопустимо, но опять же не стал сильно заморачиваться, ибо это тестовая работа.

В качестве инструмента нагрузочного тестирования я использовал Apache JMeter
При 1 нагрузочном тестировании время отработки основного сценария составляло 3.27 секунды за 10 запросов, затем я подкорректировал async/await функции, вынес сохранение данных после тика counter после redirect и время выполнения стало уже 1.4 секунды, дальше добавил кеширование и время 10 запросов сократилось до 0.1 секунды.
