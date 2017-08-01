# ECHONET Lite WebAPI (EL-WebAPI) 仕様書

2017.07.28 Draft version 1.0.0  
2017.08.01 Draft version 1.0.1  
- TYPOの修正  

## 1. Abstract
　ECHONET Lite機器をプロトコルブリッジを介してHTTP(REST)で制御することを想定しWebAPIを提案する。このドキュメントはリソースの構成とAPIの定義を記述する。機器毎のPropertyに関しては「EL-WebAPI Property詳細規定」を参照されたし。なおこのドミュメントはECHONET Liteの仕様を理解している者を対象に記述している。ECHONET Liteの仕様に関しては、エコーネットコンソーシアムのWeb Siteを参照すること。

## 2. 想定するシステム構成
　WebAPIを設計するにあたり下記(A)のシステム構成だけでなく、(B)や(C)のシステム構成も想定することで今後の拡張性や汎用性も考慮する。  
(A)プロトコルブリッジを介してECHONET Lite機器をEL-WebAPI対応する場合  
(B)デバイスがEL-WebAPIを実装する場合    
(C)プロトコルブリッジを介してその他のAPI対応機器をEL-WebAPI対応する場合  


![システム構成](_graphics/system.png)  

　ECHONET Lite機器に関しては、Node Profileに機器オブジェクトが１つ存在する構成だけでなく、同一オブジェクトが複数存在する構成や、複数の異なるオブジェクトが存在する構成も想定する。


## 3. Basic Concept

　照明のON/OFF状態を取得する場合と照明をONにする場合を例にしてECHONET Lite protocolとEL-WebAPIを簡単に説明する。  

### 3.1 ECHONET Lite
ECHONET Lite protocolはUDPでバイナリデータを送受信する。ECHONET Liteの電文は以下の要素で構成されている。  
> EHD:  ECHONET Header(2byte)  
> TID:  Transaction ID(2byte)  
> SEOJ: Source ECHONET Object: 送信元の機器を指定するデータ(3byte)  
> DEOJ: Destination ECHONET Object: 送信先の機器を指定するデータ(3byte)  
> ESV:  Echonet Service: コマンドの種類(GET, GET\_RES, SET\_C, SET\_RESなど)を示すデータ(1byte)  
> OPC:  Operation Count: 命令の個数を表すデータ(1byte)  
> EPC:  ECHONET Property Code: Propertyを指定するデータ(1byte)  
> PDC:  Property Data Count: EDTのデータ長(1byte)  
> EDT:  ECHONET Data: Propertyのデータ(可変長)  

####照明のON/OFF状態を取得する  

```
SEND: 
EHD=0x1081, TID=0x0001, SEOJ=0x05FF01(コントローラ), DEOJ=0x029001(一般照明), ESV=0x62(GET), 
OPC=0x01, EPC=0x80(Operating Status), PDC=0x00  
	1081 0001 05FF01 029001 62 01 80 00
RECEIVE: 
EHD=0x1081, TID=0x0001, SEOJ=0x029001, DEOJ=0x05FF01, ESV=0x72(GET_RES), 
OPC=0x01, EPC=0x80, PDC=0x01, EDT=0x31(OFF)  
	1081 0001 029001 05FF01 72 01 80 01 31
```
####照明をONにする  
```
SEND: ESV=0x61(SET_C), EDT=0x30(ON)  
	1081 0002 05FF01 029001 61 01 80 01 30
RECEIVE: ESV=0x71(SET_RES)  
	1081 0002 029001 05FF01 71 01 80 00
```
### 3.2 EL-WebAPI
　EL-WebAPIでは機器の状態や機能に対してPropertyというリソース(URL)を定義する。ON/OFF状態のPropertyは"On"とする。以下の例では、照明のIP Addressは192.168.11.201、照明のDevice Idは"GeneralLighting_1"とする。  
  
#### 照明のON/OFF状態を取得する  

```
// request  
GET http://192.168.11.201/v1/GeneralLighting_1/On

// response  
{"value":false}
// 消灯状態
```

#### 照明をONにする  

```
// request  
PUT http://192.168.11.201/v1/GeneralLighting_1/On

// body  
{"value":true}
```

