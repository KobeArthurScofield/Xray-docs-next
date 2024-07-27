# Мониторинг подключений

Компонент мониторинга подключений использует HTTP-пинги для проверки состояния подключения исходящих прокси.  Результаты мониторинга могут использоваться другими компонентами, например, балансировщиком нагрузки.  
В настоящее время доступны два режима: [observatory](#observatoryobject) (фоновый мониторинг подключений) и [burstObservatory](#burstobservatoryobject) (мониторинг параллельных подключений).  
Выберите один из них в соответствии с вашими потребностями.

## ObservatoryObject

```json
{
  "subjectSelector":[
    "outbound"
  ],
  "probeUrl": "https://www.google.com/generate_204",
  "probeInterval": "10s",
  "enableConcurrency": false
}
```

> `subjectSelector`: \[ string \]

Массив строк, каждый элемент которого будет использоваться для сопоставления с префиксом тега исходящего подключения.  
Например, для следующих тегов исходящих подключений: `[ "a", "ab", "c", "ba" ]`, `"subjectSelector": ["a"]` будет соответствовать `[ "a", "ab" ]`.

> `probeUrl`: string

URL-адрес, используемый для проверки состояния подключения исходящего прокси.

> `probeInterval`: string

Интервал между проверками.  
Формат времени: число + единица измерения, например `"10s"`, `"2h45m"`.  
Поддерживаемые единицы измерения: `ns`, `us`, `ms`, `s`, `m`, `h` (наносекунды, микросекунды, миллисекунды, секунды, минуты, часы).

> `enableConcurrency`: true | false

- `true` - проверять все соответствующие исходящие прокси одновременно, после чего сделать паузу на время, указанное в `probeInterval`.
- `false` - проверять соответствующие исходящие прокси по очереди, делая паузу на время, указанное в `probeInterval`, после проверки каждого прокси.

## BurstObservatoryObject

```json
{
  "subjectSelector":[
    "outbound"
  ],
  "pingConfig": {}
}
```

> `subjectSelector`: \[ string \]

Массив строк, каждый элемент которого будет использоваться для сопоставления с префиксом тега исходящего подключения.  
Например, для следующих тегов исходящих подключений: `[ "a", "ab", "c", "ba" ]`, `"subjectSelector": ["a"]` будет соответствовать `[ "a", "ab" ]`.

> `pingConfig`: [PingConfigObject](#PingConfigObject)


### PingConfigObject

```json
{
  "destination": "https://connectivitycheck.gstatic.com/generate_204",
  "connectivity": "",
  "interval": "1h",
  "sampling": 3,
  "timeout": "30s"
}
```

> `destination`: string

URL-адрес, используемый для проверки состояния подключения исходящего прокси.  
Этот URL-адрес должен возвращать код состояния HTTP 204.

> `connectivity`: string

URL-адрес, используемый для проверки подключения к локальной сети.  
Пустая строка означает, что проверка подключения к локальной сети не выполняется.

> `interval`: string

Проверить все соответствующие исходящие прокси в течение указанного времени, отправляя `sampling` + 1 запросов на каждый прокси.  
Формат времени: число + единица измерения, например `"10s"`, `"2h45m"`.  
Поддерживаемые единицы измерения: `ns`, `us`, `ms`, `s`, `m`, `h` (наносекунды, микросекунды, миллисекунды, секунды, минуты, часы).

> `sampling`: number

Количество последних результатов проверок, которые нужно сохранить.

> `timeout`: string

Время ожидания ответа при проверке.  
Формат такой же, как и у `interval`.





