# ECHONET Lite WebAPI (EL-WebAPI) Device Description

|Date|Version |Description|
|:-----------|:-----|:-----|
|2017.12.11|version 1.0.0||
|2017.12.17|version 1.0.1|Device DescriptionのDataのvalueの表記をobjectからarrayに変更<br>Data Typeを修正(levelとpercentageを削除）|
|2017.12.18|version 1.0.2|Property nameに関する軽微な修正（共通項目の"Operating Status" -> "Operation Status"|
|2017.12.20|version 1.0.3|data type "level" の復活<br>電気温水器のnumberOfEnergyShiftsに"unit"を追加<br>電気錠のislocked 開錠->解錠<br>クッキングヒーターの過熱状態を修正 key->object<br>低圧スマート電力量メータの瞬時電流計測値のdata type修正 integer -> number<br>高圧スマート電力量メータの定時需要電力（30分平均電力）の"unit"修正 kWh->kW<br>電気自動車充電器のDescription修正 電気自動車充放電器 -> 電気自動車充電器|
|2018.01.30|version 1.0.4|Data Type "key" の名前を "enum" に修正|
|2018.01.30|version 1.0.5|Typo修正　instataneous -> instantaneous|
|2018.02.13|version 1.0.6|湿度センサの data type: percentage -> integer<br>Arrayのmember名elementをdataに変更|
|2018.03.08|version 1.0.7|Data type levelを廃止<br>Data type integerをnumberに統合<br>Data type numberにproperty "minimumDigit"を追加|
|2018.03.09|version 1.0.8|eoj, epc, edtを追加|
|2018.03.13|version 1.0.9|Data type objectのmember名fieldをelementsに変更<br>Data type名dateをdate-timeに変更|
|2018.03.14|version 1.0.10|eventsを削除|
|2018.03.15|version 1.0.11|複数queryに対応するためqueryをarrayにする<br>低圧スマート電力量メータの内容を修正<br>高圧スマート電力量メータの内容を修正<br>燃料電池の内容を修正<br>蓄電池の内容を修正<br>電気自動車充放電器の内容を修正<br>拡張照明システムを追加<br>温度の"unit"を"℃"から"Celsius"に変更|
|2018.04.03|version 1.0.12|Common, 家庭用エアコン、電気温水器、瞬間式給湯器、燃料電池、蓄電池、電気自動車充放電器、低圧スマート電力量メータ、高圧スマート電力量メータ、一般照明、電気自動車充電器、拡張照明システムの一部修正|

## 1. Abstract
　このドキュメントはECHONET Lite WebAPI(EL-WebAPI)のDevice Descriptionを記述する。Device Descriptionの定義は __ECHONET Lite WebAPI Specification__ を参照のこと。

### 1.1 JSON file
　ECHONET Lite WebAPI Device DescriptionはJSON fileで提供される。JSON fileの構成は以下の通り。

Format  

```
{
    "metaData": {
        "date":<date>,
        "release":<Release情報>
    },
    "common": {
          <device description>
    },
    "devices":[
        { <device description> },
        { <device description> },
        ...
    ]
}
```

|Property|Type|Required|Description| Example|
|:-----------|:-----|:-----|:-----|:-----|
|metaData|object|yes|ECHONET Lite WebAPI device description fileのmeta data||
|metaData.date|string|yes|Fileの作成日 "yyyy-mm-dd"|"2017-10-12"|
|metaData.release|string|yes|ECHONET機器オブジェクト詳細規定のRelease情報|"J"|
|common|object|yes|device description objectのformatでEPC=0x80から0x9Fの情報を記述する||
|devices|array|yes|device description objectのformatで各機器の情報を記述する||

Example

```
{
    "metaData": {
        "date":"2017-10-12",
        "release":"J"
    },
    "common": {
          <device description of common>
    },
    "devices": [
        { <device description of Temperature Sensor> },
        { <device description of Home Air Conditioner> },
        ...
    ]
}
```


## 2. Scope
　このドキュメントで記述する対象機器は、重点８機器とECHONET Lite認証取得製品が存在する機器を前提とする。このドキュメントで記述するPropertyに関しては、ECHONET Liteで必須とされているものを前提とする。対象とするECHONET 機器オブジェクト詳細規定のバージョンは Release Jとする。

## 3. Sample

以下に一般照明のDevice Descriptionを例として示す。

__Example__

```
{
    "type":"generalLighting",
    "eoj":"0x0290",
    "description":{"ja":"一般照明", "en":"General Lighting"},
    "properties":[
        {
            "name":"operationStatus",
            "epc":"0x80",
            "description":{ "ja":"動作状態", "en":"Operation Status" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"ON", "en":"ON", "edt":"0x30"},
                    {"value":false, "ja":"OFF", "en":"OFF", "edt":"0x31"}
                ]
            }
        },
        {
            "name":"isAtFault",
            "epc":"0x88",
            "description":{ "ja":"異常発生状態", "en":"Fault Status" },
            "writable":false,
            "observable":true,
            "data":{
                "type":"boolean",
                "value":[
                    {"value":true, "ja":"異常あり", "en":"Fault", "edt":"0x41"},
                    {"value":false, "ja":"異常無し", "en":"No Fault", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"brightness",
            "epc":"0xB0",
            "description":{ "ja":"輝度", "en":"Brightness" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"operationMode",
            "epc":"0xB6",
            "description":{ "ja":"動作モード", "en":"Operating Mode" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"auto", "ja":"自動", "en":"Auto Lighting", "edt":"0x41"},
                    {"value":"normal", "ja":"通常灯", "en":"Normal Lighting", "edt":"0x42"},
                    {"value":"night", "ja":"常夜灯", "en":"Night Lighting", "edt":"0x43"},
                    {"value":"color", "ja":"カラー灯", "en":"Color Lighting", "edt":"0x45"}
                ]
            }
        },
        {
            "name":"rgb",
            "epc":"0xC0",
            "description":{ "ja":"RGB設定", "en":"RGB Value" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"r",
                        "description":{ "ja":"赤", "en":"Red" },
                        "data":{
                            "type":"number",
                            "minimum":0,
                            "maximum":255
                        }
                    },
                    {
                        "name":"r",
                        "description":{ "ja":"赤", "en":"Red" },
                        "data":{
                            "type":"number",
                            "minimum":0,
                            "maximum":255
                        }
                    },
                    {
                        "name":"r",
                        "description":{ "ja":"赤", "en":"Red" },
                        "data":{
                            "type":"number",
                            "minimum":0,
                            "maximum":255
                        }
                    }
                ]
            }
        }
    ],
    "actions":[]
}
```

## 4. Device Description

### 4.1 共通項目  
　全ての機器で共通なpropertyを記述する。  

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| operationStatus |GET, PUT|boolean|0x80|動作状態<br>Operation Status||
|installationLocation|GET|string|0x81|設置場所<br>Installation location||
|instantaneousPowerConsumption|GET|number|0x84|瞬時消費電力計測値<br>Measured instantaneous power consumption||
|integralPowerConsumption|GET|number|0x85|積算消費電力計測値<br>Measured cumalative power consumption||
|manufactureFaultCode|GET|string|0x86|メーカー異常コード<br>Manufacture fault code||
|isAtFault|GET|boolean|0x88|異常発生状態<br>Fault status||
|productCode|GET|string|0x8C|商品コード<br>Product code||
|serialNumber|GET|string|0x8D|製造番号<br>Serial number||
|powerSaving|GET|boolean|0x8F|節電動作設定<br>Power-saving operation setting||
|currentDateAndTime|GET|time|0x98|現在⽇時<br>Current date and time||
|powerLimit|GET|number|0x99|電力制限設定<br>Power limit setting||
|integralOperationTime|GET|number|0x9A|積算運転時間<br>Integral operation time||

### Device Description (Common Items)

```
{
    "type":"common",
    "eoj":"0x00",
    "description":{"ja":"共通項目", "en":"Common items"},
    "properties":[
        {
            "name":"operationStatus",
            "epc":"0x80",
            "description":{ "ja":"動作状態状態", "en":"Operation Status" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"ON", "en":"ON", "edt":"0x30"},
                    {"value":false, "ja":"OFF", "en":"OFF", "edt":"0x31"}
                ]
            }
        },
        {
            "name":"installationLocation",
            "epc":"0x81",
            "description":{"ja":"設置場所", "en":"Installation location"},
            "writable":true,
            "observable":true,
            "data":{ "type":"string" }
        },
        {
            "name":"instantaneousPowerConsumption",
            "epc":"0x84",
            "description":{"ja":"瞬時消費電力計測値", "en":"Measured instantaneous power consumption"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"string",
                "unit":"W",    
                "minimum":0,
                "maximum":65533
            }
        },
        {
            "name":"integralPowerConsumption",
            "epc":"0x85",
            "description":{"ja":"積算消費電力計測値", "en":"Measured cumalative power consumption"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh",
                "minimum":0,
                "maximum":999999.999,
                "minimumDigit":0.001
            }
        },
        {
            "name":"manufactureFaultCode",
            "epc":"0x86",
            "description":{"ja":"メーカー異常コード", "en":"Manufacture fault code"},
            "writable":false,
            "observable":false,
            "data":{ "type":"string" }
        },
        {
            "name":"isAtFault",
            "epc":"0x88",
            "description":{ "ja":"異常発生状態", "en":"Fault Status" },
            "writable":false,
            "observable":true,
            "data":{
                "type":"boolean",
                "values":[
                    {"valu":true, "ja":"異常あり", "en":"Fault", "edt":"0x41"},
                    {"value":false, "ja":"異常無し", "en":"No Fault", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"productCode",
            "epc":"0x8C",
            "description":{"ja":"商品コード", "en":"Product code"},
            "writable":false,
            "observable":false,
            "data":{ "type":"string" }
        },
        {
            "name":"serialNumber",
            "epc":"0x8D",
            "description":{"ja":"製造番号", "en":"Serial Number"},
            "writable":false,
            "observable":false,
            "data":{ "type":"string" }
        },
        {
            "name":"powerSaving",
            "epc":"0x8F",
            "description":{ "ja":"節電動作設定", "en":"Power-saving operation setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"節電動作", "en":"Power saving operation", "edt":"0x41"},
                    {"value":false, "ja":"通常動作", "en":"Normal operation", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"currentDateAndTime",
            "epc":"0x98",
            "description":{"ja":"現在⽇時", "en":"Current date and time"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"date-time"
            }
        },
        {
            "name":"powerLimit",
            "epc":"0x99",
            "description":{"ja":"電力制限設定", "en":"Power limit setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":65533
            }
        },
        {
            "name":"integralOperationTime",
            "epc":"0x9A",
            "description":{"ja":"積算運転時間", "en":"Integral operation time"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"hour",
                "minimum":0,
                "maximum":4294967295
            }
        }
    ]
}
```

### 4.2 機器毎のDevice Description
以下に機器毎のDevice Descriptionを記述する。実際のDevice Descriptionは前節の共通項目をマージしたものとなる。
### List

|機器名|Device Type|EOJ|Note|
|:------|:------------|:----|:----|
|照度センサ|illuminanceSensor|0x000D|
|温度センサ|temperatureSensor|0x0011|
|湿度センサ|humiditySensor|0x0012|
|電力量センサ|electricPowerSensor|0x0022|
|気圧センサ|airPressureSensor|0x002D|
|家庭用エアコン|homeAirConditioner|0x0130|\*1|
|換気扇|ventilationFan|0x0133|
|空気清浄器|airCleaner|0x0135|
|電動ブラインド・日よけ|electricBlind|0x0260|
|電動雨戸・シャッター|electricRainDoor|0x0263|
|電気温水器|electricWaterHeater|0x026B |\*1|
|電気錠|electricKey|0x026F |
|瞬間式給湯器|instantaneousWaterHeater|0x0272 |\*1|
|浴室暖房乾燥機|bathroomHeaterDryer|0x0273|
|住宅用太陽光発電|pvPowerGeneration|0x0279 |
|冷温水熱源機|hotWaterHeatSource|0x027A|
|床暖房|floorHeater|0x027B|
|燃料電池|fuelCell|0x027C |\*1|
|蓄電池|storageBattery|0x027D |\*1|
|電気自動車充放電器|evChargerDischarger|0x027E |\*1|
|分電盤メータリング|powerDistributionBoard|0x0287|
|低圧スマート電力量メータ|lvSmartElectricEnergyMeter|0x0288 |\*1|
|高圧スマート電力量メータ|hvSmartElectricEnergyMeter|0x028A |\*1|
|一般照明|generalLighting|0x0290 |\*1|
|単機能照明|monoFunctionalLighting|0x0291|
|電気自動車充電器|evCharger|0x02A1|\*1|
|拡張照明システム|Lighting|0x02A4 |\*1|
|冷凍冷蔵庫|refrigerator|0x03B7|
|オーブンレンジ|combinationMicrowaveOven|0x03B8|
|クッキングヒータ|cookingHeater|0x03B9|
|洗濯乾燥機|washerDryer|0x03D3|
|スイッチ|switch|0x05FD|
|コントローラ|controller|0x05FF|

\*1 ECHONETコンソーシアムのWebAPI対応機器

## 照度センサ:illuminanceSensor:0x000D

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|illuminance|GET|number|0xE0|照度計測値1<br>Measured Illuminance value1|

### Device Description

```
{
    "type":"illuminanceSensor",
    "eoj":"0x000D",
    "description":{"ja":"照度センサ", "en":"Illuminance Sensor"},
    "properties":[
        {
            "name":"illuminance",
            "epc":"0xE0",
            "description":{ "ja":"照度計測値1", "en":"Illuminance value1" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Lux"
            }
        }
    ]
}
```

## 温度センサ:temperatureSensor:0x0011

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|temperature|GET|number|0xE0|温度計測値<br>Measured temperature value|

### Device Description

```
{
    "type":"temperatureSensor",
    "eoj":"0x0011",
    "description":{"ja":"温度センサ", "en":"Temperature Sensor"},
    "properties":[
        {
            "name":"temperature",
            "epc":"0xE0",
            "description":{ "ja":"温度計測値", "en":"Measured temperature value" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Celsius",
                "minimumDigit":0.1
            }
        }
    ]
}
```

## 湿度センサ:humiditySensor:0x0012

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|humidity|GET|number|0xE0|相対湿度計測値<br>Measured value of relative humidity|

### Device Description

```
{
    "type":"humiditySensor",
    "eoj":"0x0012",
    "description":{"ja":"湿度センサ", "en":"Humidity Sensor"},
    "properties":[
        {
            "name":"humidity",
            "epc":"0xE0",
            "description":{ "ja":"相対湿度計測値", "en":"Measured value of relative humidity" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        }
    ]
}
```

## 電力量センサ:electricPowerSensor:0x0022

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|electricEnergy|GET|number|0xE0|積算電力量計測値<br>Integral electric energy|

### Device Description

```
{
    "type":"electricPowerSensor",
    "eoj":"0x0022",
    "description":{"ja":"電力量センサ", "en":"Electric Power Sensor"},
    "properties":[
        {
            "name":"electricEnergy",
            "epc":"0xE0",
            "description":{ "ja":"積算電力量計測値", "en":"Integral electric energy" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh",    
                "minimumDigit":0.001
            }
        }
    ]
}
```

## 気圧センサ:airPressureSensor:0x002D

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|airPressure|GET|number|0xE0|気圧計測値<br>Air pressure measurement|

### Device Description

```
{
    "type":"airPressureSensor",
    "eoj":"0x0011",
    "description":{"ja":"気圧センサ", "en":"Air Pressure Sensor"},
    "properties":[
        {
            "name":"airPressure",
            "epc":"0xE0",
            "description":{ "ja":"気圧計測値", "en":"Air pressure measurement" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"hPa",
                "minimumDigit":0.1
            }
        }
    ]
}
```

## 家庭用エアコン:homeAirConditioner:0x0130

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|powerSaving|GET, PUT|boolean|0x8F|節電動作設定<br>Power-saving operation setting|\*1|
|airFlowLevel|GET, PUT|number|0xA0|風量設定<br>Air flow rate setting|\*1|
|operationMode|GET, PUT|enum|0xB0|運転モード設定<br>Operation mode setting|\*1|
|targetTemperature|GET, PUT|number |0xB3|温度設定値<br>Set temperature value|\*1|
|humidity|GET|number |0xBA|室内相対湿度計測値<br>Measured value of room relative humidity|\*1|
|roomTemperature|GET|number|0xBB|室内温度計測値<br>Measured value of room temperature|\*1|
|airFlowTemperature|GET|number|0xBD|吹き出し温度計測値<br>Measured cooled air temperature|\*1|
|outdoorTemperature|GET|number|0xBE|外気温度計測値<br>Measured outdoor air temperature|\*1|

\*1 AIF対応プロパティ

### Device Description

```
{
    "type":"homeAirConditioner",
    "eoj":"0x0130",
    "description":{"ja":"家庭用エアコン", "en":"Home Air Conditioner"},
    "properties":[
        {
            "name":"powerSaving",
            "epc":"0x8F",
            "description":{ "ja":"節電動作設定", "en":"Power-saving operation setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"節電動作", "en":"Power saving operation", "edt":"0x41"},
                    {"value":false, "ja":"通常動作", "en":"Normal operation", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"airFlowLevel",
            "epc":"0xA0",
            "description":{ "ja":"風量設定", "en":"Air flow rate setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"number",
                "minimum":1,
                "maximum":8,
                "alternatives":[
                    {"value":"auto", "ja":"自動", "en":"Auto", "edt":"0x41"}
                ]
            }
        },
        {
            "name":"operationMode",
            "epc":"0xB0",
            "description":{ "ja":"運転モード設定", "en":"Operation mode setting" },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"enum",
                "values":[
                    {"value":"auto", "ja":"自動", "en":"Auto", "edt":"0x41"},
                    {"value":"cooling", "ja":"冷房", "en":"Cooling", "edt":"0x42"},
                    {"value":"heating", "ja":"暖房", "en":"Heating", "edt":"0x43"},
                    {"value":"dehumidification", "ja":"除湿", "en":"Dehumidification", "edt":"0x44"},
                    {"value":"circulation", "ja":"送風", "en":"Circulation", "edt":"0x45"},
                    {"value":"other", "ja":"その他", "en":"Other", "edt":"0x40"}
                ]
            }
        },
        {
            "name":"targetTemperature",
            "epc":"0xB3",
            "description":{ "ja":"温度設定値", "en":"Set temperature value" },
            "writable":true,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Celsius",
                "minimum":0,
                "maximum":50
            }
        },
        {
            "name":"humidity",
            "epc":"0xBA",
            "description":{
                "ja":"室内相対湿度計測値",
                "en":"Measured value of room relative humidity"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"roomTemperature",
            "epc":"0xBB",
            "description":{ "ja":"室内温度計測値", "en":"Measured value of room temperature" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Celsius",
                "minimum":-127,
                "maximum":125
            }
        },
        {
            "name":"airFlowTemperature",
            "epc":"0xBD",
            "description":{ "ja":"吹き出し温度計測値", "en":"Measured cooled air temperature" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Celsius",
                "minimum":-127,
                "maximum":125
            }
        },
        {
            "name":"outdoorTemperature",
            "epc":"0xBE",
            "description":{ "ja":"外気温度計測値", "en":"Measured outdoor air temperature" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Celsius",
                "minimum":-127,
                "maximum":125
            }
        }
    ]
}
```

## 換気扇:ventilationFan:0x0133

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|airFlowLevel|GET, PUT|number|0xA0|風量設定<br>Air flow rate setting|
|autoVentilation|GET, PUT|boolean|0xBF|換気自動設定<br>Ventilation auto setting|

### Device Description

```
{
    "type":"ventilationFan",
    "eoj":"0x0133",
    "description":{"ja":"換気扇", "en":"Ventilation Fan"},
    "properties":[
        {
            "name":"airFlowLevel",
            "epc":"0xA0",
            "description":{ "ja":"風量設定", "en":"Air flow rate setting" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "minimum":1,
                "maximum":8,
                "alternatives":[
                    {"value":"auto", "ja":"自動", "en":"Auto", "edt":"0x41"}
                ]
            }
        },
        {
            "name":"autoVentilation",
            "epc":"0xBF",
            "description":{ "ja":"換気自動設定", "en":"Ventilation auto setting" },
            "writable":true,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"自動", "en":"AUTO", "edt":"0x41"},
                    {"value":false, "ja":"非自動", "en":"Non AUTO", "edt":"0x42"}
                ]
            }
        }
    ]
}
```

## 空気清浄器:airCleaner:0x0135

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|airFlowLevel|GET, PUT|number|0xA0|風量設定<br>Air flow rate setting|

### Device Description

```
{
    "type":"airCleaner",
    "eoj":"0x0135",
    "description":{"ja":"空気清浄器", "en":"Air Cleaner"},
    "properties":[
        {
            "name":"airFlowLevel",
            "epc":"0xA0",
            "description":{ "ja":"風量設定", "en":"Air flow rate setting" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "minimum":1,
                "maximum":8,
                "alternatives":[
                    {"value":"auto", "ja":"自動", "en":"Auto", "edt":"0x41"}
                ]
            }
        }
    ]
}
```

## 電動ブラインド・日よけ:electricBlind:0x0260

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|blindMotion<br>開閉動作設定|GET, PUT|enum|0xE0|開閉（張出し／収納）動作設定<br>Open/close(extension/retraction) setting|INF|
|openingDegree<br>開度レベル|GET, PUT|number|0xE1|開度レベル設定<br>Degree-of-opening level|

### Device Description

```
{
    "type":"electricBlind",
    "eoj":"0x0260",
    "description":{"ja":"電動ブラインド・日よけ", "en":"Electric Blind"},
    "properties":[
        {
            "name":"blindMotion",
            "epc":"0xE0",
            "description":{
                "ja":"開閉（張出し／収納）動作設定",
                "en":"Open/close(extension/retraction) setting"
            },
            "writable":true,
            "observable":true,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"open", "ja":"開", "en":"open", "edt":"0x41"},
                    {"value":"close", "ja":"閉", "en":"close", "edt":"0x42"},
                    {"value":"stop", "ja":"停止", "en":"stop", "edt":"0x43"}
                ]
            }
        },
        {
            "name":"openingDegree",
            "epc":"0xE1",
            "description":{ "ja":"開度レベル設定", "en":"Degree-of-opening level" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        }
    ]
}
```

## 電動雨戸・シャッター:electricRainDoor:0x0263

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|blindMotion|GET, PUT|enum|0xE0|開閉動作設定<br>Open/Close setting|INF|
|openingDegree|GET, PUT|number|0xE1|開度レベル設定<br>Degree-of-opening level|\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"electricRainDoor",
    "eoj":"0x0263",
    "description":{"ja":"電動雨戸・シャッター", "en":"Electric Rain Door"},
    "properties":[
        {
            "name":"blindMotion",
            "epc":"0xE0",
            "description":{ "ja":"開閉動作設定", "en":"Open/Close setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"open", "ja":"開", "en":"open", "edt":"0x41"},
                    {"value":"close", "ja":"閉", "en":"close", "edt":"0x42"},
                    {"value":"stop", "ja":"停止", "en":"stop", "edt":"0x43"}
                ]
            }
        },
        {
            "name":"openingDegree",
            "epc":"0xE1",
            "description":{ "ja":"開度レベル設定", "en":"Degree-of-opening level" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        }
    ]
}
```

## 電気温水器:electricWaterHeater:0x026B 

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|automaticWaterHeating|GET, PUT|enum|0xB0|沸き上げ自動設定<br>Automatic water heating setting|\*1|
|waterHeatingStatus|GET|boolean|0xB2|沸き上げ中状態<br>Water heater status|\*1|
|daytimeReheatingPermission|GET, PUT|boolean|0xC0|昼間沸き増し許可設定<br>Daytime reheating permission setting|\*1|
|alarmStatus|GET|object|0xC2|警報発生状態<br>Alarm status|\*1|
|hotWaterSupplyStatus|GET|boolean|0xC3|給湯中状態<br>Hot water supply status|\*1|
|energyShiftParticipation|GET, PUT|boolean|0xC7|エネルギーシフト参加状態|\*1|
|waterHeatingStartTime|GET|number|0xC8|沸き上げ開始基準時刻|\*1|
|numberOfEnergyShifts|GET|number|0xC9|エネルギーシフト回数|\*1|
|waterHeatingTime1|GET, PUT|number |0xCA|昼間沸き上げシフト時刻１|\*1|
|estimatedEnergyConsumption1|GET|object|0xCB|昼間沸き上げシフト時刻１での沸き上げ予測電力量|\*1|
|energyConsumptionRate1|GET|object|0xCC|時間あたり消費電力量１|\*1|
|waterHeatingTime2|GET, PUT|number |0xCD|昼間沸き上げシフト時刻２|\*1|
|estimatedEnergyConsumption2|GET|object|0xCE|昼間沸き上げシフト時刻２での沸き上げ予測電力量|\*1|
|energyConsumptionRate2|GET|object|0xCF|時間あたり消費電力量２| \*1|
|automaticBathWaterHeating|GET, PUT|boolean|0xE3|風呂自動モード設定<br>Automatic bath water heating mode setting|\*1|
|bathOperationStatus|GET|enum|0xEA|風呂動作状態監視<br>Bath operation status monitor|\*1|

\*1 AIF対応プロパティ

### Device Description

```
{
    "type":"electricWaterHeater",
    "eoj":"0x026B",
    "description":{"ja":"電気温水器", "en":"Electric WaterHeater"},
    "properties":[
        {
            "name":"automaticWaterHeating",
            "epc":"0xB0",
            "description":{ "ja":"沸き上げ自動設定", "en":"Automatic water heating setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"auto", "ja":"自動沸き上げ", "en":"Auto Heating", "edt":"0x41"},
                    {"value":"manualNoHeating","ja":"手動沸き上げ停止","en":"Manual No Heating", "edt":"0x43"},
                    {"value":"manualHeating", "ja":"手動沸き上げ", "en":"Manual Heating", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"waterHeatingStatus",
            "epc":"0xB2",
            "description":{ "ja":"沸き上げ中状態", "en":"Water heater status" },
            "writable":false,
            "observable":true,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"沸き上げ中", "en":"Heating", "edt":"0x41"},
                    {"value":false, "ja":"沸き上げ無し", "en":"Not Heating", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"daytimeReheatingPermission",
            "epc":"0xC0",
            "description":{
                "ja":"昼間沸き増し許可設定",
                "en":"Daytime reheating permission setting"
            },
            "writable":true,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"許可", "en":"Permitted", "edt":"0x41"},
                    {"value":false, "ja":"禁止", "en":"Not Permitted", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"alarmStatus",
            "epc":"0xC2",
            "description":{ "ja":"警報発生状態", "en":"Alarm status" },
            "writable":false,
            "observable":true,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"noHotWater",
                        "description":{ "ja":"湯切れ警報", "en":"No Hot Water" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"発生", "en":"Alarm", "edt":0},
                                {"value":false, "ja":"正常", "en":"No Alarm", "edt":1}
                            ]
                        }
                    },
                    {
                        "name":"leaking",
                        "description":{ "ja":"漏水警報", "en":"No Hot Water" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"発生", "en":"Alarm", "edt":0},
                                {"value":false, "ja":"正常", "en":"No Alarm", "edt":1}
                            ]
                        }
                    },
                    {
                        "name":"freezing",
                        "description":{ "ja":"凍結警報", "en":"No Hot Water" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"発生", "en":"Alarm", "edt":0},
                                {"value":false, "ja":"正常", "en":"No Alarm", "edt":1}
                            ]
                        }
                    }
                ]
            }
        },
        {
            "name":"hotWaterSupplyStatus",
            "epc":"0xC3",
            "description":{ "ja":"給湯中状態", "en":"Hot water supply status" },
            "writable":false,
            "observable":true,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"給湯中", "en":"Supplying", "edt":"0x41"},
                    {"value":false, "ja":"非給湯中", "en":"Not Supplying", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"energyShiftParticipation",
            "epc":"0xC7",
            "description":{"ja":"エネルギーシフト参加状態", "en":"Participation in energy shift"},
            "writable":true,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"参加", "en":"Participation", "edt":"0x01"},
                    {"value":false, "ja":"不参加", "en":"Non Participation", "edt":"0x00"}
                ]
            }
        },
        {
            "name":"waterHeatingStartTime",
            "epc":"0xC8",
            "description":{ "ja":"沸き上げ開始基準時刻", "en":"Water Heating Start Time" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"time",
                "from":"20:00:00",
                "to":"1:00:00"
            },
            "note":{
                "ja":"分と秒の指定は無視される",
                "en":"number of minutes and seconds are ignored"
            }
        },
        {
            "name":"numberOfEnergyShifts",
            "epc":"0xC9",
            "description":{ "ja":"エネルギーシフト回数", "en":"Number Of Energy Shifts" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "minimum":1,
                "maximum":2
            }
        },
        {
            "name":"waterHeatingTime1",
            "epc":"0xCA",
            "description":{ "ja":"昼間沸き上げシフト時刻１", "en":"Water Heating Time1" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"time",
                "from":"9:00:00",
                "to":"17:00:00"
            },
            "note":{
                "ja":"分と秒の指定は無視される",
                "en":"number of minutes and seconds are ignored"
            }
        },
        {
            "name":"estimatedEnergyConsumption1",
            "epc":"0xCB",
            "description":{
                "ja":"昼間沸き上げシフト時刻１での沸き上げ予測電力量",
                "en":"Estimated Energy Consumption1"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"10:00",
                        "description":{ "ja":"10:00", "en":"10:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"13:00",
                        "description":{ "ja":"13:00", "en":"13:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"15:00",
                        "description":{ "ja":"15:00", "en":"15:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"17:00",
                        "description":{ "ja":"17:00", "en":"17:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    }
                ]
            }
        },
        {
            "name":"energyConsumptionRate1",
            "epc":"0xCC",
            "description":{ "ja":"時間あたり消費電力量１", "en":"Energy Consumption Rate1" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"10:00",
                        "description":{ "ja":"10:00", "en":"10:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"13:00",
                        "description":{ "ja":"13:00", "en":"13:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"15:00",
                        "description":{ "ja":"15:00", "en":"15:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"17:00",
                        "description":{ "ja":"17:00", "en":"17:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    }
                ]
            }
        },
        {
            "name":"waterHeatingTime2",
            "epc":"0xCD",
            "description":{ "ja":"昼間沸き上げシフト時刻２", "en":"Water Heating Time2" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"time",
                "from":"10:00:00",
                "to":"17:00:00"
            },
            "note":{
                "ja":"分と秒の指定は無視される",
                "en":"number of minutes and seconds are ignored"
            }
        },
        {
            "name":"estimatedEnergyConsumption2",
            "epc":"0CE",
            "description":{
                "ja":"昼間沸き上げシフト時刻２での沸き上げ予測電力量",
                "en":"Estimated Energy Consumption2"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"13:00",
                        "description":{ "ja":"13:00", "en":"13:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"15:00",
                        "description":{ "ja":"15:00", "en":"15:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"17:00",
                        "description":{ "ja":"17:00", "en":"17:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    }
                ]
            }
        },
        {
            "name":"energyConsumptionRate2",
            "epc":"0xCF",
            "description":{ "ja":"時間あたり消費電力量２", "en":"Energy Consumption Rate2" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"13:00",
                        "description":{ "ja":"13:00", "en":"13:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"15:00",
                        "description":{ "ja":"15:00", "en":"15:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"17:00",
                        "description":{ "ja":"17:00", "en":"17:00" },
                        "data":{
                            "type":"number",
                            "unit":"Wh"
                        }
                    }
                ]
            }
        },
        {
            "name":"automaticBathWaterHeating",
            "epc":"0xE3",
            "description":{
                "ja":"風呂自動モード設定",
                "en":"Automatic Bath Water Heating Mode Setting"
            },
            "writable":true,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"自動", "en":"Auto", "edt":"0x41"},
                    {"value":false, "ja":"非自動", "en":"Non Auto", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"bathOperationStatus",
            "epc":"0xEA",
            "description":{ "ja":"風呂動作状態監視", "en":"Bath Operation Status Monitor" },
            "writable":false,
            "observable":true,
            "data":{ 
                "type":"enum",
                "values":[
                    {"value":"runningHotWater", "ja":"湯張り中", "en":"Running Hot Water", "edt":"0x41"},
                    {"value":"noOperation", "ja":"停止中", "en":"No Operation", "edt":"0x42"},
                    {"value":"keepingTemperature", "ja":"保温中", "en":"Keeping Temperature", "edt":"0x43"}
                ]
            }
        }
    ]
}
```

## 電気錠:electricKey:0x026F 

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|islocked|GET, PUT|boolean|0xE0|施錠設定1<br>Lock setting 1|INF|

### Device Description

```
{
    "type":"electricKey",
    "eoj":"0x026F",
    "description":{"ja":"電気錠", "en":"Electric Key"},
    "properties":[
        {
            "name":"islocked",
            "epc":"0xE0",
            "description":{ "ja":"施錠設定1", "en":"Lock setting1" },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"施錠", "en":"Lock", "edt":"0x41"},
                    {"value":false, "ja":"解錠", "en":"Unlock", "edt":"0x42"}
                ]
            }
        }
    ]
}
```

## 瞬間式給湯器:instantaneousWaterHeater:0x0272 

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|tapWaterHeatingStatus|GET|boolean|0xD0|給湯器燃焼状態<br>Hot water heating status|\*1|
|bathWaterHeatingStatus|GET|boolean|0xE2|風呂給湯器燃焼状態<br>Bath water heater status|\*1|
|bathAutomaticMode|GET, PUT|boolean|0xE3|風呂自動モード設定<br>Bath auto mode setting|\*1|
|bathOperatingStatus|GET|enum|0xEF|風呂動作状態監視<br>Bath operation status monitor||

\*1 AIF対応プロパティ


### Device Description

```
{
    "type":"instantaneousWaterHeater",
    "eoj":"0x0272",
    "description":{"ja":"瞬間式給湯器", "en":"Instantaneous Water Heater"},
    "properties":[
        {
            "name":"tapWaterHeatingStatus",
            "epc":"0xD0",
            "description":{ "ja":"給湯器燃焼状態", "en":"Hot water heating status" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"燃焼状態有", "en":"Heating", "edt":"0x41"},
                    {"value":false, "ja":"燃焼状態無", "en":"Not Heating", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"bathWaterHeatingStatus",
            "epc":"0xE2",
            "description":{ "ja":"風呂給湯器燃焼状態", "en":"Bath water heater status" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"燃焼状態有", "en":"Heating", "edt":"0x41"},
                    {"value":false, "ja":"燃焼状態無", "en":"Not Heating", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"bathAutomaticMode",
            "epc":"0xE3",
            "description":{ "ja":"風呂自動モード設定", "en":"Bath auto mode setting" },
            "writable":true,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"ON", "en":"ON", "edt":"0x41"},
                    {"value":false, "ja":"OFF", "en":"OFF", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"bathOperatingStatus",
            "epc":"0xEF",
            "description":{ "ja":"風呂動作状態監視", "en":"Bath operation status monitor" },
            "writable":false,
            "observable":true,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"runningHotWater", "ja":"湯張り中", "en":"Running Hot Water", "edt":"0x41"},
                    {"value":"noOperation", "ja":"停止中", "en":"No Operation", "edt":"0x42"},
                    {"value":"keepingTemperature", "ja":"保温中", "en":"Keeping Temperature", "edt":"0x43"}
                ]
            }
        }
    ]
}
```

## 浴室暖房乾燥機:bathroomHeaterDryer:0x0273

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|operationMode|GET, PUT|enum|0xB0|運転設定<br>Operation setting|
|dryLevel|GET, PUT|number|0xB4|乾燥運転設定<br>Bathroom dryer operation setting|

### Device Description

```
{
    "type":"bathroomHeaterDryer",
    "eoj":"0x0273",
    "description":{"ja":"浴室暖房乾燥機", "en":"Bathroom Heater Dryer"},
    "properties":[
        {
            "name":"operationMode",
            "epc":"0xB0",
            "description":{ "ja":"運転設定", "en":"Operation setting" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"ventilation", "ja":"換気運転", "en":"Ventilation", "edt":"0x10"},
                    {"value":"preheating", "ja":"入浴前予備暖房運転", "en":"Preheating", "edt":"0x20"},
                    {"value":"heating", "ja":"入浴中暖房運転", "en":"Heating", "edt":"0x30"},
                    {"value":"drying", "ja":"乾燥運転", "en":"Drying", "edt":"0x40"},
                    {"value":"cooling", "ja":"涼風運転", "en":"Cooling", "edt":"0x50"},
                    {"value":"stop", "ja":"停止", "en":"Stop", "edt":"0x00"}
                ]
            }
        },
        {
            "name":"dryLevel",
            "epc":"0xB4",
            "description":{ "ja":"乾燥運転設定", "en":"Bathroom dryer operation setting" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "minimum":1,
                "maximum":8,
                "alternatives":[
                    {"value":"auto", "ja":"自動", "en":"Auto", "edt":"0x41"},
                    {"value":"standard", "ja":"標準", "en":"Standard", "edt":"0x42"}
                ]
            }
        }
    ]
}
```

## 住宅用太陽光発電:pvPowerGeneration:0x0279 

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|instantaneousPowerGeneration|GET|number|0xE0|瞬時発電電力計測値<br>Measured instantaneous amount of electricity generated|\*1|
|integralEnergyGeneration|GET|number|0xE1|積算発電電力量計測値<br>Measured cumulative amount of electric energy generated|\*1|

\*1 AIF対応プロパティ

### Device Description

```
{
    "type":"pvPowerGeneration",
    "eoj":"0x0279",
    "description":{"ja":"住宅用太陽光発電", "en":"PV Power Generation"},
    "properties":[
        {
            "name":"instantaneousPowerGeneration",
            "epc":"0xE0",
            "description":{
                "ja":"瞬時発電電力計測値",
                "en":"Measured instantaneous amount of electricity generated"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W"
            }
        },
        {
            "name":"integralEnergyGeneration",
            "epc":"0xE1",
            "description":{
                "ja":"積算発電電力量計測値",
                "en":"Measured cumulative amount of electric energy generated"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh",
                "minimumDigit":0.001
            }
        }
    ]
}
```

## 冷温水熱源機:hotWaterHeatSource:0x027A

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|waterTemperature1|GET, PUT|number|0xE1|水温設定1<br>Water temperature setting 1|\*1|
|waterTemperature2|GET, PUT|number|0xE2|水温設定2<br>Water temperature setting 2|\*1|

\*1) どちらかの実装が必須

### Device Description

```
{
    "type":"hotWaterHeatSource",
    "eoj":"0x027A",
    "description":{"ja":"冷温水熱源機", "en":"Hot Water Heat Source"},
    "properties":[
        {
            "name":"waterTemperature1",
            "epc":"0xE1",
            "description":{ "ja":"水温設定1", "en":"waterTemperature1" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Celsius",
                "minimum":0,
                "maximum":100,
                "alternatives":[
                    {"value":"auto", "ja":"自動", "en":"Auto", "edt":"0x71"}
                ]
            }
        },
        {
            "name":"waterTemperature2",
            "epc":"0xE2",
            "description":{ "ja":"水温設定2", "en":"waterTemperature2" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "minimum":1,
                "maximum":31,
                "alternatives":[
                    {"value":"auto", "ja":"自動", "en":"Auto", "edt":"0x41"}
                ]
            }
        }
    ]
}
```

## 床暖房:floorHeater:0x027B

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|waterTemperature1|GET, PUT|number|0xE0|温度設定1<br>Temperature setting 1|\*1|
|waterTemperature2|GET, PUT|number|0xE1|温度設定2<br>Temperature setting 2|\*1|

\*1) どちらかの実装が必須

### Device Description

```
{
    "type":"floorHeater",
    "eoj":"0x027B",
    "description":{"ja":"床暖房", "en":"Floor Heater"},
    "properties":[
        {
            "name":"waterTemperature1",
            "epc":"0xE0",
            "description":{ "ja":"温度設定1", "en":"Temperature1" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Celsius",
                "minimum":0,
                "maximum":50,
                "alternatives":[
                    {"value":"auto", "ja":"自動", "en":"Auto", "edt":"0x41"}
                ]
            }
        },
        {
            "name":"waterTemperature2",
            "epc":"0xE1",
            "description":{ "ja":"温度設定2", "en":"Temperature2" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "minimum":1,
                "maximum":15,
                "alternatives":[
                    {"value":"auto", "ja":"自動", "en":"Auto", "edt":"0x41"}
                ]
            }
        }
    ]
}
```

## 燃料電池:fuelCell:0x027C 

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|ratedPowerGeneration|GET|number|0xC2|定格発電出力<br>Rated power generation output|\*1|
|instantaneousPowerGeneration|GET|number|0xC4|瞬時発電電力計測値<br>Measured instantaneous power generation output|\*1|
|integralEnergyGeneration|GET|number|0xC5|積算発電電力量計測値<br>Measured cumulative power generation output|\*1|
|powerGenerationStatus|GET|enum|0xCB|発電動作状態<br>Power generation status|\*1|
|powerSystemInterconnectionStatus|GET|enum|0xD0|系統連系状態<br>System interconnected type|\*1|
|requestedTimeForGeneration|GET, PUT|object|0xD1|発電要請時刻設定<br>Power generation request time setting|\*1|
|powerGenerationMode|GET, PUT|enum|0xD2|指定発電状態<br>Assigned power generation status|\*1|

\*1 AIF対応プロパティ

### Device Description

```
{
    "type":"fuelCell",
    "eoj":"0x027C",
    "description":{"ja":"燃料電池", "en":"Fuel Cell"},
    "properties":[
        {
            "name":"ratedPowerGeneration",
            "epc":"0xC2",
            "description":{
                "ja":"定格発電出力",
                "en":"Rated power generation output"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":65533
            }
        },
        {
            "name":"instantaneousPowerGeneration",
            "epc":"0xC4",
            "description":{
                "ja":"瞬時発電電力計測値",
                "en":"Measured instantaneous power generation output"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":65533
            }
        },
        {
            "name":"integralEnergyGeneration",
            "epc":"0xC5",
            "description":{
                "ja":"積算発電電力量計測値",
                "en":"Measured cumulative power generation output"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh",
                "minimum":0,
                "maximum":999999.999,
                "minimumDigit":0.001
            }
        },
        {
            "name":"powerGenerationStatus",
            "epc":"0xCB",
            "description":{ "ja":"発電動作状態", "en":"Power generation status" },
            "writable":false,
            "observable":true,
            "data":{ 
                "type":"enum",
                "values":[
                    {"value":"generating", "ja":"発電中", "en":"Generating", "edt":"0x41"},                    {"value":"stopped", "ja":"停⽌中", "en":"Stopped", "edt":"0x42"},                    {"value":"starting", "ja":"起動中", "en":"Starting", "edt":"0x43"},                    {"value":"stopping", "ja":"停⽌動作中", "en":"Stopping", "edt":"0x44"},                    {"value":"idling", "ja":"アイドル中", "en":"Idling", "edt":"0x45"}
                ]
            }
        },
        {
            "name":"powerSystemInterconnectionStatus",
            "epc":"0xD0",
            "description":{ "ja":"系統連系状態", "en":"System interconnected type" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"enum",
                "values":[
                    {
                        "value":"reversePowerFlowAcceptable",
                        "ja":"系統連系（逆潮流可）",
                        "en":"System Interconnected Type(revese power flow acceptable)",
                        "edt":"0x00"
                    },
                    {
                        "value":"independent",
                        "ja":"独立",
                        "en":"Independent Type",
                        "edt":"0x01"
                    },
                    {
                        "value":"reversePowerFlowNotAcceptable",
                        "ja":"系統連系（逆潮流不可）",
                        "en":"System Interconnected Type(revese power flow not acceptable)",
                        "edt":"0x02"
                    }                ]
            }
        },
        {
            "name":"requestedTimeForGeneration",
            "epc":"0xD1",
            "description":{ "ja":"発電要請時刻設定", "en":"Power generation request time setting" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"startTime",
                        "description":{ "ja":"開始時刻：時", "en":"Start time" },
                        "data":{
                            "type":"time"
                        }
                    },
                    {
                        "name":"endTime",
                        "description":{ "ja":"終了時刻：時", "en":"End time" },
                        "data":{
                            "type":"time"
                        }
                    }
                ]
            },
            "note":{
                "ja":"秒の指定は無視される",
                "en":"number of seconds is ignored"
            }
        },
        {
            "name":"powerGenerationMode",
            "epc":"0xD2",
            "description":{ "ja":"指定発電状態", "en":"Assigned power generation status" },
            "writable":true,
            "observable":false,
            "data":{ 
                "type":"enum",
                "values":[
                    {"value":"maximumRating", "ja":"定格最⼤", "en":"Maximum Rating", "edt":"0x41"},                    {"value":"loadFollowing", "ja":"不可追従", "en":"Load Following", "edt":"0x42"}                ]
            }
        }
    ]
}
```

## 蓄電池:storageBattery:0x027D 

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|effectiveChargingCapacity|GET|number|0xA0|AC実効容量（充電）<br>AC effective capacity(charging)|\*1|
|effectiveDischargingCapacity|GET|number|0xA1|AC実効容量（放電）<br>AC effective capacity(discharging)|\*1|
|chargeableCapacity|GET|number|0xA2|充電可能容量<br>AC chargeable capacity|\*1|
|dischargeableCapacity|GET|number|0xA3|放電可能容量<br>AC dischargeable capacity|\*1|
|chargeableEnergy|GET|number|0xA4|充電可能量<br>AC chargeable electric energy|\*1|
|dischargeableEnergy|GET|number|0xA5|放電可能量<br>AC dischargeable electric energy|\*1|
|integralChargingEnergy|GET|number|0xA8|AC積算充電電力量計測値<br>AC measured cumulative charging electric energy|\*1|
|integralDischargingEnergy|GET|number|0xA9|AC積算放電電力量計測値<br>AC measured cumulative discharging electric energy|\*1|
|targetChargingEnergy|GET, PUT|number|0xAA|AC充電量設定値<br>AC charge amount setting value|\*1|
|targetDischargingEnergy|GET, PUT|number|0xAB|AC放電量設定値<br>AC discharge amount setting value|\*1|
|minimumAndMaximumChargingPower|GET|object|0xC8|最⼩最⼤充電電力値<br>Minimum/maximum charging electric energy|\*1|
|minimumAndMaximumDischargingPower|GET|object|0xC9|最⼩最⼤放電電力値<br>Minimum/maximum discharging electric energy|\*1|
|actualOperationMode|GET|enum|0xCF|運転動作状態<br>Working operation status|\*1|
|ratedElectricEnergy|GET|number|0xD0|定格電力量<br>Rated electric energy|\*1|
|ratedCapacity|GET|number|0xD1|定格容量<br>Rated capacity|\*1|
|ratedVoltage|GET|number|0xD2|定格電圧<br>Rated voltage|\*1|
|operationMode|GET, PUT|enum|0xDA|運転モード設定<br>Operation mode setting|\*1|
|ratedElectricEnergy",|GET|enum|0xDB|系統連系状態<br>System interconnected type|\*1|
|remainingStoredEnergy1|GET|number|0xE2|蓄電残量1<br>Remaining stored electricity 1|\*1|
|remainingStoredEnergy2|GET|number|0xE3|蓄電残量2<br>Remaining stored electricity 2|\*1|
|remainingStoredEnergy3|GET|number|0xE4|蓄電残量3<br>Remaining stored electricity 3|\*1|
|batteryType|GET|enum|0xE6|蓄電池タイプ<br>Battery type|\*1|

\*1 AIF対応プロパティ

### Device Description

```
{
    "type":"storageBattery",
    "eoj":"0x027D",
    "description":{"ja":"蓄電池", "en":"Storage Battery"},
    "properties":[
        {
            "name":"effectiveChargingCapacity",
            "epc":"0xA0",
            "description":{ "ja":"AC実効容量（充電）", "en":"AC effective capacity(charging)" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,                "maximum":999999999
            }
        },
        {
            "name":"effectiveDischargingCapacity",
            "epc":"0xA1",
            "description":{
                "ja":"AC実効容量（放電）",
                "en":"AC effective capacity(discharging)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,                "maximum":999999999
            }
        },
        {
            "name":"chargeableCapacity",
            "epc":"0xA2",
            "description":{ "ja":"充電可能容量", "en":"AC chargeable capacity" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Wh",
                "minimum":0,                "maximum":999999999
            }
        },
        {
            "name":"dischargeableCapacity",
            "epc":"0xA3",
            "description":{ "ja":"放電可能容量", "en":"AC dischargeable capacity" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Wh",
                "minimum":0,                "maximum":999999999
            }
        },
        {
            "name":"chargeableEnergy",
            "epc":"0xA4",
            "description":{ "ja":"充電可能量", "en":"AC chargeable electric energy" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Wh",
                "minimum":0,                "maximum":999999999
            }
        },
        {
            "name":"dischargeableEnergy",
            "epc":"0xA5",
            "description":{ "ja":"放電可能量", "en":"AC dischargeable electric energy" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Wh",
                "minimum":0,                "maximum":999999999
            }
        },
        {
            "name":"integralChargingEnergy",
            "epc":"0xA8",
            "description":{
                "ja":"AC積算充電電力量計測値<br>",
                "en":"AC measured cumulative charging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"kWh",
                "minimum":0,                "maximum":999999.999,
                "minimumDigit":0.001
            }
        },
        {
            "name":"integralDischargingEnergy",
            "epc":"0xA9",
            "description":{
                "ja":"AC積算放電電力量計測値<br>",
                "en":"AC measured cumulative discharging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"kWh",
                "minimum":0,                "maximum":999999.999,
                "minimumDigit":0.001
            }
        },
        {
            "name":"targetChargingEnergy",
            "epc":"0xAA",
            "description":{ "ja":"AC充電量設定値", "en":"AC charge amount setting value" },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"number",
                "unit":"Wh",
                "minimum":0,                "maximum":999999999
            }
        },
        {
            "name":"targetDischargingEnergy",
            "epc":"0xAB",
            "description":{ "ja":"AC放電量設定値", "en":"AC discharge amount setting value" },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"number",
                "unit":"Wh",
                "minimum":0,                "maximum":999999999
            }
        },
        {
            "name":"minimumAndMaximumChargingPower",
            "epc":"0xC8",
            "description":{
                "ja":"最小最大充電電力値",
                "en":"Minimum/maximum charging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"minimumPower",
                        "description":{ "ja":"最小値", "en":"minimum value" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    },
                    {
                        "name":"maximumPower",
                        "description":{ "ja":"最大値", "en":"maximum value" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    }
                ]
            }
        },
        {
            "name":"minimumAndMaximumDischargingPower",
            "epc":"0xC9",
            "description":{
                "ja":"最小最大放電電力値",
                "en":"Minimum/maximum discharging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"minimumPower",
                        "description":{ "ja":"最小値", "en":"minimum value" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    },
                    {
                        "name":"maximumPower",
                        "description":{ "ja":"最大値", "en":"maximum value" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    }
                ]
            }
        },
        {
            "name":"actualOperationMode",
            "epc":"0xCF",
            "description":{ "ja":"運転動作状態", "en":"Working operation status" },
            "writable":false,
            "observable":true,
            "data":{ 
                "type":"enum",
                "values":[
                    {"value":"rapidCharging", "ja":"急速充電", "en":"rapidCharging", "edt":"0x41"},
                    {"value":"charging", "ja":"充電", "en":"charging", "edt":"0x42"},
                    {"value":"discharging", "ja":"放電", "en":"discharging", "edt":"0x43"},
                    {"value":"standby", "ja":"待機", "en":"standby", "edt":"0x44"},
                    {"value":"test", "ja":"テスト", "en":"test", "edt":"0x45"},
                    {"value":"auto", "ja":"自動", "en":"auto", "edt":"0x46"},
                    {"value":"restart", "ja":"再起動", "en":"restart", "edt":"0x48"},
                    {"value":"capacityRecalculation","ja":"実行容量再計算処理","en":"capacityRecalculation", "edt":"0x49"},
                    {"value":"other", "ja":"その他", "en":"other", "edt":"0x40"}
                ]
            }
        },
        {
            "name":"ratedElectricEnergy",
            "epc":"0xD0",
            "description":{ "ja":"定格電力量", "en":"Rated electric energy" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,                "maximum":999999999
            }
        },
        {
            "name":"ratedCapacity",
            "epc":"0xD1",
            "description":{ "ja":"定格容量", "en":"Rated capacity" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Ah",
                "minimum":0,                "maximum":3276.6,
                "minimumDigit":0.1
            }
        },
        {
            "name":"ratedVoltage",
            "epc":"0xD2",
            "description":{ "ja":"定格電圧", "en":"Rated voltage" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"V",
                "minimum":0,                "maximum":32766
            }
        },
        {
            "name":"operationMode",
            "epc":"0xDA",
            "description":{ "ja":"運転モード設定", "en":"Operation mode setting" },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"enum",
                "values":[
                    {"value":"rapidCharging", "ja":"急速充電", "en":"rapidCharging", "edt":"0x41"},
                    {"value":"charging", "ja":"充電", "en":"charging", "edt":"0x42"},
                    {"value":"discharging", "ja":"放電", "en":"discharging", "edt":"0x43"},
                    {"value":"standby", "ja":"待機", "en":"standby", "edt":"0x44"},
                    {"value":"test", "ja":"テスト", "en":"test", "edt":"0x45"},
                    {"value":"auto", "ja":"自動", "en":"auto", "edt":"0x46"},
                    {"value":"restart", "ja":"再起動", "en":"restart", "edt":"0x48"},
                    {"value":"capacityRecalculation","ja":"実行容量再計算処理","en":"capacityRecalculation", "edt":"0x49"},
                    {"value":"other", "ja":"その他", "en":"other", "edt":"0x40"}
                ]
            },
            "note":{
                "ja":"このプロパティをGetして取得できる値は設定値である。実際の運転状態はactualOperationModeをGETすること",
                "en":"The value you get with this property is the value you set. Get actualOperationMode for the current status."
            }
        },
        {
            "name":"powerSystemInterconnectionStatus",
            "epc":"0xDB",
            "description":{ "ja":"系統連系状態", "en":"System interconnected type" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"enum",
                "values":[
                    {
                        "value":"reversePowerFlowAcceptable",
                        "ja":"系統連系（逆潮流可）",
                        "en":"System Interconnected Type(revese power flow acceptable)",
                        "edt":"0x00"
                    },
                    {
                        "value":"independent",
                        "ja":"独立",
                        "en":"Independent Type",
                        "edt":"0x01"
                    },
                    {
                        "value":"reversePowerFlowNotAcceptable",
                        "ja":"系統連系（逆潮流不可）",
                        "en":"System Interconnected Type(revese power flow not acceptable)",
                        "edt":"0x02"
                    }                ]
            }
        },
        {
            "name":"remainingStoredEnergy1",
            "epc":"0xE2",
            "description":{ "ja":"蓄電残量1", "en":"Remaining stored electricity 1" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"remainingStoredEnergy2",
            "epc":"0xE3",
            "description":{ "ja":"蓄電残量2", "en":"Remaining stored electricity 2" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"Ah",
                "minimum":0,
                "maximum":3276.6,
                "minimumDigit":0.1
            }
        },
        {
            "name":"remainingStoredEnergy3",
            "epc":"0xE4",
            "description":{ "ja":"蓄電残量3", "en":"Remaining stored electricity 3" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"batteryType",
            "epc":"0xE6",
            "description":{ "ja":"蓄電池タイプ", "en":"Battery type" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"enum",
                "values":[
                    {"value":"unknown", "ja":"不明", "en":"unknown", "edt":"0x00"},
                    {"value":"lead", "ja":"鉛", "en":"lead", "edt":"0x01"},
                    {"value":"ni-mh", "ja":"NiH", "en":"ni-mh", "edt":"0x02"},
                    {"value":"ni-cd", "ja":"NiCd", "en":"ni-cd", "edt":"0x03"},
                    {"value":"lib", "ja":"Li-ion", "en":"lib", "edt":"0x04"},
                    {"value":"zinc", "ja":"Zn", "en":"zinc", "edt":"0x05"},
                    {"value":"alkaline", "ja":"充電式アルカリ", "en":"alkaline", "edt":"0x06"}
                ]
            }
        }
    ]
}
```

## 電気自動車充放電器:evChargerDischarger:0x027E 

　ECHONET LiteのProperty「車両接続・充放電可否状態（EPC=0xC7）」は、ECHONET LiteでデータをGETする前に「車両接続確認（EPC=0xCD)」をSETする必要がある。ブリッジがEL-WebAPIでGET:ChargeDischargeStatusを受信した場合、ブリッジはECHONET Liteで「車両接続確認」をSETしたのち「積算電力量計測値履歴1」をGETする。

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|dischargeableCapacity1|GET|number|0xC0|車載電池の放電可能容量値１<br>Dischargeable capacity of vehicle mounted battery 1|\*1|
|dischargeableCapacity2|GET|number|0xC1|車載電池の放電可能容量値２<br>Dischargeable capacity of vehicle mounted battery 2|\*1|
|remainingDischargeableCapacity1|GET|number|0xC2|車載電池の放電可能残容量１<br>Remaining dischargeable capacity of vehicle mounted battery 1|\*1|
|remainingDischargeableCapacity2|GET|number|0xC3|車載電池の放電可能残容量２<br>Remaining dischargeable capacity of vehicle mounted battery 2|\*1|
|remainingDischargeableCapacity3|GET|number|0xC4|車載電池の放電可能残容量３<br>Remaining dischargeable capacity of vehicle mounted battery 3|\*1|
|ratedChargePower|GET|number|0xC5|定格充電能力<br>Rated charge capacity|\*1|
|ratedDischargePower|GET|number|0xC6|定格放電能力<br>Rated discharge capacity|\*1|
|chargeDischargeStatus|GET|enum|0xC7|車両接続・充放電可否状態<br>Vehicle connection and chargeable/dischargeable status|\*1|
|minimumAndMaximumChargingPower|GET|object|0xC8|最小大充電電力値<br>Minimum/maximum charging electric energy|\*1|
|minimumAndMaximumDischargingPower|GET|object|0xC9|最小最大放電電力値<br>Minimum/maximum discharging electric energy|\*1|
|minimumAndMaximumChargeCurrent|GET|object|0xCA|最小最大充電電流値<br>Minimum/maximum charging current|\*1|\*1|
|minimumAndMaximumDischargeCurrent|GET|object|0xCB|最小最大放電電流値<br>Minimum/maximum discharging current|\*1|
|equipmentType|GET|enum|0xCC|充放電器タイプ<br>Charger/Discharger type|\*1|
|chargeableCapacity|GET|number|0xCE|車載電池の充電可能容量値<br>Chargeable capacity of vehicle mounted battery|\*1|
|remainingChargeableCapacity|GET|number|0xCF|車載電池の充電可能残容量値<br>Remaining Chargeable capacity of vehicle mounted battery|\*1|
|usedCapacity1|GET|number|0xD0|車載電池の使⽤容量値１<br>Used capacity of vehicle mounted battery 1|\*1|
|usedCapacity2|GET|number|0xD1|車載電池の使⽤容量値２<br>Used capacity of vehicle mounted battery 2|\*1|
|ratedVoltage|GET|number|0xD2|定格電圧<br>Rated voltage|\*1|
|measuredInstantaneousPower|GET|number|0xD3|瞬時充放電電力計測値<br>Measured instantaneous charging/discharging electric energy|\*1|
|measuredInstantaneousCurrent|GET|number|0xD4|瞬時充放電電流計測値<br>Measured instantaneous charging/discharging electric current|\*1|
|measuredInstantaneousVoltage|GET|number|0xD5|瞬時充放電電圧計測値<br>Measured instantaneous charging/discharging electric voltage|\*1|
|measuredCumulativeDischargeEnergy|GET|number|0xD6|積算放電電力量計測<br>Measured cumulative amount of discharging electric energy|\*1|
|measuredCumulativeChargeEnergy|GET|number|0xD8|積算充電電力量計測値<br>Measured cumulative amount of charging electric energy|\*1|
|operationMode|GET, PUT|enum|0xDA|運転モード設定<br>Operation mode setting|\*1|
|powerSystemInterconnectionStatus|GET|enum|0xDB|系統連系状態<br>System interconnected type|\*1|
|remainingStoredEnergy1|GET|number|0xE2|車載電池の電池残容量１<br>Remaining stored electricity of vehicle mounted battery1|\*1|
|remainingStoredEnergy2|GET|number|0xE3|車載電池の電池残容量２<br>Remaining stored electricity of vehicle mounted battery2|\*1|
|remainingStoredEnergy3|GET|number|0xE4|車載電池の電池残容量３<br>Remaining stored electricity of vehicle mounted battery3|\*1|
|vehicleID|GET|string|0xE6|車両ID<br>Identification of electric vehicle|\*1|
|targetChargingEnergy1|GET, PUT|number|0xE7|充電量設定値１<br>Charging amount 1|\*1|
|targetChargingEnergy2|GET, PUT|number|0xE9|充電量設定値２<br>Charging amount 2|\*1|
|targetDischargingEnergy|GET, PUT|number|0xEA|放電量設定値<br>Discharging amaout setting|\*1|
|chargingPower|GET, PUT| number |0xEB|充電電力設定値<br>Charging electrnic energy setting|\*1|
|dischargingPower|GET, PUT| number |0xEC|放電電力設定値<br>Discharging electrnic energy setting|\*1|
|chargingCurrent|GET, PUT| number |0xED|充電電流設定値<br>Charging current setting|\*1|
|dischargingCurrent|GET, PUT| number |0xEE|放電電流設定値<br>Discharging current setting|\*1|
|ratedVoltageOfIndependentOperation|GET| number |0xEF|定格電圧（独立時）<br>Rated voltage(Independent)|\*1|

\*1 AIF対応プロパティ

### Device Description

```
{
    "type":"evChargerDischarger",
    "eoj":"0x027E",
    "description":{"ja":"電気自動車充放電器", "en":"EV Charger Discharger"},
    "properties":[
        {
            "name":"dischargeableCapacity1",
            "epc":"0xC0",
            "description":{
                "ja":"車載電池の放電可能容量値１",
                "en":"Dischargeable capacity of vehicle mounted battery 1"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"dischargeableCapacity2",
            "epc":"0xC1",
            "description":{
                "ja":"車載電池の放電可能容量値２",
                "en":"Dischargeable capacity of vehicle mounted battery 2"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Ah",
                "minimum":0,
                "maximum":3276.6,
                "minimumDigit":0.1
            }
        },
        {
            "name":"remainingDischargeableCapacity1",
            "epc":"0xC2",
            "description":{
                "ja":"車載電池の放電可能残容量１",
                "en":"Remaining dischargeable capacity of vehicle mounted battery 1"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"remainingDischargeableCapacity2",
            "epc":"0xC3",
            "description":{
                "ja":"車載電池の放電可能残容量２",
                "en":"Remaining dischargeable capacity of vehicle mounted battery 2"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Ah",
                "minimum":0,
                "maximum":3276.6,
                "minimumDigit":0.1
            }
        },
        {
            "name":"remainingDischargeableCapacity3",
            "epc":"0xC4",
            "description":{
                "ja":"車載電池の放電可能残容量３",
                "en":"Remaining dischargeable capacity of vehicle mounted battery 3"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"ratedChargePower",
            "epc":"0xC5",
            "description":{ "ja":"定格充電能力", "en":"Rated charge capacity" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"ratedDischargePower",
            "epc":"0xC6",
            "description":{ "ja":"定格放電能力", "en":"Rated discharge capacity" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"chargeDischargeStatus",
            "epc":"0xC7",
            "description":{
                "ja":"車両接続・充放電可否状態",
                "en":"Vehicle connection and chargeable/dischargeable status"
                   },
            "writable":false,
            "observable":true,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"undefined", "ja":"不定", "en":"Undefined", "edt":"0xFF"},
                    {"value":"notConnected", "ja":"車両未接続", "en":"Not Connected", "edt":"0x30"},
                    {"value":"connected", "ja":"車両接続・充電不可・放電不可", "en":"Connected", "edt":"0x40"},
                    {"value":"chargeableDischargeable",
                     "ja":"車両接続・充電可・放電不可", "en":"Chargeable Dischargeable", "edt":"0x41"},
                    {"value":"dischargeable",
                     "ja":"車両接続・充電不可・放電可", "en":"Dischargeable", "edt":"0x42"},
                    {"value":"chargeable", "ja":"車両接続・ 充電可 ・放電可", "en":"Chargeable", "edt":"0x43"},
                    {"value":"unknown", "ja":"車両接続・ 充電可否不明", "en":"Unknown of Chargeability", "edt":"0x44"}
                ]
            }
        },
        {
            "name":"minimumAndMaximumChargingPower",
            "epc":"0xC8",
            "description":{
                "ja":"最小最大充電電力値",
                "en":"Minimum/maximum charging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"minimumPower",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    },
                    {
                        "name":"maximumPower",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    }
                ]
            }
        },
        {
            "name":"minimumAndMaximumDischargingPower",
            "epc":"0xC9",
            "description":{
                "ja":"最小最大放電電力値",
                "en":"Minimum/maximum discharging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"minimumPower",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    },
                    {
                        "name":"maximumPower",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    }
                ]
            }
        },
        {
            "name":"minimumAndMaximumChargeCurrent",
            "epc":"0xCA",
            "description":{
                "ja":"最小最大充電電流値",
                "en":"Minimum/maximum charging electric current"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"minimumCurrent",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"number",
                            "unit":"A",
                            "minimum":0,
                            "maximum":3276.6,
                            "minimumDigit":0.1
                        }
                    },
                    {
                        "name":"maximumCurrent",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"number",
                            "unit":"A",
                            "minimum":0,
                            "maximum":3276.6,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"minimumAndMaximumDischargeCurrent",
            "epc":"0xCB",
            "description":{
                "ja":"最小最大放電電流値",
                "en":"Minimum/maximum discharging electric current"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"minimumCurrent",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"number",
                            "unit":"A",
                            "minimum":0,
                            "maximum":3276.6,
                            "minimumDigit":0.1
                        }
                    },
                    {
                        "name":"maximumCurrent",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"number",
                            "unit":"A",
                            "minimum":0,
                            "maximum":3276.6,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"equipmentType",
            "epc":"0xCC",
            "description":{ "ja":"充放電器タイプ", "en":"Charger/Discharger type" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"AC_CPLT", "ja":"AC_CPLT", "en":"AC_CPLT", "edt":"0x11"},
                    {"value":"AC_HLC_Charge", "ja":"AC_HLC（充電のみ）", "en":"AC_HLC_Charge", "edt":"0x12"},
                    {"value":"AC_HLC_ChargeDischarge",
                     "ja":"AC_HLC（充放電可）", "en":"AC_HLC_ChargeDischarge", "edt":"0x13"},
                    {"value":"DC_AA_Charge", "ja":"DCタイプ_AA（充電のみ）", "en":"DC_AA_Charge", "edt":"0x21"},
                    {"value":"DC_AA_ChargeDischarge",
                     "ja":"DCタイプ_AA（充放電可）", "en":"DC_AA_ChargeDischarge", "edt":"0x22"},
                    {"value":"DC_AA_Discharge",
                     "ja":"DCタイプ_AA（放電のみ）", "en":"DC_AA_Discharge", "edt":"0x23"},
                    {"value":"DC_BB_Charge", "ja":"DCタイプ_BB（充電のみ）", "en":"DC_BB_Charge", "edt":"0x31"},
                    {"value":"DC_BB_ChargeDischarge",
                     "ja":"DCタイプ_BB（充放電可）", "en":"DC_BB_ChargeDischarge", "edt":"0x32"},
                    {"value":"DC_BB_Discharge",
                     "ja":"DCタイプ_BB（放電のみ）", "en":"DC_BB_Discharge", "edt":"0x33"},
                    {"value":"DC_EE_Charge", "ja":"DCタイプ_EE（充電のみ）", "en":"DC_EE_Charge", "edt":"0x41"},
                    {"value":"DC_EE_ChargeDischarge",
                     "ja":"DCタイプ_EE（充放電可）", "en":"DC_EE_ChargeDischarge", "edt":"0x42"},
                    {"value":"DC_EE_Discharge",
                     "ja":"DCタイプ_EE（放電のみ）", "en":"DC_EE_Discharge", "edt":"0x43"},
                    {"value":"DC_FF_Charge", "ja":"DCタイプ_FF（充電のみ）", "en":"DC_FF_Charge", "edt":"0x51"},
                    {"value":"DC_FF_ChargeDischarge",
                     "ja":"DCタイプ_FF（充放電可）", "en":"DC_FF_ChargeDischarge", "edt":"0x52"},
                    {"value":"DC_FF_Discharge",
                     "ja":"DCタイプ_FF（放電のみ）", "en":"DC_FF_Discharge", "edt":"0x53"}
                ]
            }
        },
        {
            "name":"chargeableCapacity",
            "epc":"0xCE",
            "description":{ "ja":"車載電池の充電可能容量値", "en":"Chargeable capacity of vehicle mounted battery" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"remainingChargeableCapacity",
            "epc":"0xCF",
            "description":{ "ja":"車載電池の充電可能残容量値", "en":"Remaining Chargeable capacity of vehicle mounted battery" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"usedCapacity1",
            "epc":"0xD0",
            "description":{
                "ja":"車載電池の使用容量値１",
                "en":"Used capacity of vehicle mounted battery 1"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"usedCapacity2",
            "epc":"0xD1",
            "description":{
                "ja":"車載電池の使用容量値２",
                "en":"Used capacity of vehicle mounted battery 2"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Ah",
                "minimum":0,
                "maximum":3276.6,
                "minimumDigit":0.1
            }
        },
        {
            "name":"ratedVoltage",
            "epc":"0xD2",
            "description":{
                "ja":"定格電圧",
                "en":"Rated voltage"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"V",
                "minimum":0,
                "maximum":32766
            }
        },
        {
            "name":"measuredInstantaneousPower",
            "epc":"0xD3",
            "description":{
                "ja":"瞬時充放電電力計測値",
                "en":"Measured instantaneous charging/discharging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":-999999999,
                "maximum":999999999
            }
        },
        {
            "name":"measuredInstantaneousCurrent",
            "epc":"0xD4",
            "description":{
                "ja":"瞬時充放電電流計測値",
                "en":"Measured instantaneous charging/discharging electric current"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"A",
                "minimum":-3276.7,
                "maximum":3276.6,
                "minimumDigit":0.1
            }
        },
        {
            "name":"measuredInstantaneousVoltage",
            "epc":"0xD5",
            "description":{
                "ja":"瞬時充放電電圧計測値",
                "en":"Measured instantaneous charging/discharging electric voltage"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"V",
                "minimum":-32767,
                "maximum":32766
            }
        },
        {
            "name":"measuredintegralDischargeEnergy",
            "epc":"0xD6",
            "description":{
                "ja":"積算放電電力量計測値",
                "en":"Measured cumulative amount of discharging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh",
                "minimum":0,
                "maximum":999999.999,
                "minimumDigit":0.001
            }
        },
        {
            "name":"measuredintegralChargeEnergy",
            "epc":"0xD8",
            "description":{
                "ja":"積算充電電力量計測値",
                "en":"Measured cumulative amount of charging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh",
                "minimum":0,
                "maximum":999999.999,
                "minimumDigit":0.001
            }
        },
        {
            "name":"operationModeSetting",
            "epc":"0xDA",
            "description":{ "ja":"運転モード設定", "en":"Operation mode setting" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"charge", "ja":"充電", "en":"Charge", "edt":"0x42"},
                    {"value":"discharge", "ja":"放電", "en":"Discharge", "edt":"0x43"},
                    {"value":"standby", "ja":"待機", "en":"Standby", "edt":"0x44"},
                    {"value":"idle", "ja":"停止", "en":"Idle", "edt":"0x47"},
                    {"value":"other", "ja":"その他", "en":"Other", "edt":"0x40"}
                ]
            },
            "note":{
                "ja":"このプロパティをGetして取得できる値は設定値である。実際の運転状態はoperationStatusをGETすること",
                "en":"The value you get with this property is the value you set. You need to get operationStatus to get the current operation status."
            }
        },
        {
            "name":"powerSystemInterconnectionStatus",
            "epc":"0xDB",
            "description":{ "ja":"系統連系状態", "en":"System interconnected type" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"enum",
                "values":[
                    {
                        "value":"reversePowerFlowAcceptable",
                        "ja":"系統連系（逆潮流可）",
                        "en":"System Interconnected Type(revese power flow acceptable)",
                        "edt":"0x00"
                    },
                    {
                        "value":"independent",
                        "ja":"独立",
                        "en":"Independent Type",
                        "edt":"0x01"
                    },
                    {
                        "value":"reversePowerFlowNotAcceptable",
                        "ja":"系統連系（逆潮流不可）",
                        "en":"System Interconnected Type(revese power flow not acceptable)",
                        "edt":"0x02"
                    }                ]
            }
        },
        {
            "name":"remainingStoredEnergy1",
            "epc":"0xE2",
            "description":{
                "ja":"車載電池の電池残容量１",
                "en":"Remaining stored electricity of vehicle mounted battery1"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"remainingStoredEnergy2",
            "epc":"0xE3",
            "description":{
                "ja":"車載電池の電池残容量２",
                "en":"Remaining stored electricity of vehicle mounted battery2"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Ah",
                "minimum":0,
                "maximum":3276.6,
                "minimumDigit":0.1
            }
        },
        {
            "name":"remainingStoredEnergy3",
            "epc":"0xE4",
            "description":{
                "ja":"車載電池の電池残容量３",
                "en":"Remaining stored electricity of vehicle mounted battery3"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"vehicleID",
            "epc":"0xE6",
            "description":{
                "ja":"車両ID",
                "en":"Identification of electric vehicle"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"string"
            }
        },
        {
            "name":"targetChargingEnergy1",
            "epc":"0xE7",
            "description":{"ja":"充電量設定値１", "en":"Charging amount setting 1"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"targetChargingEnergy2",
            "epc":"0xE9",
            "description":{"ja":"充電量設定値２", "en":"Charging amount setting 2"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Ah",
                "minimum":0,
                "maximum":3276.6,
                "minimumDigit":0.1
            }
        },
        {
            "name":"targetDischargingEnergy",
            "epc":"0xEA",
            "description":{"ja":"放電量設定値", "en":"Discharging amount setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"chargingPower",
            "epc":"0xEB",
            "description":{"ja":"充電電力設定値", "en":"Charging electric energy setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"dischargingPower",
            "epc":"0xEC",
            "description":{"ja":"放電電力設定値", "en":"Discharging electric energy setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"chargingCurrent",
            "epc":"0xED",
            "description":{"ja":"充電電流設定値", "en":"Charging current setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"A",
                "minimum":0,
                "maximum":6553.3,
                "minimumDigit":0.1
            }
        },
        {
            "name":"dischargingCurrent",
            "epc":"0xEE",
            "description":{"ja":"放電電流設定値", "en":"Discharging current setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"A",
                "minimum":0,
                "maximum":6553.3,
                "minimumDigit":0.1
            }
        },
        {
            "name":"ratedVoltageOfIndependentOperation",
            "epc":"0xEF",
            "description":{"ja":"定格電圧（独立時）", "en":"Rated voltage(Independent)"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"V",
                "minimum":0,
                "maximum":32766
            }
        }
    ],
    "actions":[
        {
            "name":"resetIntegralDischargeEnergy",
            "epc":"0xD7",
            "description":{ "ja":"積算放電電力量リセット設定", "en":"Cumulative amount of discharging electric energy reset setting" }
        },
        {
            "name":"resetCumulativeChargeEnergy",
            "epc":"0xD9",
            "description":{ "ja":"積算充電電力量リセット設定", "en":"Cumulative amount of charging electric energy reset setting" }
        }
    ]
}
```

## 分電盤メータリング:powerDistributionBoard:0x0287

以下のECHONET LiteのPropertyは、ECHONET LiteでGETした値に積算電力量単位（EPC=0xC2）を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

> 積算電力量計測値（正方向計測値）  
> 積算電力量計測値（逆方向計測値）  

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|normDirIntegralEnergy|GET|number|0xC0|積算電力量計測値（正方向）<br>Measured cumulative amount of electric energy (normal direction)|
|revDirIntegralEnergy|GET|number|0xC1|積算電力量計測値（逆方向）<br>Measured cumulative amount of electric energy (reverse direction)|

### Device Description

```
{
    "type":"powerDistributionBoard",
    "eoj":"0x0287",
    "description":{"ja":"分電盤メータリング", "en":"Power Distribution Board"},
    "properties":[
        {
            "name":"normDirIntegralEnergy",
            "epc":"0xC0",
            "description":{
                "ja":"積算電力量計測値（正方向）",
                "en":"Measured cumulative amount of electric energy (normal direction)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh"
            }
        },
        {
            "name":"revDirIntegralEnergy",
            "epc":"0xC1",
            "description":{
                "ja":"積算電力量計測値（逆方向）",
                "en":"Measured cumulative amount of electric energy (reverse direction)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh"
            }
        }
    ]
}
```

## 低圧スマート電力量メータ:lvSmartElectricEnergyMeter:0x0288 

　以下のECHONET LiteのPropertyは、ECHONET LiteでGETした値に係数（EPC=0xD3）と積算電力量単位（EPC=0xE1）を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

> 積算電力量計測値（正方向計測値）  
> 積算電力量計測値履歴1（正方向計測値）  
> 積算電力量計測値（逆方向計測値）  
> 積算電力量計測値履歴1（逆方向計測値）  
> 定時積算電力量計測値（正方向計測値）  
> 定時積算電力量計測値（逆方向計測値）  

　以下のECHONET LiteのPropertyは、ECHONET LiteでデータをGETする前に「積算履歴収集日1（EPC=0xE5)」をSETする必要がある。EL-WebAPIでは以下のPropertyをGET(REST)する際に「積算履歴収集日1」の値をQueryとして指定する。ブリッジはECHONET Liteで「積算履歴収集日1」をSETしたのち「積算電力量計測値履歴1」をGETする。なお、EL-WebAPIでqueryを付加しない場合は「積算履歴収集日1」=0として扱う。

> 積算電力量計測値履歴1（正方向計測値）  
> 積算電力量計測値履歴1（逆方向計測値）  

　以下のECHONET LiteのPropertyは、ECHONET LiteでデータをGETする前に「積算履歴収集日2（EPC=0xED)」をSETする必要がある。EL-WebAPIでは以下のPropertyをGET(REST)する際に「積算履歴収集日2」の値をQueryとして指定する。ブリッジはECHONET Liteで「積算履歴収集日2」をSETしたのち「積算電力量計測値履歴2」をGETする。

> 積算電力量計測値履歴2（正方向、逆方向計測値）  


### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|normDirIntegralEnergy|GET|number|0xE0|積算電力量計測値（正方向計測値）<br>Measured cumulative amount of electric energy (normal direction)|\*1|
|normDirIntegralEnergyLog1|GET|object|0xE2|積算電力量計測値履歴1（正方向計測値）<br>Historical data of measured cumulative amounts of electric energy 1(normal direction)|\*1, \*2|
|revDirIntegralEnergy|GET|number|0xE3|積算電力量計測値（逆方向計測値）<br>Measured cumulative amounts of electric energy (reverse direction)|\*1|
|revDirIntegralEnergyLog1|GET|object|0xE4|積算電力量計測値履歴1（逆方向計測値）<br>Historical data of measured cumulative amounts of electric energy 1(reverse direction)|\*1,\*2|
|instantaneousPower|GET|number|0xE7|瞬時電力計測値<br>Measured instantaneous electric energy|\*1|
|instantaneousCurrent|GET| object|0xE8|瞬時電流計測値<br>Measured instantaneous currents|\*1|
|normDirIntegralEnergyEvery30Min|GET|object|0xEA|定時積算電力量計測値（正方向計測値）<br>Cumulative amounts of electric energy measured at fixed time (normal direction)|\*1|
|revDirIntegralEnergyEvery30Min|GET|object|0xEB|定時積算電力量計測値（逆方向計測値）<br>Cumulative amounts of electric energy measured at fixed time (reverse direction)|\*1|
|IntegralEnergyLog2|GET|object|0xEC|積算電力量計測値履歴2（正方向、逆方向計測値）<br>Historical data of measured cumulative amounts of electric energy 2(normal and reverse direction)|\*1,\*2|

\*1 AIF対応プロパティ
\*2 queryあり  

### Device Description

```
{
    "type":"lvSmartElectricEnergyMeter",
    "eoj":"0x0288",
    "description":{"ja":"低圧スマート電力量メータ", "en":"Low Voltage Smart Electric Energy Meter"},
    "properties":[
        {
            "name":"normDirIntegralEnergy",
            "epc":"0xE0",
            "description":{
                "ja":"積算電力量計測値（正方向計測値）",
                "en":"Measured cumulative amount of electric energy (normal direction)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh",
                "minimum":0,
                "maximum":99999.9,
                "minimumDigit":0.1
            }
        },
        {
            "name":"normDirIntegralEnergyLog1",
            "epc":"0xE2",
            "description":{
                "ja":"積算電力量計測値履歴1（正方向計測値）",
                "en":"Historical data of measured cumulative amounts of electric energy 1(normal direction)"
            },
            "writable":false,
            "observable":false,
            "query":[
                {
                    "name":"day",
                    "description":{ "ja":"履歴収集日", "en":"Day for collection" },
                    "data":{
                        "type":"number",
                        "minimum":0,
                        "maximum":99
                    }
                }
            ],
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"day",
                        "description":{ "ja":"履歴収集日", "en":"Day for collection" },
                        "data":{
                            "type":"number",
                            "minimum":0,
                            "maximum":99
                        }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"array",
                            "data":{
                                "type":"number",
                                "unit":"kWh",
                                "minimum":0,
                                "maximum":99999.9,
                                "minimumDigit":0.1
                            }
                        }
                    }
                ]
            }
        },
        {
            "name":"revDirIntegralEnergy",
            "epc":"0xE3",
            "description":{
                "ja":"積算電力量計測値（逆方向計測値）",
                "en":"Measured cumulative amount of electric energy (reverse direction)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh",
                "minimum":0,
                "maximum":99999.9,
                "minimumDigit":0.1
            }
        },
        {
            "name":"revDirIntegralEnergyLog1",
            "epc":"0xE4",
            "description":{
                "ja":"積算電力量計測値履歴1（逆方向計測値）",
                "en":"Historical data of measured cumulative amounts of electric energy 1(reverse direction)"
            },
            "writable":false,
            "observable":false,
            "query":[
                {
                    "name":"day",
                    "description":{ "ja":"履歴収集日", "en":"Day for collection" },
                    "data":{
                        "type":"number",
                        "minimum":0,
                        "maximum":99
                    }
                }
            ],
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"day",
                        "description":{ "ja":"履歴収集日", "en":"Day for collection" },
                        "data":{
                            "type":"number",
                            "minimum":0,
                            "maximum":99
                        }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"array",
                            "data":{
                                "type":"number",
                                "unit":"kWh",
                                "minimum":0,
                                "maximum":99999.9,
                                "minimumDigit":0.1
                            }
                        }
                    }
                ]
            }
        },
        {
            "name":"instantaneousPower",
            "epc":"0xE7",
            "description":{
                "ja":"瞬時電力計測値",
                "en":"Measured instantaneous electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":-2147483647,
                "maximum":2147483645
            }
        },
        {
            "name":"instantaneousCurrent",
            "epc":"0xE8",
            "description":{ "ja":"瞬時電流計測値", "en":"Measured instantaneous currents" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"r",
                        "description":{ "ja":"R相", "en":"R phase" },
                        "data":{
                            "type":"number",
                            "unit":"A",
                            "minimum":-3276.7,
                            "maximum":3276.5,
                            "minimumDigit":0.1
                        }
                    },
                    {
                        "name":"t",
                        "description":{ "ja":"T相", "en":"T phase" },
                        "data":{
                            "type":"number",
                            "unit":"A",
                            "minimum":-3276.7,
                            "maximum":3276.5,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"normDirIntegralEnergyEvery30Min",
            "epc":"0xEA",
            "description":{
                "ja":"定時積算電力量計測値（正方向計測値）",
                "en":"Cumulative amounts of electric energy measured at fixed time (normal direction)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and time" },
                        "data":{ "type":"date-time" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"number",
                            "unit":"kWh",
                            "minimum":0,
                            "maximum":99999.9,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"revDirIntegralEnergyEvery30Min",
            "epc":"0xEB",
            "description":{
                "ja":"定時積算電力量計測値（逆方向計測値）",
                "en":"Cumulative amounts of electric energy measured at fixed time (reverse direction)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and time" },
                        "data":{ "type":"date-time" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"number",
                            "unit":"kWh",
                            "minimum":0,
                            "maximum":99999.9,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"integralEnergyLog2",
            "epc":"0xEC",
            "description":{
                "ja":"積算電力量計測値履歴2（正方向、逆方向計測値）",
                "en":"Historical data of measured cumulative amounts of electric energy 2(normal and reverse directions)"
            },
            "writable":false,
            "observable":false,
            "query":[
                {
                    "name":"dateAndTime",
                    "description":{ "ja":"日時", "en":"Date and time" },
                    "data":{"type":"date-time"}
                },
                {
                    "name":"numberOfCollectionSegments",
                    "description":{ "ja":"収集コマ数", "en":"Number of collection segments" },
                    "data":{
                        "type":"number",
                        "minimum":1,
                        "maximum":12
                    }
                }
            ],
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"dayAndTime",
                        "description":{ "ja":"日時", "en":"Day and Time" },
                        "data":{"type":"day-time"}
                    },
                    {
                        "name":"numberOfCollectionSegments",
                        "description":{ "ja":"収集コマ数", "en":"Number of collection segments" },
                        "data":{
                            "type":"number",
                            "minimum":1,
                            "maximum":12
                        }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"積算電力量", "en":"Integral energy" },
                        "data":{
                            "type":"array",
                            "data":{
                                "type":"object",
                                "elements":[
                                    {
                                        "name":"normDirEnergy",
                                        "description":{ "ja":"正方向", "en":"Normal direction" },
                                        "data":{
                                            "type":"number",
                                            "unit":"kWh",
                                            "minimum":0,
                                            "maximum":99999.9,
                                            "minimumDigit":0.1
                                        }
                                    },
                                    {
                                        "name":"revDirEnergy",
                                        "description":{ "ja":"逆方向", "en":"Reverse direction" },
                                        "data":{
                                            "type":"number",
                                            "unit":"kWh",
                                            "minimum":0,
                                            "maximum":99999.9,
                                            "minimumDigit":0.1
                                        }
                                    }
                                ]
                            }
                        }
                    }
                ]
            },
            "note":{
                "ja":"queryの分の値は0または30とする。それ以外の値は0または30に丸められる。秒の値は無視される",
                "en":"about query, value of minutes should be either 0 or 30, and value of seconds is ignored"
            }
        }
    ]
}
```

## 高圧スマート電力量メータ:hvSmartElectricEnergyMeter:0x028A 

　以下のECHONET LiteのPropertyは、ECHONET LiteでGETした値に係数（EPC=0xD3）と係数の倍率（EPC=0xD4）と積算有効電力量単位（EPC=0xE6）を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

> 積算有効電力量計測値  
> 定時積算有効電力量計測値  
> 積算有効電力量計測値履歴  

　以下のECHONET LiteのPropertyは、ECHONET LiteでGETした値に係数（EPC=0xD3）と係数の倍率（EPC=0xD4）と需要電力単位（EPC=0xC5）を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

> 月間最大需要電力  
> 定時需要電力(30分平均電力)  
> 需要電力計測値履歴  

　以下のECHONET LiteのPropertyは、ECHONET LiteでデータをGETする前に「積算履歴収集日（EPC=0xE1)」をSETする必要がある。EL-WebAPIでは以下のPropertyをGET(REST)する際に「積算履歴収集日」の値をqueryとして指定する。ブリッジはECHONET Liteで「積算履歴収集日」をSETしたのち「需要電力計測値履歴」または「積算有効電力量計測値履歴」をGETする。なお、EL-WebAPIでqueryを付加しない場合は「積算履歴収集日」=0として扱う。

> 需要電力計測値履歴  
> 積算有効電力量計測値履歴  

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|monthlyMaxPowerDemand|GET|number|0xC1|月間最大需要電力<br>Monthly maximum electric power demand|\*1|
|integralMaxPowerDemand|GET|number|0xC2|累積最大需要電力<br>Cumulative maximum electric power demand|\*1|
|averagePowerDemand|GET|object|0xC3|定時需要電力（30分平均電力）<br>Electric power demand at fixed time(30-minute average electric power)|\*1|
|PowerDemandLog|GET|object|0xC6|需要電力計測値履歴<br>Historical data of measured electric power demand|\*1, \*2|
|integralReactivePowerConsumption|GET|object|0xCA|力測積算無効電力量(遅れ)計測値<br>Measurement data of reactive electric power consumption (lag) for power factor measurement|\*1|
|integralReactivePowerConsumptionEvery30Min|GET|object|0xCB|定時力測積算無効電力量(遅れ)計測値<br>Measurement data of cumulative amount of reactive electric power consumption (lag) at fixed time for power factor measurement|\*1|
|integralReactivePowerConsumptionLog|GET|object|0xCE|力測積算無効電力量(遅れ)計測値履歴<br>Historical data of measurement data of cumulative amount of reactive electric power consumption (lag) for power factor measurement|\*1|
|fixedDate|GET|number|0xE0|確定日<br>Fixed date|\*1|
|integralActiveEnergy|GET|object|0xE2|積算有効電力量計測値<br>Measured cumulative amounts of active electric energy|\*1|
|integralActiveEnergyEvery30Min|GET|object|0xE3|定時積算有効電力量計測値<br>Cumulative amounts of active electric energy at fixed time|\*1|
|integralActiveEnergyForPowerFactor|GET|object|0xE4|力測積算有効電力量計測値<br>Measurement data of cumulative amounts of active electric energy for power factor measurement|\*1|
|activeEnergyLog|GET|object|0xE7|積算有効電力量計測値履歴<br>Historical data of measured cumulative amount of active electric energy|\*1, \*2|

\*1 AIF対応プロパティ
\*2 queryあり  

### Device Description

```
{
    "type":"hvSmartElectricEnergyMeter",
    "eoj":"0x028A",
    "description":{"ja":"高圧スマート電力量メータ", "en":"High Voltage Smart Electric Energy Meter"},
    "properties":[
        {
            "name":"monthlyMaxPowerDemand",
            "epc":"0xC1",
            "description":{ "ja":"月間最大需要電力", "en":"Monthly maximum electric power demand" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kW",
                "minimum":0,
                "maximum":9999999.9,
                "minimumDigit":0.1
            }
        },
        {
            "name":"integralMaxPowerDemand",
            "epc":"0xC2",
            "description":{ "ja":"累積最大需要電力", "en":"Cumulative maximum electric power demand" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kW",
                "minimum":0,
                "maximum":9999999.9,
                "minimumDigit":0.1
            }
        },
        {
            "name":"averagePowerDemand",
            "epc":"0xC3",
            "description":{
                "ja":"定時需要電力（30分平均電力）",
                "en":"Electric power demand at fixed time(30-minute average electric power)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and time" },
                        "data":{ "type":"date-time" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"Energy" },
                        "data":{
                            "type":"number",
                            "unit":"kW",
                            "minimum":0,
                            "maximum":9999999.9,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"PowerDemandLog",
            "epc":"0xC6",
            "description":{
                "ja":"需要電力計測値履歴",
                "en":"Historical data of measured electric power demand"
            },
            "writable":false,
            "observable":false,
            "query":[
                {
                    "name":"day",
                    "description":{ "ja":"履歴収集日", "en":"Day for collection" },
                    "data":{
                        "type":"number",
                        "minimum":0,
                        "maximum":99
                    }
                }
            ],
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"day",
                        "description":{ "ja":"履歴収集日", "en":"Day for collection" },
                        "data":{
                            "type":"number",
                            "unit":"day",
                            "minimum":0,
                            "maximum":99
                        }
                    },
                    {
                        "name":"power",
                        "description":{ "ja":"需要電力", "en":"Power demand" },
                        "data":{
                            "type":"array",
                            "data":{
                                "type":"number",
                                "unit":"kW",
                                "minimum":0,
                                "maximum":9999999.9,
                                "minimumDigit":0.1
                            }
                        }
                    }
                ]
            }
        },
        {
            "name":"integralReactivePowerConsumption",
            "epc":"0xCA",
            "description":{
                "ja":"力測積算無効電力量(遅れ)計測値",
                "en":"Measurement data of reactive electric power consumption (lag) for power factor measurement"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and time" },
                        "data":{ "type":"date-time" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"無効電力量", "en":"Reactive electric power consumption" },
                        "data":{
                            "type":"number",
                            "unit":"kvarh",
                            "minimum":0,
                            "maximum":9999999.9,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"integralReactivePowerConsumptionEvery30Min",
            "epc":"0xCB",
            "description":{
                "ja":"定時力測積算無効電力量(遅れ)計測値",
                "en":"Measurement data of cumulative amount of reactive electric power consumption (lag) at fixed time for power factor measurement"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and time" },
                        "data":{ "type":"date-time" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"無効電力量", "en":"Reactive Electric Power Consumption" },
                        "data":{
                            "type":"number",
                            "unit":"kvarh",
                            "minimum":0,
                            "maximum":9999999.9,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"integralReactivePowerConsumptionLog",
            "epc":"0xCE",
            "description":{
                "ja":"力測積算無効電力量(遅れ)計測値履歴",
                "en":"Historical data of measurement data of cumulative amount of reactive electric power consumption (lag) for power factor measurement"
            },
            "writable":false,
            "observable":false,
            "query":[
                {
                    "name":"day",
                    "description":{ "ja":"履歴収集日", "en":"Day for collection" },
                    "data":{
                        "type":"number",
                        "minimum":0,
                        "maximum":99
                    }
                }
            ],
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"day",
                        "description":{ "ja":"履歴収集日", "en":"Day for collection" },
                        "data":{
                            "type":"number",
                            "unit":"day",
                            "minimum":0,
                            "maximum":99
                        }
                    },
                    {
                        "name":"power",
                        "description":{ "ja":"無効電力量", "en":"Reactive Electric Power Consumption" },
                        "data":{
                            "type":"array",
                            "data":{
                                "type":"number",
                                "unit":"kvarh",
                                "minimum":0,
                                "maximum":9999999.9,
                                "minimumDigit":0.1
                            }
                        }
                    }
                ]
            }
        },
        {
            "name":"fixedDate",
            "epc":"0xE0",
            "description":{ "ja":"確定日", "en":"Fixed Date" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "minimum":1,
                "maximum":31
            }
        },
        {
            "name":"integralActiveEnergy",
            "epc":"0xE2",
            "description":{
                "ja":"積算有効電力量計測値",
                "en":"Measured cumulative amounts of active electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and time" },
                        "data":{ "type":"date-time" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"number",
                            "unit":"kWh",
                            "minimum":0,
                            "maximum":9999999.9,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"integralActiveEnergyEvery30Min",
            "epc":"0xE3",
            "description":{
                "ja":"定時積算有効電力量計測値",
                "en":"Cumulative amounts of active electric energy at fixed time"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and time" },
                        "data":{ "type":"date-time" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"number",
                            "unit":"kWh",
                            "minimum":0,
                            "maximum":9999999.9,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"integralActiveEnergyForPowerFactor",
            "epc":"0xE4",
            "description":{
                "ja":"力測積算有効電力量計測値",
                "en":"Measurement data of cumulative amounts of active electric energy for power factor measurement"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and time" },
                        "data":{ "type":"date-time" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"number",
                            "unit":"kWh",
                            "minimum":0,
                            "maximum":9999999.9,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"activeEnergyLog",
            "epc":"0xE7",
            "description":{
                "ja":"積算有効電力量計測値履歴",
                "en":"Historical data of measured cumulative amount of active electric energy"
            },
            "writable":false,
            "observable":false,
            "query":[
                {
                    "name":"day",
                    "description":{ "ja":"履歴収集日", "en":"Day for collection" },
                    "data":{
                        "type":"number",
                        "minimum":0,
                        "maximum":99
                    }
                }
            ],
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"day",
                        "description":{ "ja":"日", "en":"Day" },
                        "data":{
                            "type":"number",
                            "unit":"day"
                        }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"Energy" },
                        "data":{
                            "type":"array",
                            "data":{
                                "type":"number",
                                "unit":"kWh",
                                "minimum":0,
                                "maximum":9999999.9,
                                "minimumDigit":0.1
                            }
                        }
                    }
                ]
            }
        }
    ]
}
```

## 一般照明:generalLighting:0x0290
### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|brightness|GET, PUT|number|0xB0|照度レベル設定<br>Illuminance level|\*1|
|operationMode|GET, PUT|enum|0xB6|点灯モード設定<br>Lighting mode setting|\*1|
| rgb |GET, PUT|object|0xC0|カラー灯モード時RGB設定<br>RGB setting for color lighting|

\*1 AIF対応プロパティ

### Device Description

```
{
    "type":"generalLighting",
    "eoj":"0x0290",
    "description":{"ja":"一般照明", "en":"General Lighting"},
    "properties":[
        {
            "name":"brightness",
            "epc":"0xB0",
            "description":{ "ja":"照度レベル設定", "en":"Illuminance level" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"operationMode",
            "epc":"0xB6",
            "description":{ "ja":"点灯モード設定", "en":"Lighting mode setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"auto", "ja":"自動", "en":"Auto Lighting", "edt":"0x41"},
                    {"value":"normal", "ja":"通常灯", "en":"Normal Lighting", "edt":"0x42"},
                    {"value":"night", "ja":"常夜灯", "en":"Night Lighting", "edt":"0x43"},
                    {"value":"color", "ja":"カラー灯", "en":"Color Lighting", "edt":"0x45"}
                ]
            }
        },
        {
            "name":"rgb",
            "epc":"0xC0",
            "description":{ "ja":"カラー灯モード時RGB設定", "en":"RGB setting for color lighting" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"r",
                        "description":{ "ja":"赤", "en":"Red" },
                        "data":{
                            "type":"number",
                            "minimum":0,
                            "maximum":255
                        }
                    },
                    {
                        "name":"g",
                        "description":{ "ja":"緑", "en":"Green" },
                        "data":{
                            "type":"number",
                            "minimum":0,
                            "maximum":255
                        }
                    },
                    {
                        "name":"b",
                        "description":{ "ja":"青", "en":"Blue" },
                        "data":{
                            "type":"number",
                            "minimum":0,
                            "maximum":255
                        }
                    }
                ]
            }
        }
    ]
}
```

## 単機能照明:monoFunctionalLighting:0x0291 
### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|brightness|GET, PUT|number|0xB0|照度レベル設定<br>Illuminance level|\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"monoFunctionalLighting",
    "eoj":"0x0291",
    "description":{"ja":"単機能照明", "en":"Mono Functional Lighting"},
    "properties":[
        {
            "name":"brightness",
            "epc":"0xB0",
            "description":{ "ja":"照度レベル設定", "en":"Illuminance level" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        }
    ]
}
```

