# Connecting to a {{ CH }} database

In the {{ yandex-cloud }} infrastructure, [{{ CH }}](https://{{ ch-domain }}/) server clusters are deployed and maintained using the {{ mch-short-name }} service.

To utilize a {{ mch-short-name }} cluster host as a data source for {{ ml-platform-name }}:

1. [Create](../../../managed-clickhouse/operations/cluster-create.md) a new {{ mch-short-name }} cluster with public access for the host. You can also use an existing cluster with publicly available hosts.
1. [Configure](../../../managed-clickhouse/operations/connect.md#configuring-security-groups) cluster security groups.

## Before you start {#before-begin}

If a project is already open, open the tab with a notebook.

If not, open the project:

1. {% include [include](../../../_includes/datasphere/first-step.md) %}
1. Go to the **Projects** tab.
1. Select the project you want to open and click ![image](../../../_assets/datalens/horizontal-ellipsis.svg).
1. Choose **Open** and wait for the project to open.

## Connecting to a {#connect-to-host} host

To connect to {{ mch-short-name }} cluster hosts:

1. Get an SSL certificate: To do this, enter the following command in a notebook cell:

    ```bash
    #!:bash
    mkdir ~/.clickhouse-client
    wget "https://storage.yandexcloud.net/cloud-certs/CA.pem" -O ~/.clickhouse-client/root.crt && \
    chmod 0600 ~/.clickhouse-client/root.crt
    ```

1. Establish a connection to the database. To do this, enter the following command in a notebook cell:

    {% list tabs %}

    - Using the requests library

      ```python
      import requests
      url = 'https://{host}:8443/?database={db}&query={query}'.format(
              host='<ClickHouse host FQDN >',
              db='<DB name>',
              query='SELECT version()')
      auth = {
              'X-ClickHouse-User': '<DB username>',
              'X-ClickHouse-Key': '<DB user password>',
          }
      cacert = '/home/jupyter/.clickhouse-client/root.crt'
      rs = requests.get(url, headers=auth, verify=cacert)
      rs.raise_for_status()
      print(rs.text)
      ```

      A successful cluster connection and test query will display the {{ CH }} version:

      ```
      21.3.13.9
      ```

    - Using the clickhouse-driver library

      ```python
      from clickhouse_driver import Client
      client = Client(host='<ClickHouse host FQDN>', 
                      user='<DB username>', 
                      password='<DB user password>', 
                      database='<DB name>', 
                      secure=True)
      client.execute('SELECT version()')
      ```

      A successful cluster connection and test query will display the {{ CH }} version:

      ```
      [('21.3.13.9',)]
      ```

    {% endlist %}
