---
sourcePath: ru/ydb/ydb-docs-core/ru/core/yql/reference/yql-core/syntax/_includes/not_yet_supported.md
sourcePath: ru/ydb/yql/reference/yql-core/syntax/_includes/not_yet_supported.md
---
# Ещё не поддерживаемые конструкции из классического SQL

## \[NOT\] \[EXISTS|INTERSECT\|EXCEPT] {#not-exists}

Доступный альтернативный вариант — `EXISTS` синтаксически доступен, но из-за отсутствия поддержки коррелированных подзапросов не очень полезен. Также можно переписать через `JOIN`.

## UNION {#union}

Доступный альтернативный вариант — `SELECT DISTINCT` по явно перечисленным колонкам + `UNION ALL`, при необходимости с использованием подзапросов.

## NATURAL JOIN {#natural-join}

Доступный альтернативный вариант — явно перечислить совпадающие с обеих сторон колонки.

## NOW() / CURRENT_TIME() {#now}

Доступный альтернативный вариант — воспользоваться функциями [CurrentUtcDate, CurrentUtcDatetime и CurrentUtcTimestamp](../../builtins/basic.md#current-utc).
