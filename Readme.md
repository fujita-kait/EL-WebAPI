# Readme ECHONET Lite WebAPI (EL-WebAPI)

2017.08.01  

　ECHONET Lite機器をプロトコルブリッジを介してHTTP(REST)で制御することを想定しWebAPIを提案する。リソースの構成とAPIの定義を記述したドキュメント「ECHONET Lite WebAPI (EL-WebAPI) 仕様書」と、機器毎のPropertyとDataを記述したドキュメント「EL-WebAPI Property詳細規定」で構成される。  
　WebAPIを設計するにあたり下記(A)のシステム構成だけでなく、(B)や(C)のシステム構成も想定することで今後の拡張性や汎用性も考慮する。  
(A)プロトコルブリッジを介してECHONET Lite機器をEL-WebAPI対応する場合  
(B)デバイスがEL-WebAPIを実装する場合    
(C)プロトコルブリッジを介してその他のAPI対応機器をEL-WebAPI対応する場合  


![システム構成](_graphics/system.png)  

　ECHONET Lite機器に関しては、Node Profileに機器オブジェクトが１つ存在する構成だけでなく、同一オブジェクトが複数存在する構成や、複数の異なるオブジェクトが存在する構成も想定する。