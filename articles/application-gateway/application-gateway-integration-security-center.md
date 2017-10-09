---
title: aaaApplication Gateway-integration med Azure Security Center | Microsoft Docs
description: "Den här sidan innehåller information om hur Application Gateway är integrerat i Azure Security Center."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: gwallace
ms.openlocfilehash: 6f6ace105e84c01f525ab02938e81ce040c5c9d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Översikt över integrering mellan Programgateway och Azure Security Center

Lär dig hur Programgateway och Security Center skydda resurser i ditt webbprogram. Brandvägg för programmet gateway webbaserade program (Brandvägg) kan integreras med [Security Center](../security-center/security-center-intro.md) tooprovide en sömlös visa tooprevent identifierar och åtgärdar toothreats toounprotected webbprogram i din miljö.

## <a name="overview"></a>Översikt

Programmet Gateway Brandvägg är en rekommendation i Security Center för att skydda webbprogram från kryphål och säkerhetsproblem. Aktiverad webbresurser som inte skyddas av Brandvägg visas i hello security center som Hög allvarlighetsgrad rekommendationer. Rekommendationer för web application brandväggar visas på hello **översikt** sidan under **program**.

![integrering med security center][1]

Om du klickar på några rekommendationer om Brandvägg för webbaserade program öppnas ett nytt blad som visar hello information om hello rekommendation.

## <a name="add-a-web-application-firewall-tooan-existing-resource"></a>Lägg till ett program brandväggen tooan befintliga webbresurs

Navigera för**fler tjänster** > **säkerhet + identitet** > **Security Center** och på hello **Security Center - översikt**  bladet, klickar du på **program**. På hello **Security Center - program** bladet hello tabellen innehåller en lista över program som Security Center identifieras i din prenumeration.

![webbprogram][3]

Genom att klicka på ett webbprogram med ett allvarligt problem kan du få hello **programmet säkerhetshälsa** bladet. Hej webbprogram som inte skyddas av en brandvägg för webbaserade program i hello bilden nedan. 

![webbresurser som inte skyddas][2]

Klicka på **lägga till en brandvägg för webbaserade program** under **rekommendationer** tooopen hello **lägga till en brandvägg för webbaserade program** bladet.

Om du inte har en befintlig Programgateway eller vill toocreate en ny, klickar du på **Skapa nytt** och på hello **skapa en ny Brandvägg för webbaserade program** bladet och klickar på **Microsoft - Programgateway**. Detta tar dig igenom hello steg toocreate en Programgateway. Webbprogrammet har nu lagts till som en skyddad resurs, Security Center nu spårar att den här resursen skyddas av en brandvägg för webbaserade program. Detta lägger inte till den som en medlem för backend-poolen.

Om du har en befintlig Programgateway, kan du välja den under **med befintliga lösning**

![Brandvägg för webbaserade program lägger du till bladet][4]

När du lägger till en web application tooan Programgateway via Security Center inte hello resurs som medlem backend-adresspool, måste du göra det på hello programresursen gateway direkt.

## <a name="add-a-resource-tooan-existing-web-application-firewall"></a>Lägg till en resurs tooan befintliga Brandvägg för webbaserade program

Navigera för**fler tjänster** > **säkerhet + identitet** > **Security Center** och på hello **Security Center - översikt**  bladet, klickar du på **partnerlösningar**. Befintliga gateways för Security Center-medvetna programmet visas i hello **partnerlösningar** bladet.

![partnerlösningar][7]

Klicka på **Link app** tooopen hello **länken program** bladet här du ges hello alternativ tooselect befintliga program. Välj hello program tooprotect och på **OK**. Detta lägger inte till hello web toohello backend programpool för hello Programgateway. Detta anger hello resurser som en skyddad resurs så kan spåra av Security Center. tooadd hello resurs som medlem backend-adresspool, måste du göra det på hello Programgateway, från hello aktuella bladet kan du klicka på **lösning konsolen** toobe tas toohello programresursen gateway där du kan lägga till hello web toohello backend programpoolen.

![partner solutions program][6]

## <a name="finalize-configuration"></a>Slutför konfiguration

Security Center spårar program till tooan Programgateway som en skyddad resurs.  Den övervakar hello hälsotillståndet för den här resursen och garanterar att den skyddas av en Programgateway. hello nästa steg är tooadd hello privata IP-, offentlig IP-adress eller nätverkskort på din virtuella toohello serverdelspool för hello Programgateway. Tills detta görs en ytterligare rekommendation av **Slutför programskydd** visas förrän hello resursen har lagts till.

![Brandvägg för webbaserade program lägger du till bladet][5]

## <a name="security-alerts"></a>Säkerhetsaviseringar

Navigera för i Security Center**identifiering** > **säkerhetsaviseringar**.  Här hittar du Brandvägg aviseringar för din programgatewayer. Aviseringar är fördelade på Brandvägg regeln.

![säkerhetsaviseringar][8]

Klicka på en regel ger en lista över aviseringar för den specifika regeln Brandvägg. Varje avisering visas ytterligare information på hello söka efter. hello information innehåller en länk toohello application gateway.
 
![aviseringsinformation][9]

## <a name="next-steps"></a>Nästa steg

toolearn hur Brandvägg för tooenable webbaserade program på en befintlig Programgateway finns [skapa eller uppdatera en Azure Programgateway med Brandvägg för webbaserade program](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png