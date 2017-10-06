---
title: aaaConfigure SSL-avlastning - Azure Application Gateway - Azure-portalen | Microsoft Docs
description: "Den här sidan finns instruktioner toocreate en Programgateway med SSL-avlastning med hello-portalen"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: e87ac0bbe10ac45e307c18802741c7bc31764a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-portal"></a>Konfigurera en Programgateway för SSL-avlastning med hello-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-ssl-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kan vara konfigurerade tooterminate hello Secure Sockets Layer (SSL)-session på hello gateway tooavoid kostsamma SSL dekryptering uppgifter toohappen på hello webbservergrupp. SSL-avlastning förenklar också hello front-end-serverinstallation och hantering av hello webbprogram.

## <a name="scenario"></a>Scenario

hello följande scenario går igenom hur du konfigurerar SSL avlasta på en befintlig Programgateway. hello scenariot förutsätter att du har redan följt hello stegen för[skapa en Programgateway](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Innan du börjar

tooconfigure SSL-avlastning med en Programgateway, krävs ett certifikat. Det här certifikatet har lästs in på hello Programgateway används tooencrypt och dekryptera hello trafik som skickas via SSL. hello certifikat måste toobe Personal Information Exchange (pfx)-format. Det här filformatet tillåter hello privata nyckel toobe exporteras som krävs för hello programmet gateway tooperform hello kryptering och dekryptering av trafik.

## <a name="add-an-https-listener"></a>Lägg till en HTTPS-lyssnare

HTTPS-lyssnare för hello söker efter trafik baserat på dess konfiguration och hjälper väg hello trafik toohello serverdelspooler.

### <a name="step-1"></a>Steg 1

Navigera toohello Azure-portalen och väljer en befintlig Programgateway

### <a name="step-2"></a>Steg 2

Lyssnare och klicka på knappen tooadd för hello Lägg till en lyssnare.

![Översikt över App nätverksgateway-bladet][1]

### <a name="step-3"></a>Steg 3

Fyll i hello information som krävs för hello-lyssnare och överför hello .pfx-certifikat, när du är klar klickar du på OK.

**Namnet** -värdet är ett eget namn på hello lyssnare.

**Frontend-IP-konfiguration** -värdet är hello klientdelens IP-konfiguration som används för hello-lyssnaren.

**Klientdelsport (namn/Port)** -ett eget namn för hello-port som används på hello klientdel hello Programgateway och hello faktiska porten som används.

**Protokollet** -en växel toodetermine om https eller http används för hello klientdelen.

**Certifikatet (och lösenord)** - avlastning om SSL används ett PFX-certifikat krävs för den här inställningen och ett eget namn och lösenord krävs.

![lyssnare bladet Lägg till][2]

## <a name="create-a-rule-and-associate-it-toohello-listener"></a>Skapa en regel och associera den toohello lyssnare

hello-lyssnare har skapats. Det är tid toocreate regeln toohandle hello trafik från hello-lyssnaren. Regler som definierar hur trafik dirigeras toohello serverdelspooler baserat på flera konfigurationsinställningar, inklusive om cookie-baserad session tillhörighet används, protokoll, port och hälsoavsökningar.

### <a name="step-1"></a>Steg 1

Klicka på hello **regler** för hello Programgateway, och klicka sedan på Lägg till.

![appen gateway regler bladet][3]

### <a name="step-2"></a>Steg 2

På hello **Lägg till regel** bladet ange hello eget namn för hello regeln och välj hello lyssnare har skapats i hello föregående steg. Välj hello lämpliga serverdelspool och HTTP-inställningen och klicka på **OK**

![fönster för HTTPS-inställningar][4]

hello inställningar sparas nu toohello Programgateway. hello processen för de här inställningarna kan ta en stund innan de tillgängliga tooview via hello portal eller PowerShell. När du har sparat hello Programgateway hanterar hello kryptering och dekryptering av trafik. All trafik mellan hello Programgateway och hello backend-webbservrar hanteras via http. Kommunikation tillbaka toohello klienter om initieras via https returneras toohello klienten krypteras.

## <a name="next-steps"></a>Nästa steg

toolearn hur tooconfigure anpassade hälsa avsökning med Azure Application Gateway finns i [skapa en anpassad hälsoavsökningen](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
