# ofd-doc

## QPOS
- ZReport
    - [GetZReportCount](#GetZReportCount)
    - [GetZReportInfo](#GetZReportInfo)
    - [OpenZReport](#OpenZReport)
    - [CloseZReport](#CloseZReport)
- Receipt
    - [SaleReceipt](#SaleReceipt)
    - [RefundReceipt](#RefundReceipt)
    - [AdvanceReceipt](#AdvanceReceipt)
- Info
    - [FiscalModulInfo](#GetInfo)
    - [SendReceipt](#SendReceipt)
    
# Вкладка ZReport
ZReport - раздел памяти ФМ который хранит суммы и кол-во пробитых чеков с даты времени начало открытия до даты времени ее закрытия.

## GetZReportCount
Кол-во открытых/закрытых (включая текущий) ZReport. При закрытии и открытии нового ZReport, увеличивается на единицу.

### Parameters

| Name   | Mandatory | Example                    | Note                  |
| ------ | --------- | -------------------------- | --------------------- |
| method | yes       | "Api.GetZReportCount"      |                       |


### Errors

| HTTP Code | Code   | Message                             | Data                      |
| --------- | ------ | ----------------------------------- | ------------------------- |
| 200       | 0      | Success                             | OK                        |

### Example requests/responses

#### Request:

```shell script
curl --location --request POST '127.0.0.1:2040/api' \
--header 'Content-Type: application/json' \
--data-raw '{
    "method": "Api.GetZReportCount"
}'
```

#### Success response:

```json
{
"result":{
        "AppletVersion":"0302",
"Count":6 },
"id":68934,
   "jsonrpc":"2.0"
}
```
| Field         | Type      | Note                                                      | 
| ------------- | --------- | --------------------------------------------------------- |
| AppletVersion | string    | "Версия апплета в ФМ"                                     |
| Count         | uint16    | "Кол-во открытых/закрытых (включая текущий) ZReport"      |


## GetZReportInfo
Получить состав ZReport по индексу указанному в поле Number. Индекс = 0 - текущий ZReport, Индекс = 1 - предыдущий ZReport, и т.д.

### Parameters

| Name   | Mandatory | Example                    | Note                  |
| ------ | --------- | -------------------------- | --------------------- |
| method | yes       | "Api.ZReportInfo"          |                       |


### Errors

| HTTP Code | Code   | Message                             | Data                      |
| --------- | ------ | ----------------------------------- | ------------------------- |
| 200       | 0      | Success                             | OK                        |

### Example requests/responses

#### Request:

```shell script
curl --location --request POST '127.0.0.1:2040/api' \
--header 'Content-Type: application/json' \
--data-raw '{
    "method": "Api.ZReportInfo",
    "params": {
        "Number": 0
    }
}'
```

#### Success response:

```json
{
    "code": 0,
    "message": "OK",
    "data": {
        "AppletVersion": "0323",
        "TotalRefundCount": 0,
        "TotalSaleVAT": 20563479,
        "TotalSaleCard": 191925800,
        "Count": 10,
        "TotalRefundCard": 0,
        "FirstReceiptSeq": "40",
        "TotalRefundCash": 0,
        "TotalSaleCash": 0,
        "OpenTime": "2023-08-22 13:01:44",
        "Number": 2,
        "CloseTime": "2023-08-28 12:10:40",
        "LastReceiptSeq": "42",
        "TotalRefundVAT": 0,
        "TerminalID": "NA000000036172",
        "TotalSaleCount": 3
    }
}
```
| Field              | Type      | Note                                                                                                   | 
| ------------------ | --------- | ------------------------------------------------------------------------------------------------------ |
| AppletVersion      | string    | "Версия апплета в ФМ"                                                                                  |
| TerminalID         | string    | "Серийный номер ФМ"                                                                                    |
| Number             | uint16    | "Порядковый номер ZReport"                                                                             |
| Count              | uint16    | "Кол-во открытых/закрытых (включая текущий) ZReport"                                                   |
| OpenTime           | string    | "Дата время открытия ZReport (формат ГГГГ-ММ-ДД ЧС-МН-СК). Если пустой, то ZReport не был открыт"      |
| CloseTime          | string    | "Дата время закрытия ZReport (формат ГГГГ-ММ-ДД ЧС-МН-СК). Если пустой, то ZReport не был закрыт"      |
| FirstReceiptSeq    | string    | "Номер первого зарегистрированного чека в данном ZReport после ее открытия"                            |
| LastReceiptSeq     | string    | "Номер последнего зарегистрированного чека в данном ZReport"                                           |
| TotalSaleCount     | uint16    | "Кол-во операций продаж в данном ZReport (от 1 до 9999)"                                               |
| TotalRefundCount   | uint16    | "Кол-во операций возврата в данном ZReport (от 1 до 9999)"                                             |
| TotalSaleCash      | uint64    | "Сумма наличности по продажам в данном ZReport (от 0 до 999999999999) в тийин"                         |
| TotalRefundCash    | uint64    | "Сумма наличности по возвратам в данном ZReport (от 0 до 999999999999) в тийин"                        |
| TotalSaleCard      | uint64    | "Сумма безналичности по продажам в данном ZReport (от 0 до 999999999999) в тийин"                      |
| TotalRefundCard    | uint64    | "Сумма безналичности по возвратам в данном ZReport (от 0 до 999999999999) в тийин"                     |
| TotalSaleVAT       | uint64    | "Сумма НДС по продажам в данном ZReport (от 0 до 999999999999) в тийин"                                |
| TotalRefundVAT     | uint64    | "Сумма НДС по возвратам в данном ZReport (от 0 до 999999999999) в тийин"                               |

## OpenZReport
Открыть ZReport передав текущее дату время

### Errors

| HTTP Code | Code   | Message                             | Data                      |
| --------- | ------ | ----------------------------------- | ------------------------- |
| 200       | 0      | Success                             | OK                        |

### Example requests/responses

#### Request:

```shell script
curl --location --request POST '127.0.0.1:2040/api' \
--header 'Content-Type: application/json' \
--data-raw '{
    "method": "Api.OpenZReport"
}'
```

#### Success response:

```json
{
    "code": 0,
    "message": "OK",
    "data": "Z-Report succesfuly open!"
}
```

#### Error response:

```json
{
    "code": 50021,
    "message": "ERROR_ZREPORT_IS_ALREADY_OPEN",
    "data": "Please, close Z-Report firstly!"
}
```

## CloseZReport
Закрыть ZReport передав текущее дату время

### Parameters

| Name   | Mandatory | Example                    | Note                  |
| ------ | --------- | -------------------------- | --------------------- |
| method | yes       | "Api.CloseZReport"         |                       |


### Errors

| HTTP Code | Code   | Message                             | Data                      |
| --------- | ------ | ----------------------------------- | ------------------------- |
| 200       | 0      | Success                             | OK                        |

### Example requests/responses

#### Request:

```shell script
curl --location --request POST '176.96.254.153:2040/api' \
--header 'Content-Type: application/json' \
--data-raw '{
    "method": "Api.CloseZReport"
}'
```

#### Success response:

```json
{
    "code": 0,
    "message": "OK",
    "data": {
        "AppletVersion": "0324",
        "UnsentZReportsCount": 1,
        "ErrorsCount": 0,
        "AwaitingToSendZReportsCount": 1
    }
}
```

| Field                        | Type      | Note                                                                                                   | 
| ---------------------------- | --------- | ------------------------------------------------------------------------------------------------------ |
| AppletVersion                | string    | "Версия апплета в ФМ"                                                                                  |
| UnsentZReportsCount          | uint16    | "Кол-во неотправленных в ОФД ZReport"                                                                  |
| AwaitingToSendZReportsCount  | uint16    | "Кол-во ожидающих отправки в ОФД ZReport"                                                              |
| ErrorsCount                  | uint16    | "Кол-во ошибок при формировании файлов ZReport для отправки в ОФД"                                     |

#### Error response:

```json
{
    "code": 50021,
    "message": "ERROR_CURRENT_ZREPORT_IS_EMPTY",
    "data": "Z-Report empty or already closed!"
}
```

# Вкладка SaleReceipt
SaleReceipt - чек который хранит всю информацию о товарах/услугах и сумму. SaleReceipt не хранится в памяти ФМ, после регистрации суммы чека формируется файл SaleReceipt и сохраняется в файле БД. ФМ хранит только сумму чека (без информации о товарах/услугах) после ее регистрации.

## SaleReceipt
Формирует SaleReceipt файл чека от продажи и сохраняет в БД, регистрирует сумму чека в ФМ, возвращает порядковый номер и фискальный признак чека. Сервис FiscalDriveAPI по истечения определенного времени отправляет FullReceipt файлы на сервер ОФД, получает файл подтверждения и передает его в ФМ для удаления чека из ее памяти.

### Parameters

| Name                    | Mandatory | Example                    | Note                                                                       |
| ----------------------- | --------- | -------------------------- | -------------------------------------------------------------------------- |
| method                  | yes       | "SaleReceipt"              |                                                                            |
| CashierName             | yes       | "Имоилова Шахло"           | String - Кассир                                                            |
|                                    RECEIPT                                                                                                    |
| ReceiptNumber           | yes       | "0"                        | string - Должна быть уникальной. При повторе вернёт тот же qr_url          |
| ReceivedCash            | yes       | 0                          | uint64 - Наличные в тийин                                                  |
| ReceivedCard            | yes       | 10000                      | uint64 - Безналичные в тийин                                               |
| Time                    | yes       | "2023-06-16 17:51:15"      | string - Формат YYYY-MM-DD HH:MM:SS                                        |
|                                    LOCATION                                                                                                   |
| Latitude                | yes       | 41.29659100918041          | float64 - Широта                                                           |
| Longitude               | yes       | 69.21791944847777          | float64 - Долгота                                                          |
|                                    ExtraInfo                                                                                                  |
| CardType                | no        | 1 - Корп., 2 - Физ.        | uint64                                                                     |
|                                    ITEM                                                                                                       |
| Name                    | yes       | "TEST ITEM"                | String - Наименование товара/услуги                                        |
| Barcode                 | yes       | "4712012121213"            | String - Товарный код                                                      |
| SPIC                    | yes       | "02106999999000000"        | String - ИКПУ                                                              |
| Label                   | yes       | "01047120121212130121"     | String - Код маркировки                                                    |
| Units                   | yes       | 1                          | uint64 - Ед. измерения                                                     |
| PackageCode             | yes       | "57870871489456137366"     | String - Код упаковки                                                      |
| Amount                  | yes       | 2000                       | uint64 - Кол-во                                                            |
| Price                   | yes       | 560875                     | uint64 - Сумма без скидки                                                  |
| Discount                | yes       | 186958                     | uint64 - Скидка                                                            |
| VATPercent              | yes       | 12                         | uint8  - % НДС                                                             |
| VAT                     | yes       | 560875                     | uint64 - Сумма НДС                                                         |
| OwnerType               | yes       | 0                          | uint64                                                                     |
| CommissionInfo          | yes       | {}                         | Object - Признак комиссионного товара                                      |
|                                    CommissionInfo                                                                                             |
| TIN                     | yes       | "123456789"                | String - ИНН                                                               |
| PINFL                   | yes       | "12345678901234"           | String - ПИНФЛ                                                             |

❗️ OwnerType maydonida aks ettirilgan belgilar quyidagi ma’lumotlarni ifodalaydi:
• 0 (nol) – “Sotib olib, sotayotgan”;  
• 1 (bir) – “O‘zi ishlab chiqargan”; 
• 2 (ikki) – “Xizmat ko‘rsatuvchi”.
• Bo'sh jo'natilsa avtomat ravishda 99 shakllanib yuboriladi

<img width="847" alt="Screenshot 2023-09-24 at 14 57 29" src="https://github.com/qurash98/ofd_doc/assets/32934268/42f11f40-01e5-4d78-b1fd-0e8ec96dbf4a">


### Errors

| HTTP Code | Code   | Message                             | Data                      |
| --------- | ------ | ----------------------------------- | ------------------------- |
| 200       | 0      | Success                             | OK                        |

### Example requests/responses

#### Request:

```
{
    "method": "SaleReceipt",
    "params": {
        "Cashier": "Имоилова Шахло",
        "ReceiptNumber": "138",
        "ReceivedCard": 0,
        "ReceivedCash": 19000,
        "Time": "2025-12-24 11:34:34",
        "Location": {
            "Latitude": 40.121601,
            "Longitude": 65.368889
        },
        "ExtraInfo": {
            "CardType": 1
        },
        "Items": [
            {
                "Name": "200 АСКОРБИНКА С ГЛЮКОЗОЙ + СА, ТАБ. №1",
                "SPIC": "02106999028144288",
                "Barcode": "4780046713435",
                "Label": "",
                "Units": 1,
                "PackageCode": "1334671",
                "OwnerType": 0,
                "Amount": 1000,
                "VATPercent": 12,
                "Price": 20000,
                "Discount": 1000,
                "Other": 0,
                "VAT": 2036
            }
        ]
    }
}
```

#### Success response:

```json
{
    "code": 0,
    "message": "OK",
    "data": {
       "AppletVersion":"0302",
        "QRCodeURL":"https:\/\/ofd.soliq.uz\/check?t=ZZ9...&s=345221994543",
        "TerminalID":"ZZ999999999956",
        "ReceiptSeq":"280",
        "DateTime":"20201114170239",
        "FiscalSign":"345221994543"
    }
}
```
| Field         | Type      | Note                                                      | 
| ------------- | --------- | --------------------------------------------------------- |
| AppletVersion | string    | "Версия апплета в ФМ"                                     |
| TerminalID    | string    | "Серийный номер ФМ"                                       |
| ReceiptSeq    | string    | "Номер чека"                                              |
| DateTime      | string    | "Время регистрации чека (формат ГГГГММДДЧСМНСК)"          |
| FiscalSign    | string    | "Фискальный признак чека"                                 |


## RefundReceipt

### Parameters

| Name   | Mandatory | Example                    | Note                  |
| ------ | --------- | -------------------------- | --------------------- |
| method | yes       | "RefundReceipt"            |                       |


### Errors

| HTTP Code | Code   | Message                             | Data                      |
| --------- | ------ | ----------------------------------- | ------------------------- |
| 200       | 0      | Success                             | OK                        |

### Example requests/responses

#### Request:

```
{
    "method": "RefundReceipt",
    "params": {
        "ReceiptNumber": "138",
        "Cashier": "Имоилова Шахло",
        "ReceivedCash": 300000,
        "Time": "2023-08-29 16:56:42",
        "Items": [
            {
                "SPIC": "03808001001000000",
                "VATPercent": 12,
                "Discount": 0,
                "Price": 300000,
                "CommissionInfo": {
                    "PINFL": "",
                    "TIN": ""
                },
                "Barcode": "2000030000009",
                "Amount": 1000,
                "VAT": 0,
                "Units": 1476512,
                "Name": "EEEEEE EEEEEE EE EEEEEEE EEEEEEEE EEEEE",
                "Other": 0
            }
        ],
        "ReceivedCard": 0,
        "Location": {
            "Latitude": 41.29659100918041,
            "Longitude": 69.21791944847777
        },
        "RefundInfo": {
            "TerminalID": "NA000000036172",
            "ReceiptSeq": "49",
            "DateTime": "20230829163133",
            "FiscalSign": "155580527246"
        }
    }
}
```

#### Success response:

```json
{
    "code": 0,
    "message": "OK",
    "data": {
       "AppletVersion":"0302",
        "QRCodeURL":"https:\/\/ofd.soliq.uz\/check?t=ZZ9...&s=345221994543",
        "TerminalID":"ZZ999999999956",
        "ReceiptSeq":"280",
        "DateTime":"20201114170239",
        "FiscalSign":"345221994543"
    }
}
```
| Field         | Type      | Note                                                      | 
| ------------- | --------- | --------------------------------------------------------- |
| AppletVersion | string    | "Версия апплета в ФМ"                                     |
| Count         | uint16    | "Кол-во открытых/закрытых (включая текущий) ZReport"      |

# Вкладка AdvanceReceipt
JSON RPC v2 запрос аналогичен SaleReceipt ([SaleReceipt](#SaleReceipt)), но поле metod должно содержать значение
“AdvanceReceipt”

## AdvanceReceipt
Формирует FullReceipt файл авансового чека. Т .к. чек не регистрируется в ФМ, то
в ответе не будет номера чека и фискального признака.

### Parameters

| Name                    | Mandatory | Example                    | Note                                                                       |
| ----------------------- | --------- | -------------------------- | -------------------------------------------------------------------------- |
| method                  | yes       | "AdvanceReceipt"              |                                                                            |
|                                    RECEIPT                                                                                                    |
| ReceivedCash            | yes       | 0                          | uint64 - Наличная сумма полученная от продажи в тийин                      |
| ReceivedCard            | yes       | 10000                      | uint64 - Безналичная сумма полученная от продажи в тийин                   |
| Time                    | yes       | "2023-06-16 17:51:15"      | string - Текущая дата, время регистрации чека (формат ГГГГ-ММ-ДД ЧС-МН-СК) |
|                                    LOCATION                                                                                                   |
| Latitude                | yes       | 41.29659100918041          | float64 - Широта                                                           |
| Longitude               | yes       | 69.21791944847777          | float64 - Долгота                                                          |
|                                    ExtraInfo                                                                                                  |
| CardType                | no        | 1 - Корп., 2 - Физ.        | uint64                                                                     |
|                                    ITEM                                                                                                       |
| Name                    | yes       | "TEST ITEM"                | String - Наименование товара или услуги                                    |
| Barcode                 | yes       | "4712012121213"            | String - Товарный код                                                      |
| SPIC                    | yes       | "02106999999000000"        | String - ИКПУ                                                              |
| Label                   | yes       | "01047120121212130121"     | String - Код маркировки                                                    |
| Units                   | yes       | 1                          | uint64 - Единица измерения                                                 |
| PackageCode             | yes       | "57870871489456137366"     | String - Код упаковки                                                      |
| Amount                  | yes       | 2000                       | uint64 - Количество товара                                                 |
| Price                   | yes       | 560875                     | uint64 - Общая сумма позиции без учета скидок                              |
| Discount                | yes       | 186958                     | uint64 - Cкидка                                                            |
| VATPercent              | yes       | 12                         | uint8  - % НДС                                                             |
| VAT                     | yes       | 560875                     | uint64 - НДС сумма                                                         |
| OwnerType               | yes       | 0                          | uint64 - OwnerType                                                         |
| CommissionInfo          | yes       | {}                         | Object - Признак комиссионного товара/услуги                               |
|                                    CommissionInfo                                                                                             |
| TIN                     | yes       | {}                         | String - ИНН                                                               |
| PINFL                   | yes       | {}                         | String - ПИНФЛ                                                             |

❗️ OwnerType maydonida aks ettirilgan belgilar quyidagi ma’lumotlarni ifodalaydi:
• 0 (nol) – “Sotib olib, sotayotgan”;  
• 1 (bir) – “O‘zi ishlab chiqargan”; 
• 2 (ikki) – “Xizmat ko‘rsatuvchi”.
• Bo'sh jo'natilsa avtomat ravishda 99 shakllanib yuboriladi

<img width="847" alt="Screenshot 2023-09-24 at 14 57 29" src="https://github.com/qurash98/ofd_doc/assets/32934268/42f11f40-01e5-4d78-b1fd-0e8ec96dbf4a">


### Errors

| HTTP Code | Code   | Message                             | Data                      |
| --------- | ------ | ----------------------------------- | ------------------------- |
| 200       | 0      | Success                             | OK                        |

### Example requests/responses

#### Request:

```
{
    "method": "AdvanceReceipt",
    "params": {
        "ReceivedCash": 0,
        "ReceivedCard": 100000,
        "Time": "2025-07-17 12:53:15",
        "Location": {
            "Latitude": 41.29659100918041,
            "Longitude": 69.21791944847777
        },
        "ExtraInfo": {
            "CardType": 1
        },
        "Items": [
            {
                "Name": "TEST LOGIX",
                "Barcode": "2000291000008",
                "SPIC": "02106999999000000",
                "Units": 1,
                "PackageCode": "57870871489456137366",
                "VATPercent": 12,
                "Discount": 0,
                "Price": 100000,
                "CommissionInfo": {
                    "PINFL": "",
                    "TIN": ""
                },
                "Amount": 2,
                "VAT": 1,
                "Other": 1,
                "OwnerType": 0
            }
        ]
    }
}
```

#### Success response:

```json
{
    "code": 0,
    "message": "OK",
    "data": {
        "AppletVersion": "0322",
        "QRCodeURL": "",
        "TerminalID": "UZ210317201079",
        "ReceiptSeq": "",
        "DateTime": "2025-07-17 12:53:15",
        "FiscalSign": ""
    }
}
```
| Field         | Type      | Note                                                      | 
| ------------- | --------- | --------------------------------------------------------- |
| AppletVersion | string    | "Версия апплета в ФМ"                                     |
| TerminalID    | string    | "Серийный номер ФМ"                                       |
| DateTime      | string    | "Время регистрации чека (формат ГГГГММДДЧСМНСК)"          |

## GetInfo
Возвращается подробная информация о фискальном модуле.

### Parameters

| Name   | Mandatory | Example                    | Note                  |
| ------ | --------- | -------------------------- | --------------------- |
| method | yes       | "Api.GetInfo"              |                       |


### Errors

| HTTP Code | Code   | Message                             | Data                      |
| --------- | ------ | ----------------------------------- | ------------------------- |
| 200       | 0      | Success                             | OK                        |

### Example requests/responses

#### Request:

```shell script
curl --location --request POST '127.0.0.1:2040/api' \
--header 'Content-Type: application/json' \
--data-raw '{
    "method": "Api.GetInfo"
}'
```

#### Success response:

```json
{
    "code": 0,
    "message": "OK",
    "data": {
        "TerminalID": "UZ210317263976",
        "AppletVersion": "0324",
        "CurrentTime": "2026-01-20 18:18:46",
        "CurrentReceiptSeq": "2515",
        "ReceiptCount": 0,
        "ReceiptMaxCount": 480,
        "ZReportCount": 228,
        "ZReportMaxCount": 640,
        "AvailablePersistentMemory": 10356,
        "AvailableResetMemory": 709,
        "AvailableDeselectMemory": 709
    }
}
```
| Field                | Type   | Description (RU)                                                                                                                     |
|----------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------|
| AppletVersion        | string | Версия апплета в ФМ                                                                                                                  |
| TerminalID           | string | Сер.№ ФМ где был зарегистрирован отзываемый чек                                                                                      |
| CurrentReceiptSeq    | string | Номер последнего зарегистрированного чека                                                                                            |
| CurrentTime          | string | Дата время последней операции ФМ (формат ГГГГ-ММ-ДД ЧС-МН-СК). Последующая дата должна быть хотя бы на 1 секунду позже               |
| ReceiptCount         | uint16 | Кол-во не отправленных чеков в ОФД                                                                                                   |
| ReceiptMaxCount      | uint16 | Вместимость для неотправленных чеков в памяти ФМ                                                                                     |
| ZReportCount         | uint16 | Кол-во открытых/закрытых (включая текущий) ZReport                                                                                   |
| ZReportMaxCount      | uint16 | Максимальный номер ZReport, по достижению которого больше нельзя будет открыть новый ZReport                                         |
| AvailableXXXXMemory  | uint16 | Свободная память ФМ                                                                                                                  |

## SendReceipt
Отправка (суммы) чека из памяти ФМ на сервер ОФД. Функция вызывается только
в том случае если файл БД хранящий FullReceipt стерся или был поврежден. Если
чек не будет отправлен в течении 2 дней то ФМ заблокируется. Функция
отправляет чек в ОФД, получает файл подтверждения и передает его в ФМ, тем
самым разблокирует ФМ.

### Parameters

| Name   | Mandatory | Example                    | Note                  |
| ------ | --------- | -------------------------- | --------------------- |
| method | yes       | "Api.SendReceipt"          |                       |


### Errors

| HTTP Code | Code   | Message                             | Data                      |
| --------- | ------ | ----------------------------------- | ------------------------- |
| 200       | 0      | Success                             | OK                        |

### Example requests/responses

#### Request:

```shell script
curl --location '127.0.0.1:2040/api' \
--header 'Content-Type: application/json' \
--data '{
    "method": "Api.SendReceipt"
}'
```

#### Success response:

```json
{
    "code": 0,
    "message": "OK",
    "data": {
        "success": true,
        "data": {
            "AppletVersion": "0324",
            "QueuedToSendCount": 0
        },
        "apiTime": "142 ms"
    }
}
```
| Field             | Type   | Description (RU)                                                                                                                                           |
|-------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AppletVersion     | string | Версия апплета в ФМ                                                                                                                                        |
| QueuedToSendCount | uint16 | Кол-во сформированных файлов чеков, ожидающих отправки                                                                                                     |
| apiTime           | string | Значение поля `apiTime` желательно отображать в интерфейсе. Если время превышает 200 мс, рекомендуется повторять запрос, пока значение не станет ниже порога или пока не будет достигнут лимит попыток. |

