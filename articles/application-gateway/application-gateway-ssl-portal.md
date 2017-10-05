---
title: Konfigurera SSL-avlastning - Azure Application Gateway - Azure-portalen | Microsoft Docs
description: "Den här sidan innehåller instruktioner för att skapa en Programgateway med SSL-avlastning med hjälp av portalen"
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
ms.openlocfilehash: f61be0cc4c9274c9914f7c468ce48a2a3d0a4f4a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Konfigurera en Programgateway för SSL-avlastning med hjälp av portalen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-ssl-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kan konfigureras att avsluta SSL-sessionen (Secure Sockets Layer) på gatewayen så att du undviker kostsamma SSL-dekrypteringsaktiviteter i webbservergruppen. SSL-avlastning förenklar också frontend-serverkonfigurationen och hanteringen av webbappen.

## <a name="scenario"></a>Scenario

Följande scenario går igenom hur du konfigurerar SSL avlasta på en befintlig gateway för programmet. Scenariot förutsätter att du redan har följt stegen för att [skapa en Programgateway](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Innan du börjar

Ett certifikat krävs för att konfigurera SSL-avlastning med en Programgateway. Det här certifikatet har lästs in på programgatewayen och används för att kryptera och dekryptera den trafik som skickas via SSL. Certifikatet måste vara i formatet Personal Information Exchange (pfx). Det här filformatet kan för den privata nyckeln exporteras som krävs av programgatewayen att genomföra kryptering och dekryptering av trafik.

## <a name="add-an-https-listener"></a>Lägg till en HTTPS-lyssnare

HTTPS-lyssnaren söker efter trafik baserat på dess konfiguration och hjälper till att vidarebefordra trafiken till backend-pooler.

### <a name="step-1"></a>Steg 1

Gå till Azure-portalen och väljer en befintlig Programgateway

### <a name="step-2"></a>Steg 2

Klicka på lyssnare och klicka på Lägg till för att lägga till en lyssnare.

![Översikt över App nätverksgateway-bladet][1]

### <a name="step-3"></a>Steg 3

Fyll i informationen som krävs för lyssnaren och ladda upp PFX-certifikat, när du är klar klickar du på OK.

**Namnet** -värdet är ett eget namn för lyssnare.

**Frontend-IP-konfiguration** -värdet är klientdelens IP-konfiguration som används för lyssnaren.

**Klientdelsport (namn/Port)** – ett eget namn för porten som används på klientdelen för programgatewayen och den faktiska porten används.

**Protokollet** -en växel för att avgöra om https eller http används för klientdelen.

**Certifikatet (och lösenord)** - avlastning om SSL används ett PFX-certifikat krävs för den här inställningen och ett eget namn och lösenord krävs.

![lyssnare bladet Lägg till][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Skapa en regel och koppla den till lyssnaren

Lyssnaren har skapats. Det är dags att skapa en regel för att hantera trafik från lyssnaren. Regler som definierar hur trafiken dirigeras till serverdelspooler baserat på flera konfigurationsinställningar, inklusive om cookie-baserad session tillhörighet används, protokoll, port och hälsoavsökningar.

### <a name="step-1"></a>Steg 1

Klicka på den **regler** för Programgateway, och klicka sedan på Lägg till.

![appen gateway regler bladet][3]

### <a name="step-2"></a>Steg 2

På den **Lägg till regel** bladet Skriv i det egna namnet för regeln och välj lyssnaren som skapades i föregående steg. Välj lämplig serverdelspool och HTTP-inställningen och klicka på **OK**

![fönster för HTTPS-inställningar][4]

Inställningarna sparas nu i programgatewayen. Spara bearbeta för de här inställningarna kan ta en stund innan de kan visa via portalen eller PowerShell. När sparat programgatewayen hanterar kryptering och dekryptering av trafik. All trafik mellan programgatewayen och backend-webbservrar hanteras via http. All kommunikation till klienten om initieras via https returneras till klienten krypteras.

## <a name="next-steps"></a>Nästa steg

Information om hur du konfigurerar en anpassad hälsoavsökningen med Azure Programgateway finns [skapa en anpassad hälsoavsökningen](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
