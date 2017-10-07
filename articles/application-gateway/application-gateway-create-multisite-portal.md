---
title: aaaHost flera platser med Azure Programgateway | Microsoft Docs
description: "Den här sidan finns instruktioner tooconfigure en befintlig Azure-program-gateway som värd för flera webbprogram på hello samma gateway med hello Azure-portalen."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 2172aa2c80720f6f1ab7dd91745b44654bcaee00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfigurera en befintlig Programgateway som värd för flera webbprogram

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

Värd för flera plats kan du toodeploy mer än ett webbprogram på hello samma Programgateway. Det är beroende av förekomsten av värdhuvudet i hello inkommande HTTP-begäran, toodetermine vilka lyssnare skulle ta emot trafik. hello lyssnare dirigerar sedan trafik tooappropriate serverdelspool som konfigurerats i hello regler definition av hello gateway. I SSL aktiverat webbprogram beroende Programgateway hello Server Servernamnsindikation (SNI)-tillägget toochoose hello rätt lyssnare för hello webbtrafik. Ett vanligt användningsområde för värd för flera plats är tooload begäranden för annan webbplats domäner toodifferent backend-serverpooler. På liknande sätt hello flera underdomäner i hello samma rotdomänen kan också finnas på samma Programgateway.

## <a name="scenario"></a>Scenario

I följande exempel hello, Programgateway betjäna trafik för contoso.com och fabrikam.com med två backend-server-adresspooler: contoso-serverpoolen och fabrikam-serverpoolen. Liknande installationsprogrammet kan vara används toohost underdomäner som app.contoso.com och blog.contoso.com.

![Multisite scenario][multisite]

## <a name="before-you-begin"></a>Innan du börjar

Det här scenariot lägger till stöd för flera platser tooan befintliga Programgateway. toocomplete det här scenariot kan en befintlig Programgateway måste toobe tillgängliga tooconfigure. Besök [skapar en Programgateway med hello portal](application-gateway-create-gateway-portal.md) toolearn hur toocreate en basic-Programgateway hello-portalen.

hello följande är hello steg behövs tooupdate hello Programgateway:

1. Skapa backend-pooler toouse för varje plats.
2. Skapa en lyssnare för varje plats Programgateway stöder.
3. Skapa regler toomap varje lyssnare med hello lämpliga serverdel.

## <a name="requirements"></a>Krav

* **Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar. hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP. FQDN kan också användas.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.
* **Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway. Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning). För flera platser aktiverade programmet gateway läggs värdnamn och SNI indikatorer till.
* **Regel:** hello regeln Binder hello lyssnare, hello backend-serverpoolen, och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare. Regler bearbetas i hello ordning och trafik dirigeras via hello första regeln som överensstämmer oavsett särskilda egenskaper. Till exempel om du har en regel med en grundläggande lyssnare och en regel med en flera platser lyssnare båda på samma port hello regel med hello måste hello flera platser lyssnare anges innan hello regeln med grundläggande hello-lyssnare för hello flera platser regeln toofunction som förväntades. 
* **Certifikat:** varje lyssnare kräver ett unikt certifikat, i det här exemplet skapas 2-lyssnare för flera platser. Två PFX-certifikat och hello lösenord för dem måste toobe skapas.

## <a name="create-back-end-pools-for-each-site"></a>Skapa backend-pooler för varje plats

En backend-poolen för varje plats att programmet gateway stöder krävs, i det här fallet 2 är att skapa en för contoso11.com och en för fabrikam11.com.

### <a name="step-1"></a>Steg 1

Navigera tooan befintliga Programgateway i hello Azure-portalen (https://portal.azure.com). Välj **serverdelspooler** och på **Lägg till**

![Lägg till serverdelspooler][7]

### <a name="step-2"></a>Steg 2

Fyll i hello för hello backend-adresspool **pool1**, lägger till hello IP-adresser eller FQDN för hello backend-servrar och på **OK**

![backend pool1 poolinställningarna][8]

### <a name="step-3"></a>Steg 3

På hello serverdelspooler bladet klickar du på **Lägg till** tooadd en ytterligare backend-adresspool **pool2**, lägger till hello IP-adresser eller FQDN för hello backend-servrar och på **OK**

![backend pool2 poolinställningarna][9]

## <a name="create-listeners-for-each-back-end"></a>Skapa lyssnare för varje serverdel

Programgateway är beroende av HTTP 1.1 värden huvuden toohost mer än en webbplats på hello samma offentliga IP-adress och port. grundläggande hello lyssnare har skapats i hello portal innehåller inte den här egenskapen.

### <a name="step-1"></a>Steg 1

Klicka på **lyssnare** hello befintliga Programgateway och klicka på **flera platser** tooadd hello första lyssnare.

![lyssnare översikt bladet][1]

### <a name="step-2"></a>Steg 2

Fyll i hello information för hello-lyssnaren. I det här exemplet SSL-avslutning har konfigurerats, skapa en ny frontend-port. Överför hello PFX-certifikat toobe används för SSL-avslutning. hello enda skillnaden på det här bladet jämfört med toohello standard grundläggande lyssnare bladet är hello värdnamn.

![lyssnare egenskapsbladet][2]

### <a name="step-3"></a>Steg 3

Klicka på **flera platser** och skapa en annan lyssnare enligt beskrivningen i hello föregående steg för hello andra platsen. Se till att toouse ett annat certifikat för andra hello-lyssnaren. hello enda skillnaden på det här bladet jämfört med toohello standard grundläggande lyssnare bladet är hello värdnamn. Fylla i hello information för hello-lyssnaren och klicka på **OK**.

![lyssnare egenskapsbladet][3]

> [!NOTE]
> Skapa lyssnare i hello Azure-portalen för Programgateway är en tidskrävande uppgift kan det ta några tid toocreate hello två lyssnare i det här scenariot. När fullständig hello lyssnare visas i hello portal som visas i följande bild hello:

![Översikt över lyssnare][4]

## <a name="create-rules-toomap-listeners-toobackend-pools"></a>Skapa regler toomap lyssnare toobackend pooler

### <a name="step-1"></a>Steg 1

Navigera tooan befintliga Programgateway i hello Azure-portalen (https://portal.azure.com). Välj **regler** och välj hello befintliga standardregel **regel 1** och på **redigera**.

### <a name="step-2"></a>Steg 2

Fyll i hello regler bladet som visas i följande bild hello. Välja hello första lyssnare och första poolen och klicka på **spara** när du är klar.

![Redigera en befintlig regel][6]

### <a name="step-3"></a>Steg 3

Klicka på **regel** toocreate hello andra regeln. Fyll i formuläret hello med hello andra lyssnare och andra serverdelspool och klicka på **OK** toosave.

![Lägg till regel bladet][10]

Det här scenariot har slutförts konfigurerar en Programgateway med befintliga med stöd för flera platser via hello Azure-portalen.

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooprotect dina webbplatser med [Programgateway - Brandvägg för webbaserade program](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
