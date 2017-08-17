# ECHONET Lite WebAPI (EL-WebAPI) Property 詳細規定

2017.07.28 Draft version 1.0.0  
2017.08.01 Draft version 1.0.1  
- Typoの修正
- 4. Property DataのData Type "level"の定義の修正  
2017.08.17 Draft version 1.0.2  
- HomeAirConditionerのPropertyNameの修正  
　(Temperature -> RoomTemperature, AirTemperature -> AirFlowTemperature)

## 1. Abstract
　このドキュメントはECHONET Lite WebAPI(EL-WebAPI)のPropertyを記述する。各PropertyはECHONET Property Code(EPC)に対応する。ただし以下の２つの場合はWebAPIでの利便性を上げるために複数のEPCをまとめて１つのPropertyとする。詳細は後述する。  
A) ECHONET LiteでGETする前にSETが必要な場合  
B) ECHONET LiteでGETした値を他のEPCの値で換算が必要な場合

## 2. Scope
　対象機器は重点８機器からはじめ順次拡大する。対象PropertyはECHONET Liteで必須とされているもの及びその他有用なものとする。対象とするECHONET 機器オブジェクト詳細規定のバージョンは Release I。

## 3. 構成
　各機器のPropertyを共通Propertyと個別Propertyに分けて記述する。共通PropertyとはECHONET LiteのEPCが 0x80...0x9F に対応するものであり、ECHONET Liteで機器オブジェクトのスーパークラスとして定義されたEPCと以下のEPCから成る。  

- 0x8F: 節電動作設定
- 0x90: ONタイマ予約設定
- 0x91: ONタイマ時刻設定値
- 0x92: ONタイマ相対時刻設定値
- 0x94: OFFタイマ予約設定
- 0x95: OFFタイマ時刻設定値
- 0x96: OFFタイマ相対時刻設定値

　個別Propertyは「ECHONET機器オブジェクト詳細規定」第３章機器オブジェクト詳細規定に対応する機器毎のものである。

## 4. Property Data
　5章以降で具体的なPropertyのDataを記述する。ここではそこに記述されている各項目の説明を行う。  

- PropertyName, Description  
EL-WebAPIでPropertyを指定するための名前とdescription。

- Method  
RESTのアクセスメソッド。GET または PUT。

- Data  
EL-WebAPIで取得または設定するPropertyのdata。Data typeには以下のものがある  
	- boolean: true or false。付加情報としてkey-valueを data:{true:\<value>, false:\<value>} で表現する。
	- string: 文字列
	- number: 整数または固定小数。optionはunit（単位）, min（最小）, max（最大）
	- kvs: Key-Value data。付加情報としてkey-valueを data:{\<key>:\<value>} で表現する。
	- level: 1から始まる整数でレベル（強度）を表現する。付加情報としてレベルの最大値を max: で表現する。値を設定する場合は整数の他に"min", "max"を利用することもできる。  

　　例1）type:boolean, data:{true:On, false:Off}  
　　例2）type:number, unit:℃  
　　例3）type:kvs, data:{auto:自動, cooling:冷房, heating:暖房}  
　　例4）type:level, max:8  

　　特定のPropertyが複数種類の data type のうちどれかの値を利用する場合、以下のような表記を行う。  
　　例）{type:number} or {type:kvs}  

　　Property Dataが複数の要素で構成される場合は以下のような表記を行う。  

```
{
　　<element>: <data expression>,
　　<element>: <data expression>,
　　<element>: <data expression>
}
```
　　
Property dataの例を以下に示す。

```
Single element
{ "value" : "on" }

Multiple elements
{
  "value" : 
  {
    "year" : 2016,
    "month" : 2,
    "day": 23
  }
}

```

- EL:EPC, EL:プロパティ名称, Property name  
対応する ECHONET Lite のEPCやプロパティ名称。機能の詳細は「ECHONET機器オブジェクト詳細規定」を参照すること。  


