---
title: Изменение настроек кластера Elasticsearch
description: 'После создания кластера вы можете изменить настройки класса хостов, увеличить размер хранилища или изменить дополнительные настройки кластера Elasticsearch.'
keywords:
  - настройки Elasticsearch
  - настройки кластера Elasticsearch
  - Elasticsearch
---

# Изменение настроек кластера

После создания кластера вы можете:

* [{#T}](#change-service-account).
* [{#T}](#change-resource-preset).
* [{#T}](#change-disk-size) (недоступно для [хранилища](../concepts/storage.md) на нереплицируемых SSD-дисках).
* [{#T}](#change-admin-password).
* [{#T}](#change-additional-settings).

Вы также можете обновить версию и изменить редакцию {{ ES }}. Подробнее см. в разделе [{#T}](./cluster-version-update.md).

## Изменить настройки сервисного аккаунта {#change-service-account}

{% list tabs %}

- Консоль управления

    1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mes-name }}**.
    1. Выберите кластер и нажмите кнопку **Редактировать** на панели сверху.

    
    1. Выберите нужный сервисный аккаунт из списка или [создайте новый](../../iam/operations/sa/create.md). Подробнее о настройке сервисного аккаунта см. в разделе [{#T}](s3-access.md).

       {% include [mdb-service-account-update](../../_includes/mdb/service-account-update.md) %}

{% endlist %}


## Изменить класс хостов {#change-resource-preset}

{% list tabs %}

- Консоль управления

  1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mes-name }}**.
  1. Выберите кластер и нажмите кнопку **Редактировать** на панели сверху.
  1. Чтобы изменить класс хостов {{ ES }} с ролью *Data node*:
     1. Выберите вкладку **Data node**.
     1. В блоке **Класс хоста** выберите нужный класс для хоста.
  1. Чтобы изменить класс хостов {{ ES }} с ролью *Master node*:
     1. Выберите вкладку **Master node**.
     1. В блоке **Класс хоста** выберите нужный класс для хоста.
  1. Нажмите кнопку **Сохранить**.

- CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы изменить [класс хостов](../concepts/instance-types.md) для кластера:

    1. Посмотрите описание команды CLI для изменения кластера:

        ```bash
        {{ yc-mdb-es }} cluster update --help
        ```

    1. Укажите нужный класс для хостов с ролью _Data node_ и _Master node_ в команде изменения кластера:

        ```bash
        {{ yc-mdb-es }} cluster update <имя или идентификатор кластера> \
          --datanode-resource-preset <класс хостов с ролью Data node> \
          --masternode-resource-preset <класс хостов с ролью Master node>
        ```

    Имя и идентификатор кластера можно запросить со [списком кластеров в каталоге](cluster-list.md#list-clusters).

- API

  Воспользуйтесь методом [update](../api-ref/Cluster/update.md) и передайте в запросе:
  * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
  * Класс хостов в параметрах:
    * `configSpec.elasticsearchSpec.dataNode.resources.resourcePresetId`  — для хостов с ролью *Data node*.
    * `configSpec.elasticsearchSpec.masterNode.resources.resourcePresetId` — для хостов с ролью *Master node*.
  * Список настроек, которые необходимо изменить, в параметре `updateMask`. Если не задать этот параметр, метод API сбросит на значения по умолчанию все настройки кластера, которые не были явно указаны в запросе.

  Список поддерживаемых значений запрашивайте методом [list](../api-ref/ResourcePreset/list.md) для ресурсов `ResourcePreset`.

{% endlist %}

## Увеличить размер хранилища {#change-disk-size}

{% include [note-increase-disk-size](../../_includes/mdb/note-increase-disk-size.md) %}

{% list tabs %}

- Консоль управления

  Чтобы увеличить размер хранилища для кластера:

  1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mes-name }}**.
  1. Выберите кластер и нажмите кнопку **Редактировать** на панели сверху.
  1. Чтобы увеличить размер диска для хостов {{ ES }} с ролью *Data node*:
     1. Выберите вкладку **Data node**.
     1. В блоке **Хранилище** укажите необходимый размер диска.
  1. Чтобы увеличить размер диска для хостов {{ ES }} с ролью *Master node*:
     1. Выберите вкладку **Master node**.
     1. В блоке **Хранилище** укажите необходимый размер диска.
  1. Нажмите кнопку **Сохранить**.

- CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы увеличить размер хранилища для кластера:

    1. Посмотрите описание команды CLI для изменения конфигурации:

        ```bash
        {{ yc-mdb-es }} cluster update --help
        ```

    1. Укажите новые [параметры хранилища](../concepts/storage.md) для хостов с ролью _Data node_ и _Master node_ в команде изменения кластера:

        ```bash
        {{ yc-mdb-es }} cluster update <имя или идентификатор кластера> \
          --datanode-disk-size <размер хранилища в гигабайтах для хостов с ролью Data node> \
          --masternode-disk-size <размер хранилища в гигабайтах для хостов с ролью Master node>
        ```

    Имя и идентификатор кластера можно запросить со [списком кластеров в каталоге](cluster-list.md#list-clusters).


- API

  Чтобы увеличить размер хранилища для кластера, воспользуйтесь методом [update](../api-ref/Cluster/update.md) и передайте в запросе:
  * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
  * Необходимый размер хранилища (в байтах) в параметрах:
    * `configSpec.elasticsearchSpec.dataNode.resources.diskSize`  — для хостов с ролью *Data node*.
    * `configSpec.elasticsearchSpec.masterNode.resources.diskSize` — для хостов с ролью *Master node*.
  * Список настроек, которые необходимо изменить, в параметре `updateMask`. Если не задать этот параметр, метод API сбросит на значения по умолчанию все настройки кластера, которые не были явно указаны в запросе.

{% endlist %}

## Изменить пароль пользователя admin {#change-admin-password}

{% list tabs %}

- Консоль управления

  1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mes-name }}**.
  1. Выберите кластер и нажмите кнопку **Редактировать** на панели сверху.
  1. Укажите новый пароль пользователя `admin` в блоке **Пользователь**.
  1. Нажмите кнопку **Сохранить**.

- CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    Чтобы изменить пароль пользователя `admin` для кластера:

    1. Посмотрите описание команды CLI для изменения кластера:

        ```bash
        {{ yc-mdb-es }} cluster update --help
        ```

    1. Укажите новый пароль в команде изменения кластера одним из возможных способов:

       * Ввод пароля в интерактивном режиме.

         ```bash
         {{ yc-mdb-es }} cluster update <имя кластера> \
           --read-admin-password
         ```

       * Ввод пароля открытым текстом (менее безопасный способ).

         ```bash
         {{ yc-mdb-es }} cluster update <имя кластера> \
           --admin-password <новый пароль>
         ```

       * Автоматическая генерация пароля. Сгенерированный пароль будет выведен в консоль.

         ```bash
         {{ yc-mdb-es }} cluster update <имя кластера> \
           --generate-admin-password
         ```

    {{ mes-short-name }} запустит операцию изменения пароля `admin` для кластера.

- API

  Воспользуйтесь методом [update](../api-ref/Cluster/update.md) и передайте в запросе:
  * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
  * Новый пароль в параметре `configSpec.adminPassword`. Длина пароля — до 128 символов.
  * Список настроек, которые необходимо изменить, в параметре `updateMask`. Если не задать этот параметр, метод API сбросит на значения по умолчанию все настройки кластера, которые не были явно указаны в запросе.

{% endlist %}

## Изменить дополнительные настройки кластера {#change-additional-settings}

{% list tabs %}

- Консоль управления

    1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mes-name }}**.
    1. Выберите кластер и нажмите кнопку **Редактировать** на панели сверху.
    1. Чтобы изменить сервисный аккаунт, используемый для работы с кластером, выберите его из выпадающего списка.
    1. Измените дополнительные настройки кластера:

        {% include [Дополнительные настройки кластера MES](../../_includes/mdb/mes/extra-settings.md) %}

    1. Задайте настройки СУБД:

        Доступна настройка параметра `Fielddata cache size` — процент или абсолютное значение области динамической памяти, которая выделена для кеша `fielddata`, например: 10% или 512 МБ.

    1. Нажмите кнопку **Сохранить**.

- CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    1. Посмотрите описание команды CLI для изменения кластера:

        ```bash
        {{ yc-mdb-es }} cluster update --help
        ```

    1. Выполните команду, передав список настроек, которые хотите изменить:

        ```bash
        {{ yc-mdb-es }} cluster update <идентификатор или имя кластера> \
           --plugins <имя расширения 1>,...,<имя расширения N> \
           --maintenance-window type=<тип технического обслуживания: anytime или weekly>,`
                               `day=<день недели для типа weekly>,`
                               `hour=<час дня для типа weekly> \
           --deletion-protection=<защита от удаления кластера: true или false>
        ```

    Вы можете изменить следующие настройки:

    * `--plugins` — список [расширений {{ ES }}](cluster-extensions.md#elasticsearch), доступных в кластере. При этом расширения, не упомянутые в списке, будут выключены.

    * {% include [maintenance-window](../../_includes/mdb/cli/maintenance-window.md) %}

    * {% include [Deletion protection](../../_includes/mdb/cli/deletion-protection.md) %}

        {% include [Ограничения защиты от удаления кластера](../../_includes/mdb/deletion-protection-limits-data.md) %}

    Идентификатор и имя кластера можно [получить со списком кластеров в каталоге](cluster-list.md#list-clusters).

- API

    Воспользуйтесь методом [update](../api-ref/Cluster/update.md) и передайте в запросе:
    * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](./cluster-list.md#list-clusters).
    * Список [расширений {{ ES }}](cluster-extensions.md#elasticsearch) в параметре `plugins`.

    
    * Идентификатор [сервисного аккаунта](../../iam/concepts/users/service-accounts.md), используемого для работы с кластером, в параметре `serviceAccountId`.

    * {% include [maintenance-window](../../_includes/mdb/api/maintenance-window.md) %}

    * Настройки защиты от удаления кластера в параметре `deletionProtection`.

        {% include [Ограничения защиты от удаления кластера](../../_includes/mdb/deletion-protection-limits-data.md) %}

    * Список настроек, которые необходимо изменить, в параметре `updateMask`. Если не задать этот параметр, метод API сбросит на значения по умолчанию все настройки кластера, которые не были явно указаны в запросе.

    {% note warning %}

    Этот метод API сбросит все настройки кластера, которые не были явно переданы в запросе, на значения по умолчанию. Чтобы избежать этого, обязательно передайте название полей, подлежащих изменению, в параметре `updateMask`.

    {% endnote %}

{% endlist %}
