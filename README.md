# Домашнее задание к занятию " `Резервное копирование баз данных` " - `Сулименков Алексей`

---

## 1 Задание 1. Резервное копирование

Кейс

Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования.

Необходимо описать, какие варианты резервного копирования подходят в случаях:

1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.

1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.

1.3.\* Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.

Приведите ответ в свободной форме.

### Ответ

1.1.

<details> <summary>Differential backup</summary>
Необходимо настроить, допустим, еженедельный full backup и ежедневный differential backup, differential обрабатывает файлы, измененные или созданные с момента выполнения предыдущего полного бэкапа.
</details>

1.2.

<details> <summary> Incremental Backup</summary>
Разбор журналов, создание списка изменённых страниц и резервирование страниц, включённых в список. Наличие инкрементальных копий не отменяет требований к наличию журналов для восстановления на произвольную точку во времени. Поэтому необходимо журналы постоянно переписывать на внешний носитель, а резервные копии, создавать по расписанию.
</details>

1.3.

<details> <summary>Master-Master</summary>
Необходимо использовать репликацию master-master replication совместно с sql proxy.
</details>
---

## Задание 2. PostgreSQL

2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).

### Ответ

<details> <summary>pgdump</summary>

```SQL
pg_dump <параметры> <имя базы> > <файл для сохранения копии>
```

```SQl
параметры утилиты pg_dump.

-d <имя_бд>, —dbname=имя_бд — база данных, к которой выполняется подключение.

-h <сервер>, —host=сервер — имя сервера.

-p <порт>, —port=порт — порт для подключения.

-U <пользователь>, —username=пользователь) — учетная запись, используемое для подключения.

-w, —no-password — деактивация требования ввода пароля.

-W, —password — активация требования ввода пароля.

—role=имя роли — роль, от имени которой генерируется резервная копия.

-a, —data-only — вывод только данных, вместо схемы объектов (DDL).

-b, —blobs — параметр добавляет в выгрузку большие объекты.

-c, —clean — добавление команд DROP перед командами CREATE в файл резервной копии.

-C, —create — генерация реквизитов для подключения к базе данных в файле резервной копии.

-E <кодировка>, —encoding=кодировка — определение кодировки резервной копии.

-f <файл>, —file=файл — задает имя файла, в который будет сохраняться вывод утилиты.

-F <формат>, —format=формат — параметр определяет формат резервной копии. Доступные форматы:

p, plain) — формирует текстовый SQL-скрипт;
c, custom) — формирует резервную копию в архивном формате;
d, directory) — формирует копию в directory-формате;
t, tar) — формирует копию в формате tar.
-j <число_заданий>, —jobs=число_заданий — параметр активирует параллельную выгрузку для одновременной обработки нескольких таблиц (равной числу заданий). Работает только при выгрузке копии в формате directory.

-n <схема>, —schema=схема — выгрузка в файл копии только определенной схемы.

-N <схема>, —exclude-schema=схема — исключение из выгрузки определенных схем.

-o, —oids — добавляет в выгрузку идентификаторы объектов (OIDs) вместе с данными таблиц.

-O, —no-owner — деактивация создания команд, определяющих владельцев объектов в базе данных.

-s, —schema-only —добавление в выгрузку только схемы данных, без самих данных.

-S <пользователь>, —superuser=пользователь — учетная запись привилегированного пользователя, которая должна использоваться для отключения триггеров.

-t <таблица>, —table=таблица — активация выгрузки определенной таблицы.

-T <таблица>, —exclude-table=таблица —исключение из выгрузки определенной таблицы.

-v, —verbose — режим подробного логирования.

-V, —version — вывод версии pg_dump.

-Z 0..9, —compress=0..9 — установка уровня сжатия данных. 0 — сжатие выключено.
```

</details>

<details> <summary>pgrestore</summary>

```SQL
pg_restore -d <имя восстанавливаемой БД> <файл для сохраненой копии>
```

```SQL
синтаксис утилиты pg_restore.

-h <сервер>, —host=сервер — имя сервера, на котором работает база данных.

-p <порт>, —port=порт — TCP-порт, через база данных принимает подключения.

-U <пользователь>, —username=пользователь — имя пользователя для подключения..

-w, —no-password — деактивация требования ввода пароля.

-W, —password — активация требования ввода пароля.

—role=имя роли — роль, от имени которой выполняется восстановление резервная копия.

<имя_файла> — расположение восстанавливаемых данных.

-a, —data-only — восстановление данных без схемы.

-c, —clean — добавление операторов DROP перед операторами CREATE.

-C, —create — создание базы данных перед запуском процесса восстановления.

-d <имя_бд>, —dbname=имя_бд — имя целевой базы данных.

-e, —exit-on-error — завершение работы в случае возникновения ошибки при выполнении SQL-команд.

-f <имя_файла>, —file=имя_файла — файл для вывода сгенерированного скрипта.

-F <формат>, —format=формат — формат резервной копии. Допустимые форматы:

p, plain — формирует текстовый SQL-скрипт;
c, custom — формирует резервную копию в архивном формате;
d, directory — формирует копию в directory-формате;
t, tar — формирует копию в формате tar.
-I <индекс>, —index=индекс — восстановление только заданного индекса.

-j <число-заданий>, —jobs=число-заданий — запуск самых длительных операций в нескольких параллельных потоках.

-l, —list) — активация вывода содержимого архива.

-L <файл-список>, —use-list=файл-список — восстановление из архива элементов, перечисленных в файле-списке в соответствующем порядке.

-n <пространство_имен>, —schema=схема — восстановление объектов в указанной схеме.

-O, —no-owner — деактивация генерации команд, устанавливающих владение объектами по образцу исходной базы данных.

-P <имя-функции(тип-аргумента[, …])>, —function=имя-функции(тип-аргумента[, …]) — восстановление только указанной функции.

-s, —schema-only — восстановление только схемы без самих данных.

-S <пользователь>, —superuser=пользователь — учетная запись привилегированного пользователя, используемая для отключения триггеров.

-t <таблица>, —table=таблица — восстановление определенной таблицы.

-T <триггер>, —trigger=триггер — восстановление конкретного триггера.

-v, —verbose — режим подробного логирования.

-V, —version — вывод версии утилиты pg_restore.
```

</details>

## Задание 3. MySQL

3.1. С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL.

### Ответ

<details> <summary>incremental backup</summary>

Можно так сделать

1. Сделать full backup

```SQL
mysqldump -uUSER -p --all-databases --single-transaction --flush-logs --master-data=2 > full_backup.sql
```

2. Сбросить журнал

```SQL
mysqladmin -uUSER -p flush-logs
```

3. Для восстановления необходимо восстановить полный бекап, а затем

```SQL
mysqlbinlog /var/log/mysql/mysql-bin.НОМЕР_ЛОГА| mysql -uUSER -p
```

Или так сделать инкрементное копирование. Перед - необходимо выполнить full backup

```SQL
mysqlbackup --defaults-file=/home/dbadmin/my.cnf \
  --incremental --incremental-base=history:last_backup \
  --backup-dir=/home/dbadmin/temp_dir \
  --backup-image=incremental_image1.bi \
   backup-to-image
```

</details>

---