## 5. 共通 Property  
|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| On<br>On/Off状態 | GET<br>PUT| type:boolean,<br>data:{true:On, false:Off} | 0x80 | 動作状態<br>Operation status |
| InstallationLocation<br>設置場所 | GET<br>PUT| type:string | 0x81 | 設置場所<br>Installation location |\*1|
| DeviceObjectStandardVersion<br>機器オブジェクト詳細規定のVersion<br>（例：A, B, C...） | GET| type:string |  0x82 | 規格Version情報<br>Standard version information |
| IdentificationNumber<br>機器固有の17byteデータ | GET| type:String |  0x83 | 識別番号<br>Identification number |\*2|
| FaultStatus<br>異常状態 |GET| type:boolean,<br>data:{true:有, false:無}|0x88| 異常発生状態<br>Fault status |
| ManufacturerName<br>メーカー名（日本語及び英語）| GET| {<br>ja:{type:string},<br>en:{type:string}<br>}| 0x8A | メーカコード<br>Manufacturer code |
| PowerSaving | GET<br>PUT| type:boolean,<br>data:{true:節電動作, false:通常動作} | 0x8F | 節電動作設定<br>Power-saving operation setting |*3|
| PUTPropertyList<br>PUTに対応するPropertyのリスト | GET|[ {type:string} ] | 0x9E | Setプロパティマップ<br>Set property map |\*4|
| GETPropertyList<br>GETに対応するPropertyのリスト | GET|[ {type:string} ]| 0x9F | GETプロパティマップ<br>GET property map |\*4|

\*1) Dataは\<keyword>と\<id>で構成される。\<id>はoption。例：Room\_1, Living。\<keyword>は以下の通り。
Living, Dining, Kitchen, Bathroom, Lavatory, Washroom, Hallway, Room, Stairway, Entrance, Storeroom, Garden, Garage, Balcony, Other  
\*2) 蓄電池では必須機能  
\*3) エアコンでは必須機能  
\*4) 「ECHONET機器オブジェクト詳細規定」付録１に記述されているように、プロパティの数が16以上の場合はECHONET Lite機器からはBitmap形式でデータが送られてくる。ブリッジではこのデータをdecodeしてEL-Web APIのPropertyNameに変換してリストを作成する。

## 6. 個別 Property
### 家庭用エアコン: HomeAirConditioner: EOJ=0x0130

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| AirFlowLevel<br>風量レベル | GET<br>PUT | {type:level, max:8} or<br>{type:kvs, data:{auto:自動}}| 0xA0 | 風量設定<br>Air flow rate setting |
| OperatingMode<br>動作モード | GET<br>PUT |type:kvs, <br>data:{auto:自動, cooling:冷房, heating:暖房, dehumidification:除湿, circulation:送風, other:その他}| 0xB0 | 運転モード設定<br>Operation mode setting |
| TargetTemperature<br>設定温度 | GET<br>PUT |{type:number, unit:℃} or<br>{type:kvs, data:{undefined:設定値不明}}| 0xB3 | 温度設定値<br>Set temperature value |
| Humidity<br>湿度| GET|{type:number, unit:%} or<br>{type:kvs, data:{overflow:Overflow, underflow:Underflow, notApplicable:Not Applicable}} | 0xBA | 室内相対湿度計測値<br>Measured value of room relative humidity |\*1|
| RoomTemperature<br>室内気温 | GET|{type:number, unit:℃} or<br>{type:kvs, data:{overflow:Overflow, underflow:Underflow, notApplicable:Not Applicable}}| 0xBB | 室内温度計測値<br>Measured value of room temperature |
| AirFlowTemperature<br>吹き出し口温度 | GET|{type:number, unit:℃} or<br>{type:kvs, data:{notApplicable:Not Applicable}}| 0xBD | 吹き出し温度計測値<br>Measured cooled air temperature |\*1|
| OutdoorTemperature<br>室外気温 | GET|{type:number, unit:℃} or<br>{type:kvs, data:{notApplicable:Not Applicable}}| 0xBE | 外気温度計測値<br>Measured outdoor air temperature |\*1|

\*1) 必須項目ではない

