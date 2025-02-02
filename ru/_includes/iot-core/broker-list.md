{% list tabs %}

- Консоль управления

  1. В [консоли управления]({{ link-console-main }}) выберите каталог, в котором хотите получить список брокеров.
  1. Выберите сервис **{{ iot-short-name }}**.
  1. На панели слева выберите **Брокеры**. Список брокеров отобразится на странице **Брокеры**.

- CLI

  {% include [cli-install](../cli-install.md) %}
  
  {% include [default-catalogue](../default-catalogue.md) %}

  Получите список брокеров в каталоге:

  ```
  yc iot broker list
  ```

  Результат:

  ```
  +----------------------+-------------+
  |          ID          |    NAME     |
  +----------------------+-------------+
  | b91ki3851h********** | my-broker   |
  +----------------------+-------------+
  ```

- API

  Чтобы получить список брокеров, воспользуйтесь методом REST API [list](../../iot-core/broker/api-ref/Broker/list.md) для ресурса [Broker](../../iot-core/broker/api-ref/Broker/index.md) или вызовом gRPC API [BrokerService/List](../../iot-core/broker/api-ref/grpc/broker_service.md#List).

{% endlist %}