## 4. URLの構成  
リソースを指定するURLは以下の構成とする。

```
/<VersionID>/<DeviceID>/<PropertyName>?<Query>
```

- VersionID:  
EL-WebAPIのバージョン番号。  
例：v1  

- DeviceID:  
機器を指定するためのstringデータ。DeviceNameとID番号(10進数)で構成される。  
例：GeneralLighting\_1, HomeAirConditioner\_1  

	- DeviceName：ECHONET LiteのEOJの先頭2Byteに対応した機器種別の名前。EL-WebAPIとして定義する。  
	- ID番号：1から始まる連番。プロトコルブリッジが採番する。DeviceIDが付与された機器がネットワークから一時的にオフラインとなりその後オンラインとなった場合には同一のDeviceIDを付与する。

- PropertyName:  
ECHONET LiteのEPCに対応したProperty（属性）の名前。EL-WebAPIとして定義する。  
例：On: (EPC=0x80)  

- Query:  
ほとんどの場合は不要である。以下の例のようにECHONET LiteでGetを実行する前にSetでパラメータを指定することが必要な場合に利用する。  
例：EOJ=0x0288（低圧スマート電力量メータ）  
EPC=0xE2（積算電力量計測値履歴１）をGETする前にEPC=0xE5（積算履歴収集日１）をSETする必要がある。  
プロトコルブリッジがこのrequestを受けた場合は、Set EPC=0xE5 (EDT=0x00) を実行した後に Get EPC=0xE2 を実行する。  

    ```
    Method: GET 
    http://192.168.50.11/v1/LVSmartElectricEnergyMeter_1/NormDirIntegralEnergyLog1?value=0
    ```


## 5. Node Profile  
　ECHONET Liteを実装している機器は、IP通信を行うNode Profileと機器毎の機能が実装された機器オブジェクトで構成される。Node Profileに定義されているEPCのうちEL-WebAPIとして必要な情報は EPC=0xD6 自分ノードインスタンスリストS（機器オブジェクトのリスト）のみであり、EL-WebAPIではこの情報を取得するAPIを定義する。以下、本ドキュメント内でEPCという場合は機器オブジェクトのEPCを意味する。

## 6. API reference

###1. GET /

- Description  
プロトコルブリッジがサポートしているEL-WebAPIのバージョンを取得する。 
- Response  
プロトコルブリッジがサポートしているEL-WebAPIのバージョン  

	```
	{ "version": [<version>] }
	```
	
- Example  

    ```
    GET http://192.168.50.11/

    // response
    {
	"version" : [v1, v2]
    }
    ```


###2. GET /\<VersionID>

- Description  
プロトコルブリッジに接続されている機器のDeviceIDと付加情報を取得する。  
- Response  
プロトコルブリッジに接続されている機器のDeviceIDと付加情報（IP Address, Protocol, Manufacturer Name(Japanese), Manufacturer Name(English)）。機器が複数台存在する場合は全ての機器の情報を列挙する。  

	```
	{
		<DeviceID>: {
			"ip": <ip address>,
			"protocol": <protocol>,
			"ManufacturerNameJa" : <Manufacturer Name(日本語)>,
			"ManufacturerNameEn" : <Manufacturer Name(英語)>
		}
	}
	```

- Example  

    ```
    GET http://192.168.50.11/v1

    // response
   {
	"GeneralLighting_1": {
		"ip": "192.168.50.5",
		"protocol": "ECHONET Lite",
		"ManufacturerNameJa" : "神奈川工科大学",
		"ManufacturerNameEn" : "Kanagawa Institute of Technology"
		}
	},
	"GeneralLighting_2": {
		"ip": "192.168.50.6",
		"protocol": "EL-WebAPI",
		"ManufacturerNameJa": "東芝",
		"ManufacturerNameEn": "Toshiba"
		}
	},
	"GeneralLighting_3": {
		"ip": "192.168.50.7",
		"protocol": "HUE_API",
		"ManufacturerNameJa": "Philips" ,
		"ManufacturerNameEn": "Philips"
		}
	},
	"HomeAirConditioner_1": {
		"ip": "192.168.50.8",
		"protocol": "ECHONET Lite",
		"ManufacturerNameJa": "Panasonic",
		"ManufacturerNameEn": "Panasonic"
		}
    }
    ```