## 電気自動車充電器:evCharger:0x02A1

　ECHONET LiteのProperty「車両接続・充放電可否状態（EPC=0xC7）」は、ECHONET LiteでデータをGETする前に「車両接続確認（EPC=0xCD)」をSETする必要がある。ブリッジがEL-WebAPIでGET:ChargeDischargeStatusを受信した場合、ブリッジはECHONET Liteで「車両接続確認」をSETしたのち「積算電力量計測値履歴1」をGETする。

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|ratedChargePower|GET|number|0xC5|定格充電能力<br>Rated charge capacity|\*1|
|chargeStatus|GET|enum|0xC7|車両接続・充電可否状態<br>Vehicle connection and chargeable status|\*1|
|minimumAndMaximumChargingPower|GET|object|0xC8|最小大充電電力値<br>Minimum/maximum charging electric energy|\*1|
|minimumAndMaximumChargeCurrent|GET|object|0xCA|最小最大充電電流値<br>Minimum/maximum charging current|\*1|\*1|
|equipmentType|GET|enum|0xCC|充電器タイプ<br>Charger type|\*1|
|chargeableCapacity|GET|number|0xCE|車載電池の充電可能容量値<br>Chargeable capacity of vehicle mounted battery|\*1|
|remainingChargeableCapacity|GET|number|0xCF|車載電池の充電可能残容量値<br>Remaining Chargeable capacity of vehicle mounted battery|\*1|
|usedCapacity1|GET|number|0xD0|車載電池の使用容量値1<br>Used capacity of vehicle mounted battery 1|\*1|
|ratedVoltage|GET|number|0xD2|定格電圧<br>Rated voltage|\*1|
|measuredInstantaneousPower|GET|number|0xD3|瞬時充電電力計測値<br>Measured instantaneous chargingelectric energy|\*1|
|measuredCumulativeChargeEnergy|GET|number|0xD8|積算充電電力量計測値<br>Measured cumulative amount of charging electric energy|\*1|
|operationMode|GET, PUT|enum|0xDA|運転モード設定<br>Operation mode setting|\*1|
|remainingStoredEnergy1|GET|number|0xE2|車載電池の電池残容量1<br>Remaining stored electricity of vehicle mounted battery1|\*1|
|remainingStoredEnergy3|GET|number|0xE4|車載電池の電池残容量3<br>Remaining stored electricity of vehicle mounted battery3|\*1|
|vehicleID|GET|string|0xE6|車両ID<br>Identification of electric vehicle|\*1|
|targetChargingEnergy1|GET, PUT|number|0xE7|充電量設定値１<br>Charging amount 1|\*1|
|chargingPower|GET, PUT| number |0xEB|充電電力設定値<br>Charging electrnic energy setting|\*1|
|chargingCurrent|GET, PUT| number |0xED|充電電流設定値<br>Charging current setting|\*1|

