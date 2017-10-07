---
title: "aaaIssuer namn och nyckel utfärdaren i BizTalk-tjänst | Microsoft Docs"
description: "Lär dig hur tooretrieve Utfärdares namn och utfärdaren nyckeln för Service Bus eller Access Control (ACS) i BizTalk-tjänst. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: cc84c2820724ae3e7fc7c40ddbcd83a169add911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk Services: Utfärdarens namn och nyckel

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk-tjänster använder hello Service Bus Utfärdarens namn och utfärdare, och hello Access Control Utfärdarens namn och utfärdaren nyckeln. Närmare bestämt:

| Aktivitet | Vilka Utfärdarens namn och en utfärdare nyckel |
| --- | --- |
| Distribuera programmet från Visual Studio |Access Control Utfärdarens namn och en utfärdare nyckel |
| Konfigurera hello Azure BizTalk-Services-portalen |Access Control Utfärdarens namn och en utfärdare nyckel |
| Skapa LOB-reläer med hello BizTalk Adapter tjänster i Visual Studio |Service Bus Utfärdarens namn och en utfärdare nyckel |

Det här avsnittet visar hello steg tooretrieve hello Utfärdarens namn och nyckeln för utfärdaren. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Access Control Utfärdarens namn och en utfärdare nyckel
hello Access Control Utfärdarens namn och utfärdaren nyckeln används av hello följande:

* Ditt program för Azure BizTalk-tjänst som skapats i Visual Studio: toosuccessfully distribuera BizTalk Service programmet i Visual Studio tooAzure, anger du hello Access Control Utfärdarens namn och nyckeln för utfärdaren. 
* hello Azure BizTalk-Services-portalen: när du skapar en BizTalk Service och öppna hello BizTalk-Services-portalen, din Access Control Utfärdarens namn och nyckeln för utfärdaren registreras automatiskt för dina distributioner med hello samma värden för åtkomstkontroll.

### <a name="get-hello-access-control-issuer-name-and-issuer-key"></a>Hämta hello Access Control Utfärdarens namn och en utfärdare nyckel

toouse ACS för autentisering och get Hej Utfärdarens namn och nyckel för utfärdaren, hello övergripande stegen innefattar:

1. Installera hello [Azure Powershell-cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Lägg till ditt Azure-konto:`Add-AzureAccount`
3. Returnera namnet på din prenumeration:`get-azuresubscription`
4. Välj din prenumeration:`select-azuresubscription <name of your subscription>` 
5. Skapa ett nytt namnområde:`new-azuresbnamespace <name for hello service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`

    Exempel:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`
      
5. När hello nya ACS-namnområdet har skapats (vilket kan ta flera minuter), anges hello Utfärdarens namn och nyckeln för utfärdaren värdena i anslutningssträngen för hello: 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

toosummarize:  
Utfärdarens namn = SharedSecretIssuer  
Utfärdaren nyckel = SharedSecretKey

Mer om hello [ny AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) cmdlet. 

## <a name="service-bus-issuer-name-and-issuer-key"></a>Service Bus Utfärdarens namn och en utfärdare nyckel
Service Bus Utfärdarens namn och nyckeln för utfärdaren som används av BizTalk-tjänst för nätverkskortet. I projektet BizTalk-tjänst i Visual Studio kan du använda hello BizTalk Adapter Services tooconnect tooan lokalt branschspecifika (LOB) system. tooconnect, du skapar hello LOB Relay och ange din information för LOB-system. När du gör detta måste ange du också hello Service Bus Utfärdarens namn och nyckeln för utfärdaren.

### <a name="tooretrieve-hello-service-bus-issuer-name-and-issuer-key"></a>tooretrieve hello Service Bus Utfärdarens namn och en utfärdare nyckel
1. Logga in toohello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello vänstra navigeringsfönstret och välj **Service Bus**.
3. Välj namnområdet. Markera i Aktivitetsfältet hello **anslutningsinformationen**. Detta visar hello **standard utfärdaren** (Utfärdarens namn) och **standard nyckeln** (utfärdaren nyckel). Deras värden kan kopieras.  

toosummarize:  
Utfärdarens namn = standard utfärdare  
Utfärdaren nyckel = Standardnyckeln

## <a name="next"></a>Nästa
Avsnitt om ytterligare Azure BizTalk-tjänst:

* [Installera hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Självstudier: Azure BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Hur jag börja använda hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Se även
* [Så här: använda ACS hanteringstjänsten tooConfigure Tjänsteidentiteter](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Etablering av statusdiagram](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Säkerhetskopiering och återställning](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Begränsning](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