###3. GET /\<VersionID>/\<DeviceID>

- Description  
DeviceIDで指定した機器の全てのPropertyの値を取得する。  
- Response  
指定した機器の全てのpropertyの値。プロトコルブリッジの実装の都合で特定のpropertyの値が"undefined"となることがある。その場合はpropertyを個別に指定して値を取得すること。  
- Example  

    ```
    http://192.168.50.11/v1/GeneralLighting_1

    // response
    {
        "On" : true,
        "Brightness" : 50,
        "OperatingMode": "color",
        "RGB": {"r":20, "g":255, "b":0}
    }
    ```

###4 GET /\<VersionID>/\<DeviceID>/\<PropertyName>?\<Query>  

- Description  
指定したpropertyの値を取得する  
- Query  
通常は不要。ECHONET LiteのGETを実行する前にSETの実行が必要な場合に、SETの引数を指定する。  
{ "value": \<value> }
- Response  
指定したpropertyの値。  
{ "value": \<value> }
- Example  

    ```
	// Example-1
	// request
    http://192.168.50.11/v1/GeneralLighting_1/OperatingMode

    // response
    { "value" : "color" }
    
	// Example-2    
	// request
    http://192.168.50.11/v1/LVSmartElectricEnergyMeter_1/NormDirIntegralEnergyLog1?value=0

    // response
    { "value" : {"day":0, "energy":[20, 34, 59, 109...] } }
    ```

###5 PUT /\<VersionID>/\<DeviceID>/\<PropertyName>  

- Description  
指定したpropertyの値を設定する  
- Body Data  
{ "value": \<value> }
- Response  
コマンド実行の成否
- Example  

    ```
    PUT http://192.168.50.11/v1/GeneralLighting_1/OperatingMode
    Body: 
    { "value":"color" }

    // response
    { "status" : 200 }
    ```

## 7. DeviceName  
　ECHONET Liteの仕様書に記載されている機器オブジェクト名とEL-WebAPIのDeviceNameの対応を以下に示す。DeviceNameはECHONET Liteの仕様書に記載されている機器の英語名を参照して定義した。機器オブジェクト名が赤字のものは重点８機器である。