\*1 AIF対応プロパティ

### Device Description

```
{
    "type":"evChargerDischarger",
    "eoj":"0x02A1",
    "description":{"ja":"電気自動車充電器", "en":"EV Charger"},
    "properties":[
        {
            "name":"ratedChargePower",
            "epc":"0xC5",
            "description":{ "ja":"定格充電能力", "en":"Rated charge capacity" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"chargeStatus",
            "epc":"0xC7",
            "description":{
                "ja":"車両接続・充電可否状態",
                "en":"Vehicle connection and chargeable status"
            },
            "writable":false,
            "observable":true,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"undefined", "ja":"不定", "en":"Undefined", "edt":"0xFF"},
                    {"value":"notConnected", "ja":"車両未接続", "en":"Not Connected", "edt":"0x30"},
                    {"value":"notChargeable", "ja":"車両接続・ 充電不可", "en":"Not Chargeable", "edt":"0x40"},
                    {"value":"chargeable", "ja":"車両接続・ 充電可", "en":"Chargeable", "edt":"0x41"},
                    {"value":"unknown", "ja":"車両接続・ 充電可否不明", "en":"Unknown", "edt":"0x44"}
                ]
            }
        },
        {
            "name":"minimumAndMaximumChargingPower",
            "epc":"0xC8",
            "description":{
                "ja":"最小最大充電電力値",
                "en":"Minimum/maximum charging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"minimumPower",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    },
                    {
                        "name":"maximumPower",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"number",
                            "unit":"W",
                            "minimum":0,
                            "maximum":999999999
                        }
                    }
                ]
            }
        },
        {
            "name":"minimumAndMaximumChargeCurrent",
            "epc":"0xCA",
            "description":{
                "ja":"最小最大充電電流値",
                "en":"Minimum/maximum charging electric current"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"minimumCurrent",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"number",
                            "unit":"A",
                            "minimum":0,
                            "maximum":3276.6,
                            "minimumDigit":0.1
                        }
                    },
                    {
                        "name":"maximumCurrent",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"number",
                            "unit":"A",
                            "minimum":0,
                            "maximum":3276.6,
                            "minimumDigit":0.1
                        }
                    }
                ]
            }
        },
        {
            "name":"equipmentType",
            "epc":"0xCC",
            "description":{ "ja":"充放電器タイプ", "en":"Charger/Discharger type" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"AC_CPLT", "ja":"AC_CPLT", "en":"AC_CPLT", "edt":"0x11"},
                    {"value":"AC_HLC_Charge", "ja":"AC_HLC（充電のみ）", "en":"AC_HLC_Charge", "edt":"0x12"},
                    {"value":"DC_AA_Charge", "ja":"DCタイプ_AA（充電のみ）", "en":"DC_AA_Charge", "edt":"0x21"},
                    {"value":"DC_BB_Charge", "ja":"DCタイプ_BB（充電のみ）", "en":"DC_BB_Charge", "edt":"0x31"},
                    {"value":"DC_EE_Charge", "ja":"DCタイプ_EE（充電のみ）", "en":"DC_EE_Charge", "edt":"0x41"},
                    {"value":"DC_FF_Charge", "ja":"DCタイプ_FF（充電のみ）", "en":"DC_FF_Charge", "edt":"0x51"}
                ]
            }
        },
        {
            "name":"chargeableCapacity",
            "epc":"0xCE",
            "description":{ "ja":"車載電池の充電可能容量値", "en":"Chargeable capacity of vehicle mounted battery" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"remainingChargeableCapacity",
            "epc":"0xCF",
            "description":{ "ja":"車載電池の充電可能残容量値", "en":"Remaining Chargeable capacity of vehicle mounted battery" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"usedCapacity1",
            "epc":"0xD0",
            "description":{
                "ja":"車載電池の使用容量値1",
                "en":"Used capacity of vehicle mounted battery 1"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh"
            }
        },
        {
            "name":"ratedVoltage",
            "epc":"0xD2",
            "description":{
                "ja":"定格電圧",
                "en":"Rated voltage"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"V",
                "minimum":0,
                "maximum":32766
            }
        },
        {
            "name":"measuredInstantaneousPower",
            "epc":"0xD3",
            "description":{
                "ja":"瞬時充電電力計測値",
                "en":"Measured instantaneous charging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":-999999999,
                "maximum":999999999
            }
        },
        {
            "name":"measuredintegralChargeEnergy",
            "epc":"0xD8",
            "description":{
                "ja":"積算充電電力量計測値",
                "en":"Measured cumulative amount of charging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"kWh",
                "minimum":0,
                "maximum":999999.999,
                "minimumDigit":0.001
            }
        },
        {
            "name":"operationMode",
            "epc":"0xDA",
            "description":{ "ja":"運転モード設定", "en":"Operation mode setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"enum",
                "values":[
                    {"value":"charge", "ja":"充電", "en":"Charge", "edt":"0x42"},
                    {"value":"standby", "ja":"待機", "en":"Standby", "edt":"0x44"},
                    {"value":"idle", "ja":"停止", "en":"Idle", "edt":"0x47"},
                    {"value":"other", "ja":"その他", "en":"Other", "edt":"0x40"}
                ]
            }
        },
        {
            "name":"remainingStoredEnergy1",
            "epc":"0xE2",
            "description":{
                "ja":"車載電池の電池残容量1",
                "en":"Remaining stored electricity of vehicle mounted battery1"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh"
            }
        },
        {
            "name":"remainingStoredEnergy3",
            "epc":"0xE4",
            "description":{
                "ja":"車載電池の電池残容量3",
                "en":"Remaining stored electricity of vehicle mounted battery3"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"vehicleID",
            "epc":"0xE6",
            "description":{
                "ja":"車両ID",
                "en":"Identification of electric vehicle"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"string"
            }
        },
        {
            "name":"targetChargingEnergy1",
            "epc":"0xE7",
            "description":{"ja":"充電量設定値１", "en":"Charging amount setting 1"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"Wh",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"chargingPower",
            "epc":"0xEB",
            "description":{"ja":"充電電力設定値", "en":"Charging electric energy setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":999999999
            }
        },
        {
            "name":"chargingCurrent",
            "epc":"0xED",
            "description":{"ja":"充電電流設定値", "en":"Charging current setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"A",
                "minimum":0,
                "maximum":6553.3,
                "minimumDigit":0.1
            }
        }
    ]
}
```