### 電気温水器: ElectricWaterHeater: EOJ=0x026B 

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| AutomaticWaterHeating<br>自動沸き上げ設定 | GET<br>PUT | type:kvs, <br>data:{auto:自動沸き上げ, manualNoHeating:手動沸き上げ停止, manualHeating:手動沸き上げ}| 0xB0 | 沸き上げ自動設定<br>Automatic water heating setting |
| WaterHeatingStatus<br>沸き上げ状態 | GET | type:boolean, <br>data:{true:沸き上げ中, false:沸き上げ無し} | 0xB2 | 沸き上げ中状態<br>Water heater status |
| DaytimeReheatingPermission<br>昼間沸き増し許可 | GET<br>PUT | type:boolean, <br>data:{true:許可, false:禁止} | 0xC0 | 昼間沸き増し許可設定<br>Daytime reheating permission setting |
| AlarmStatus<br>警報発生状態<br>湯切れ・漏水・凍結 | GET |{<br>noHotWater:{type:boolean, data:{true:発生, false:正常}}, <br>leaking:{type:boolean, data:{true:発生, false:正常}},<br>freezing:{type:boolean, data:{true:発生, false:正常}}<br>} | 0xC2 | 警報発生状態<br>Alarm status |
| HotWaterSupplyStatus<br>給湯中状態 | GET | type:boolean, <br>data:{true:給湯中, false:非給湯中} | 0xC3 | 給湯中状態<br>Hot water supply status |
| EnergyShiftJoining<br>エネルギーシフト参加 | GET<br>PUT | type:boolean, <br>data:{true:参加, false:不参加} | 0xC7 | エネルギーシフト参加状態 |
| WaterHeatingStartTime<br>沸き上げ開始基準時刻 | GET | type:number, <br>data:[1,20,21,22,23,24] | 0xC8 | 沸き上げ開始基準時刻 |
| NumberOfEnergyShift<br>エネルギーシフト回数 | GET | type:number,<br>data:[1,2] | 0xC9 | エネルギーシフト回数 |
| WaterHeatingTime1<br>昼間沸き上げシフト時刻１ | GET<br>PUT |{type:number, min:9, max:17}, or<br>{type:kvs, data:{undefined:未設定}}| 0xCA | 昼間沸き上げシフト時刻１ |
| EstimatedEnergyConsumption1<br>昼間沸き上げシフト時刻１での沸き上げ予測電力量 | GET | {<br>10:00:{type:number,unit:Wh},<br>13:00:{type:number,unit:Wh},<br>15:00:{type:number,unit:Wh},<br>17:00:{type:number,unit:Wh}<br>} | 0xCB | 昼間沸き上げシフト時刻１での沸き上げ予測電力量 | 
| EnergyConsumptionRate1<br>時間あたり消費電力量１ |GET|{<br>10:00:{type:number,unit:Wh},<br>13:00:{type:number,unit:Wh},<br>15:00:{type:number,unit:Wh},<br>17:00:{type:number,unit:Wh}<br>}| 0xCC | 時間あたり消費電力量１ |
| WaterHeatingTime2<br>昼間沸き上げシフト時刻２ | GET<br>PUT |{type:number, min:10, max:17}, or<br>{type:kvs, data:{undefined:未設定}}| 0xCD | 昼間沸き上げシフト時刻２ |
| EstimatedEnergyConsumption2<br>昼間沸き上げシフト時刻２での沸き上げ予測電力量 | GET | {<br>13:00:{type:number,unit:Wh},<br>15:00:{type:number,unit:Wh},<br>17:00:{type:number,unit:Wh}<br>} | 0xCE | 昼間沸き上げシフト時刻２での沸き上げ予測電力量 |
| EnergyConsumptionRate2<br>時間あたり消費電力量２ |GET | {<br>13:00:{type:number,unit:Wh},<br>15:00:{type:number,unit:Wh},<br>17:00:{type:number,unit:Wh}<br>} | 0xCF | 時間あたり消費電力量２ | 
| AutomaticBathWaterHeating<br>自動風呂沸き上げ設定 | GET<br>PUT | type:boolean, <br>data:{true:auto, false:manual} | 0xE3 | 風呂自動モード設定<br>Automatic bath water heating mode setting |
| BathOperatingStatus<br>風呂動作状態 | GET | type:kvs, <br>data:{runningHotWater:湯張り中, noOperation:停止中, keepingTemperature:保温中} | 0xEA | 風呂動作状態監視<br>Bath operation status monitor |

