---
title: "aaaCommunicate med valfri slutpunkt över HTTP - Azure Logic Apps | Microsoft Docs"
description: "Skapa logikappar som kan kommunicera med valfri slutpunkt över HTTP"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: 9793601839437a2b880bdb81e15881270cacc963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http-action"></a>Kom igång med hello HTTP-åtgärd

Med hello HTTP-åtgärd, kan du utöka arbetsflöden för din organisation och kommunicera tooany endpoint via HTTP.

Du kan:

* Skapa logik app arbetsflöden som aktiverar (utlösare) när en webbplats som du hanterar kraschar.
* Kommunicera tooany endpoint via HTTP tooextend dina arbetsflöden till andra tjänster.

tooget igång med hello HTTP-åtgärden i en logikapp finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-hello-http-trigger"></a>Använda hello HTTP-utlösare
En utlösare är en händelse som kan använda toostart hello arbetsflöde som har definierats i en logikapp. [Mer information om utlösare](connectors-overview.md).

Här är en Exempelsekvens av hur tooset in hello HTTP utlösare i hello logik App Designer.

1. Lägg till hello HTTP-utlösaren i din logikapp.
2. Fyll i hello parametrar för hello HTTP-slutpunkt som du vill toopoll.
3. Ändra hello upprepningsintervallet på hur ofta den ska avsöka.

   Hej logikapp utlöses nu med allt innehåll som returneras vid varje kontroll.

   ![HTTP-utlösare](./media/connectors-native-http/using-trigger.png)

### <a name="how-hello-http-trigger-works"></a>Så här fungerar hello http-utlösare