## 拡張照明システム:monoFunctionalLighting:0x02A4 
### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|brightness|GET, PUT|number|0xB0|照度レベル設定<br>Illuminance level|\*1|
|sceneId|GET, PUT|number|0xC0|シーン制御設定<br>Scene Control Setting|\*1|
|maxNumberOfsceneId|GET|number|0xC1|シーン制御設定可能数<br>XX||
|powerConsumptionRateList|GET|number|0xC2|電力消費率リスト<br>Power Consumption Rate List|\*1|
|powerConsumptionAtFullLighting|GET|number|0xC3|全灯時消費電力<br>Power Consumption at Full Lighting|\*1|
|powerConsumptionWillBeSaved|GET|number|0xC4|節電可能消費電力<br>Power Consumption that will be saved|\*1|
|powerConsumptionLimitSetting|GET, PUT|number|0xC5|消費電力制限設定<br>Power Consumption Limit Setting|\*1|

\*1 AIF対応プロパティ

```
{
    "type":"enhancedLightingSystem",
    "eoj":"0x02A4",
    "description":{"ja":"拡張照明システム","en":"Enhanced Lighting System"},
    "properties":[
        {
            "name":"brightness",
            "epc":"0xB0",
            "description":{ "ja":"照度レベル設定","en":"Brightness"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"integer",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"sceneControlSetting",
            "epc":"0xC0",
            "description":{ "ja":"シーン制御設定","en":"Scene Control Setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"integer",
                "minimum":1,
                "maximum":253
            },
            "note":{"ja":"最大値はmaxNumberOfsceneControlの値", "en":"Maximum value is the value of maxNumberOfsceneControl"}
        },
        {
            "name":"maxNumberOfsceneControl",
            "epc":"0xC1",
            "description":{ "ja":"シーン制御設定可能数","en":"XXX"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"integer",
                "minimum":1,
                "maximum":253
            }
        },
        {
            "name":"powerConsumptionRateList",
            "epc":"0xC2",
            "description":{ "ja":"電力消費率リスト","en":"Power Consumption Rate List"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"array",
                "data":{
                    "type":"integer",
                    "unit":"%",
                    "minimum":0,
                    "maximum":100
                }
            }
        },
        {
            "name":"powerConsumptionAtFullLighting",
            "epc":"0xC3",
            "description":{"ja":"全灯時消費電力","en":"Power Consumption at Full Lighting"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":65533
            }
        },
        {
            "name":"powerConsumptionWillBeSaved",
            "epc":"0xC4",
            "description":{"ja":"節電可能消費電力","en":"Power Saving Power Consumption"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":65533
            }
        },
        {
            "name":"powerConsumptionLimitSetting",
            "epc":"0xC5",
            "description":{"ja":"消費電力制限設定","en":"Power Consumption Limit Setting"},
            "writable":true,
            "observable":false,
            "data":{
                "type":"number",
                "unit":"W",
                "minimum":0,
                "maximum":65533
            }
        }
    ]
}
```