### 瞬間式給湯器: InstataneousWaterHeater: EOJ=0x0272 

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| WaterHeatingStatus<br>沸き上げ状態 | GET | type:boolean, <br>data:{true:沸き上げ中, false:沸き上げ無し} | 0xD0 | 給湯器燃焼状態<br>Hot water heating status |
| BathWaterHeatingStatus<br>風呂給湯器燃焼状態 | GET | type:boolean, <br>data:{true:沸き上げ中, false:沸き上げ無し} | 0xE2 | 風呂給湯器燃焼状態<br>Bath water heater status |
| AutomaticBathWaterHeating<br>自動風呂沸き上げ設定 | GET<br>PUT | type:boolean, <br>data:{true:auto, false:manual} | 0xE3 | 風呂自動モード設定<br>Bath auto mode setting |
| BathOperatingStatus<br>風呂動作状態 | GET | type:kvs, <br>data:{runningHotWater:湯張り中, noOperation:停止中, keepingTemperature:保温中}| 0xEF | 風呂動作状態監視<br>Bath operation status monitor |


### 住宅用太陽光発電: PVPowerGeneration: EOJ=0x0279 

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| InstantaneousPowerGeneration<br>瞬時発電電力計測値 | GET | type:number,<br>unit:W | 0xE0 | 瞬時発電電力計測値<br>Measured instantaneous amount of electricity generated |
| IntegralEnergyGeneration<br>積算発電電力量計測値 | GET | type:number,<br>kWh | 0xE1 | 積算発電電力量計測値<br>Measured cumulative amount of electric energy generated |\*1|

\*1) 積算発電電力量計測値は、ECHONET LiteでGETした値に0.001を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。


### 燃料電池: FuelCell: EOJ=0x027C 

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| InstantaneousPowerGeneration<br>瞬時発電電力計測値 | GET<br>PUT | type:number,<br>unit:W | 0xC4 | 瞬時発電電力計測値<br>Measured instantaneous power generation output |
| IntegralEnergyGeneration<br>積算発電電力量計測値 | GET<br>PUT | type:number,<br>unit:kWh | 0xC5 | 積算発電電力量計測値<br>Measured cumulative power generation output |\*1|

\*1) 積算発電電力量計測値は、ECHONET LiteでGETした値に0.001を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

### 蓄電池: StorageBattery: EOJ=0x027D 

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| EffectiveChargingCapacity<br>AC実効容量（充電） | GET | type:number,<br>unit:Wh | 0xA0 | AC実効容量（充電）<br>AC effective capacity(charging) |
| EffectiveDischargingCapacity<br>AC実効容量（放電） | GET | type:number,<br>unit:Wh | 0xA1 | AC実効容量（放電）<br>AC effective capacity(discharging) |
| ChargeableCapacity<br>充電可能容量 | GET | type:number,<br>unit:Wh|0xA2| 充電可能容量<br>AC chargeable capacity |
| DischargeableCapacity<br>放電可能容量 | GET | type:number,<br>unit:Wh| 0xA3 | 放電可能容量<br>AC dischargeable capacity |
| ChargeableEnergy<br>充電可能量 | GET | type:number,<br>unit:Wh| 0xA4 | 充電可能量<br>AC chargeable electric energy |
| DischargeableEnergy<br>放電可能量 | GET | type:number,<br>unit:Wh| 0xA5 | 放電可能量<br>AC dischargeable electric energy |
| IntegralChargingEnergy<br>AC積算充電電力量計測値 | GET | type:number,<br>unit:kWh| 0xA8 | AC積算充電電力量計測値<br>AC measured cumulative charging electric energy |\*1|
| IntegralDischargingEnergy<br>AC積算放電電力量計測値 | GET | type:number,<br>unit:kWh| 0xA9 | AC積算放電電力量計測値<br>AC measured cumulative discharging electric energy |\*1|
| TargetChargingEnergy<br>AC充電量設定値 | GET<br>PUT | type:number,<br>unit:Wh| 0xAA | AC充電量設定値<br>AC charge amount setting value |
| TargetDischargingEnergy<br>AC放電量設定値 | GET<br>PUT | type:number,<br>unit:Wh| 0xAB | AC放電量設定値<br>AC discharge amount setting value |
| OperatingStatus<br>運転動作状態 | GET | type:kvs, <br>data:{rapidCharging:急速充電, charging:充電, discharging:放電, standby:待機, test:テスト, auto:自動, restart:再起動, capacityRecalculation:実行容量再計算処理, other:その他} | 0xCF | 運転動作状態<br>Working operation status |
| OperatingMode<br>運転モード | GET<br>PUT | type:kvs, <br>data:{rapidCharging:急速充電, charging:充電, discharging:放電, standby:待機, test:テスト, auto:自動, restart:再起動, capacityRecalculation:実行容量再計算処理, other:その他} | 0xDA | 運転モード設定<br>Operation mode setting |
| RemainingStoredEnergy1<br>蓄電残量1 | GET | type:number,<br>unit:Wh | 0xE2 | 蓄電残量1<br>Remaining stored electricity 1 |
| RemainingStoredEnergy2<br>蓄電残量2 | GET | type:number,<br>unit:Ah | 0xE3 | 蓄電残量2<br>Remaining stored electricity 2 |\*2|
| RemainingStoredEnergy3<br>蓄電残量3 | GET | type:number,<br>unit:% | 0xE4 | 蓄電残量3<br>Remaining stored electricity 3 |
| BatteryType<br>蓄電池タイプ | GET | type:kvs, <br>data:{unknown:不明, lead:鉛, ni-mh:NiH, ni-cd:NiCd, lib:Li-ion, zinc:Zn, alkaline:充電式アルカリ} | 0xE6 | 蓄電池タイプ<br>Battery type |

