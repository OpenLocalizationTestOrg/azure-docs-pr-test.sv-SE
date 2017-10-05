---
title: "Hur du använder Power BI Embedded med REST | Microsoft Docs"
description: "Lär dig hur du använder Power BI Embedded med REST "
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 8bcef780-cca0-4f30-9a9b-9daa1a7ae865
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 02/06/2017
ms.author: asaxton
ms.openlocfilehash: 31624b9d15772a4f08cf013ac713b3aa636acfca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-power-bi-embedded-with-rest"></a>Hur du använder Power BI Embedded med REST

## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI Embedded: Vad det är och vad det är för

En översikt över Power BI Embedded beskrivs i officiellt [Power BI Embedded plats](https://azure.microsoft.com/services/power-bi-embedded/), men låt oss ta en titt på innan vi går in information om hur du använder med övriga.

Det är ganska enkel verkligen. Du kanske vill använda dynamiska datavisualiseringar av [Power BI](https://powerbi.microsoft.com) i ditt eget program.

De flesta anpassade program behöver för att leverera data för sina kunder, inte nödvändigtvis användare i den egna organisationen. Till exempel om du levererar vi en tjänst för både företag A och företag B, bör användare i företaget A bara se data för sina egna företag A. Det vill säga krävs flera innehavare för leverans.

Det anpassa programmet kan också att erbjuda sin egen autentiseringsmetoder för formulärautentisering, grundläggande autentisering, t.ex.. Sedan måste inbäddning lösningen samarbeta med den här befintliga autentiseringsmetoder på ett säkert sätt. Det är också nödvändigt om användarna ska kunna använda dessa ISV-program utan extra inköp eller licensieringen av en Power BI-prenumeration.

 **Power BI Embedded** är utformad för exakt dessa typer av scenarier. Därför nu när vi har att snabb introduktion undan ska vi gå in viss information

Du kan använda .NET \(C#) eller Node.js SDK enkelt kan skapa ditt program med Power BI Embedded. Men i den här artikeln förklarar vi om HTTP-flöde \(inklusive AuthN) Power BI utan SDK: er. Förstå detta flöde, du kan skapa ditt program **med alla programmeringsspråk**, och du kan förstå djupt huvudsakligen Power BI Embedded.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Skapa Power BI-arbetsytesamling och få åtkomst till nyckeln \(etablering)

Power BI Embedded är en Azure-tjänster. Endast ISV: er som använder Azure Portal debiteras för användning avgifter \(per varje timme användarsession), och användare som visar rapporten inte debiteras eller även kräva en Azure-prenumeration.
Innan du startar våra programutveckling kan vi skapa den **Power BI-arbetsytesamling** med hjälp av Azure Portal.

Vi kan lägga till flera arbetsytor i varje arbetsytesamling varje arbetsyta i Power BI Embedded är arbetsytan för varje kund (klient). Åtkomst till samma nyckel används i varje arbetsytesamling. I kraft arbetsytan samlingen är säkerhetsgränsen för Power BI Embedded.

![](media/power-bi-embedded-iframe/create-workspace.png)

När vi har skapat arbetsytesamling, kopiera åtkomstnyckeln från Azure-portalen.

![](media/power-bi-embedded-iframe/copy-access-key.png)

> [!NOTE]
> Vi kan också etablera arbetsytan samlingen och hämta åtkomstnyckel för via REST API. Läs mer i [Power BI Resource Provider API: er](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Skapa .PBX-fil med Power BI Desktop

Därefter måste skapa vi dataanslutningen och rapporter som ska bäddas in.
För den här uppgiften finns det ingen programmering eller kod. Vi använder bara Power BI Desktop.
Vi kommer inte gå igenom informationen om hur du använder Power BI Desktop i den här artikeln. Om du vill ha några hjälp här, se [komma igång med Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). I vårt exempel vi bara använder den [Retail analys exempel](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media/power-bi-embedded-iframe/power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Skapa en Power BI-arbetsyta

Nu när etableringen är allt klart nu sätter vi igång skapar en kund arbetsytan i arbetsytesamling via REST API: er. I följande HTTP POST begäran (REST) är att skapa den nya arbetsytan i vår befintliga arbetsytesamling. Det här är den [POST arbetsytan API](https://msdn.microsoft.com/library/azure/mt711503.aspx). I vårt exempel samlingsnamn arbetsytan är **mypbiapp**. Vi anger snabbtangent, som vi tidigare kopieras som **AppKey**. Det är mycket enkelt autentisering!

**HTTP-begäran**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-svar**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Den returnerade **workspaceId** används för följande efterföljande API-anrop. Appen måste behålla det här värdet.

## <a name="import-pbix-file-into-the-workspace"></a>Importera .PBX-fil till arbetsytan

Varje rapport i en arbetsyta som motsvarar en enskild Power BI Desktop-fil med en datamängd \(inklusive inställningar för datakälla). Vi kan importera våra .PBX-fil till arbetsytan som visas i koden nedan. Som du ser överföra vi av .PBX-fil med hjälp av MIME multipart i HTTP-binärfil.

Uri-fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** är workspaceId och frågeparameter **datasetDisplayName** är att skapa dataset-namnet. Skapade datamängden innehåller alla data som är relaterade artefakter i .pbix filen så som importerade data, pekaren till datakällan, osv.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Denna import uppgift kan köras på ett tag. När du är färdig be vårt program Aktivitetsstatusen använder import-id. Import-id i vårt exempel är **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Följande frågar status med import-id:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Om aktiviteten inte är klar, kan HTTP-svaret vara så här:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Om aktiviteten är slutförd, kan HTTP-svaret vara mer så här:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Anslutningen för datakällan \(och flera innehavare av data)

Nästan alla artefakter i .PBX-fil har importerats till vår arbetsytan är autentiseringsuppgifterna för datakällorna inte. Som ett resultat när du använder **DirectQuery-läge**, den inbäddade rapporten kan inte visas korrekt. Men när du använder **importläge**, vi kan visa rapporten med befintliga data som importeras. I sådana fall måste vi ange autentiseringsuppgifter med följande steg via REST-anrop.

Vi måste först hämta gateway-datakällan. Vi vet datauppsättningen **id** den tidigare returnerade ID: n.

**HTTP-begäran**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-svar**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Med returnerade gateway-id och datasource id \(finns i den tidigare **gatewayId** och **id** i det returnerade resultatet), kan vi ändra autentiseringsuppgifterna för datakällan på följande sätt:

**HTTP-begäran**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-svar**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

Vi kan också ange olika anslutningssträngen för varje arbetsyta med hjälp av REST-API i produktionen. \(engångsfaktorautentisering, vi separera databasen för varje kunder.)

Följande ändrar anslutningssträngen för datakällan via REST.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Eller, kan vi använda säkerhet på radnivå i Power BI Embedded och vi kan separera data för varje användare i en rapport. Vi kan as a result, etablera varje rapport med samma .pbix \(UI, etc.) och olika datakällor.

> [!NOTE]
> Om du använder **importläge** i stället för **DirectQuery-läge**, det går inte att uppdatera modeller via API. Och lokala datakällor till Power BI gateway stöds ännu inte i Power BI Embedded. Men du verkligen vill hålla ett öga på den [Power BI-blogg](https://powerbi.microsoft.com/blog/) för nyheter och nyheter i framtida versioner.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Autentisering och värd (inbäddning) rapporter i vår webbsida

I tidigare REST-API kan vi använda snabbtangent **AppKey** sig själv som authorization-huvud. Eftersom dessa anrop kan hanteras på backend-servern är säker.

Men när vi bädda in rapporten i vår webbsida den här typen av säkerhetsinformation skulle hanteras med hjälp av JavaScript \(klientdel). Sedan måste huvudvärde auktorisering skyddas. Om vår åtkomstnyckel identifieras av en obehörig användare eller skadlig kod, anropa de eventuella åtgärder med hjälp av den här nyckeln.

När vi bädda in rapporten i vår webbsida vi använda den beräknade token i stället åtkomstnyckeln **AppKey**. Appen måste skapa OAuth Json Web Token \(JWT) som består av anspråk och beräknade digital signatur. Som på bilden nedan är den här OAuth JWT punkt-avgränsad kodad sträng token.

![](media/power-bi-embedded-iframe/oauth-jwt.png)

Vi måste först förbereda indatavärdet som är signerat senare. Det här värdet är base64-kodade url (rfc4648)-strängen till följande json och dessa avgränsas med en punkt \(.) tecken. Senare, förklarar vi hur du hämtar id för rapporten.

> [!NOTE]
> Om vi vill använda raden nivå säkerhet (RLS) med Power BI Embedded, vi måste även ange **användarnamn** och **roller** i anspråken.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Därefter måste vi skapa base64-kodad sträng av HMAC \(signaturen) med SHA256-algoritmen. Det här signerade indatavärdet är föregående sträng.

Senaste, måste kombineras värdet och signatur Indatasträngen med period \(.) tecken. Den färdiga strängen är apptoken för inbäddning av rapporten. Även om app-token har identifierats av en obehörig användare kan de få den ursprungliga åtkomstnyckeln. Den här appen token upphör att gälla snabbt.

Här är ett PHP-exempel för de här stegen:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Slutligen bädda in rapporten i webbsidan

För inbäddning våra rapporten måste vi få embed url och rapporten **id** med hjälp av följande REST-API.

**HTTP-begäran**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-svar**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Vi kan bädda in rapporten i vår webbapp med föregående token i appen.
Om vi tittar på nästa exempelkoden är den tidigare delen samma som i föregående exempel. I den senare delen i det här exemplet visar den **embedUrl** \(tidigare resultatet) i iframe, och bokföring app-token i iframe.

> [!NOTE]
> Du behöver ändra rapporten ID-värde till en egen. Dessutom på grund av ett fel i vårt system för innehållshantering läses iframe-tagg i kodexemplet bokstavligt. Ta bort capped texten från taggen om du kopierar och klistrar in den här exempelkoden.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

Och här är vår resultat:

![](media/power-bi-embedded-iframe/view-report.png)

För tillfället visar Power BI Embedded bara rapporten i iframe. Men hålla ett öga på den [Power BI-bloggen](https://powerbi.microsoft.com/blog/). Framtida förbättringar kan använda nya klientsidan API: er som kommer Låt oss skicka information till iframe samt hämta information om. Spännande material!

## <a name="see-also"></a>Se även
* [Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)

Fler frågor? [Försök med Power BI Community](http://community.powerbi.com/)

