erDiagram
    УЧЕНИКИ {
        bigint id PK
        text имя
        text фамилия
        date дата_рождения
    }
    КЛАССЫ {
        bigint id PK
        text название
        smallint уровень
    }
    УЧИТЕЛЯ {
        bigint id PK
        text имя
        text фамилия
        text предмет
    }
    ПОСЕЩАЕМОСТЬ {
        bigint id PK
        bigint ученик_id FK
        bigint урок_id FK
        text статус
    }

    КЛАССЫ ||--o{ УЧЕНИКИ : "содержит"
    УЧИТЕЛЯ ||--o{ КЛАССЫ : "классный руководитель"
    УЧЕНИКИ ||--o{ ПОСЕЩАЕМОСТЬ : "фиксируется"