\*1) AC積算充電電力量計測値・AC積算放電電力量計測値は、ECHONET LiteでGETした値に0.001を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。  
\*2) 蓄電残量2は、ECHONET LiteでGETした値に0.1を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

### 電気自動車充放電器: EVChargerDischarger: EOJ=0x027E 

　ECHONET LiteのProperty「車両接続・充放電可否状態（EPC=0xC7）」は、ECHONET LiteでデータをGETする前に「車両接続確認（EPC=0xCD)」をSETする必要がある。ブリッジがEL-WebAPIでGET: ChargeDischargeStatusを受信した場合、ブリッジはECHONET Liteで「車両接続確認」をSETしたのち「積算電力量計測値履歴1」をGETする。

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| DischargeableCapacity1<br>車載電池の放電可能容量値1 | GET | type:number,<br>unit:Wh | 0xC0 | 車載電池の放電可能容量値1<br>Dischargeable capacity of vehicle mounted battery 1 |
| RemainingDischargeableCapacity1<br>車載電池の放電可能残容量1 | GET | type:number,<br>unit:Wh | 0xC2 | 車載電池の放電可能残容量1<br>Remaining dischargeable capacity of vehicle mounted battery 1 |
| RemainingDischargeableCapacity3<br>車載電池の放電可能残容量3 | GET | type:number,<br>unit:% | 0xC4 | 車載電池の放電可能残容量3<br>Remaining dischargeable capacity of vehicle mounted battery 3 |
| RatedChargePower<br>定格充電能力 | GET | type:number,<br>unit:W| 0xC5 | 定格充電能力<br>Rated charge capacity |
| RatedDischargePower<br>定格放電能力 | GET | type:number,<br>unit:W | 0xC6 | 定格放電能力<br>Rated discharge capacity |
| ChargeDischargeStatus<br>車両接続・充放電可否状態 | GET | type:kvs, <br>data:{undefined:不定, notConnected:車両未接続, connected:車両接続・ 充電不可 ・放電不可, chargeableDischargeable:車両接続・ 充電可 ・放電可, dischargeable:車両接続・ 充電不可 ・放電可, chargeable:車両接続・ 充電可 ・放電不可} | 0xC7 | 車両接続・充放電可否状態<br>Vehicle connection and chargeable/dischargeable status |
| MinMaxChargePower<br>最小最大充電電力値 | GET |{<br>min:{type:number,unit:W},<br>max:{type:number,unit:W}<br>}| 0xC8 | 最小最大充電電力値<br>Minimum/maximum charging electric energy |
| MinMaxDischargePower<br>最小最大放電電力値 | GET |{<br>min:{type:number,unit:W},<br>max:{type:number,unit:W}<br>}| 0xC9 | 最小最大放電電力値<br>Minimum/maximum discharging electric energy |
| MinMaxChargeCurrent<br>最小最大充電電流値 | GET |{<br>min:{type:number,unit:A},<br>max:{type:number,unit:A}<br>}| 0xCA | 最小最大充電電流値<br>Minimum/maximum charging current |\*1|
| MinMaxDischargeCurrent<br>最小最大放電電流値 | GET |{<br>min:{type:number,unit:A},<br>max:{type:number,unit:A}<br>}| 0xCB | 最小最大放電電流値<br>Minimum/maximum discharging current |\*1|
| EquipmentType<br>充放電器タイプ | GET | type:kvs, <br>data:{AC\_CPLT:AC\_CPLT, AC\_HLC\_Charge:AC\_HLC（充電のみ）, AC\_HLC\_ChargeDischarge:AC\_HLC（充放電可）, DC\_AA\_Charge:DCタイプ\_AA（充電のみ）, DC\_AA\_ChargeDischarge:DCタイプ\_AA（充放電可）, DC\_AA\_Discharge, DC\_BB\_Charge:DCタイプ\_BB（充電のみ）, DC\_BB\_ChargeDischarge:DCタイプ\_BB（充放電可）, DC\_BB\_Discharge:DCタイプ\_BB（放電のみ）, DC\_EE\_Charge:DCタイプ\_EE（充電のみ）, DC\_EE\_ChargeDischarge:DCタイプ\_EE（充放電可）, DC\_EE\_Discharge:DCタイプ\_EE（放電のみ）, DC\_FF\_Charge:DCタイプ\_FF（充電のみ）, DC\_FF\_ChargeDischarge:DCタイプ\_FF（充放電可）, DC\_FF\_Discharge:DCタイプ\_FF（放電のみ）} | 0xCC | 充放電器タイプ<br>Charger/Discharger type |
| UsedCapacity1<br>車載電池の使用容量値1 | GET | type:number,<br>unit:Wh | 0xD0 | 車載電池の使用容量値1<br>Used capacity of vehicle mounted battery 1 |
| OperatingMode | GET<br>PUT | type:kvs, <br>data:{charge:充電, discharge:放電, standby:待機, idle:停止, other:その他} | 0xDA | 運転モード設定<br>Operation mode setting |
| RemainingStoredEnergy1<br>車載電池の電池残容量1 | GET | type:number,<br>unit:Wh | 0xE2 | 車載電池の電池残容量1<br>Remaining stored electricity of vehicle mounted battery1 |
| RemainingStoredEnergy3<br>車載電池の電池残容量3 | GET | type:number,<br>unit:% | 0xE4 | 車載電池の電池残容量3<br>Remaining stored electricity of vehicle mounted battery3 |