hello HTTP-utlösaren skickar en anropet tooHTTP slutpunkt med återkommande intervall. Som standard gör HTTP-svarskoden som är lägre än 300 en logik app toorun. toospecify om hello logikapp ska utlösas du kan redigera hello logikapp i kodvy och Lägg till ett villkor som utvärderar efter hello http-anrop. Här är ett exempel på en HTTP-utlösare som utlöses när hello returnerade statuskoden är större än eller lika med för`400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Fullständig information om parametrar för hello HTTP-utlösaren finns på [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-hello-http-action"></a>Använd hello HTTP-åtgärd

En åtgärd är en åtgärd som utförs av hello arbetsflöde som har definierats i en logikapp. 
[Mer information om åtgärder](connectors-overview.md).

1. Välj **nytt steg** > **lägga till en åtgärd**.
3. Skriv i sökrutan för hello åtgärd **http** toolist hello HTTP-åtgärder.
   
    ![Välj hello HTTP-åtgärd](./media/connectors-native-http/using-action-1.png)

4. Lägg till eventuella parametrar för hello HTTP-anrop.
   
    ![Fullständig hello HTTP-åtgärd](./media/connectors-native-http/using-action-2.png)

5. På hello designer verktygsfältet **spara**. Din logikapp sparas och publiceras (aktiverad) på hello samtidigt.

## <a name="http-trigger"></a>HTTP-utlösare
Här följer hello information för hello utlösare som har stöd för den här anslutningen. hello HTTP-anslutningen har en utlösare.

| Utlösare | Beskrivning |
| --- | --- |
| HTTP |Gör ett HTTP-anrop och returnerar hello svar innehåll. |

## <a name="http-action"></a>HTTP-åtgärd
Här följer hello information för hello-åtgärd som har stöd för den här anslutningen. hello HTTP-anslutningen har en möjlig åtgärd.

| Åtgärd | Beskrivning |
| --- | --- |
| HTTP |Gör ett HTTP-anrop och returnerar hello svar innehåll. |

## <a name="http-details"></a>HTTP-information
hello följande tabeller beskrivs hello nödvändiga och valfria indatafält för hello åtgärd och hello motsvarande utdata information som är associerade med åtgärden hello.

#### <a name="http-request"></a>HTTP-begäran
hello följande är inmatningsfält för hello åtgärden, vilket gör en utgående HTTP-begäran.
A * innebär att det är ett obligatoriskt fält.

| Visningsnamn | Egenskapsnamn | Beskrivning |
| --- | --- | --- |
| Metoden * |Metoden |hello HTTP-verbet toouse |
| URI * |URI: N |hello URI för hello HTTP-begäran |
| Rubriker |Rubriker |Ett JSON-objekt i HTTP-huvuden tooinclude |
| Innehåll |Brödtext |hello text för HTTP-begäran |
| Autentisering |Autentisering |Mer information i hello [autentisering](#authentication) avsnitt |

<br>

#### <a name="output-details"></a>Information för utdata
hello nedan visas utdata information för hello HTTP-svar.

| Egenskapsnamn | Datatyp | Beskrivning |
| --- | --- | --- |
| Rubriker |Objektet |Svarsrubriker |
| Innehåll |Objektet |Objektet Response |
| Statuskod |int |HTTP-statuskod |

## <a name="authentication"></a>Autentisering
hello Logic Apps-funktionen kan du toouse olika typer av autentisering mot HTTP-slutpunkter. Du kan använda den här autentiseringen med hello **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, och  **[HTTP Webhook](connectors-native-webhook.md)**  kopplingar. hello följande typer av autentisering kan konfigureras:

* [Grundläggande autentisering](#basic-authentication)
* [Autentisering av klientcertifikat](#client-certificate-authentication)
* [Azure Active Directory (AD Azure) OAuth-autentisering](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Grundläggande autentisering

hello efter autentiseringsobjekt krävs för grundläggande autentisering.
A * innebär att det är ett obligatoriskt fält.

| Egenskapsnamn | Datatyp | Beskrivning |
| --- | --- | --- |
| Typen * |typ |Typ av autentisering (måste vara `Basic` för grundläggande autentisering) |
| Användarnamn * |användarnamn |Användaren namnet tooauthenticate |
| Lösenord * |lösenord |Lösenord tooauthenticate |

> [!TIP]
> Om du vill toouse ett lösenord som inte kan hämtas från hello definition använder en `securestring` parametern och hello `@parameters()`  
>  [definition arbetsflödesfunktion](http://aka.ms/logicappdocs).

Exempel:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Autentisering med klientcertifikat

hello krävs följande autentiseringsobjekt för autentisering av klientcertifikat. A * innebär att det är ett obligatoriskt fält.

| Egenskapsnamn | Datatyp | Beskrivning |
| --- | --- | --- |
| Typen * |typ |Hej typ av autentisering (måste vara `ClientCertificate` för SSL-klientcertifikat) |
| PFX * |Pfx |hello Base64-kodad innehållet i hello Personal Information Exchange (PFX)-fil |
| Lösenord * |lösenord |hello lösenord tooaccess hello PFX-fil |

> [!TIP]
> toouse en parameter som inte är läsbara i hello definition när du har sparat hello logikappen som du kan använda en `securestring` parametern och hello `@parameters()`  
>  [definition arbetsflödesfunktion](http://aka.ms/logicappdocs).

Exempel:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD OAuth-autentisering
hello efter autentiseringsobjekt krävs för Azure AD OAuth-autentisering. A * innebär att det är ett obligatoriskt fält.

| Egenskapsnamn | Datatyp | Beskrivning |
| --- | --- | --- |
| Typen * |typ |Hej typ av autentisering (måste vara `ActiveDirectoryOAuth` för Azure AD OAuth) |
| Klient * |Klient |hello klient-ID för hello Azure AD-klient |
| Målgruppen * |målgrupp |hello-resurs som du begär godkännande toouse. Exempel: `https://management.core.windows.net/` |
| Klient -ID * |clientId |Hej klient-ID för hello Azure AD-program |
| Hemligt * |hemligt |hello hemligheten för hello-klient som begär hello-token |

> [!TIP]
> Du kan använda en `securestring` parametern och hello `@parameters()` [definition arbetsflödesfunktion](http://aka.ms/logicappdocs) toouse en parameter som inte är läsbara i hello definition när du har sparat.
> 
> 

Exempel:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Nästa steg
Prova nu hello plattform och [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Du kan utforska hello andra tillgängliga kopplingar i Logic Apps genom att titta på vår [API: er listan](apis-list.md).

