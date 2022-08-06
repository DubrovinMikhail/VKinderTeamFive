# Работа на проектом курсовой  
  
* `env_example` лежит шаблон для `.env`,  который надо заполнить своими данными

* `basic_code.py`  это шаблон для бота
* `database.py` это модуль для открытия database
* `users.py`  модуль дляобработки запросов к базе данных ВК
* `bot_auth.py` модуль для аутентификации приложения
* `access_token.py` модуль для автоматического получения ключей для работы приложения. Реализовано через SELENIUM, необходимые токены пишутся в файл `.env`.

* `requirements.txt` необходимые импортированные библиотеки `pip install -r requirements.txt`
## Функции модуля `database.py` для записи в БД  


  - в базе 4 таблицы `persons`, `photos`, `whitelist`, `blacklist`
 
  - `add_person_to_base()`  добавляет юзера в базу
  - `add_photos()` добавляет в базу фото пользователей
  - `add_whitelist(user_id, select_id, **kwargs)` добавляет в белый лист и записывает
  выбранного в базу данных
  - `add_blacklist(user_id, select_id, **kwargs)` добавляет в черный лист, в таблицу `Person` ничего не пишет
  - `choose_favorite(vk_id)`   выбирает всех фаворитов для выбранного юзера
  - `check_blacklist(user_id, select_id)` проверяет есть ли выбранная личность в черном списке

## Функции в модуле `users.py`
* в модуле  `users.py`находится три  функции 
  - `users_info(vk_id: int, gr_params)`  функция принимает `vk_id` id  клиента информацию по которому запрашиваем
  - в ответ приходит словарь следующего вида
  ```
   user_dict_info { 'vk_id': int, 'first_name': srt, 'last_name': str, 'sex': int
                             'city_id': int, 'city':  str ,'birth_date': str
                             }
  ```
    если каких то данных нету, в ответе `''`
* `photos_get(vk_id: int, us_params)` получает также  `vk_id` клиента и формирует список словарей следующего вида
  ```
  "photo_list список словарей [ {'id': int, 'likes': int,
                  'vk_id': int, 'photo_name': str (photo12345_12345)}, ... ,  
  ```
* `search_users(user_id: int, gr_params: str , us_params: str, offset=0)` получает информацию о клиенте `user_id`. Выполняет поисковый запрос на условиях `city_id` , `age_from`, `age_to`, а также поиск пропивоположного пола, 
Кроме этого, производится отбраковка анкет которые закрыты для просмотра `is_closed==True`
## Функцианал модуля `access_token.py`
* Модуль   `access_token.py`  автоматически получает токены необходимые для работы программы. Для корректной работы необходимо в `.env` заполнить поля `VK_LOGIN` и `VK_PASSWORD`. Полученные токены
также записываются в файл `.env`  под именами `GROUP_TOKEN` и `USER_TOKEN`.  Для корректной работы требуется самостоятельная установка `chromedriver` для версии chrome, установленного на вашем компьютере

## Модуль  аутентификации `bot_auth.py`  
  - Создает класс `Auth`, который используется для авторизации запросов к базе данных ВК всеми другими модулями
  - Все действия автоматические при обращению к классу.
  - Существуют два типа ключей : групповые для сообществ в ВК (используется приствка gr_) и индивидуальная для конкретного пользователя ( используется приставка us_)

## Основной модуль `basic_code.py`

- основной класс `DatingBot`, принимает и обрабатывает сообщения пользователей
 ---
 - основная функция `messangerBot`  принивает сообщения клиента фильтрует их.
 - `show_selected(self, keyboard)` подготавливает отправку фотографий для показа пользователю
 - `write_msg(self, params)` отправка новых сообщений 
 - `edit_msg(self, params)` редактирование и отправка
 - 
 - `make_keyboard(0-4)` формирует различные клавиатуры для сообщений
 - метод `iteration` класса  `DatingBot` вместе с дополнительным классом `SelectedIterator()` обеспечивают показ сообщений поочередно.