\*1) 最小最大充電電流値・最小最大放電電流値は、ECHONET LiteでGETした値に0.1を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。


### 低圧スマート電力量メータ: LVSmartElectricEnergyMeter: EOJ=0x0288 

　以下のECHONET LiteのPropertyは、ECHONET LiteでGETした値に係数（EPC=0xD3）と積算電力量単位（EPC=0xE1）を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

> 積算電力量計測値（正方向計測値）  
> 積算電力量計測値履歴1（正方向計測値）  
> 積算電力量計測値（逆方向計測値）  
> 積算電力量計測値履歴1（逆方向計測値）  
> 定時積算電力量計測値（正方向計測値）  
> 定時積算電力量計測値（逆方向計測値）  

　以下のECHONET LiteのPropertyは、ECHONET LiteでデータをGETする前に「積算履歴収集日1（EPC=0xE5)」をSETする必要がある。EL-WebAPIでは以下のPropertyをGET(REST)する際に「積算履歴収集日1」の値をQueryとして指定する。ブリッジはECHONET Liteで「積算履歴収集日1」をSETしたのち「積算電力量計測値履歴1」をGETする。なお、EL-WebAPIでQueryを付加しない場合は「積算履歴収集日1」=0として扱う。

> 積算電力量計測値履歴1（正方向計測値）  
> 積算電力量計測値履歴1（逆方向計測値）  


