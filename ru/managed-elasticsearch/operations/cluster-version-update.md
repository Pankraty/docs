# Изменение версии и редакции {{ ES }}

В кластере {{ mes-name }} вы можете [обновить версию](#version-update) и [изменить редакцию](#start-edition-update) {{ ES }}.

## Обновление версии {{ ES }} {#version-update}

Вы можете обновить кластер {{ mes-name }} до более новой [версии {{ ES }}](../concepts/update-policy.md#versioning-policy).

### Узнать доступные версии {{ ES }} {#version-list}

{% list tabs %}

- Консоль управления

    1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mes-name }}**.
    1. Выберите кластер и нажмите кнопку **Редактировать**.
    1. Откройте список в поле **Версия**.

{% endlist %}

### Перед обновлением версии {#before-version-update}

Перед обновлением версии {{ ES }} убедитесь, что это не нарушит работу ваших приложений:

* Просмотрите [историю изменений](https://www.elastic.co/downloads/past-releases#elasticsearch) {{ ES }}, чтобы узнать, как обновления могут повлиять на работу ваших приложений.
* Попробуйте обновить версию на тестовом кластере. Тестовый кластер можно развернуть из резервной копии основного кластера.
* [Создайте резервную копию](cluster-backups.md#create-backup) кластера непосредственно перед обновлением версии.

### Обновить версию {{ ES }} {#start-version-update}

{% list tabs %}

- Консоль управления

    1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mes-name }}**.
    1. Выберите кластер и нажмите кнопку **Редактировать**.
    1. В поле **Версия** выберите нужную версию {{ ES }}.
    1. Нажмите кнопку **Сохранить**.

- CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    1. Получите список ваших кластеров {{ ES }}:

        ```bash
        {{ yc-mdb-es }} cluster list
        ```

    1. Получите информацию о нужном кластере и проверьте версию в свойстве `config.version`:

        ```bash
        {{ yc-mdb-es }} cluster get <имя или идентификатор кластера>
        ```

    1. Запустите изменение версии:

        ```bash
        {{ yc-mdb-es }} cluster update <имя или идентификатор кластера> --version <версия {{ ES }}>
        ```

- API

    Воспользуйтесь методом API [update](../api-ref/Cluster/update.md) и передайте в запросе:

    * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
    * Новую версию {{ ES }} в параметре `configSpec.version`.
    * Список полей конфигурации кластера, подлежащих изменению (в данном случае — `configSpec.version`) в параметре `updateMask`.

    {% note warning %}

    Этот метод API сбросит все настройки кластера, которые не были явно переданы в запросе, на значения по умолчанию. Чтобы избежать этого, перечислите настройки, которые вы хотите изменить, в параметре `updateMask` (одной строкой через запятую).

    {% endnote %}

{% endlist %}

## Изменить редакцию {{ ES }} {#start-edition-update}

Вы можете изменить используемую кластером [редакцию {{ ES }}](../concepts/es-editions.md). Прежде чем понижать используемую редакцию, убедитесь, что сокращение функциональных возможностей не нарушит работу ваших приложений.

{% list tabs %}

- Консоль управления

    1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ mes-name }}**.
    1. Выберите кластер и нажмите кнопку **Редактировать**.
    1. В поле **Редакция** выберите нужную редакцию {{ ES }}: `Basic`, `Gold` или `Platinum`.
    1. Нажмите кнопку **Сохранить**.

- CLI

    {% include [cli-install](../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../_includes/default-catalogue.md) %}

    1. Получите список ваших кластеров {{ ES }}:

        ```bash
        {{ yc-mdb-es }} cluster list
        ```

    1. Получите информацию о нужном кластере и проверьте редакцию в свойстве `config.edition`:

        ```bash
        {{ yc-mdb-es }} cluster get <имя или идентификатор кластера>
        ```

    1. Запустите изменение редакции:

        ```bash
        {{ yc-mdb-es }} cluster update <имя или идентификатор кластера> \
           --edition <редакция {{ ES }}: basic, gold или platinum>
        ```

- Terraform

    1. Откройте актуальный конфигурационный файл {{ TF }} с планом инфраструктуры.

        О том, как создать такой файл, см. в разделе [{#T}](cluster-create.md).

    1. Добавьте к описанию кластера {{ mes-name }} поле `config.edition` или измените его значение, если поле уже существует:

        ```hcl
        resource "yandex_mdb_elasticsearch_cluster" "<имя кластера>" {
          ...
          config {
            edition = "<редакция {{ ES }}: basic, gold или platinum>"
            ...
          }
          ...
        }
        ```

    1. Проверьте корректность настроек.

        {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

    1. Подтвердите изменение ресурсов.

        {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

    Подробнее см. в [документации провайдера {{ TF }}](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/mdb_elasticsearch_cluster).

- API

    Воспользуйтесь методом API [update](../api-ref/Cluster/update.md) и передайте в запросе:

    * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
    * Новую редакцию {{ ES }} в параметре `configSpec.edition`.
    * Список полей конфигурации кластера, подлежащих изменению (в данном случае — `configSpec.edition`) в параметре `updateMask`.

    {% note warning %}

    Этот метод API сбросит все настройки кластера, которые не были явно переданы в запросе, на значения по умолчанию. Чтобы избежать этого, перечислите настройки, которые вы хотите изменить, в параметре `updateMask` (одной строкой через запятую).

    {% endnote %}

{% endlist %}
