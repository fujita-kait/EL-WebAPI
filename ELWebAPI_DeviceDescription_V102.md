# ECHONET Lite WebAPI (EL-WebAPI) Device Description

| Date | Version  | Description |
|:-----------|:-----|:-----|
| 2017.12.11 | version 1.0.0 |  |
| 2017.12.17 | version 1.0.1 | Device DescriptionのDataのvalueの表記をobjectからarrayに変更<br>Data Typeを修正(levelとpercentageを削除） |
| 2017.12.18 | version 1.0.2 | Property nameに関する軽微な修正（共通項目の"Operating Status" -> "Operation Status" |

## 1. Abstract
　このドキュメントはECHONET Lite WebAPI(EL-WebAPI)のDevice Descriptionを記述する。Device Descriptionの定義は __ECHONET Lite WebAPI Specification__ を参照のこと。

## 2. Scope
　このドキュメントで記述する対象機器は、重点８機器とECHONET Lite認証取得製品が存在する機器を前提とする。このドキュメントで記述するPropertyに関しては、ECHONET Liteで必須とされているものを前提とする。対象とするECHONET 機器オブジェクト詳細規定のバージョンは Release Iとする。

## 3. Sample

以下に一般照明のDevice Descriptionを例として示す。

__Example__

```
{
    "type":"generalLighting",
    "description":{"ja":"一般照明", "en":"General Lighting"},
    "properties":[
        {
            "name":"on",
            "description":{ "ja":"動作状態", "en":"Operation Status" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"ON", "en":"ON"}, 
                    {"value":false, "ja":"OFF", "en":"OFF"}
                ]
            }
        },
        {
            "name":"isAtFault",
            "description":{ "ja":"異常発生状態", "en":"Fault Status" },
            "writable":false,
            "observable":true,
            "data":{
                "type":"boolean",
                "value":[
                    {"value":true, "ja":"異常あり", "en":"Fault"},
                    {"value":false, "ja":"異常無し", "en":"No Fault"}
                ]
            }
        },
        {
            "name":"brightness",
            "description":{ "ja":"輝度", "en":"Brightness" },
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
            "name":"operatingMode",
            "description":{ "ja":"動作モード", "en":"Operating Mode" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"key",
                "values":[
                    {"value":"normal", "ja":"通常灯", "en":"Normal Lighting"},
                    {"value":"night", "ja":"常夜灯", "en":"Night Lighting"},
                    {"value":"color", "ja":"カラー灯", "en":"Color Lighting"}
                ]
            }
        },
        {
            "name":"rgb",
            "description":{ "ja":"RGB設定", "en":"RGB Value" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"r",
                        "description":{ "ja":"赤", "en":"Red" },
                        "data":{
                            "type":"integer",
                            "minimum":0,
                            "maximum":255
                        }
                    },
                    {
                        "name":"r",
                        "description":{ "ja":"赤", "en":"Red" },
                        "data":{
                            "type":"integer",
                            "minimum":0,
                            "maximum":255
                        }
                    },
                    {
                        "name":"r",
                        "description":{ "ja":"赤", "en":"Red" },
                        "data":{
                            "type":"integer",
                            "minimum":0,
                            "maximum":255
                        }
                    }
                ]
            }
        }
    ],
    "actions":[],
    "events":[
        { "name":"on" },
        { "name":"isAtFault" }
    ]
}
```

## 4. Device Description

### 4.1 共通項目  
　Propertiesのメンバーの "on", "isAtFault"は全ての機器で共通である。  

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| on | GET, PUT| boolean | 0x80 | 動作状態<br>Operation Status |INF|
| isAtFault |GET| boolean|0x88| 異常発生状態<br>Fault status |INF|

### Device Description (Common Items)

```
{
    "properties":[
        {
            "name":"on",
            "description":{ "ja":"動作状態状態", "en":"Operation Status" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"ON", "en":"ON"}, 
                    {"value":false, "ja":"OFF", "en":"OFF"}
                ]
            }
        },
        {
            "name":"isAtFault",
            "description":{ "ja":"異常発生状態", "en":"Fault Status" },
            "writable":false,
            "observable":true,
            "data":{
                "type":"boolean",
                "values":[
                    {"valu":true, "ja":"異常あり", "en":"Fault"},
                    {"value":false, "ja":"異常無し", "en":"No Fault"}
                ]
            }
        }
    ],
    "actions":[],
    "events":[
        { "name":"on" },
        { "name":"isAtFault" }
    ]
}
```

### 4.2 機器毎のDevice Description
以下に機器毎のDevice Descriptionを記述する。実際のDevice Descriptionは前節の共通項目をマージしたものとなる。
### List

| 機器名 | Device Type | EOJ |
|:------|:------------|:----|
| 照度センサ| illuminanceSensor| 0x000D |
| 温度センサ| temperatureSensor| 0x0011 |
| 湿度センサ| humiditySensor| 0x0012 |
| 電力量センサ| electricPowerSensor| 0x0022 |
| 気圧センサ| airPressureSensor| 0x002D |
| 家庭用エアコン| homeAirConditioner| 0x0130 |
| 換気扇| ventilationFan| 0x0133 |
| 空気清浄器| airCleaner| 0x0135 |
| 電動ブラインド・日よけ| electricBlind| 0x0260 |
| 電動雨戸・シャッター| electricRainDoor | 0x0263 |
| 電気温水器| electricWaterHeater| 0x026B  |
| 電気錠| electricKey| 0x026F  |
| 瞬間式給湯器| instataneousWaterHeater| 0x0272  |
| 浴室暖房乾燥機| bathroomHeaterDryer| 0x0273 |
| 住宅用太陽光発電| pvPowerGeneration| 0x0279  |
| 冷温水熱源機| hotWaterHeatSource| 0x027A |
| 床暖房| floorHeater| 0x027B |
| 燃料電池| fuelCell| 0x027C  |
| 蓄電池| storageBattery| 0x027D  |
| 電気自動車充放電器| evChargerDischarger| 0x027E  |
| 分電盤メータリング| powerDistributionBoard| 0x0287 |
| 低圧スマート電力量メータ| lvSmartElectricEnergyMeter| 0x0288  |
| 高圧スマート電力量メータ| hvSmartElectricEnergyMeter| 0x028A  |
| 一般照明| generalLighting| 0x0290  |
| 単機能照明| monoFunctionalLighting| 0x0291  |
| 電気自動車充電器| evCharger| 0x02A1 |
| 冷凍冷蔵庫| refrigerator| 0x03B7 |
| オーブンレンジ| combinationMicrowaveOven| 0x03B8 |
| クッキングヒータ| cookingHeater| 0x03B9 |
| 洗濯乾燥機| washerDryer| 0x03D3 |
| スイッチ| switch| 0x05FD |
| コントローラ| controller| 0x05FF |

## 照度センサ:illuminanceSensor:0x000D

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| illuminance | GET | integer | 0xE0 | 照度計測値1<br>Measured Illuminance value1 |

### Device Description

```
{
    "type":"illuminanceSensor",
    "description":{"ja":"照度センサ", "en":"Illuminance Sensor"},
    "properties":[
        {
            "name":"illuminance",
            "description":{ "ja":"照度計測値1", "en":"Illuminance value1" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer",
                "unit":"Lux"
            }
        }
    ]
}
```

## 温度センサ:temperatureSensor:0x0011

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| temperature | GET | number | 0xE0 | 温度計測値<br>Measured temperature value |

### Device Description

```
{
    "type":"temperatureSensor",
    "description":{"ja":"温度センサ", "en":"Temperature Sensor"},
    "properties":[
        {
            "name":"temperature",
            "description":{ "ja":"温度計測値", "en":"Measured temperature value" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number",
                "unit":"℃"
            }
        }
    ]
}
```

## 湿度センサ:humiditySensor:0x0012

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| humidity | GET | percentage| 0xE0 | 相対湿度計測値<br>Measured value of relative humidity |

### Device Description

```
{
    "type":"humiditySensor",
    "description":{"ja":"湿度センサ", "en":"Humidity Sensor"},
    "properties":[
        {
            "name":"humidity",
            "description":{ "ja":"相対湿度計測値", "en":"Measured value of relative humidity" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"integer",
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

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| electricEnergy | GET | integer| 0xE0 | 積算電力量計測値<br>Integral electric energy |

### Device Description

```
{
    "type":"electricPowerSensor",
    "description":{"ja":"電力量センサ", "en":"Electric Power Sensor"},
    "properties":[
        {
            "name":"electricEnergy",
            "description":{ "ja":"積算電力量計測値", "en":"Integral electric energy" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer",
                "unit":"kWh"            
            }
        }
    ]
}
```

## 気圧センサ:airPressureSensor:0x002D

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| airPressure | GET | integer| 0xE0 | 気圧計測値<br>Air pressure measurement |

### Device Description

```
{
    "type":"airPressureSensor",
    "description":{"ja":"気圧センサ", "en":"Air Pressure Sensor"},
    "properties":[
        {
            "name":"airPressure",
            "description":{ "ja":"気圧計測値", "en":"Air pressure measurement" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"hPa"
            }
        }
    ]
}
```

## 家庭用エアコン:homeAirConditioner:0x0130

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| powerSaving | GET, PUT| boolean | 0x8F | 節電動作設定<br>Power-saving operation setting | INF |
| airFlowLevel | GET, PUT | integer | 0xA0 | 風量設定<br>Air flow rate setting | INF |
| operatingMode | GET, PUT | key | 0xB0 | 運転モード設定<br>Operation mode setting | INF |
| targetTemperature | GET, PUT |integer | 0xB3 | 温度設定値<br>Set temperature value |
| humidity| GET|integer | 0xBA | 室内相対湿度計測値<br>Measured value of room relative humidity |\*1|
| roomTemperature | GET|number | 0xBB | 室内温度計測値<br>Measured value of room temperature |
| airFlowTemperature | GET|number | 0xBD | 吹き出し温度計測値<br>Measured cooled air temperature |\*1|
| outdoorTemperature | GET|number | 0xBE | 外気温度計測値<br>Measured outdoor air temperature |\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"homeAirConditioner",
    "description":{"ja":"家庭用エアコン", "en":"Home Air Conditioner"},
    "properties":[
        {
            "name":"powerSaving",
            "description":{ "ja":"節電動作設定", "en":"Power-saving operation setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"節電動作設定", "en":"Power Saving"},
                    {"value":false, "ja":"節電動作解除", "en":"No Power Saving"}
                ]
            }
        },
        {
            "name":"airFlowLevel",
            "description":{ "ja":"風量設定", "en":"Air flow rate setting" },
            "writable":true, 
            "observable":true,
            "data":{
                "type":"object", 
                "field":[
                    {
                        "name":"level",
                        "description":{ "ja":"レベル", "en":"Level" },
                        "data":{
                            "type":"integer",
                            "unit":"level",
                            "minimum":1,
                            "maximum":8
                        }
                    },
                    {
                        "name":"auto",
                        "description":{ "ja":"自動設定", "en":"Auto" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"自動", "en":"AUTO"}, 
                                {"value":false, "ja":"マニュアル", "en":"MANUAL"}
                            ]
                        }                        
                    }
                ]
            }
        },
        {
            "name":"operatingMode",
            "description":{ "ja":"運転モード設定", "en":"Operation mode setting" },
            "writable":true, 
            "observable":true,
            "data":{ 
                "type":"key",
                "values":[
                    {"value":"auto", "ja":"自動", "en":"Auto"}, 
                    {"value":"cooling", "ja":"冷房", "en":"Cooling"}, 
                    {"value":"heating", "ja":"暖房", "en":"Heating"},
                    {"value":"dehumidification", "ja":"除湿", "en":"Dehumidification"}, 
                    {"value":"circulation", "ja":"送風", "en":"Circulation"},
                    {"value":"other", "ja":"その他", "en":"Other"}
                ]
            }
        },
        {
            "name":"targetTemperature",
            "description":{ "ja":"温度設定値", "en":"Set temperature value" },
            "writable":true, 
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"℃", 
                "minimum":0, 
                "maximum":50
            }
        },
        {
            "name":"humidity",
            "description":{
                "ja":"室内相対湿度計測値",
                "en":"Measured value of room relative humidity"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"integer",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"roomTemperature",
            "description":{ "ja":"室内温度計測値", "en":"Measured value of room temperature" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number", 
                "unit":"℃"
            }
        },
        {
            "name":"airFlowTemperature",
            "description":{ "ja":"吹き出し温度計測値", "en":"Measured cooled air temperature" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number", 
                "unit":"℃"
            }
        },
        {
            "name":"outdoorTemperature",
            "description":{ "ja":"外気温度計測値", "en":"Measured outdoor air temperature" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"number", 
                "unit":"℃"
            }
        }
    ],
    "events":[
        { "name":"powerSaving" },
        { "name":"airFlowLevel" },
        { "name":"operatingMode" }
    ]
}
```

## 換気扇:ventilationFan:0x0133

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| airFlowLevel | GET, PUT | integer | 0xA0 | 風量設定<br>Air flow rate setting |
| autoVentilation | GET, PUT | boolean | 0xBF | 換気自動設定<br>Ventilation auto setting |

### Device Description

```
{
    "type":"ventilationFan",
    "description":{"ja":"換気扇", "en":"Ventilation Fan"},
    "properties":[
        {
            "name":"airFlowLevel",
            "description":{ "ja":"風量設定", "en":"Air flow rate setting" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"object", 
                "field":[
                    {
                        "name":"level",
                        "description":{ "ja":"レベル", "en":"Level" },
                        "data":{
                            "type":"integer",
                            "unit":"level",
                            "minimum":1,
                            "maximum":8
                        }
                    },
                    {
                        "name":"auto",
                        "description":{ "ja":"自動設定", "en":"Auto" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"自動", "en":"AUTO"}, 
                                {"value":false, "ja":"マニュアル", "en":"MANUAL"}
                            ]
                        }                        
                    }
                ]
            }
        },
        {
            "name":"autoVentilation",
            "description":{ "ja":"換気自動設定", "en":"Ventilation auto setting" },
            "writable":true,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"自動", "en":"AUTO"}, 
                    {"value":false, "ja":"非自動", "en":"Non AUTO"}
                ]
            }
        }
    ]
}
```

## 空気清浄器:airCleaner:0x0135

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| airFlowLevel | GET, PUT | integer | 0xA0 | 風量設定<br>Air flow rate setting |

### Device Description

```
{
    "type":"airCleaner",
    "description":{"ja":"空気清浄器", "en":"Air Cleaner"},
    "properties":[
        {
            "name":"airFlowLevel",
            "description":{ "ja":"風量設定", "en":"Air flow rate setting" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"object", 
                "field":[
                    {
                        "name":"level",
                        "description":{ "ja":"レベル", "en":"Level" },
                        "data":{
                            "type":"integer",
                            "unit":"level",
                            "minimum":1,
                            "maximum":8
                        }
                    },
                    {
                        "name":"auto",
                        "description":{ "ja":"自動設定", "en":"Auto" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"自動", "en":"AUTO"}, 
                                {"value":false, "ja":"マニュアル", "en":"MANUAL"}
                            ]
                        }                        
                    }
                ]
            }
        }
    ]
}
```

## 電動ブラインド・日よけ:electricBlind:0x0260

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| blindMotion<br>開閉動作設定 | GET, PUT | key | 0xE0 | 開閉（張出し／収納）動作設定<br>Open/close(extension/retraction) setting | INF |
| openingDegree<br>開度レベル | GET, PUT | percentage | 0xE1 | 開度レベル設定<br>Degree-of-opening level |

### Device Description

```
{
    "type":"electricBlind",
    "description":{"ja":"電動ブラインド・日よけ", "en":"Electric Blind"},
    "properties":[
        {
            "name":"blindMotion",
            "description":{
                "ja":"開閉（張出し／収納）動作設定",
                "en":"Open/close(extension/retraction) setting"
            },
            "writable":true,
            "observable":true,
            "data":{
                "type":"key",
                "values":[
                    {"value":"open", "ja":"開", "en":"open"},
                    {"value":"close", "ja":"閉", "en":"close"},
                    {"value":"stop", "ja":"停止", "en":"stop"}
                ]
            }
        },
        {
            "name":"openingDegree",
            "description":{ "ja":"開度レベル設定", "en":"Degree-of-opening level" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"integer",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        }
    ],
    "events":[
        { "name":"blindMotion" }
    ]
}
```

## 電動雨戸・シャッター:electricRainDoor:0x0263

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| blindMotion | GET, PUT | key | 0xE0 | 開閉動作設定<br>Open/Close setting | INF |
| openingDegree | GET, PUT | percentage | 0xE1 | 開度レベル設定<br>Degree-of-opening level |\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"electricRainDoor",
    "description":{"ja":"電動雨戸・シャッター", "en":"Electric Rain Door"},
    "properties":[
        {
            "name":"blindMotion",
            "description":{ "ja":"開閉動作設定", "en":"Open/Close setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"key",
                "values":[
                    {"value":"open", "ja":"開", "en":"open"},
                    {"value":"close", "ja":"閉", "en":"close"},
                    {"value":"stop", "ja":"停止", "en":"stop"}
                ]
            }
        },
        {
            "name":"openingDegree",
            "description":{ "ja":"開度レベル設定", "en":"Degree-of-opening level" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"integer",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        }
    ],
    "events":[
        { "name":"blindMotion" }
    ]
}
```

## 電気温水器:electricWaterHeater:0x026B 

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| automaticWaterHeating | GET, PUT | key | 0xB0 | 沸き上げ自動設定<br>Automatic water heating setting | INF |
| heatingWater | GET | boolean | 0xB2 | 沸き上げ中状態<br>Water heater status | INF |
| daytimeReheatingIsPermitted | GET, PUT | boolean | 0xC0 | 昼間沸き増し許可設定<br>Daytime reheating permission setting |
| alarmStatus | GET | object | 0xC2 | 警報発生状態<br>Alarm status | INF |
| supplyingHotWater | GET | boolean | 0xC3 | 給湯中状態<br>Hot water supply status | INF |
| energyShiftJoining | GET, PUT | boolean | 0xC7 | エネルギーシフト参加状態 |
| waterHeatingStartTime | GET | integer | 0xC8 | 沸き上げ開始基準時刻 |
| numberOfEnergyShifts | GET | integer | 0xC9 | エネルギーシフト回数 |
| waterHeatingTime1 | GET, PUT |integer | 0xCA | 昼間沸き上げシフト時刻１ |
| estimatedEnergyConsumption1 | GET | object | 0xCB | 昼間沸き上げシフト時刻１での沸き上げ予測電力量 | 
| energyConsumptionRate1 |GET| object | 0xCC | 時間あたり消費電力量１ |
| waterHeatingTime2 | GET, PUT |integer | 0xCD | 昼間沸き上げシフト時刻２ |
| estimatedEnergyConsumption2 | GET | object | 0xCE | 昼間沸き上げシフト時刻２での沸き上げ予測電力量 |
| energyConsumptionRate2 |GET | object | 0xCF | 時間あたり消費電力量２ | 
| automaticBathWaterHeating | GET, PUT | boolean | 0xE3 | 風呂自動モード設定<br>Automatic bath water heating mode setting |
| bathOperatingStatus | GET | key | 0xEA | 風呂動作状態監視<br>Bath operation status monitor | \*1<br>INF |
\*1) 必須項目ではない
### Device Description

```
{
    "type":"electricWaterHeater",
    "description":{"ja":"電気温水器", "en":"Electric WaterHeater"},
    "properties":[
        {
            "name":"automaticWaterHeating",
            "description":{ "ja":"沸き上げ自動設定", "en":"Automatic water heating setting" },
            "writable":true,
            "observable":true,
            "data":{
                "type":"key",
                "values":[
                    {"value":"auto", "ja":"自動沸き上げ", "en":"Auto Heating"},
                    {"value":"manualNoHeating","ja":"手動沸き上げ停止","en":"Manual No Heating"},
                    {"value":"manualHeating", "ja":"手動沸き上げ", "en":"Manual Heating"}
                ]
            }
        },
        {
            "name":"heatingWater",
            "description":{ "ja":"沸き上げ中状態", "en":"Water heater status" },
            "writable":false,
            "observable":true,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"沸き上げ中", "en":"Heating"}, 
                    {"value":false, "ja":"沸き上げ無し", "en":"Not Heating"}
                ]
            }
        },
        {
            "name":"daytimeReheatingIsPermitted",
            "description":{
                "ja":"昼間沸き増し許可設定",
                "en":"Daytime reheating permission setting"
            },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"許可", "en":"Permitted"}, 
                    {"value":false, "ja":"禁止", "en":"Not Permitted"}
                ]
            }
        },
        {
            "name":"alarmStatus",
            "description":{ "ja":"警報発生状態", "en":"Alarm status" },
            "writable":false,
            "observable":true,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"noHotWater",
                        "description":{ "ja":"湯切れ警報", "en":"No Hot Water" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"発生", "en":"Alarm"}, 
                                {"value":false, "ja":"正常", "en":"No Alarm"}
                            ]
                        }
                    },
                    {
                        "name":"leaking",
                        "description":{ "ja":"漏水警報", "en":"No Hot Water" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"発生", "en":"Alarm"}, 
                                {"value":false, "ja":"正常", "en":"No Alarm"}
                            ]
                        }
                    },
                    {
                        "name":"freezing",
                        "description":{ "ja":"凍結警報", "en":"No Hot Water" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"発生", "en":"Alarm"}, 
                                {"value":false, "ja":"正常", "en":"No Alarm"}
                            ]
                        }
                    }
                ]
            }
        },
        {
            "name":"supplyingHotWater",
            "description":{ "ja":"給湯中状態", "en":"Hot water supply status" },
            "writable":false,
            "observable":true,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"給湯中", "en":"Supplying"}, 
                    {"value":false, "ja":"非給湯中", "en":"Not Supplying"}
                ]
            }
        },
        {
            "name":"energyShiftParticipation",
            "description":{
                "ja":"エネルギーシフト参加状態", 
                "en":"Participation in Energy Shift"
            },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"参加", "en":"Participation"}, 
                    {"value":false, "ja":"不参加", "en":"Non Participation"}
                ]
            }
        },
        {
            "name":"waterHeatingStartTime",
            "description":{ "ja":"沸き上げ開始基準時刻", "en":"Water Heating Start Time" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"hour"
            }
        },
        {
            "name":"numberOfEnergyShifts",
            "description":{ "ja":"エネルギーシフト回数", "en":"Number Of Energy Shifts" },
            "writable":false,
            "observable":false,
            "data":{ "type":"integer" }
        },
        {
            "name":"waterHeatingTime1",
            "description":{ "ja":"昼間沸き上げシフト時刻１", "en":"Water Heating Time1" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"hour", 
                "minimum":9, 
                "maximum":17
            }
        },
        {
            "name":"estimatedEnergyConsumption1",
            "description":{
                "ja":"昼間沸き上げシフト時刻１での沸き上げ予測電力量", 
                "en":"Estimated Energy Consumption1"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"10:00",
                        "description":{ "ja":"10:00", "en":"10:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"13:00",
                        "description":{ "ja":"13:00", "en":"13:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"15:00",
                        "description":{ "ja":"15:00", "en":"15:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"17:00",
                        "description":{ "ja":"17:00", "en":"17:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    }
                ]
            }
        },
        {
            "name":"energyConsumptionRate1",
            "description":{ "ja":"時間あたり消費電力量１", "en":"Energy Consumption Rate1" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"10:00",
                        "description":{ "ja":"10:00", "en":"10:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"13:00",
                        "description":{ "ja":"13:00", "en":"13:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"15:00",
                        "description":{ "ja":"15:00", "en":"15:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"17:00",
                        "description":{ "ja":"17:00", "en":"17:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    }
                ]
            }
        },
        {
            "name":"waterHeatingTime2",
            "description":{ "ja":"昼間沸き上げシフト時刻２", "en":"Water Heating Time2" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"hour", 
                "minimum":10, 
                "maximum":17
            }
        },
        {
            "name":"estimatedEnergyConsumption2",
            "description":{
                "ja":"昼間沸き上げシフト時刻２での沸き上げ予測電力量",
                "en":"Estimated Energy Consumption2"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"13:00",
                        "description":{ "ja":"13:00", "en":"13:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"15:00",
                        "description":{ "ja":"15:00", "en":"15:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"17:00",
                        "description":{ "ja":"17:00", "en":"17:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    }
                ]
            }
        },
        {
            "name":"energyConsumptionRate2",
            "description":{ "ja":"時間あたり消費電力量２", "en":"Energy Consumption Rate2" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"13:00",
                        "description":{ "ja":"13:00", "en":"13:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"15:00",
                        "description":{ "ja":"15:00", "en":"15:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    },
                    {
                        "name":"17:00",
                        "description":{ "ja":"17:00", "en":"17:00" },
                        "data":{
                            "type":"integer",
                            "unit":"Wh"
                        }
                    }
                ]
            }
        },
        {
            "name":"automaticBathWaterHeating",
            "description":{
                "ja":"風呂自動モード設定",
                "en":"Automatic Bath Water Heating Mode Setting"
            },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"自動", "en":"Auto"}, 
                    {"value":false, "ja":"非自動", "en":"Non Auto"}
                ]
            }
        },
        {
            "name":"bathOperatingStatus",
            "description":{ "ja":"風呂動作状態監視", "en":"Bath Operation Status Monitor" },
            "writable":false,
            "observable":true,
            "data":{ 
                "type":"key",
                "values":[
                    {"value":"runningHotWater", "ja":"湯張り中", "en":"Running Hot Water"},
                    {"value":"noOperation", "ja":"停止中", "en":"No Operation"},
                    {"value":"keepingTemperature", "ja":"保温中", "en":"Keeping Temperature"}
                ]
            }
        }
    ],
    "events":[
        { "name":"automaticWaterHeating" },
        { "name":"heatingWater" },
        { "name":"alarmStatus" },
        { "name":"supplyingHotWater" },
        { "name":"bathOperatingStatus" }
    ]
}
```

## 電気錠:electricKey:0x026F 

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| islocked | GET, PUT | boolean | 0xE0 | 施錠設定1<br>Lock setting 1 | INF |

### Device Description

```
{
    "type":"electricKey",
    "description":{"ja":"電気錠", "en":"Electric Key"},
    "properties":[
        {
            "name":"islocked",
            "description":{ "ja":"施錠設定1", "en":"Lock setting1" },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"施錠", "en":"Lock"}, 
                    {"value":false, "ja":"開錠", "en":"Unlock"}
                ]
            }
        }
    ],
    "events":[
        { "name":"islocked" }
    ]
}
```

## 瞬間式給湯器:instataneousWaterHeater:0x0272 

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| heatingWater | GET | boolean | 0xD0 | 給湯器燃焼状態<br>Hot water heating status |
| heatingBathWater | GET | boolean | 0xE2 | 風呂給湯器燃焼状態<br>Bath water heater status |
| automaticBathWaterHeating | GET, PUT | boolean | 0xE3 | 風呂自動モード設定<br>Bath auto mode setting |
| bathOperatingStatus | GET | key | 0xEF | 風呂動作状態監視<br>Bath operation status monitor | \*1<br>INF |
\*1) 必須項目ではない

### Device Description

```
{
    "type":"instataneousWaterHeater",
    "description":{"ja":"瞬間式給湯器", "en":"Instataneous Water Heater"},
    "properties":[
        {
            "name":"heatingWater",
            "description":{ "ja":"給湯器燃焼状態", "en":"Hot water heating status" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"燃焼中", "en":"Heating"}, 
                    {"value":false, "ja":"燃焼無し", "en":"Not Heating"}
                ]
            }
        },
        {
            "name":"heatingBathWater",
            "description":{ "ja":"風呂給湯器燃焼状態", "en":"Bath water heater status" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"燃焼中", "en":"Heating"}, 
                    {"value":false, "ja":"燃焼無し", "en":"Not Heating"}
                }
            }
        },
        {
            "name":"automaticBathWaterHeating",
            "description":{ "ja":"風呂自動モード設定", "en":"Bath auto mode setting" },
            "writable":true,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"自動", "en":"Auto"}, 
                    {"value":false, "ja":"非自動", "en":"Non Auto"}
                ]
            }
        },
        {
            "name":"bathOperatingStatus",
            "description":{ "ja":"風呂動作状態監視", "en":"Bath operation status monitor" },
            "writable":false, 
            "observable":true,
            "data":{
                "type":"key",
                "values":[
                    {"value":"runningHotWater", "ja":"湯張り中", "en":"Running Hot Water"},
                    {"value":"noOperation", "ja":"停止中", "en":"No Operation"},
                    {"value":"keepingTemperature", "ja":"保温中", "en":"Keeping Temperature"}
                ]
            }
        }
    ],
    "events":[
        { "name":"bathOperatingStatus" }
    ]
}
```

## 浴室暖房乾燥機:bathroomHeaterDryer:0x0273

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| operatingMode | GET, PUT | key | 0xB0 | 運転設定<br>Operation setting |
| dryLevel | GET, PUT | integer | 0xB4 | 乾燥運転設定<br>Bathroom dryer operation setting |

### Device Description

```
{
    "type":"bathroomHeaterDryer",
    "description":{"ja":"浴室暖房乾燥機", "en":"Bathroom Heater Dryer"},
    "properties":[
        {
            "name":"operatingMode",
            "description":{ "ja":"運転設定", "en":"Operation setting" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"key",
                "values":[
                    {"value":"ventilation", "ja":"換気運転", "en":"Ventilation"},
                    {"value":"preheating", "ja":"入浴前予備暖房運転", "en":"Preheating"},
                    {"value":"heating", "ja":"入浴中暖房運転", "en":"Heating"},
                    {"value":"drying", "ja":"乾燥運転", "en":"Drying"},
                    {"value":"cooling", "ja":"涼風運転", "en":"Cooling"},
                    {"value":"stop", "ja":"停止", "en":"Stop"}
                ]
            }
        },
        {
            "name":"dryLevel",
            "description":{ "ja":"乾燥運転設定", "en":"Bathroom dryer operation setting" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"object", 
                "field":[
                    {
                        "name":"level",
                        "description":{ "ja":"レベル", "en":"Level" },
                        "data":{
                            "type":"integer",
                            "unit":"level",
                            "minimum":1,
                            "maximum":8
                        }
                    },
                    {
                        "name":"auto",
                        "description":{ "ja":"自動設定", "en":"Auto" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"自動", "en":"AUTO"}, 
                                {"value":false, "ja":"マニュアル", "en":"MANUAL"}
                            ]
                        }                        
                    }
                ]
            }
        }
    ]
}
```

## 住宅用太陽光発電:pvPowerGeneration:0x0279 

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| instantaneousPowerGeneration | GET | integer | 0xE0 | 瞬時発電電力計測値<br>Measured instantaneous amount of electricity generated |
| integralEnergyGeneration | GET | integer | 0xE1 | 積算発電電力量計測値<br>Measured cumulative amount of electric energy generated |\*1|

\*1) 積算発電電力量計測値は、ECHONET LiteでGETした値に0.001を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

### Device Description

```
{
    "type":"pvPowerGeneration",
    "description":{"ja":"住宅用太陽光発電", "en":"PV Power Generation"},
    "properties":[
        {
            "name":"instantaneousPowerGeneration",
            "description":{
                "ja":"瞬時発電電力計測値",
                "en":"Measured instantaneous amount of electricity generated"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"W"
            }
        },
        {
            "name":"integralEnergyGeneration",
            "description":{
                "ja":"積算発電電力量計測値",
                "en":"Measured cumulative amount of electric energy generated"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"kWh"
            }
        }
    ]
}
```

## 冷温水熱源機:hotWaterHeatSource:0x027A

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| waterTemperature1 | GET, PUT | integer | 0xE1 | 水温設定1<br>Water temperature setting 1 | \*1 |
| waterTemperature2 | GET, PUT | integer | 0xE2 | 水温設定2<br>Water temperature setting 2 | \*1 |

\*1) どちらかの実装が必須

### Device Description

```
{
    "type":"hotWaterHeatSource",
    "description":{"ja":"冷温水熱源機", "en":"Hot Water Heat Source"},
    "properties":[
        {
            "name":"waterTemperature1",
            "description":{ "ja":"水温設定1", "en":"waterTemperature1" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"℃", 
                "minimum":0, 
                "maximum":100
            }
        },
        {
            "name":"waterTemperature2",
            "description":{ "ja":"水温設定2", "en":"waterTemperature2" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"object", 
                "field":[
                    {
                        "name":"level",
                        "description":{ "ja":"レベル", "en":"Level" },
                        "data":{
                            "type":"integer",
                            "unit":"level",
                            "minimum":1,
                            "maximum":8
                        }
                    },
                    {
                        "name":"auto",
                        "description":{ "ja":"自動設定", "en":"Auto" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"自動", "en":"AUTO"}, 
                                {"value":false, "ja":"マニュアル", "en":"MANUAL"}
                            ]
                        }                        
                    }
                ]
            }
        }
    ]
}
```

## 床暖房:floorHeater:0x027B

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| waterTemperature1 | GET, PUT | integer | 0xE0 | 温度設定1<br>Temperature setting 1 | \*1 |
| waterTemperature2 | GET, PUT | integer | 0xE1 | 温度設定2<br>Temperature setting 2 | \*1 |

\*1) どちらかの実装が必須

### Device Description

```
{
    "type":"floorHeater",
    "description":{"ja":"床暖房", "en":"Floor Heater"},
    "properties":[
        {
            "name":"waterTemperature1",
            "description":{ "ja":"温度設定1", "en":"Temperature1" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"object", 
                "field":[
                    {
                        "name":"temperature",
                        "description":{ "ja":"温度", "en":"Temperature" },
                        "data":{
                            "type":"integer", 
                            "unit":"℃", 
                            "minimum":0, 
                            "maximum":50
                        }                        
                    },
                    {
                        "name":"auto",
                        "description":{ "ja":"自動設定", "en":"Auto" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"自動", "en":"AUTO"}, 
                                {"value":false, "ja":"マニュアル", "en":"MANUAL"}
                            ]
                        }                        
                    }
                ]
            }
        },
        {
            "name":"waterTemperature2",
            "description":{ "ja":"温度設定2", "en":"Temperature2" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"object", 
                "field":[
                    {
                        "name":"level",
                        "description":{ "ja":"レベル", "en":"Level" },
                        "data":{
                            "type":"integer",
                            "unit":"level",
                            "minimum":1,
                            "maximum":8
                        }
                    },
                    {
                        "name":"auto",
                        "description":{ "ja":"自動設定", "en":"Auto" },
                        "data":{
                            "type":"boolean",
                            "values":[
                                {"value":true, "ja":"自動", "en":"AUTO"}, 
                                {"value":false, "ja":"マニュアル", "en":"MANUAL"}
                            ]
                        }                        
                    }
                ]
            }
        }
    ]
}
```

## 燃料電池:fuelCell:0x027C 

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| instantaneousPowerGeneration | GET | integer | 0xC4 | 瞬時発電電力計測値<br>Measured instantaneous power generation output |
| integralEnergyGeneration | GET | integer | 0xC5 | 積算発電電力量計測値<br>Measured cumulative power generation output |\*1|

\*1) 積算発電電力量計測値は、ECHONET LiteでGETした値に0.001を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

### Device Description

```
{
    "type":"fuelCell",
    "description":{"ja":"燃料電池", "en":"Fuel Cell"},
    "properties":[
        {
            "name":"instantaneousPowerGeneration",
            "description":{
                "ja":"瞬時発電電力計測値",
                "en":"Measured instantaneous power generation output"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"W"
            }
        },
        {
            "name":"integralEnergyGeneration",
            "description":{
                "ja":"積算発電電力量計測値",
                "en":"Measured cumulative power generation output"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"kWh"
            }
        }
    ]
}
```

## 蓄電池:storageBattery:0x027D 

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| effectiveChargingCapacity | GET | integer | 0xA0 | AC実効容量（充電）<br>AC effective capacity(charging) |
| effectiveDischargingCapacity | GET | integer | 0xA1 | AC実効容量（放電）<br>AC effective capacity(discharging) |
| chargeableCapacity | GET | integer|0xA2| 充電可能容量<br>AC chargeable capacity |
| dischargeableCapacity | GET | integer| 0xA3 | 放電可能容量<br>AC dischargeable capacity |
| chargeableEnergy | GET | integer| 0xA4 | 充電可能量<br>AC chargeable electric energy |
| dischargeableEnergy | GET | integer| 0xA5 | 放電可能量<br>AC dischargeable electric energy |
| integralChargingEnergy | GET | integer| 0xA8 | AC積算充電電力量計測値<br>AC measured cumulative charging electric energy |\*1|
| integralDischargingEnergy | GET | integer| 0xA9 | AC積算放電電力量計測値<br>AC measured cumulative discharging electric energy |\*1|
| targetChargingEnergy | GET, PUT | integer| 0xAA | AC充電量設定値<br>AC charge amount setting value |INF|
| targetDischargingEnergy | GET, PUT | integer| 0xAB | AC放電量設定値<br>AC discharge amount setting value |INF|
| OperatingStatus | GET | key | 0xCF | 運転動作状態<br>Working operation status |INF|
| operatingMode | GET, PUT | key | 0xDA | 運転モード設定<br>Operation mode setting |INF|
| remainingStoredEnergy1 | GET | integer | 0xE2 | 蓄電残量1<br>Remaining stored electricity 1 | |
| remainingStoredEnergy2 | GET | integer | 0xE3 | 蓄電残量2<br>Remaining stored electricity 2 |\*2|
| remainingStoredEnergy3 | GET | percentage | 0xE4 | 蓄電残量3<br>Remaining stored electricity 3 | |
| batteryType | GET | key | 0xE6 | 蓄電池タイプ<br>Battery type |

\*1) AC積算充電電力量計測値・AC積算放電電力量計測値は、ECHONET LiteでGETした値に0.001を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。  
\*2) 蓄電残量2は、ECHONET LiteでGETした値に0.1を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

### Device Description

```
{
    "type":"storageBattery",
    "description":{"ja":"蓄電池", "en":"Storage Battery"},
    "properties":[
        {
            "name":"effectiveChargingCapacity",
            "description":{ "ja":"AC実効容量（充電）", "en":"AC effective capacity(charging)" },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"effectiveDischargingCapacity",
            "description":{
                "ja":"AC実効容量（放電）",
                "en":"AC effective capacity(discharging)"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"chargeableCapacity",
            "description":{ "ja":"充電可能容量", "en":"AC chargeable capacity" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"dischargeableCapacity",
            "description":{ "ja":"放電可能容量", "en":"AC dischargeable capacity" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"chargeableEnergy",
            "description":{ "ja":"充電可能量", "en":"AC chargeable electric energy" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"dischargeableEnergy",
            "description":{ "ja":"放電可能量", "en":"AC dischargeable electric energy" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"integralChargingEnergy",
            "description":{
                "ja":"AC積算充電電力量計測値<br>",
                "en":"AC measured cumulative charging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"kWh"
            }
        },
        {
            "name":"integralDischargingEnergy",
            "description":{
                "ja":"AC積算放電電力量計測値<br>",
                "en":"AC measured cumulative discharging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"kWh"
            }
        },
        {
            "name":"targetChargingEnergy",
            "description":{ "ja":"AC充電量設定値", "en":"AC charge amount setting value" },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"targetDischargingEnergy",
            "description":{ "ja":"AC放電量設定値", "en":"AC discharge amount setting value" },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"operatingStatus",
            "description":{ "ja":"運転動作状態", "en":"Working operation status" },
            "writable":false,
            "observable":true,
            "data":{ 
                "type":"key",
                "values":[
                    {"value":"rapidCharging", "ja":"急速充電", "en":"rapidCharging"},
                    {"value":"charging", "ja":"充電", "en":"charging"},
                    {"value":"discharging", "ja":"放電", "en":"discharging"},
                    {"value":"standby", "ja":"待機", "en":"standby"},
                    {"value":"test", "ja":"テスト", "en":"test"},
                    {"value":"auto", "ja":"自動", "en":"auto"},
                    {"value":"restart", "ja":"再起動", "en":"restart"},
                    {"value":"capacityRecalculation","ja":"実行容量再計算処理","en":"capacityRecalculation"},
                    {"value":"other", "ja":"その他", "en":"other"}
                ]
            }
        },
        {
            "name":"operatingMode",
            "description":{ "ja":"運転モード設定", "en":"Operation mode setting" },
            "writable":true,
            "observable":true,
            "data":{ 
                "type":"key",
                "values":[
                    {"value":"rapidCharging", "ja":"急速充電", "en":"rapidCharging"},
                    {"value":"charging", "ja":"充電", "en":"charging"},
                    {"value":"discharging", "ja":"放電", "en":"discharging"},
                    {"value":"standby", "ja":"待機", "en":"standby"},
                    {"value":"test", "ja":"テスト", "en":"test"},
                    {"value":"auto", "ja":"自動", "en":"auto"},
                    {"value":"restart", "ja":"再起動", "en":"restart"},
                    {"value":"capacityRecalculation","ja":"実行容量再計算処理","en":"capacityRecalculation"},
                    {"value":"other", "ja":"その他", "en":"other"}
                ]
            }
        },
        {
            "name":"remainingStoredEnergy1",
            "description":{ "ja":"蓄電残量1", "en":"Remaining stored electricity 1" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"remainingStoredEnergy2",
            "description":{ "ja":"蓄電残量2", "en":"Remaining stored electricity 2" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"integer", 
                "unit":"Ah"
            }
        },
        {
            "name":"remainingStoredEnergy3",
            "description":{ "ja":"蓄電残量3", "en":"Remaining stored electricity 3" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"integer",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"batteryType",
            "description":{ "ja":"蓄電池タイプ", "en":"Battery type" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"key",
                "values":[
                    {"value":"unknown", "ja":"不明", "en":"unknown"},
                    {"value":"lead", "ja":"鉛", "en":"lead"},
                    {"value":"ni-mh", "ja":"NiH", "en":"ni-mh"},
                    {"value":"ni-cd", "ja":"NiCd", "en":"ni-cd"},
                    {"value":"lib", "ja":"Li-ion", "en":"lib"},
                    {"value":"zinc", "ja":"Zn", "en":"zinc"},
                    {"value":"alkaline", "ja":"充電式アルカリ", "en":"alkaline"}
                ]
            }
        }
    ],
    "events":[
        { "name":"targetChargingEnergy" },
        { "name":"targetDischargingEnergy" },
        { "name":"operatingStatus" },
        { "name":"operatingMode" }
    ]
}
```

## 電気自動車充放電器:evChargerDischarger:0x027E 

　ECHONET LiteのProperty「車両接続・充放電可否状態（EPC=0xC7）」は、ECHONET LiteでデータをGETする前に「車両接続確認（EPC=0xCD)」をSETする必要がある。ブリッジがEL-WebAPIでGET:ChargeDischargeStatusを受信した場合、ブリッジはECHONET Liteで「車両接続確認」をSETしたのち「積算電力量計測値履歴1」をGETする。

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| dischargeableCapacity1 | GET | integer | 0xC0 | 車載電池の放電可能容量値1<br>Dischargeable capacity of vehicle mounted battery 1 |
| remainingDischargeableCapacity1 | GET | integer | 0xC2 | 車載電池の放電可能残容量1<br>Remaining dischargeable capacity of vehicle mounted battery 1 |
| remainingDischargeableCapacity3 | GET | percentage | 0xC4 | 車載電池の放電可能残容量3<br>Remaining dischargeable capacity of vehicle mounted battery 3 |
| ratedChargePower | GET | integer| 0xC5 | 定格充電能力<br>Rated charge capacity |
| ratedDischargePower | GET | integer | 0xC6 | 定格放電能力<br>Rated discharge capacity |
| chargeDischargeStatus | GET | key | 0xC7 | 車両接続・充放電可否状態<br>Vehicle connection and chargeable/dischargeable status |INF|
| minMaxChargePower | GET | object | 0xC8 | 最小大充電電力値<br>Minimum/maximum charging electric energy |
| minMaxDischargePower | GET | object | 0xC9 | 最小最大放電電力値<br>Minimum/maximum discharging electric energy |
| minMaxChargeCurrent | GET | object | 0xCA | 最小最大充電電流値<br>Minimum/maximum charging current |\*1|
| minMaxDischargeCurrent | GET | object | 0xCB | 最小最大放電電流値<br>Minimum/maximum discharging current |\*1|
| equipmentType | GET | key | 0xCC | 充放電器タイプ<br>Charger/Discharger type |
| usedCapacity1 | GET | integer | 0xD0 | 車載電池の使用容量値1<br>Used capacity of vehicle mounted battery 1 |
| operatingMode | GET, PUT | key | 0xDA | 運転モード設定<br>Operation mode setting |
| remainingStoredEnergy1 | GET | integer | 0xE2 | 車載電池の電池残容量1<br>Remaining stored electricity of vehicle mounted battery1 |
| remainingStoredEnergy3 | GET | percentage | 0xE4 | 車載電池の電池残容量3<br>Remaining stored electricity of vehicle mounted battery3 |

\*1) 最小最大充電電流値・最小最大放電電流値は、ECHONET LiteでGETした値に0.1を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

### Device Description

```
{
    "type":"evChargerDischarger",
    "description":{"ja":"電気自動車充放電器", "en":"EV Charger Discharger"},
    "properties":[
        {
            "name":"dischargeableCapacity1",
            "description":{
                "ja":"車載電池の放電可能容量値1",
                "en":"Dischargeable capacity of vehicle mounted battery 1"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"remainingDischargeableCapacity1",
            "description":{
                "ja":"車載電池の放電可能残容量1",
                "en":"Remaining dischargeable capacity of vehicle mounted battery 1"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"remainingDischargeableCapacity3",
            "description":{
                "ja":"車載電池の放電可能残容量3",
                "en":"Remaining dischargeable capacity of vehicle mounted battery 3"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        },
        {
            "name":"ratedChargePower",
            "description":{ "ja":"定格充電能力", "en":"Rated charge capacity" },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"W"
            }
        },
        {
            "name":"ratedDischargePower",
            "description":{ "ja":"定格放電能力", "en":"Rated discharge capacity" },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"W"
            }
        },
        {
            "name":"chargeDischargeStatus",
            "description":{
                "ja":"車両接続・充放電可否状態",
                "en":"Vehicle connection and chargeable/dischargeable status"
            	   },
            "writable":false, 
            "observable":true,
            "data":{
                "type":"key",
                "values":[
                    {"value":"undefined", "ja":"不定", "en":"Undefined"},
                    {"value":"notConnected", "ja":"車両未接続", "en":"Not Connected"},
                    {"value":"connected", "ja":"車両接続・充電不可・放電不可", "en":"Connected"},
                    {"value":"chargeableDischargeable",
                     "ja":"車両接続・充電可・放電可", "en":"Chargeable Dischargeable"},
                    {"value":"dischargeable",
                     "ja":"車両接続・充電不可・放電可", "en":"Dischargeable"},
                    {"value":"chargeable", "ja":"車両接続・ 充電可 ・放電不可", "en":"Chargeable"}
                ]
            }
        },
        {
            "name":"minMaxChargePower",
            "description":{
                "ja":"最小最大充電電力値",
                "en":"Minimum/maximum charging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"minimum",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"integer",
                            "unit":"W"
                        }
                    },
                    {
                        "name":"maximum",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"integer",
                            "unit":"W"
                        }
                    }
                ]
            }
        },
        {
            "name":"minMaxDischargePower",
            "description":{
                "ja":"最小最大放電電力値",
                "en":"Minimum/maximum discharging electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"minimum",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"integer",
                            "unit":"W"
                        }
                    },
                    {
                        "name":"maximum",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"integer",
                            "unit":"W"
                        }
                    }
                ]
            }
        },
        {
            "name":"minMaxChargeCurrent",
            "description":{
                "ja":"最小最大充電電流値",
                "en":"Minimum/maximum charging electric current"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"minimum",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"integer",
                            "unit":"A"
                        }
                    },
                    {
                        "name":"maximum",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"integer",
                            "unit":"A"
                        }
                    }
                ]
            }
        },
        {
            "name":"minMaxDischargeCurrent",
            "description":{
                "ja":"最小最大放電電流値",
                "en":"Minimum/maximum discharging electric current"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"minimum",
                        "description":{ "ja":"最小値", "en":"minimum" },
                        "data":{
                            "type":"integer",
                            "unit":"A"
                        }
                    },
                    {
                        "name":"maximum",
                        "description":{ "ja":"最大値", "en":"maximum" },
                        "data":{
                            "type":"integer",
                            "unit":"A"
                        }
                    }
                ]
            }
        },
        {
            "name":"equipmentType",
            "description":{ "ja":"充放電器タイプ", "en":"Charger/Discharger type" },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"key",
                "values":[
                    {"value":"AC_CPLT", "ja":"AC_CPLT", "en":"AC_CPLT"},
                    {"value":"AC_HLC_Charge", "ja":"AC_HLC（充電のみ）", "en":"AC_HLC_Charge"},
                    {"value":"AC_HLC_ChargeDischarge",
                     "ja":"AC_HLC（充放電可）", "en":"AC_HLC_ChargeDischarge"},
                    {"value":"DC_AA_Charge", "ja":"DCタイプ_AA（充電のみ）", "en":"DC_AA_Charge"},
                    {"value":"DC_AA_ChargeDischarge",
                     "ja":"DCタイプ_AA（充放電可）", "en":"DC_AA_ChargeDischarge"},
                    {"value":"DC_AA_Discharge",
                     "ja":"DCタイプ_BB（放電のみ）", "en":"DC_AA_Discharge"},
                    {"value":"DC_BB_Charge", "ja":"DCタイプ_BB（充電のみ）", "en":"DC_BB_Charge"},
                    {"value":"DC_BB_ChargeDischarge",
                     "ja":"DCタイプ_BB（充放電可）", "en":"DC_BB_ChargeDischarge"},
                    {"value":"DC_BB_Discharge",
                     "ja":"DCタイプ_BB（放電のみ）", "en":"DC_BB_Discharge"},
                    {"value":"DC_EE_Charge", "ja":"DCタイプ_EE（充電のみ）", "en":"DC_EE_Charge"},
                    {"value":"DC_EE_ChargeDischarge",
                     "ja":"DCタイプ_EE（充放電可）", "en":"DC_EE_ChargeDischarge"},
                    {"value":"DC_EE_Discharge",
                     "ja":"DCタイプ_EE（放電のみ）", "en":"DC_EE_Discharge"},
                    {"value":"DC_FF_Charge", "ja":"DCタイプ_FF（充電のみ）", "en":"DC_FF_Charge"},
                    {"value":"DC_FF_ChargeDischarge",
                     "ja":"DCタイプ_FF（充放電可）", "en":"DC_FF_ChargeDischarge"},
                    {"value":"DC_FF_Discharge",
                     "ja":"DCタイプ_FF（放電のみ）", "en":"DC_FF_Discharge"}
                ]
            }
        },
        {
            "name":"usedCapacity1",
            "description":{
                "ja":"車載電池の使用容量値1",
                "en":"Used capacity of vehicle mounted battery 1"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"operatingMode",
            "description":{ "ja":"運転モード設定", "en":"Operation mode setting" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"key",
                "values":[
                    {"value":"charge", "ja":"充電", "en":"Charge"},
                    {"value":"discharge", "ja":"放電", "en":"Discharge"},
                    {"value":"standby", "ja":"待機", "en":"Standby"},
                    {"value":"idle", "ja":"停止", "en":"Idle"},
                    {"value":"other", "ja":"その他", "en":"Other"}
                ]
            }
        },
        {
            "name":"remainingStoredEnergy1",
            "description":{
                "ja":"車載電池の電池残容量1",
                "en":"Remaining stored electricity of vehicle mounted battery1"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"remainingStoredEnergy3",
            "description":{
                "ja":"車載電池の電池残容量3",
                "en":"Remaining stored electricity of vehicle mounted battery3"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        }
    ],
    "events":[
        { "name":"chargeDischargeStatus" }
    ]
}
```

## 分電盤メータリング:powerDistributionBoard:0x0287

以下のECHONET LiteのPropertyは、ECHONET LiteでGETした値に積算電力量単位（EPC=0xC2）を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

> 積算電力量計測値（正方向計測値）  
> 積算電力量計測値（逆方向計測値）  

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| normDirIntegralEnergy | GET | integer | 0xC0 | 積算電力量計測値（正方向）<br>Measured cumulative amount of electric energy (normal direction) |
| revDirIntegralEnergy | GET | integer | 0xC1 | 積算電力量計測値（逆方向）<br>Measured cumulative amount of electric energy (reverse direction) |

### Device Description

```
{
    "type":"powerDistributionBoard",
    "description":{"ja":"分電盤メータリング", "en":"Power Distribution Board"},
    "properties":[
        {
            "name":"normDirIntegralEnergy",
            "description":{
                "ja":"積算電力量計測値（正方向）",
                "en":"Measured cumulative amount of electric energy (normal direction)"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"kWh"
            }
        },
        {
            "name":"revDirIntegralEnergy",
            "description":{
                "ja":"積算電力量計測値（逆方向）",
                "en":"Measured cumulative amount of electric energy (reverse direction)"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
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


### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| effectiveDigits | GET | integer | 0xD7 | 積算電力量有効桁数<br>integer of effective digits for cumulative amounts of electric energy |
| normDirIntegralEnergy | GET | integer | 0xE0 | 積算電力量計測値（正方向計測値）<br>Measured cumulative amount of electric energy (normal direction) |
| normDirIntegralEnergyLog1 | GET | object | 0xE2 | 積算電力量計測値履歴1（正方向計測値）<br>Historical data of measured cumulative amounts of electric energy 1(normal direction) | query<br>\*1|
| revDirIntegralEnergy | GET | integer | 0xE3 | 積算電力量計測値（逆方向計測値）<br>Measured cumulative amounts of electric energy (reverse direction) |
| revDirIntegralEnergyLog1 | GET | object | 0xE4 | 積算電力量計測値履歴1（逆方向計測値）<br>Historical data of measured cumulative amounts of electric energy 1(reverse direction) |query<br>\*1|
| instantaneousPower | GET | integer | 0xE7 | 瞬時電力計測値<br>Measured instantaneous electric energy |
| instantaneousCurrent | GET |  object | 0xE8 | 瞬時電流計測値<br>Measured instantaneous currents |\*2|
| normDirIntegralEnergyEvery30Min | GET | object | 0xEA | 定時積算電力量計測値（正方向計測値）<br>Cumulative amounts of electric energy measured at fixed time (normal direction) |
| revDirIntegralEnergyEvery30Min | GET | object | 0xEB | 定時積算電力量計測値（逆方向計測値）<br>Cumulative amounts of electric energy measured at fixed time (reverse direction) |

\*1) 積算履歴収集日１を指定するqueryあり。  
\*2) 瞬時電流計測値は、ECHONET LiteでGETした値に0.1を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

### Device Description

```
{
    "type":"lvSmartElectricEnergyMeter",
    "description":{"ja":"低圧スマート電力量メータ", "en":"Low Voltage Smart Electric Energy Meter"},
    "properties":[
        {
            "name":"effectiveDigits",
            "description":{
                "ja":"積算電力量有効桁数",
                "en":"Number of effective digits for cumulative amounts of electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{ "type":"integer" }
        },
        {
            "name":"normDirIntegralEnergy",
            "description":{
                "ja":"積算電力量計測値（正方向計測値）",
                "en":"Measured cumulative amount of electric energy (normal direction)"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"kWh"
            }
        },
        {
            "name":"normDirIntegralEnergyLog1",
            "description":{
                "ja":"積算電力量計測値履歴1（正方向計測値）",
                "en":"Historical data of measured cumulative amounts of electric energy 1(normal direction)"
            },
            "writable":false,
            "observable":false,
            "query":{
                "name":"day",
                "type":"integer",
                "minimum":0,
                "maximum":99
            },
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"day",
                        "description":{ "ja":"日", "en":"Day" },
                        "data":{ "type":"integer" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"array",
                            "element":{
                                "type":"integer",
                                "unit":"kWh"
                            }
                        }
                    }
                ]
            }
        },
        {
            "name":"revDirIntegralEnergy",
            "description":{
                "ja":"積算電力量計測値（逆方向計測値）",
                "en":"Measured cumulative amount of electric energy (reverse direction)"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"kWh"
            }
        },
        {
            "name":"revDirIntegralEnergyLog1",
            "description":{
                "ja":"積算電力量計測値履歴1（逆方向計測値）",
                "en":"Historical data of measured cumulative amounts of electric energy 1(reverse direction)"
            },
            "writable":false,
            "observable":false,
            "query":{
                "name":"day",
                "type":"integer",
                "minimum":0,
                "maximum":99
            },
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"day",
                        "description":{ "ja":"日", "en":"Day" },
                        "data":{ "type":"integer" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"array",
                            "element":{
                                "type":"integer",
                                "unit":"kWh"
                            }
                        }
                    }
                ]
            }
        },
        {
            "name":"instantaneousPower",
            "description":{
                "ja":"瞬時電力計測値",
                "en":"Measured instantaneous electric energy"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"W"
            }
        },
        {
            "name":"instantaneousCurrent",
            "description":{ "ja":"瞬時電流計測値", "en":"Measured instantaneous currents" },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"r",
                        "description":{ "ja":"r相", "en":"r phase" },
                        "data":{
                            "type":"integer",
                            "unit":"A"
                        }
                    },
                    {
                        "name":"t",
                        "description":{ "ja":"t相", "en":"t phase" },
                        "data":{
                            "type":"integer",
                            "unit":"A"
                        }
                    }
                ]
            }
        },
        {
            "name":"normDirIntegralEnergyEvery30Min",
            "description":{
                "ja":"定時積算電力量計測値（正方向計測値）",
                "en":"Cumulative amounts of electric energy measured at fixed time (normal direction)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and Time" },
                        "data":{ "type":"date" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"integer",
                            "unit":"kWh"
                        }
                    }
                ]
            }
        },
        {
            "name":"revDirIntegralEnergyEvery30Min",
            "description":{
                "ja":"定時積算電力量計測値（逆方向計測値）",
                "en":"Cumulative amounts of electric energy measured at fixed time (reverse direction)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and Time" },
                        "data":{ "type":"date" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"energy" },
                        "data":{
                            "type":"integer",
                            "unit":"kWh"
                        }
                    }
                ]
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

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| monthlyMaxDemandPower | GET | integer | 0xC1 | 月間最大需要電力<br>Monthly maximum electric power demand |
| averageDemandPower | GET | object | 0xC3 | 定時需要電力（30分平均電力）<br>Electric power demand at fixed time(30-minute average electric power) |
| effectiveDigits | GET | integer | 0xC4 | 需要電力有効桁数<br>Number of effective digits of electric power demand |
| demandPowerLog| GET | object |0xC6|需要電力計測値履歴<br>Historical data of measured electric power demand|query<br>\*1|
| fixedDate | GET | integer | 0xE0 | 確定日<br>Fixed date |
| integralActiveEnergy | GET | object | 0xE2 | 積算有効電力量計測値<br>Measured cumulative amounts of active electric energy |
| integralActiveEnergyEvery30Min | GET | object | 0xE3 | 定時積算有効電力量計測値<br>Cumulative amounts of active electric energy at fixed time |
| integralActiveEnergyEffectiveDigits | GET | integer | 0xE5 | 積算有効電力量有効桁数<br>Number of effective digits for cumulative amount of active electric energy |
| activeEnergyLog | GET | object | 0xE7 | 積算有効電力量計測値履歴<br>Historical data of measured cumulative amount of active electric energy |query<br>\*1|

*1) 積算履歴収集日を指定するqueryあり。

### Device Description

```
{
    "type":"hvSmartElectricEnergyMeter",
    "description":{"ja":"高圧スマート電力量メータ", "en":"High Voltage Smart Electric Energy Meter"},
    "properties":[
        {
            "name":"monthlyMaxDemandPower",
            "description":{ "ja":"月間最大需要電力", "en":"Monthly maximum electric power demand" },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"kW"
            }
        },
        {
            "name":"averageDemandPower",
            "description":{
                "ja":"定時需要電力（30分平均電力）",
                "en":"Electric power demand at fixed time(30-minute average electric power)"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and Time" },
                        "data":{ "type":"date" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"Energy" },
                        "data":{
                            "type":"integer",
                            "unit":"kWh"
                        }
                    }
                ]
            }
        },
        {
            "name":"effectiveDigits",
            "description":{
                "ja":"需要電力有効桁数",
                "en":"Number of effective digits of electric power demand"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"integer"
            }
        },
        {
            "name":"demandPowerLog",
            "description":{
                "ja":"需要電力計測値履歴",
                "en":"Historical data of measured electric power demand"
            },
            "writable":false,
            "observable":false,
            "query":{
                "name":"day",
                "type":"integer",
                "minimum":0,
                "maximum":99
            },
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"day",
                        "description":{ "ja":"日", "en":"Date" },
                        "data":{
                            "type":"integer",
                            "unit":"day"
                        }
                    },
                    {
                        "name":"power",
                        "description":{ "ja":"電力", "en":"Power" },
                        "data":{
                            "type":"array",
                            "element":{
                                "type":"integer",
                                "unit":"kW"
                            }
                        }
                    }
                ]
            }
        },
        {
            "name":"fixedDate",
            "description":{ "ja":"確定日", "en":"Fixed Date" },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"day"
            }
        },
        {
            "name":"integralActiveEnergy",
            "description":{
                "ja":"積算有効電力量計測値",
                "en":"Measured cumulative amounts of active electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and Time" },
                        "data":{ "type":"date" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"Energy" },
                        "data":{
                            "type":"integer",
                            "unit":"kWh"
                        }
                    }
                ]
            }
        },
        {
            "name":"integralActiveEnergyEvery30Min",
            "description":{
                "ja":"定時積算有効電力量計測値",
                "en":"Cumulative amounts of active electric energy at fixed time"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"dateAndTime",
                        "description":{ "ja":"日時", "en":"Date and Time" },
                        "data":{ "type":"date" }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"Energy" },
                        "data":{
                            "type":"integer",
                            "unit":"kWh"
                        }
                    }
                ]
            }
        },
        {
            "name":"integralActiveEnergyEffectiveDigits",
            "description":{
                "ja":"積算有効電力量有効桁数",
                "en":"Number of effective digits for cumulative amount of active electric energy"
            },
            "writable":false,
            "observable":false,
            "data":{ "type":"integer" }
        },
        {
            "name":"activeEnergyLog",
            "description":{
                "ja":"積算有効電力量計測値履歴",
                "en":"Historical data of measured cumulative amount of active electric energy"
            },
            "writable":false,
            "observable":false,
            "query":{
                "name":"day",
                "type":"integer",
                "minimum":0,
                "maximum":99
            },
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"day",
                        "description":{ "ja":"日", "en":"Day" },
                        "data":{
                            "type":"integer", 
                            "unit":"day"
                        }
                    },
                    {
                        "name":"energy",
                        "description":{ "ja":"電力量", "en":"Energy" },
                        "data":{
                            "type":"array",
                            "element":{
                                "type":"integer",
                                "unit":"kWh"
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

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| brightness | GET, PUT | integer | 0xB0 | 照度レベル設定<br>Illuminance level |\*1|
| operatingMode | GET, PUT | key | 0xB6 | 点灯モード設定<br>Lighting mode setting |
| rgb | GET, PUT | object | 0xC0 | カラー灯モード時RGB設定<br>RGB setting for color lighting |\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"generalLighting",
    "description":{"ja":"一般照明", "en":"General Lighting"},
    "properties":[
        {
            "name":"brightness",
            "description":{ "ja":"照度レベル設定", "en":"Illuminance level" },
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
            "name":"operatingMode",
            "description":{ "ja":"点灯モード設定", "en":"Lighting mode setting" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"key",
                "values":[
                    {"value":"auto", "ja":"自動", "en":"automatic"},
                    {"value":"normal", "ja":"通常灯", "en":"Normal Lighting"},
                    {"value":"night", "ja":"常夜灯", "en":"Night Lighting"},
                    {"value":"color", "ja":"カラー灯", "en":"Color Lighting"}
                ]
            }
        },
        {
            "name":"rgb",
            "description":{ "ja":"カラー灯モード時RGB設定", "en":"RGB setting for color lighting" },
            "writable":true,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"r",
                        "description":{ "ja":"赤", "en":"Red" },
                        "data":{
                            "type":"integer",
                            "minimum":0,
                            "maximum":255
                        }
                    },
                    {
                        "name":"g",
                        "description":{ "ja":"緑", "en":"Green" },
                        "data":{
                            "type":"integer",
                            "minimum":0,
                            "maximum":255
                        }
                    },
                    {
                        "name":"b",
                        "description":{ "ja":"青", "en":"Blue" },
                        "data":{
                            "type":"integer",
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

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| brightness | GET, PUT | percentage | 0xB0 | 照度レベル設定<br>Illuminance level |\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"monoFunctionalLighting",
    "description":{"ja":"単機能照明", "en":"Mono Functional Lighting"},
    "properties":[
        {
            "name":"brightness",
            "description":{ "ja":"照度レベル設定", "en":"Illuminance level" },
            "writable":true, 
            "observable":false,
            "data":{
                "type":"integer",
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

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| chargeStatus | GET | key | 0xC7 | 車両接続・充電可否状態<br>Vehicle connection and chargeable status |INF|
| equipmentType | GET | key | 0xCC | 充電器タイプ<br>Charger type |
| usedCapacity1 | GET | integer | 0xD0 | 車載電池の使用容量値1<br>Used capacity of vehicle mounted battery 1 |
| operatingMode | GET, PUT | key | 0xDA | 運転モード設定<br>Operation mode setting |INF|
| remainingStoredEnergy1 | GET | integer | 0xE2 | 車載電池の電池残容量1<br>Remaining stored electricity of vehicle mounted battery1 |
| remainingStoredEnergy3 | GET | percentage | 0xE4 | 車載電池の電池残容量3<br>Remaining stored electricity of vehicle mounted battery3 |

### Device Description

```
{
    "type":"evChargerDischarger",
    "description":{"ja":"電気自動車充放電器", "en":"EV Charger Discharger"},
    "properties":[
        {
            "name":"chargeStatus",
            "description":{
                "ja":"車両接続・充電可否状態",
                "en":"Vehicle connection and chargeable status"
            },
            "writable":false, 
            "observable":true,
            "data":{
                "type":"key",
                "values":[
                    {"value":"undefined", "ja":"不定", "en":"Undefined"},
                    {"value":"notConnected", "ja":"車両未接続", "en":"Not Connected"},
                    {"value":"chargeable", "ja":"車両接続・ 充電可", "en":"Chargeable"},
                    {"value":"notChargeable", "ja":"車両接続・ 充電不可", "en":"Not Chargeable"}
                ]
            }
        },
        {
            "name":"equipmentType",
            "description":{ "ja":"充放電器タイプ", "en":"Charger/Discharger type" },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"key",
                "values":[
                    {"value":"AC_CPLT", "ja":"AC_CPLT", "en":"AC_CPLT"},
                    {"value":"AC_HLC_Charge", "ja":"AC_HLC（充電のみ）", "en":"AC_HLC_Charge"},
                    {"value":"DC_AA_Charge", "ja":"DCタイプ_AA（充電のみ）", "en":"DC_AA_Charge"},
                    {"value":"DC_BB_Charge", "ja":"DCタイプ_BB（充電のみ）", "en":"DC_BB_Charge"},
                    {"value":"DC_EE_Charge", "ja":"DCタイプ_EE（充電のみ）", "en":"DC_EE_Charge"},
                    {"value":"DC_FF_Charge", "ja":"DCタイプ_FF（充電のみ）", "en":"DC_FF_Charge"}
                ]
            }
        },
        {
            "name":"usedCapacity1",
            "description":{
                "ja":"車載電池の使用容量値1",
                "en":"Used capacity of vehicle mounted battery 1"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"operatingMode",
            "description":{ "ja":"運転モード設定", "en":"Operation mode setting" },
            "writable":true, 
            "observable":true,
            "data":{
                "type":"key",
                "values":[
                    {"value":"charge", "ja":"充電", "en":"Charge"},
                    {"value":"standby", "ja":"待機", "en":"Standby"},
                    {"value":"idle", "ja":"停止", "en":"Idle"},
                    {"value":"other", "ja":"その他", "en":"Other"}
                ]
            }
        },
        {
            "name":"remainingStoredEnergy1",
            "description":{
                "ja":"車載電池の電池残容量1",
                "en":"Remaining stored electricity of vehicle mounted battery1"
            },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"integer", 
                "unit":"Wh"
            }
        },
        {
            "name":"remainingStoredEnergy3",
            "description":{
                "ja":"車載電池の電池残容量3",
                "en":"Remaining stored electricity of vehicle mounted battery3"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"integer",
                "unit":"%",
                "minimum":0,
                "maximum":100
            }
        }
    ],
    "events":[
        { "name":"chargeStatus" },
        { "name":"operatingMode" }
    ]
}
```

## 冷凍冷蔵庫:refrigerator:0x03B7

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| doorIsOpen<br>ドア開閉状態 | GET | boolean | 0xB0 | ドア開閉状態<br>Door open/close status |

### Device Description

```
{
    "type":"refrigerator",
    "description":{"ja":"冷凍冷蔵庫", "en":"refrigerator"},
    "properties":[
        {
            "name":"doorIsOpen",
            "description":{ "ja":"ドア開閉状態", "en":"Door open/close status" },
            "writable":false,
            "observable":false,
            "data":{ 
            "type":"boolean",
                "values":[
                    {"value":true, "ja":"開", "en":"Open"}, 
                    {"value":false, "ja":"閉", "en":"Close"}
                ]
            }
        }
    ]
}
```

## オーブンレンジ:combinationMicrowaveOven:0x03B8

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| doorIsOpen <br>ドア開閉状態 | GET | boolean| 0xB0 | ドア開閉状態<br>Door open/close status |\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"combinationMicrowaveOven",
    "description":{"ja":"オーブンレンジ", "en":"Combination Microwave Oven"},
    "properties":[
        {
            "name":"doorIsOpen",
            "description":{ "ja":"ドア開閉状態", "en":"Door open/close status" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"開", "en":"Open"}, 
                    {"value":false, "ja":"閉", "en":"Close"}
                ]
            }
        }
    ]
}
```

## クッキングヒータ:cookingHeater:0x03B9

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| heatingStatus | GET | key | 0xB1 | 加熱状態<br>Heating Status |

### Actions

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| cancelAll | POST |  | 0xB3 | 一括停止設定<br>"All Stop" setting |

### Device Description

```
{
    "type":"cookingHeater",
    "description":{"ja":"クッキングヒータ", "en":"Cooking Heater"},
    "properties":[
        {
            "name":"heatingStatus",
            "description":{ "ja":"過熱状態", "en":"heating Status" },
            "writable":false, 
            "observable":false,
            "data":{
                "type":"key",
                "values":[
                    {"value":"standby", "ja":"待機", "en":"standby"},
                    {"value":"heating", "ja":"運転", "en":"heating"},
                    {"value":"pause", "ja":"一時停止", "en":"pause"},
                    {"value":"heatingProhibited", "ja":"加熱禁止", "en":"heatingProhibited"},
                    {"value":"unknown", "ja":"不明", "en":"unknown"}
                ]
            }
        }
    ],
    "actions":[ "cancelAll" ]
}
```

## 洗濯乾燥機:washerDryer:0x03D3

### Properties

| Property Name | Access Method | Data Type | EPC(EL) | プロパティ名称(EL)| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| doorIsOpen <br>ドア開閉状態 | GET | boolean| 0xB0 | ドア開閉状態<br>Door open/close status |\*1|
| timeToFinish | GET | object | 0xED | 洗濯乾燥残り時間<br>Time remaining to complete washer and dryer cycle |\*1|

\*1) 必須項目ではない

### Device Description

```
{
    "type":"washerDryer",
    "description":{"ja":"洗濯乾燥機", "en":"Washer and Dryer"},
    "properties":[
        {
            "name":"doorIsOpen",
            "description":{ "ja":"ドア開閉状態", "en":"Door open/close status" },
            "writable":false,
            "observable":false,
            "data":{ 
                "type":"boolean",
                "values":[
                    {"value":true, "ja":"開", "en":"Open"}, 
                    {"value":false, "ja":"閉", "en":"Close"}
                ]
            }
        },
        {
            "name":"timeToFinish",
            "description":{
                "ja":"洗濯乾燥残り時間",
                "en":"Time remaining to complete washer and dryer cycle"
            },
            "writable":false,
            "observable":false,
            "data":{
                "type":"object",
                "field":[
                    {
                        "name":"Hour",
                        "description":{ "ja":"時間", "en":"Hour" },
                        "data":{
                            "type":"integer",
                            "unit":"hour"
                        }
                    },
                    {
                        "name":"Minute",
                        "description":{ "ja":"分", "en":"Minute" },
                        "data":{
                            "type":"integer",
                            "unit":"minute"
                        }
                    }
                ]
            }
        }
    ]
}
```
## スイッチ:switch:0x05FD
個別Propertyは存在しない

## コントローラ:controller:0x05FF
個別Propertyは存在しない