|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| EffectiveDigits<br>積算電力量有効桁数 | GET | type:number | 0xD7 | 積算電力量有効桁数<br>Number of effective digits for cumulative amounts of electric energy |
| NormDirIntegralEnergy<br>積算電力量計測値<br>（正方向計測値） | GET | {type:number, unit:kWh} or<br>{type:kvs, data:{noData:計測データなし}} | 0xE0 | 積算電力量計測値（正方向計測値）<br>Measured cumulative amount of electric energy (normal direction) |
| NormDirIntegralEnergyLog1<br>積算電力量計測値履歴1<br>（正方向計測値） | GET |{<br>day:{type:number},<br>energy:[{type:number, unit:kWh} or {type:kvs, data:{noData:計測データ無し}}]<br>}| 0xE2 | 積算電力量計測値履歴1（正方向計測値）<br>Historical data of measured cumulative amounts of electric energy 1(normal direction) |\*1|
| RevDirIntegralEnergy<br>積算電力量計測値<br>（逆方向計測値） | GET | {type:number,unit:kWh} or<br>{type:kvs, data:{noData:計測データ無し}} | 0xE3 | 積算電力量計測値（逆方向計測値）<br>Measured cumulative amounts of electric energy (reverse direction) |
| RevDirIntegralEnergyLog1<br>積算電力量計測値履歴1（逆方向計測値） | GET |{<br>day:{type:number},<br>energy:[{type:number, unit:kWh} or {type:kvs, data:{noData:計測データ無し}}]<br>}| 0xE4 | 積算電力量計測値履歴1（逆方向計測値）<br>Historical data of measured cumulative amounts of electric energy 1(reverse direction) |\*1|
| InstantaneousPower<br>瞬時電力計測値 | GET | {type:number,unit:W} or<br>{type:kvs, data:{underFlow:アンダーフロー, overFlow:オーバーフロー, noData:計測データ無し}}| 0xE7 | 瞬時電力計測値<br>Measured instantaneous electric energy |
| InstantaneousCurrent<br>瞬時電流計測値<br>(R相, T相) | GET | {<br>r:{{type:number, unit:A} or {type:kvs, data:{underFlow:アンダーフロー, overFlow:オーバーフロー, noData:計測データ無し}},<br>t:{{type:number, unit:A} or {type:kvs, data:{underFlow:アンダーフロー, overFlow:オーバーフロー, noData:計測データ無し}}<br>}| 0xE8 | 瞬時電流計測値<br>Measured instantaneous currents |\*2|
| NormDirIntegralEnergyEvery30Min<br>定時積算電力量計測値（正方向計測値） | GET |{<br>year:{type:number},<br>month:{type:number},<br>day:{type:number},<br>hour:{type:number},<br>minute:{type:number},<br>second:{type:number},<br>energy:{{type:number, unit:kWh},{type:kvs, data:{noData:計測データ無し}}}<br>}| 0xEA | 定時積算電力量計測値（正方向計測値）<br>Cumulative amounts of electric energy measured at fixed time (normal direction) |
| RevDirIntegralEnergyEvery30Min<br>定時積算電力量計測値（逆方向計測値） | GET |{<br>year:{type:number},<br>month:{type:number},<br>day:{type:number},<br>hour:{type:number},<br>minute:{type:number},<br>second:{type:number},<br>energy:{{type:number, unit:kWh},{type:kvs, data:{noData:計測データ無し}}}<br>}| 0xEB | 定時積算電力量計測値（逆方向計測値）<br>Cumulative amounts of electric energy measured at fixed time (reverse direction) |

\*1) 積算履歴収集日１を指定するqueryあり。{value:{type:number}}  
\*2) 瞬時電流計測値は、ECHONET LiteでGETした値に0.1を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

### 高圧スマート電力量メータ: HVSmartElectricEnergyMeter: EOJ=0x028A 

　以下のECHONET LiteのPropertyは、ECHONET LiteでGETした値に係数（EPC=0xD3）と係数の倍率（EPC=0xD4）と積算有効電力量単位（EPC=0xE6）を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

> 積算有効電力量計測値  
> 定時積算有効電力量計測値  
> 積算有効電力量計測値履歴  

　以下のECHONET LiteのPropertyは、ECHONET LiteでGETした値に係数（EPC=0xD3）と係数の倍率（EPC=0xD4）と需要電力単位（EPC=0xC5）を乗算することで実際の値となる。ブリッジ側でこれらの計算を行うこととする。したがってEL-WebAPIでは取得した値をそのまま利用できる。

> 月間最大需要電力  
> 定時需要電力(30分平均電力)  
> 需要電力計測値履歴  

　以下のECHONET LiteのPropertyは、ECHONET LiteでデータをGETする前に「積算履歴収集日（EPC=0xE1)」をSETする必要がある。EL-WebAPIでは以下のPropertyをGET(REST)する際に「積算履歴収集日」の値をQueryとして指定する。ブリッジはECHONET Liteで「積算履歴収集日」をSETしたのち「需要電力計測値履歴」または「積算有効電力量計測値履歴」をGETする。なお、EL-WebAPIでQueryを付加しない場合は「積算履歴収集日」=0として扱う。