## 冷凍冷蔵庫:refrigerator:0x03B7

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|doorIsOpen<br>ドア開閉状態|GET|boolean|0xB0|ドア開閉状態<br>Door open/close status|

### Device Description

```
{
    "type":"refrigerator",
    "eoj":"0x03B7",
    "description":{"ja":"冷凍冷蔵庫", "en":"refrigerator"},
    "properties":[
        {
            "name":"doorIsOpen",
            "epc":"0xB0",
            "description":{ "ja":"ドア開閉状態", "en":"Door open/close status" },
            "writable":false,
            "observable":false,
            "data":{ 
            "type":"boolean",
                "values":[
                    {"value":true, "ja":"開", "en":"Open", "edt":"0x41"},
                    {"value":false, "ja":"閉", "en":"Close", "edt":"0x42"}
                ]
            }
        }
    ]
}
```

## オーブンレンジ:combinationMicrowaveOven:0x03B8

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|doorIsOpen <br>ドア開閉状態|GET|boolean|0xB0|ドア開閉状態<br>Door open/close status|\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"combinationMicrowaveOven",
    "eoj":"0x03B8",
    "description":{"ja":"オーブンレンジ", "en":"Combination Microwave Oven"},
    "properties":[
        {
            "name":"doorIsOpen",
            "epc":"0xB0",
            "description":{ "ja":"ドア開閉状態", "en":"Door open/close status" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"開", "en":"Open", "edt":"0x41"},
                    {"value":false, "ja":"閉", "en":"Close", "edt":"0x42"}
                ]
            }
        }
    ]
}
```

## クッキングヒータ:cookingHeater:0x03B9

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|heatingStatus|GET|object|0xB1|加熱状態<br>Heating Status|

### Actions

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|cancelAll|POST||0xB3|一括停止設定<br>"All Stop" setting|

### Device Description

```
{
    "type":"cookingHeater",
    "eoj":"0x03B9",
    "description":{"ja":"クッキングヒータ", "en":"Cooking Heater"},
    "properties":[
        {
            "name":"heatingStatus",
            "epc":"0xB1",
            "description":{ "ja":"過熱状態", "en":"heating Status" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"leftStove",
                        "description":{ "ja":"左コンロ", "en":"Left Stove" },
                        "data":{
                            "type":"enum",
                            "values":[
                                {"value":"standby", "ja":"待機", "en":"standby", "edt":"0x40"},
                                {"value":"heating", "ja":"運転", "en":"heating", "edt":"0x41"},
                                {"value":"pause", "ja":"一時停止", "en":"pause", "edt":"0x42"},
                                {"value":"heatingProhibited", "ja":"加熱禁止", "en":"heatingProhibited", "edt":"0x50"},
                                {"value":"unknown", "ja":"不明", "en":"unknown", "edt":"0xFF"}
                            ]
                        }
                    },
                    {
                        "name":"rightStove",
                        "description":{ "ja":"右コンロ", "en":"Right Stove" },
                        "data":{
                            "type":"enum",
                            "values":[
                                {"value":"standby", "ja":"待機", "en":"standby", "edt":"0x40"},
                                {"value":"heating", "ja":"運転", "en":"heating", "edt":"0x41"},
                                {"value":"pause", "ja":"一時停止", "en":"pause", "edt":"0x42"},
                                {"value":"heatingProhibited", "ja":"加熱禁止", "en":"heatingProhibited", "edt":"0x50"},
                                {"value":"unknown", "ja":"不明", "en":"unknown", "edt":"0xFF"}
                            ]
                        }
                    },
                    {
                        "name":"farSideStove",
                        "description":{ "ja":"奥コンロ", "en":"Far-Side Stove" },
                        "data":{
                            "type":"enum",
                            "values":[
                                {"value":"standby", "ja":"待機", "en":"standby", "edt":"0x40"},
                                {"value":"heating", "ja":"運転", "en":"heating", "edt":"0x41"},
                                {"value":"pause", "ja":"一時停止", "en":"pause", "edt":"0x42"},
                                {"value":"heatingProhibited", "ja":"加熱禁止", "en":"heatingProhibited", "edt":"0x50"},
                                {"value":"unknown", "ja":"不明", "en":"unknown", "edt":"0xFF"}
                            ]
                        }
                    },
                    {
                        "name":"roaster",
                        "description":{ "ja":"ロースター", "en":"Roaster" },
                        "data":{
                            "type":"enum",
                            "values":[
                                {"value":"standby", "ja":"待機", "en":"standby", "edt":"0x40"},
                                {"value":"heating", "ja":"運転", "en":"heating", "edt":"0x41"},
                                {"value":"pause", "ja":"一時停止", "en":"pause", "edt":"0x42"},
                                {"value":"heatingProhibited", "ja":"加熱禁止", "en":"heatingProhibited", "edt":"0x50"},
                                {"value":"unknown", "ja":"不明", "en":"unknown", "edt":"0xFF"}
                            ]
                        }
                    }
                ]
            }
        }
    ],
    "actions":[ "cancelAll" ]
}
```

## 洗濯乾燥機:washerDryer:0x03D3

### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|doorIsOpen <br>ドア開閉状態|GET|boolean|0xB0|ドア開閉状態<br>Door open/close status|\*1|
|timeToFinish|GET|time|0xED|洗濯乾燥残り時間<br>Time remaining to complete washer and dryer cycle|\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"washerDryer",
    "eoj":"0x03D3",
    "description":{"ja":"洗濯乾燥機", "en":"Washer and Dryer"},
    "properties":[
        {
            "name":"doorIsOpen",
            "epc":"0xB0",
            "description":{ "ja":"ドア開閉状態", "en":"Door open/close status" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"開", "en":"Open", "edt":"0x41"},
                    {"value":false, "ja":"閉", "en":"Close", "edt":"0x42"}
                ]
            }
        },
        {
            "name":"timeToFinish",
            "epc":"0xED",
            "description":{
                "ja":"洗濯乾燥残り時間",
                "en":"Time remaining to complete washer and dryer cycle"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"time"
            }
        }
    ]
}
```
## スイッチ:switch:0x05FD
個別Propertyは存在しない