| EL:EOJ | EL:機器オブジェクト名(J) | EL:Device Object Name(E) | EL-WebAPI:DeviceName|
|:------:|:------------:|:------------:|:------------:|
| 0x0001 | ガス漏れセンサ | gas leak sensor | GasLeakSensor |
| 0x0002 | 防犯センサ | crime prevention sensor | CrimePreventionSensor |
| 0x0003 | 非常ボタン | emergency button | EmergencyButton |
| 0x0004 | 救急用センサ | first aid sensor | FirstAidSensor |
| 0x0005 | 地震センサ | earthquake sensor | EarthquakeSensor |
| 0x0006 | 漏電センサ | electric leak sensor | ElectricLeakSensor |
| 0x0007 | 人体検知センサ | human detection sensor | HumanDetectionSensor |
| 0x0008 | 来客センサ | visitor sensor | VisitorSensor |
| 0x0009 | 呼び出しセンサ | call sensor | CallSensor |
| 0x000A | 結露センサ | condensation sensor | CondensationSensor |
| 0x000B | 空気汚染センサ | air pollution sensor | AirPollutionSensor |
| 0x000C | 酸素センサ | oxygen sensor | OxygenSensor |
| 0x000D | 照度センサ | illuminance sensor | IlluminanceSensor |
| 0x000E | 音センサ | sound sensor | SoundSensor |
| 0x000F | 投函センサ | mailing sensor | MailingSensor |
| 0x0010 | 重荷センサ | weight sensor | WeightSensor |
| 0x0011 | 温度センサ | temperature sensor | TemperatureSensor |
| 0x0012 | 湿度センサ | humidity sensor | HumiditySensor |
| 0x0013 | 雨センサ | rain sensor | RainSensor |
| 0x0014 | 水位センサ | water level sensor | WaterLevelSensor |
| 0x0015 | 風呂水位センサ | bath water level sensor | BathWaterLevelSensor |
| 0x0016 | 風呂沸き上がりセンサ | bath heating status sensor | BathHeatingStatusSensor |
| 0x0017 | 水漏れセンサ | water leak sensor | WaterLeakSensor |
| 0x0018 | 水あふれセンサ | water overflow sensor | WaterOverflowSensor |
| 0x0019 | 火災センサ | fire sensor | FireSensor |
| 0x001A | タバコ煙センサ | cigarette smoke sensor | CigaretteSmokeSensor |
| 0x001B | ＣＯ２センサ | CO2 sensor | CO2Sensor |
| 0x001C | ガスセンサ | gas sensor | GasSensor |
| 0x001D | ＶＯＣセンサ | VOC sensor | VOCSensor |
| 0x001E | 差圧センサ | differential pressure sensor | DifferentialPressureSensor |
| 0x001F | 風速センサ | air speed sensor | AirSpeedSensor |
| 0x0020 | 臭いセンサ | odor sensor | OdorSensor |
| 0x0021 | 炎センサ | flame sensor | FlameSensor |
| 0x0022 | 電力量センサ | electric energy sensor | ElectricEnergySensor |
| 0x0023 | 電流値センサ | current value sensor | CurrentValueSensor |
| 0x0025 | 水流量センサ | water flow rate sensor | WaterFlowRateSensor |
| 0x0026 | 微動センサ | micromotion sensor | MicromotionSensor |
| 0x0027 | 通過センサ | passage sensor | PassageSensor |
| 0x0028 | 在床センサ | bed presence sensor | BedPresenceSensor |
| 0x0029 | 開閉センサ | open/close sensor | OpenCloseSensor |
| 0x002A | 活動量センサ | activity amount sensor | ActivityAmountSensor |
| 0x002B | 人体位置センサ | human body location sensor | HumanBodyLocationSensor |
| 0x002C | 雪センサ | snow sensor | SnowSensor |
| 0x002D | 気圧センサ | air pressure sensor | AirPressureSensor |
| 0x0130 | <font color="Red">家庭用エアコン | home air conditioner | HomeAirConditioner |
| 0x0133 | 換気扇 | ventilation fan | ventilation fan |
| 0x0134 | 空調換気扇 | air conditioner ventilation fan | AirConditionerVentilationFan |
| 0x0135 | 空気清浄器 | air cleaner | AirCleaner |
| 0x0139 | 加湿器 | humidifier | Humidifier |
| 0x0142 | 電気暖房器 | electric heater | ElectricHeater |
| 0x0143 | ファンヒータ | fan heater | FanHeater |
| 0x0155 | 電気蓄熱暖房器 | electric storage heater | ElectricStorageHeater |
| 0x0156 | 業務用パッケージエアコン室内機 (設備用除く)クラス | package-type commercial air conditioner indoor unit | CommercialAirConditionerIndoorUnit |
| 0x0157 | 業務用パッケージエアコン室外機 (設備用除く)クラス | package-type commercial air conditioner outdoor unit | CommercialAirConditionerOutdoorUnit | 
| 0x0260 | 電動ブラインド・日よけ | electrically operated blind/shade | ElectricBlind |
| 0x0261 | 電動シャッター | electrically operated shutter | ElectricShutter |
| 0x0263 | 電動雨戸・シャッター | electrically operated rain sliding door/shutter | ElectricRainDoor |
| 0x0264 | 電動ゲート | electrically operated gate | ElectricGate |
| 0x0265 | 電動窓 | electrically operated window | ElectricWindow |
| 0x0266 | 電動玄関ドア・引戸 | electrically operated entrance door/sliding door | ElectricDoor |
| 0x0267 | 散水器（庭用） | sprinkler (for garden) | Sprinkler |
| 0x026B | <font color="Red">電気温水器</font> | electric water heater | ElectricWaterHeater |
| 0x026E | 電気便座 | electric toilet seat | ElectricToiletSeat |
| 0x026F | 電気錠 | electric key | ElectricKey |
| 0x0272 | <font color="Red">瞬間式給湯器</font> | instataneous water heater | InstataneousWaterHeater |
| 0x0273 | 浴室暖房乾燥機 | bathroom heater and dryer | BathroomHeaterDryer |
| 0x0279 | <font color="Red">住宅用太陽光発電</font> | household solar power generation | PVPowerGeneration |
| 0x027A | 冷温水熱源機 | cold or hot water heat source equipment | ColdHotWaterHeatSourceEquipment |
| 0x027B | 床暖房 | floor heater | FloorHeater |
| 0x027C | <font color="Red">燃料電池</font> | fuel cell | FuelCell |
| 0x027D | <font color="Red">蓄電池</font> | storage battery | StorageBattery |
| 0x027E | <font color="Red">電気自動車充放電器</font> | electric vehicle charger/discharger | EVChargerDischarger |
| 0x027F | エンジンコージェネレーション | engine cogeneration | EngineCogeneration |
| 0x0280 | 電力量メータ | watt-hour meter | WattHourMeter |
| 0x0281 | 水流量メータ | water flow meter | WaterFlowMeter |
| 0x0282 | ガスメータ | gas meter | GasMeter |
| 0x0283 | LPガスメータ | LP gas meter | LPGasMeter |
| 0x0287 | 分電盤メータリング | power distribution board metering | PowerDistributionBoardMetering |
| 0x0288 | <font color="Red">低圧スマート電力量メータ</font> | low-voltage smart electric energy meter | LVSmartElectricEnergyMeter |
| 0x0289 | スマートガスメータ | smart gas meter | SmartGasMeter |
| 0x028A | <font color="Red">高圧スマート電力量メータ</font> | high-voltage smart electric energy meter | HVSmartElectricEnergyMeter |
| 0x028B | 灯油メータクラス | kerosene meter | KeroseneMeter |
| 0x028C | スマート灯油メータクラス | smart kerosene meter | SmartKeroseneMeter |
| 0x0290 | <font color="Red">一般照明</font> | general lighting | GeneralLighting |
| 0x0291 | 単機能照明 | mono functional lighting | MonoFunctionalLighting |
| 0x02A0 | ブザー | buzzer | Buzzer |
| 0x02A1 | 電気自動車充電器 | electric vehicle charger | EVCharger |
| 0x02A2 | Household small wind turbine power generation | Household small wind turbine power generation | HouseholdSmallWindTurbinePowerGeneration |
| 0x02A3 | 照明システムクラス | lighting system | LightingSystem |
| 0x03B2 | 電気ポット | electric hot water pot | ElectricHotWaterPot |
| 0x03B7 | 冷凍冷蔵庫 | refrigerator | Refrigerator |
| 0x03B8 | オーブンレンジ | combination microwave oven | CombinationMicrowaveOven |
| 0x03B9 | クッキングヒータ | cooking heater | CookingHeater |
| 0x03BB | 炊飯器 | rice cooker | RiceCooker |
| 0x03C5 | 洗濯機 | washing machine | WashingMachine |
| 0x03C6 | 衣類乾燥機 | clothes dryer | ClothesDryer |
| 0x03CE | 業務用ショーケース | commercial show case | CommercialShowCase |
| 0x03D3 | 洗濯乾燥機 | washer and dryer | WasherDryer |
| 0x03D4 | 業務用ショーケース向け室外機 | commercial show case outdoor unit | CommercialShowCaseOutdoorUnit |
| 0x0401 | 体重計 | weighing machine | WeighingMachine |
| 0x05FA | 並列処理併用型電力制御クラス | parallel processing combination-type power | ParallelProcessingCombinationTypePower |
| 0x05FB | DRイベントコントローラ | DR event controller | DREventController |
| 0x05FD | スイッチ（JEMA/HA端子対応） | switch | Switch |
| 0x05FF | コントローラ | controller | Controller |
| 0x0601 | ディスプレー | display | Display |
| 0x0602 | テレビ | television | Television |
| 0x0603 | オーディオ | audio | Audio |
| 0x0604 | ネットワークカメラ | network camera | NetworkCamera |

