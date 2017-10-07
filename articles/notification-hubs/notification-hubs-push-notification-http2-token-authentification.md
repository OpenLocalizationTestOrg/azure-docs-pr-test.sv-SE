---
title: "(HTTP/2) aaaToken-baserad autentisering för APNS i Azure Notification Hubs | Microsoft Docs"
description: "Det här avsnittet beskrivs hur tooleverage hello nya tokenautentisering för APNS"
services: notification-hubs
documentationcenter: .net
author: kpiteira
manager: erikre
editor: 
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 05/17/2017
ms.author: kapiteir
ms.openlocfilehash: 3353d7f16033ce0b68edec9ee9aeb98f47faa1fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="token-based-http2-authentication-for-apns"></a>Autentisering för tokenbaserad (HTTP/2) för APNS
## <a name="overview"></a>Översikt
Den här artikeln beskrivs hur toouse hello nya APN HTTP/2-protokollet med token baserade autentisering.

hello viktiga fördelar med att använda nya hello-protokollet är:
-   Token generering är relativt besväret ledigt (jämfört med toocertificates)
-   Ingen mer förfallodatum – har du kontroll över dina autentiseringstoken och deras återkallelse
-   Nyttolaster kan nu vara in too4 KB
- Synkron feedback
-   Du är på Apples senaste protokollet – certifikat fortfarande använda hello binära protokoll som har markerats för utfasning

Med den här nya funktionen kan du göra i två steg i ett par minuter:
1.  Hämta hello nödvändig information från hello på Apple Developer-kontoportalen
2.  Konfigurera din meddelandehubb med hello ny information

Notification Hubs är nu alla set toouse hello ny autentisering med APNS. 

Observera att om du migrerat från med hjälp av autentiseringsuppgifter för certifikat för APNS:
- hello token egenskaper skriver över ditt certifikat i vårt system
- men programmet fortsätter tooreceive meddelanden sömlöst.

## <a name="obtaining-authentication-information-from-apple"></a>Hämta autentiseringsuppgifter från Apple
tooenable tokenbaserad autentisering, behöver du hello följande egenskaper från Apple Developer-konto:
### <a name="key-identifier"></a>Nyckel-ID
hello nyckelidentifierare kan hämtas från hello ”nycklar” sida i Apple Developer-konto

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>Program-ID och programnamn
hello programnamnet är tillgängliga via hello App-ID: N sida i hello utvecklarkonto. 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

hello identifierare är tillgänglig via hello medlemskap informationssidan i hello utvecklarkonto.
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a>Token för autentisering
hello autentiseringstoken hämtas när du generera en token för ditt program. Information om hur toogenerate detta token, som finns för[Apples utvecklardokumentation](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).

## <a name="configuring-your-notification-hub-toouse-token-based-authentication"></a>Konfigurera din notification hub toouse tokenbaserad autentisering
### <a name="configure-via-hello-azure-portal"></a>Konfigurera via hello Azure-portalen
tooenable token autentisering i hello portal, logga in toohello Azure-portalen och gå tooyour Notification Hub > Notification Services > APN-panelen. 

Det finns en ny egenskap – *autentiseringsläge*. När Token kan du tooupdate din hubb med alla hello relevanta token egenskaper.

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- Ange hello egenskaper som du hämtade från Apple developer-konto 
- Välj din programläge (produktion eller Sandbox) 
- Klicka på Spara tooupdate APNS-autentiseringsuppgifterna. 

### <a name="configure-via-management-api-rest"></a>Konfigurera via Management API (REST)

Du kan använda vår [management API: er](https://msdn.microsoft.com/library/azure/dn495827.aspx) tooupdate din notification hub toouse token-baserad autentisering.
Beroende på om är hello-programmet som du konfigurerar en Sandbox eller produktion app (anges i Apple Developer-konto), använder du någon av hello motsvarande slutpunkter:

- Sandboxslutpunkt: [https://api.development.push.apple.com:443-3-enhet](https://api.development.push.apple.com:443/3/device)
- Produktion slutpunkt: [https://api.push.apple.com:443-3-enhet](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> Tokenbaserad autentisering kräver en API-version: **2017-04 eller senare**.
> 
> 

Här är ett exempel på en PUT-begäran tooupdate en hubb med tokenbaserad autentisering:


        PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
          "Properties": {
            "ApnsCredential": {
              "Properties": {
                "KeyId": "<Your Key Id>",
                "Token": "<Your Authentication Token>",
                "AppName": "<Your Application Name>",
                "AppId": "<Your Application Id>",
                "Endpoint":"<Sandbox/Production Endpoint>"
              }
            }
          }
        

### <a name="configure-via-hello-net-sdk"></a>Konfigurera via hello .NET SDK
Du kan konfigurera din hubb toouse token autentisering med hjälp av vår [senaste klient-SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8). 

Här är ett kodexempel illustrerar hello korrekt syntax:


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH tooYOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-toousing-certificate-based-authentication"></a>Återför toousing certifikatbaserad autentisering
Du kan återgå vid någon tidpunkt toousing certifikatbaserad autentisering med föregående metod och skicka hello certifikat i stället för hello token egenskaper. Att åtgärden skriver över hello tidigare lagrade autentiseringsuppgifter.
