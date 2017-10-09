---
title: "aaaLogic App gränser och konfiguration | Microsoft Docs"
description: "Översikt över hello tjänstbegränsningarna och tillgängliga för Logic Apps konfigurationsvärden."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 739509afe5c9a7b7e946ba3571951264127e5297
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="logic-app-limits-and-configuration"></a>Logic App-gränser och konfiguration

Följande är information om hello aktuella begränsningar och konfigurationsinformation för Logikappar i Azure.

## <a name="limits"></a>Begränsningar

### <a name="http-request-limits"></a>HTTP-begäran gränser

Nedan följer gränser för ett enda HTTP-begäran och/eller connector-anrop.

#### <a name="timeout"></a>Timeout

|Namn|Gräns|Anteckningar|
|----|----|----|
|Tidsgräns för förfrågan|120 sekunder|En [asynk.mönster](../logic-apps/logic-apps-create-api-app.md) eller [tills loop](logic-apps-loops-and-scopes.md) kan kompensera efter behov|

#### <a name="message-size"></a>Meddelandestorlek

|Namn|Gräns|Anteckningar|
|----|----|----|
|Meddelandestorlek|100 MB|Vissa kopplingar och API: er stöder inte 100 MB |
|Gränsen för utvärdering av uttryck|131,072 tecken|`@concat()`, `@base64()`, `string` får inte vara längre än den här gränsen|

#### <a name="retry-policy"></a>Försök princip