> 需要電力計測値履歴  
> 積算有効電力量計測値履歴  

|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| MonthlyMaxDemandPower<br>月間最大需要電力 | GET | {type:number, unit:kW} or <br>{type:kvs, data:{noData:計測データなし} | 0xC1 | 月間最大需要電力<br>Monthly maximum electric power demand |
| AverageDemandPower<br>定時需要電力<br>(30分平均電力) | GET |{<br>year:{type:number},<br>month:{type:number},<br>day:{type:number},<br>hour:number,<br>minute:{type:number},<br>second:{type:number},<br>power:{type:number, unit:kW}, or {type:kvs, data:{noData:計測データ無し}})<br>}| 0xC3 | 定時需要電力（30分平均電力）<br>Electric power demand at fixed time(30-minute average electric power) |
| EffectiveDigits<br>需要電力有効桁数 | GET | type:number | 0xC4 | 需要電力有効桁数<br>Number of effective digits of electric power demand |
| DemandPowerLog<br>需要電力計測値履歴| GET |{<br>day:{type:number},<br>power:[{type:number, unit:kW} or {type:kvs, data:{noData:計測データ無し}}]<br>}|0xC6|需要電力計測値履歴<br>Historical data of measured electric power demand|\*1|
| FixedDate<br>確定日 | GET | type:number | 0xE0 | 確定日<br>Fixed date |
| IntegralActiveEnergy<br>積算有効電力量計測値 | GET | {<br>year:number,<br>month:{type:number},<br>day:{type:number},<br>hour:{type:number},<br>minute:{type:number},<br>second:{type:number},<br>energy:({type:number, unit:kWh} or {type:kvs, data:{noData:計測データ無し}}<br>} | 0xE2 | 積算有効電力量計測値<br>Measured cumulative amounts of active electric energy |
| IntegralActiveEnergyEvery30Min<br>定時積算有効電力量計測値 | GET | {<br>year:number,<br>month:{type:number},<br>day:{type:number},<br>hour:{type:number},<br>minute:{type:number},<br>second:{type:number},<br>energy:({type:number, unit:kWh} or {type:kvs, data:{noData:計測データ無し}}<br>} | 0xE3 | 定時積算有効電力量計測値<br>Cumulative amounts of active electric energy at fixed time |
| IntegralActiveEnergyEffectiveDigits<br>積算有効電力量有効桁数 | GET | type:number | 0xE5 | 積算有効電力量有効桁数<br>Number of effective digits for cumulative amount of active electric energy |
| ActiveEnergyLog<br>積算有効電力量計測値履歴 | GET |{<br>day:{type:number},<br>energy:[{type:number, unit:kWh} or {type:kvs, data:{noData:計測データ無し}}]<br>}| 0xE7 | 積算有効電力量計測値履歴<br>Historical data of measured cumulative amount of active electric energy |\*1|

*1) 積算履歴収集日を指定するqueryあり。{value:{type:number}}

### 一般照明: GeneralLighting: EOJ=0x0290 
|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| Brightness<br>輝度 | GET<br>PUT | type:number,<br>unit:% | 0xB0 | 照度レベル設定(\*1)<br>Illuminance level |
| OperatingMode<br>動作モード | GET<br>PUT | type:kvs, <br>data:{auto:自動, normal:通常灯, night:常夜灯, color:カラー灯} | 0xB6 | 点灯モード設定<br>Lighting mode setting |
| RGB<br>カラー灯の色 | GET<br>PUT |{<br>r:{type:number, min:0, max:255},<br>g:{type:number, min:0, max:255},<br>b:{type:number, min:0, max:255}<br>}| 0xC0 | カラー灯モード時RGB設定<br>RGB setting for color lighting |\*1|

\*1) 必須項目ではない

### 単機能照明: MonoFunctionalLighting: EOJ=0x0291 
|PropertyName<br>Description|Method|Data|EL:<br>EPC|EL:プロパティ名称<br>Property name| Note |
|:--------------------------|:-----|:---|:---------|:-------------------------------|:---|
| Brightness<br>輝度 | GET<br>PUT | type:number,<br>unit:% | 0xB0 | 照度レベル設定<br>Illuminance level |\*1|

\*1) 必須項目ではない

