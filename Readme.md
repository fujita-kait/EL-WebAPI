# Readme ECHONET Lite WebAPI (EL-WebAPI)

2017.12.01  

　ECHONET Lite機器をプロトコルブリッジを介してHTTP(REST)で制御するためのWebAPI（以下EL-WebAPIと呼ぶ）を提案する。リソースの構成とAPIの定義を記述したドキュメント __ECHONET Lite WebAPI (EL-WebAPI) Specification__ と、機器毎の仕様を記述した __ECHONET Lite WebAPI Device Description__ で構成される。 

　WebAPIを設計するにあたり下記(A)のシステム構成を前提とするが、(B)や(C)のシステム構成も考慮する。  
(A)プロトコルブリッジを介してECHONET Lite機器をEL-WebAPI対応する場合  
(B)デバイスがEL-WebAPIを実装する場合    
(C)プロトコルブリッジを介してその他のAPI対応機器をEL-WebAPI対応する場合  


![システム構成](_graphics/system.png)  

　ECHONET Lite機器に関しては、Node Profileに機器オブジェクトが１つ存在する構成だけでなく、同一オブジェクトが複数存在する構成や、複数の異なるオブジェクトが存在する構成も想定する。