erDiagram
  ШКОЛЫ {
    bigint id PK
    text название
    text адрес
  }

  УЧЕБНЫЕ_ГОДА {
    bigint id PK
    bigint школа_id FK
    text наименование
    date дата_начала
    date дата_окончания
  }

  ПЕРИОДЫ {
    bigint id PK
    bigint учебный_год_id FK
    text наименование
    date дата_начала
    date дата_окончания
  }

  УЧИТЕЛЯ {
    bigint id PK
    text имя
    text фамилия
    text email
    text телефон
    date дата_приема
    boolean активен
  }

  КЛАССЫ {
    bigint id PK
    bigint учебный_год_id FK
    text название
    smallint ступень
    bigint классрук_id FK
  }

  ПРЕДМЕТЫ {
    bigint id PK
    text название
    text краткое_название
  }

  ШКАЛЫ_ОЦЕНОК {
    bigint id PK
    bigint школа_id FK
    text название
    text описание
  }

  ГРАНИЦЫ_ОЦЕНОК {
    bigint id PK
    bigint шкала_id FK
    text метка
    numeric мин_проц
    numeric макс_проц
  }

  ПОЛЬЗОВАТЕЛИ {
    bigint id PK
    text email
    text телефон
    text пароль_хеш
    boolean активен
    timestamp создано
  }

  РОЛИ {
    bigint id PK
    text код
    text название
  }

  РОЛИ_ПОЛЬЗОВАТЕЛЯ {
    bigint пользователь_id FK
    bigint роль_id FK
  }

  УЧЕНИКИ {
    bigint id PK
    bigint пользователь_id FK
    text имя
    text фамилия
    date дата_рождения
    date дата_зачисления
    text статус
  }

  РОДИТЕЛИ {
    bigint id PK
    bigint пользователь_id FK
    text имя
    text фамилия
    text email
    text телефон
  }

  УЧЕНИК_РОДИТЕЛЬ {
    bigint ученик_id FK
    bigint родитель_id FK
    text отношение
    boolean основной
  }

  ЗАЧИСЛЕНИЯ {
    bigint id PK
    bigint ученик_id FK
    bigint класс_id FK
    date с_даты
    date по_дату
  }

  КАБИНЕТЫ {
    bigint id PK
    bigint школа_id FK
    text код
    smallint вместимость
  }

  КУРСЫ {
    bigint id PK
    bigint класс_id FK
    bigint предмет_id FK
    bigint учитель_id FK
    bigint период_id FK
    bigint шкала_оценок_id FK
    numeric часов_в_нед
  }

  УРОКИ {
    bigint id PK
    bigint курс_id FK
    bigint кабинет_id FK
    timestamp начало
    timestamp конец
    text тема
    text дз_кратко
    date дата_урока
  }

  ОПРАВД_ДОКУМЕНТЫ {
    bigint id PK
    bigint ученик_id FK
    date дата_выдачи
    date действует_с
    date действует_по
    text тип
    text файл_uri
  }

  ПОСЕЩАЕМОСТЬ {
    bigint id PK
    bigint урок_id FK
    bigint ученик_id FK
    text статус
    smallint опоздание_мин
    text причина
    bigint документ_id FK
  }

  КОНТРОЛЬНЫЕ {
    bigint id PK
    bigint курс_id FK
    text название
    text тип
    date дата
    numeric вес
    numeric макс_балл
  }

  ПОПЫТКИ_ОЦЕНОК {
    bigint id PK
    bigint контрольная_id FK
    bigint ученик_id FK
    smallint номер_попытки
    numeric балл
    timestamp проставлено
    text комментарий
  }

  ДЗ {
    bigint id PK
    bigint курс_id FK
    text тема
    text описание
    timestamp выдано
    timestamp дедлайн
  }

  СДАЧИ_ДЗ {
    bigint id PK
    bigint дз_id FK
    bigint ученик_id FK
    timestamp сдано
    text контент_uri
    text комментарий
  }

  ШАБЛОНЫ_СООБЩЕНИЙ {
    bigint id PK
    text тип
    text канал
    text шаблон_темы
    text шаблон_текста
  }

  СООБЩЕНИЯ {
    bigint id PK
    timestamp создано
    text тип
    text канал
    text тема
    text текст
    bigint шаблон_id FK
    bigint автор_user_id FK
  }

  ПОЛУЧАТЕЛИ_СООБЩЕНИЙ {
    bigint id PK
    bigint сообщение_id FK
    bigint родитель_id FK
    bigint ученик_id FK
    timestamp отправлено
    text статус_доставки
    text адрес_канала
  }

  ПРЕМИИ {
    bigint id PK
    bigint учитель_id FK
    bigint период_id FK
    numeric сумма
    text причина
    timestamp создано
  }

  АУДИТ {
    bigint id PK
    timestamp когда
    bigint пользователь_id FK
    text сущность
    bigint сущность_id
    text действие
    jsonb детали
  }

  ШКОЛЫ ||--o{ УЧЕБНЫЕ_ГОДА : "имеет"
  УЧЕБНЫЕ_ГОДА ||--o{ ПЕРИОДЫ : "содержит"
  УЧЕБНЫЕ_ГОДА ||--o{ КЛАССЫ : "имеет"
  УЧИТЕЛЯ ||--o{ КЛАССЫ : "классный_руководитель"
  ПРЕДМЕТЫ ||--o{ КУРСЫ : "преподается_в"
  УЧИТЕЛЯ ||--o{ КУРСЫ : "ведет"
  КЛАССЫ ||--o{ КУРСЫ : "включает"
  ПЕРИОДЫ ||--o{ КУРСЫ : "в_периоде"
  КУРСЫ ||--o{ УРОКИ : "расписание"
  КАБИНЕТЫ ||--o{ УРОКИ : "проходит_в"
  УЧЕНИКИ ||--o{ ЗАЧИСЛЕНИЯ : "история"
  КЛАССЫ ||--o{ ЗАЧИСЛЕНИЯ : "состав"
  УРОКИ ||--o{ ПОСЕЩАЕМОСТЬ : "отметки"
  УЧЕНИКИ ||--o{ ПОСЕЩАЕМОСТЬ : "имеет"
  ОПРАВД_ДОКУМЕНТЫ ||--o{ ПОСЕЩАЕМОСТЬ : "подтверждает"
  КУРСЫ ||--o{ КОНТРОЛЬНЫЕ : "оценивание"
  КОНТРОЛЬНЫЕ ||--o{ ПОПЫТКИ_ОЦЕНОК : "попытки"
  УЧЕНИКИ ||--o{ ПОПЫТКИ_ОЦЕНОК : "получает"
  КУРСЫ ||--o{ ДЗ : "дает"
  ДЗ ||--o{ СДАЧИ_ДЗ : "получает"
  УЧЕНИКИ ||--o{ СДАЧИ_ДЗ : "сдает"
  ШКАЛЫ_ОЦЕНОК ||--o{ ГРАНИЦЫ_ОЦЕНОК : "границы"
  ШКОЛЫ ||--o{ ШКАЛЫ_ОЦЕНОК : "использует"
  КУРСЫ ||--o{ ШКАЛЫ_ОЦЕНОК : "привязка"
  ПОЛЬЗОВАТЕЛИ ||--o{ РОЛИ_ПОЛЬЗОВАТЕЛЯ : "имеет"
  РОЛИ ||--o{ РОЛИ_ПОЛЬЗОВАТЕЛЯ : "назначена"
  ПОЛЬЗОВАТЕЛИ ||--o{ УЧЕНИКИ : "аккаунт"
  ПОЛЬЗОВАТЕЛИ ||--o{ РОДИТЕЛИ : "аккаунт"
  УЧЕНИКИ }o--o{ РОДИТЕЛИ : "опека"
  ШАБЛОНЫ_СООБЩЕНИЙ ||--o{ СООБЩЕНИЯ : "по_шаблону"
  СООБЩЕНИЯ ||--o{ ПОЛУЧАТЕЛИ_СООБЩЕНИЙ : "доставляет"
  РОДИТЕЛИ ||--o{ ПОЛУЧАТЕЛИ_СООБЩЕНИЙ : "адресат"
  УЧЕНИКИ ||--o{ ПОЛУЧАТЕЛИ_СООБЩЕНИЙ : "о_ком"
  ПЕРИОДЫ ||--o{ ПРЕМИИ : "за_период"
  УЧИТЕЛЯ ||--o{ ПРЕМИИ : "получает"
  ПОЛЬЗОВАТЕЛИ ||--o{ АУДИТ : "совершил"
