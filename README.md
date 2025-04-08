# ГОСТ Р 34.11-94 Хеш-функция

Реализация российского криптографического стандарта хеширования ГОСТ Р 34.11-94 на языке Go.

## Описание алгоритма

ГОСТ Р 34.11-94 — это алгоритм хеширования, разработанный в России и использовавшийся в качестве национального стандарта с 1994 до 2012 года, когда был заменен на ГОСТ Р 34.11-2012 (Стрибог). Алгоритм создает 256-битное хеш-значение и основан на блочном шифре ГОСТ 28147-89.

## Особенности ГОСТ Р 34.11-94

- Выходной размер хеш-значения: 256 бит (32 байта)
- Размер блока обрабатываемых данных: 256 бит (32 байта)
- Использует блочный шифр ГОСТ 28147-89 в качестве основы
- Включает контрольную сумму и механизм обратной связи

## Принцип работы алгоритма

Хеш-функция ГОСТ Р 34.11-94 работает по принципу итеративного сжатия данных. Основные шаги:

1. **Инициализация:** 
   - Начальное значение хеша устанавливается в нули
   - Контрольная сумма устанавливается в ноль

2. **Обработка сообщения:**
   - Сообщение разбивается на блоки по 256 бит
   - Каждый блок обрабатывается пошагово
   
3. **Сжатие блока (функция step):**
   - Формирование ключей для ГОСТ 28147-89 на основе текущего хеша и блока данных
   - Шифрование частей текущего значения хеша этими ключами
   - Применение нелинейных преобразований (функция fChi) многократно
   - Объединение результатов для получения нового значения хеша

4. **Финализация:**
   - Добавление padding (дополнение) до полного блока
   - Добавление блока с размером сообщения в битах
   - Добавление блока с контрольной суммой
   - Обработка этих блоков функцией сжатия
   - Инвертирование порядка байтов в итоговом хеше

## Основные компоненты кода

### Структуры и константы

- `BlockSize` и `Size` — определяют размер блока данных и хеш-значения (32 байта)
- `SboxDefault` — стандартные S-блоки ГОСТ 28147-89
- `c2`, `c3`, `c4` — константы для преобразований
- Структура `Hash` — содержит состояние хеш-функции

### Основные функции

1. **Внутренние преобразования:**
   - `fA` — функция преобразования для XOR и циклических сдвигов
   - `fP` — функция перестановки байтов
   - `fChi` — функция нелинейного сжатия
   - `blockReverse` — инвертирует порядок байтов
   - `blockXor` — выполняет XOR между блоками

2. **Основной алгоритм:**
   - `step` — выполняет один шаг хеширования блока
   - `chkAdd` — обновляет контрольную сумму
   - `Write` — добавляет данные к хешируемому сообщению
   - `Sum` — формирует окончательное хеш-значение


## Схема работы алгоритма

```
Вход: сообщение M
┌────────────┐     ┌─────────────┐     ┌───────────┐
│ Разделение │     │ Функция     │     │ Финальные │
│ на блоки   │ ──> │ сжатия      │ ──> │ блоки     │ ──> Хеш H(M)
│ по 256 бит │     │ (для каждого│     │           │
└────────────┘     │  блока)     │     └───────────┘
                   └─────────────┘
```

## Безопасность

ГОСТ Р 34.11-94 обеспечивает стойкость к различным видам криптоаналитических атак за счет:
- Сложных нелинейных преобразований
- Контрольной суммы всего сообщения
- Многократного применения нелинейных функций
- Использования блочного шифра ГОСТ 28147-89

Однако стоит отметить, что этот стандарт был заменен более современным ГОСТ Р 34.11-2012 (Стрибог).

## Зависимости

Код использует внешний пакет `github.com/ftomza/gogost/gost28147` для операций блочного шифрования ГОСТ 28147-89.
