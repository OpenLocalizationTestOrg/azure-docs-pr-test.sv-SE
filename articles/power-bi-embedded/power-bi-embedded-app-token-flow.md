---
title: Autentisering och auktorisering med Power BI Embedded
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
ms.openlocfilehash: 93027f0f5467e0b21c47bbcbc84c67cdd50b26b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Autentisering och auktorisering med Power BI Embedded

Power BI Embedded tjänsten använder **nycklar** och **Apptoken** för autentisering och auktorisering, i stället för att explicit slutanvändarens autentisering. I den här modellen hanterar programmet autentisering och auktorisering för dina slutanvändare. Vid behov, din app skapar och skickar Apptoken som talar om vår tjänst för att återge den begärda rapporten. Den här designen kräver inte din app att använda Azure Active Directory för autentisering och auktorisering, även om du fortfarande kan.

## <a name="two-ways-to-authenticate"></a>Två sätt att autentisera

**Nyckeln** -du kan använda nycklar för alla Power BI Embedded REST API-anrop. Nycklarna kan hittas i den **Azure-portalen** genom att klicka på **alla inställningar** och sedan **åtkomstnycklar**. Behandla alltid din nyckel som om det vore ett lösenord. Dessa nycklar har behörighet att göra REST API: er anropa för en viss arbetsytesamling.

Om du vill använda en nyckel på ett REST-anrop, lägger du till följande authorization-huvud:            

    Authorization: AppKey {your key}

**Apptoken** -apptoken som används för inbäddning begäranden. De har utformats för att köra klientsidan, så att de är begränsad till en enstaka rapport och det är bäst att ange en förfallotid.

Apptoken är en JWT (JSON Web Token) som är signerat av någon av dina nycklar.

App-token kan innehålla följande krav:

| Begär | Beskrivning |
| --- | --- |
| **ver** |Versionen av app-token. 0.2.0 är den aktuella versionen. |
| **eller** |Den avsedda mottagaren av token. Power BI Embedded användas: ”https://analysis.windows.net/powerbi/api”. |
| **ISS** |En sträng som anger programmet som utfärdade token. |
| **typ** |Typen av apptoken som håller på att skapas. Aktuella den enda typ som stöds är **bädda in**. |
| **nätverkskonfiguration** |Arbetsytan samlingsnamn token utfärdas för. |
| **WID** |Arbetsyte-ID token utfärdas för. |
| **RID** |ID token utfärdas för en rapport. |
| **användarnamnet** (valfritt) |Används med RLS. är detta en sträng som kan hjälpa dig att identifiera användaren när du använder RLS regler. |
| **roller** (valfritt) |En sträng som innehåller rollerna kan välja vid tillämpning av säkerhet på radnivå. Om skicka fler än en roll ska de skickas som en förekomster av textsträngen-matris. |
| **SCP** (valfritt) |En sträng som innehåller behörigheter scope. Om skicka fler än en roll ska de skickas som en förekomster av textsträngen-matris. |
| **EXP** (valfritt) |Anger den tidpunkt då token upphör att gälla. Dessa ska skickas i som Unix tidsstämplar. |
| **NBF** (valfritt) |Anger den tidpunkt som startar token är giltig. Dessa ska skickas i som Unix tidsstämplar. |

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

Det finns metoder i SDK: er som gör det enklare att skapa apptokens. Till exempel för .NET du titta på den [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) klass och [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoder.

För .NET-SDK kan du gå till [scope](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).

## <a name="scopes"></a>Scope

När du använder Embed token, kanske du vill begränsa användningen av du ger åtkomst till resurser. Därför kan du generera en token med begränsade behörigheter.

Följande är tillgängliga scope för Power BI Embedded.

|Omfång|Beskrivning|
|---|---|
|Dataset.Read|Ger behörighet att läsa den angivna datamängden.|
|Dataset.Write|Ger behörighet att skriva till den angivna datamängden.|
|Dataset.ReadWrite|Ger behörighet att läsa och skriva till datamängden som är angiven.|
|Report.Read|Ger behörighet att visa den angivna rapporten.|
|Report.ReadWrite|Ger behörighet att visa och redigera den angivna rapporten.|
|Workspace.Report.Create|Ger behörighet att skapa en ny rapport på den angivna arbetsytan.|
|Workspace.Report.Copy|Ger behörighet att klona en befintlig rapport på den angivna arbetsytan.|

Du kan ange flera scope med ett blanksteg mellan scope som liknar följande.

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Nödvändiga anspråk - scope**

SCP: {scopesClaim} scopesClaim kan vara en sträng eller en matris med strängar som påpekas tillåtna behörigheter till arbetsytan resurser (rapport, Dataset, etc.)

Avkodade token, med scope som definierats, skulle se ut ungefär så här.

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
|Skapa en ny rapport utifrån en datamängd för (i minnet) och spara rapporten.|Datauppsättning|* Dataset.Read<br>* Workspace.Report.Create|
|Visa och utforska/Redigera (i minnet) en befintlig rapport. Report.Read innebär Dataset.Read. Detta tillåter inte behörighet för att spara ändringarna.|Rapport|Report.Read|
|Redigera och spara en befintlig rapport.|Rapport|Report.ReadWrite|
|Spara en kopia av en rapport (Spara som).|Rapport|* Report.Read<br>* Workspace.Report.Copy|

## <a name="heres-how-the-flow-works"></a>Här är hur flödet fungerar
1. Kopiera API-nycklar till ditt program. Du kan hämta nycklarna i **Azure Portal**.
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. Token Assert ett anspråk och har en förfallotid.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. Token hämtar signeras med ett API-åtkomstnycklar.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. Användarförfrågningar om att visa en rapport.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. Token verifieras med en API-åtkomstnycklar.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. Power BI Embedded skickar en rapport till användaren.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

Efter **Power BI Embedded** skickar en rapport till användaren, användaren kan visa rapporten i din egen app. Till exempel om du har importerat den [analysera försäljning Data PBIX exempel](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), exempelwebbapp skulle se ut så här:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Se även

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Komma igång med Microsoft Power BI Embedded exemplet](power-bi-embedded-get-started-sample.md)  
[Vanliga scenarier för Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)  
[Kom igång med Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[PowerBI CSharp Git Repo](https://github.com/Microsoft/PowerBI-CSharp)  
Fler frågor? [Försök med Power BI Community](http://community.powerbi.com/)

