---
title: aaaAuthenticating och auktorisera med Power BI Embedded
description: Autentisering och auktorisering med Power BI Embedded
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 483ca0499e8d03584e1151d3d278c0179d4b8fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Autentisering och auktorisering med Power BI Embedded

hello Power BI Embedded tjänsten använder **nycklar** och **Apptoken** för autentisering och auktorisering, i stället för att explicit slutanvändarens autentisering. I den här modellen hanterar programmet autentisering och auktorisering för dina slutanvändare. Vid behov, din app skapar och skickar hello Apptoken som talar om vår tjänst toorender hello begärda rapporten. Den här designen kräver din app toouse Azure Active Directory för autentisering och auktorisering, även om du fortfarande kan.

## <a name="two-ways-tooauthenticate"></a>Två sätt tooauthenticate

**Nyckeln** -du kan använda nycklar för alla Power BI Embedded REST API-anrop. hello nycklar finns i hello **Azure-portalen** genom att klicka på **alla inställningar** och sedan **åtkomstnycklar**. Behandla alltid din nyckel som om det vore ett lösenord. Dessa nycklar har behörighet toomake REST API: er anropa för en viss arbetsytesamling.

toouse en nyckel på ett REST-anrop, Lägg till hello följande authorization-huvud:            

    Authorization: AppKey {your key}

**Apptoken** -apptoken som används för inbäddning begäranden. De är utformad toobe kör klientsidan så att de är begränsade tooa enkel rapport och det är bästa praxis tooset en förfallotid.

Apptoken är en JWT (JSON Web Token) som är signerat av någon av dina nycklar.

App-token kan innehålla hello följande krav:

| Begär | Beskrivning |
| --- | --- |
| **ver** |hello version av hello app-token. 0.2.0 är hello aktuella versionen. |
| **eller** |hello avsedda mottagaren av hello-token. Power BI Embedded användas: ”https://analysis.windows.net/powerbi/api”. |
| **ISS** |En sträng som anger hello-program som har utfärdat hello-token. |
| **typ** |hello typ av apptoken som håller på att skapas. Den aktuella hello stöds endast typen är **bädda in**. |
| **nätverkskonfiguration** |Arbetsytan samling namn hello token utfärdas för. |
| **WID** |Arbetsytan ID hello token utfärdas för. |
| **RID** |ID hello token utfärdas för en rapport. |
| **användarnamnet** (valfritt) |Används med RLS. är detta en sträng som kan hjälpa dig att identifiera hello användare när du använder RLS regler. |
| **roller** (valfritt) |En sträng som innehåller hello roller tooselect när du använder regler för säkerhet på radnivå. Om skicka fler än en roll ska de skickas som en förekomster av textsträngen-matris. |
| **SCP** (valfritt) |En sträng som innehåller hello behörigheter scope. Om skicka fler än en roll ska de skickas som en förekomster av textsträngen-matris. |
| **EXP** (valfritt) |Anger hello tidpunkt i vilka hello token upphör att gälla. Dessa ska skickas i som Unix tidsstämplar. |
| **NBF** (valfritt) |Anger hello tidpunkt i vilka hello startar token är giltig. Dessa ska skickas i som Unix tidsstämplar. |

Ett exempel apptoken ser ut så här:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

När avkodas, ser den ut ungefär så här:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

Det finns metoder i hello SDK: er som gör det enklare att skapa apptokens. Till exempel för .NET titta på hello [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) klass och hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoder.

För hello .NET SDK, kan du läsa för[scope](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).

## <a name="scopes"></a>Scope

När du använder Embed token kan kanske du vill toorestrict användning av hello-resurser som du ger åtkomst till. Därför kan du generera en token med begränsade behörigheter.

hello följande är hello tillgängliga scope för Power BI Embedded.

|Omfång|Beskrivning|
|---|---|
|Dataset.Read|Ger behörighet tooread hello angetts dataset.|
|Dataset.Write|Ger behörighet toowrite toohello angetts dataset.|
|Dataset.ReadWrite|Ger behörighet tooread och skriva toohello angiven dataset.|
|Report.Read|Ger behörighet tooview hello angivna rapporten.|
|Report.ReadWrite|Ger behörighet tooview och redigera hello angivna rapporten.|
|Workspace.Report.Create|Ger behörighet toocreate en ny rapport i hello angivna arbetsytan.|
|Workspace.Report.Copy|Ger behörighet tooclone en befintlig rapport i hello angivna arbetsytan.|

Du kan ange flera scope med ett blanksteg mellan hello scope som hello nedan.

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Nödvändiga anspråk - scope**

SCP: {scopesClaim} scopesClaim kan vara en sträng eller en matris med strängar som påpekas hello behörigheter tooworkspace resurser (rapport, Dataset, etc.)

En avkodade token med scope som definierats, skulle se ut ungefär toohello följande.

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a>Åtgärder och scope

|Åtgärd|Målresurs|Token behörigheter|
|---|---|---|
|Skapa en ny rapport utifrån en datamängd för (i minnet).|Datauppsättning|Dataset.Read|
|Skapa en ny rapport utifrån en datamängd för (i minnet) och spara hello rapporten.|Datauppsättning|* Dataset.Read<br>* Workspace.Report.Create|
|Visa och utforska/Redigera (i minnet) en befintlig rapport. Report.Read innebär Dataset.Read. Detta tillåter inte behörigheter toosave redigeringar.|Rapport|Report.Read|
|Redigera och spara en befintlig rapport.|Rapport|Report.ReadWrite|
|Spara en kopia av en rapport (Spara som).|Rapport|* Report.Read<br>* Workspace.Report.Copy|

## <a name="heres-how-hello-flow-works"></a>Här är hur hello flödet fungerar
1. Kopiera hello API nycklar tooyour-program. Du kan hämta hello nycklar i **Azure Portal**.
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. Token Assert ett anspråk och har en förfallotid.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. Token hämtar signeras med ett API-åtkomstnycklar.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. Användaren begär tooview en rapport.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. Token verifieras med en API-åtkomstnycklar.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. Power BI Embedded skickar en rapport toouser.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

Efter **Power BI Embedded** skickar en toohello användare, hello användare kan visa hello rapport i din egen app. Till exempel om du har importerat hello [analysera försäljning Data PBIX exempel](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), hello exempelwebbapp skulle se ut så här:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Se även

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Komma igång med Microsoft Power BI Embedded exemplet](power-bi-embedded-get-started-sample.md)  
[Vanliga scenarier för Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Kom igång med Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[PowerBI CSharp Git Repo](https://github.com/Microsoft/PowerBI-CSharp)  
Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)