```
{
    "type":"switch",
    "eoj":"0x05FD",
    "description":{"ja":"スイッチ", "en":"Switch"},
    "properties":[]
}
```

## コントローラ:controller:0x05FF
### Properties

|Property Name|Access Method|Data Type|EPC(EL)|プロパティ名称(EL)|Note|
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
|controllerId|GET|string|0xC0|コントローラID<br>Controller ID|\*1|
|numberOfDevices|GET|number|0xC2|管理台数<br>Number of devices controlled|\*1|
|deviceInfo|GET|object|-|機器情報<br>Device Information|\*1 \*2 \*3|

\*1 AIF対応プロパティ
\*2 queryあり
\*3 EPC=0xC4...0xCFに対応した値をobjectで表現する

### Device Description

```
{
    "type":"controller",
    "eoj":"0x05FF",
    "description":{"ja":"コントローラ", "en":"Controller"},
    "properties":[
        {
            "name":"controllerId",
            "epc":"0xC0",
            "description":{"ja":"コントローラID","en":"Controller ID"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"string"
            }
        },
        {
            "name":"numberOfDevices",
            "epc":"0xC2",
            "description":{"ja":"管理台数","en":"Number of devices controlled"},
            "writable":false,
            "observable":false,
            "data":{
                "type":"number",
                "minimum":0,
                "maximum":65533
            }
        },
        {
            "name":"deviceInfo",
            "description":{"ja":"機器情報", "en":"Device Information"},
            "writable":false,
            "observable":false,
            "query":[
                {
                    "name":"index",
                    "description":{ "ja":"インデックス", "en":"index" },
                    "data":{
                        "type":"number",
                        "minimum":1,
                        "maximum":65533
                    }
                }
            ],
            "data":{
                "type":"object",
                "elements":[
                    {
                        "name":"deviceId",
                        "description":{ "ja":"機器ID", "en":"Device ID" },
                        "data":{
                            "type":"string"
                        }
                    },
                    {
                        "name":"deviceType",
                        "description":{ "ja":"機種", "en":"Device Type" },
                        "data":{
                            "type":"string"
                        }
                    },
                    {
                        "name":"connection",
                        "description":{ "ja":"接続状態", "en":"Connection status" },
                        "data":{
                            "type":"enum",
                            "values":[
                                {"value":"connected", "ja":"接続中", "en":"Connected", "edt":"0x41"},
                                {"value":"disconnected", "ja":"離脱中", "en":"Disconnected", "edt":"0x42"},
                                {"value":"notRegistered", "ja":"未登録", "en":"Not registered", "edt":"0x43"},
                                {"value":"deleted", "ja":"削除", "en":"Deleted", "edt":"0x44"}
                            ]
                        }
                    },
                    {
                        "name":"manufactureCode",
                        "description":{ "ja":"管理対象機器事業者コード", "en":"manufactureCode" },
                        "data":{
                            "type":"string"
                        }
                    }
                ]
            }
        }    
    ]
}
```