|Namn|Gräns|Anteckningar|
|----|----|----|
|Antal återförsök|10| Standardvärdet är 4. Konfigurera med hello [försök parametern](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Högsta antal försök|1 timme|Konfigurera med hello [försök parametern](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|
|Min tid innan nytt försök|5 SEK.|Konfigurera med hello [försök parametern](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx)|

### <a name="run-duration-and-retention"></a>Kör varaktighet och lagring

Följande är hello gränser för en enkel logikapp som körs.

|Namn|Gräns|Anteckningar|
|----|----|----|
|Kör varaktighet|90 dagar||
|Kvarhållning av lagring|90 dagar|Starta från hello starttid för körning|
|Min upprepningsintervallet|1 sekund|| 15 sekunder för logic apps med App Service-Plan
|Max upprepningsintervallet|500 dagar||

Om du räknar tooexceed kör varaktighet eller lagring kvarhållning gränser i normala bearbetningen flöde [kontaktar du oss](mailto://logicappsemail@microsoft.com) så att vi kan hjälpa dig med dina krav.


### <a name="looping-and-debatching-limits"></a>Slingor och debatching gränser

Nedan följer gränser för en enkel logikapp som körs.

|Namn|Gräns|Anteckningar|
|----|----|----|
|ForEach-objekt|100,000|Du kan använda hello [fråga åtgärd](../connectors/connectors-native-query.md) toofilter större matriser efter behov|
|Tills iterationer|5,000||
|SplitOn objekt|100,000||
|ForEach parallellitet|50| Standardvärdet är 20. Du kan ange tooa sekventiella foreach genom att lägga till `"operationOptions": "Sequential"` toohello `foreach` åtgärd eller specifika andelen parallellitet med`runtimeConfiguration`|


### <a name="throughput-limits"></a>Genomströmning gränser

Nedan följer gränser för en enskild logik app-instansen. 

|Namn|Gräns|Anteckningar|
|----|----|----|
|Åtgärder körningar per 5 minuter |100,000|Kan fördela belastningen över flera appar efter behov|
|Åtgärder samtidiga utgående anrop |~2,500|Minska antalet samtidiga begäranden eller minska hello varaktighet efter behov|
|Runtime endpoint samtidiga inkommande samtal |~1,000|Minska antalet samtidiga begäranden eller minska hello varaktighet efter behov|
|Runtime endpoint läsa anrop per 5 minuter |60,000|Kan fördela belastningen över flera appar efter behov|
|Runtime endpoint anropa anrop per 5 minuter |45,000|Kan fördela belastningen över flera appar efter behov|

Om du räknar tooexceed begränsningen i normala bearbetningen eller vill toorun belastningen testning som kan överskrider denna gräns för en viss tidsperiod, [kontaktar du oss](mailto://logicappsemail@microsoft.com) så att vi kan hjälpa dig med dina krav.

### <a name="definition-limits"></a>Definition av gränser

Nedan följer gränser för en enskild logik app definition.

|Namn|Gräns|Anteckningar|
|----|----|----|
|Åtgärder per arbetsflöde|500|Du kan lägga till inkapslade arbetsflöden tooextend denna gräns efter behov|
|Tillåtna åtgärden inkapslingsdjup|8|Du kan lägga till inkapslade arbetsflöden tooextend denna gräns efter behov|
|Arbetsflöden per region per prenumeration|1000||
|Utlösare per arbetsflöde|10||
|Växeln scope fall gränsen|25||
|Antalet variabler per arbetsflöde|250||
|Maximalt antal tecken per uttryck|8 192||
|Max `trackedProperties` storlek i antal tecken|16,000|
|`action`/`trigger`gräns för|80||
|`description`Maxlängden|256||
|`parameters`gränsen|50||
|`outputs`gränsen|10||

### <a name="integration-account-limits"></a>Gränser för integrering

Nedan följer gränser för artefakter läggs toointegration konto

|Namn|Gräns|Anteckningar|
|----|----|----|
|Schemat|8 MB|Du kan använda [blob-URI](logic-apps-enterprise-integration-schemas.md) tooupload filer som är större än 2 MB |
|Karta (XSLT-fil)|2 MB| |
|Runtime endpoint läsa anrop per 5 minuter |60,000|Kan fördela belastningen över flera konton efter behov|
|Runtime endpoint anropa anrop per 5 minuter |45,000|Kan fördela belastningen över flera konton efter behov|
|Runtime endpoint spåra anrop per 5 minuter |45,000|Kan fördela belastningen över flera konton efter behov|
|Runtime-slutpunkt som blockerar samtidiga anrop |~1,000|Minska antalet samtidiga begäranden eller minska hello varaktighet efter behov|

### <a name="b2b-protocols-as2-x12-edifact-message-size"></a>Meddelandestorlek B2B-protokoll (AS2 X12 EDIFACT)

Följande är hello gränser för B2B-protokoll

|Namn|Gräns|Anteckningar|
|----|----|----|
|AS2|50 MB|Tillämpliga toodecode och koda|
|X12|50 MB|Tillämpliga toodecode och koda|
|EDIFACT|50 MB|Tillämpliga toodecode och koda|

## <a name="configuration"></a>Konfiguration

### <a name="ip-address"></a>IP-adress

#### <a name="logic-app-service"></a>Logik för Apptjänst

Anrop från en logikapp direkt (det vill säga via [HTTP](../connectors/connectors-native-http.md) eller [HTTP + Swagger](../connectors/connectors-native-http-swagger.md)) eller andra HTTP-begäranden som kommer från hello IP-adress som anges i följande lista hello:

|Logik App Region|Utgående IP|
|-----|----|
|Östra Australien|13.75.153.66, 104.210.89.222, 104.210.89.244, 13.75.149.4, 104.210.91.55, 104.210.90.241|
|Sydöstra Australien|13.73.115.153, 40.115.78.70, 40.115.78.237, 13.73.114.207, 13.77.3.139, 13.70.159.205|
|Södra Brasilien|191.235.86.199, 191.235.95.229, 191.235.94.220, 191.235.82.221, 191.235.91.7, 191.234.182.26|
|Centrala Kanada|52.233.29.92, 52.228.39.241, 52.228.39.244|
|Östra Kanada|52.232.128.155, 52.229.120.45, 52.229.126.25|
|Indien, centrala|52.172.157.194, 52.172.184.192, 52.172.191.194, 52.172.154.168, 52.172.186.159, 52.172.185.79|
|Centrala USA|13.67.236.76, 40.77.111.254, 40.77.31.87, 13.67.236.125, 104.208.25.27, 40.122.170.198|
|Östasien|168.63.200.173, 13.75.89.159, 23.97.68.172, 13.75.94.173, 40.83.127.19, 52.175.33.254|
|Östra USA|137.135.106.54, 40.117.99.79, 40.117.100.228, 13.92.98.111, 40.121.91.41, 40.114.82.191|
|Östra USA 2|40.84.25.234, 40.79.44.7, 40.84.59.136, 40.84.30.147, 104.208.155.200, 104.208.158.174|
|Östra Japan|13.71.146.140, 13.78.84.187, 13.78.62.130, 13.71.158.3, 13.73.4.207, 13.71.158.120|
|Västra Japan|40.74.140.173, 40.74.81.13, 40.74.85.215, 40.74.140.4, 104.214.137.243, 138.91.26.45|
|Norra centrala USA|168.62.249.81, 157.56.12.202, 65.52.211.164, 168.62.248.37, 157.55.210.61, 157.55.212.238|
|Norra Europa|13.79.173.49, 52.169.218.253, 52.169.220.174, 40.113.12.95, 52.178.165.215, 52.178.166.21|
|Södra centrala USA|13.65.98.39, 13.84.41.46, 13.84.43.45, 104.210.144.48, 13.65.82.17, 13.66.52.232|
|Sydostasien|52.163.93.214, 52.187.65.81, 52.187.65.155, 13.76.133.155, 52.163.228.93, 52.163.230.166|
|Södra Indien|52.172.9.47, 52.172.49.43, 52.172.51.140, 52.172.50.24, 52.172.55.231, 52.172.52.0|
|Västra Europa|13.95.155.53, 52.174.54.218, 52.174.49.6, 40.68.222.65, 40.68.209.23, 13.95.147.65|
|Indien, västra|104.211.164.112, 104.211.165.81, 104.211.164.25, 104.211.164.80, 104.211.162.205, 104.211.164.136|
|Västra USA|52.160.90.237, 138.91.188.137, 13.91.252.184, 52.160.92.112, 40.118.244.241, 40.118.241.243|
|Storbritannien, södra|51.140.74.14, 51.140.73.85, 51.140.78.44|
|Storbritannien, västra|51.141.54.185, 51.141.45.238, 51.141.47.136|

#### <a name="connectors"></a>Anslutningar

Anrop från en [connector](../connectors/apis-list.md) kommer från hello IP-adress som anges i följande lista hello:

|Logik App Region|Utgående IP|
|-----|----|
|Östra Australien|40.126.251.213|
|Sydöstra Australien|40.127.80.34|
|Södra Brasilien|191.232.38.129|
|Centrala Kanada|52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13|
|Östra Kanada|52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52|
|Indien, centrala|104.211.98.164|
|Centrala USA|40.122.49.51|
|Östasien|23.99.116.181|
|Östra USA|191.237.41.52|
|Östra USA 2|104.208.233.100|
|Östra Japan|40.115.186.96|
|Västra Japan|40.74.130.77|
|Norra centrala USA|65.52.218.230|
|Norra Europa|104.45.93.9|
|Södra centrala USA|104.214.70.191|
|Sydostasien|13.76.231.68|
|Södra Indien|104.211.227.225|
|Västra Europa|40.115.50.13|
|Indien, västra|104.211.161.203|
|Västra USA|104.40.51.248|
|Storbritannien, södra|51.140.80.51|
|Storbritannien, västra|51.141.47.105|


## <a name="next-steps"></a>Nästa steg  

- tooget igång med Logic Apps följer hello [skapa en Logikapp](../logic-apps/logic-apps-create-a-logic-app.md) kursen.  
- [Visa vanliga exempel och scenarier](../logic-apps/logic-apps-examples-and-scenarios.md)
- [Du kan automatisera verksamhetsprocesser med Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Lär dig hur tooIntegrate dina system med Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
