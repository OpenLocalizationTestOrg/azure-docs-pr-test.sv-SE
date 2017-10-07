---
title: "aaaLayered säkerhetsarkitekturen med Apptjänstmiljöer"
description: "Implementera en skiktad säkerhetsarkitekturen med Apptjänstmiljöer."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 73ce0213-bd3e-4876-b1ed-5ecad4ad5601
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: stefsch
ms.openlocfilehash: 0627ba6fa849908506fe62c451c888c147cabc03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Implementera en skiktad säkerhetsarkitekturen med Apptjänstmiljöer
## <a name="overview"></a>Översikt
Utvecklare kan skapa en skiktad säkerhetsarkitekturen att tillhandahålla olika nivåer av nätverksåtkomst för varje nivå i Tillämpningsprogrammets fysiska eftersom Apptjänstmiljöer ger en isolerad körningsmiljö har distribuerats till ett virtuellt nätverk.

Viljan vanliga är toohide API-servrar från allmän tillgång till Internet och endast tillåta API: er toobe anropas av överordnad webbprogram.  [Nätverkssäkerhetsgrupper (NSG: er)] [ NetworkSecurityGroups] kan användas på undernät som innehåller Apptjänstmiljöer toorestrict offentlig åtkomst tooAPI program.

hello diagrammet nedan illustrerar ett exempel arkitekturen med en WebAPI baserat app som har distribuerats på en Apptjänst-miljö.  Tre separata web app-instanser, distribueras på tre separata Apptjänstmiljöer, se backend-anrop toohello samma WebAPI-app.

![Konceptuell arkitektur][ConceptualArchitecture] 

hello grönt plustecken indikerar att hello nätverkssäkerhetsgruppen på hello undernät som innehåller ”apiase” tillåter inkommande anrop från hello överordnade web apps som korrekt anrop från sig själv.  Men hello samma nätverkssäkerhetsgruppen uttryckligen nekar åtkomst till toogeneral inkommande trafik från hello Internet. 

hello resten av det här avsnittet går igenom hello steg behövs tooconfigure hello nätverkssäkerhetsgruppen på hello undernät som innehåller ”apiase”.

## <a name="determining-hello-network-behavior"></a>När du fastställer hello nätverksproblem
I ordning tooknow måste vilka nätverkssäkerhet regler krävs toodetermine vilka nätverksklienter får tooreach hello Apptjänst-miljö som innehåller hello API-app och vilka klienter kommer att blockeras.

Eftersom [nätverkssäkerhetsgrupper (NSG: er)] [ NetworkSecurityGroups] är tillämpade toosubnets och Apptjänstmiljöer har distribuerats till undernät, hello regler i en NSG tillämpas för**alla** appar som körs på en Apptjänst-miljö.  Med hjälp av hello exempelarkitektur för den här artikeln när en nätverkssäkerhetsgrupp är tillämpade toohello undernät som innehåller ”apiase” alla appar som körs på hello ”apiase” Apptjänstmiljö kommer att skyddas av hello uppsättning samma säkerhetsregler. 

* **Fastställa hello utgående IP-adressen för överordnade anropare:** vad är hello IP-adressen eller adresserna för hello överordnade anropare?  Dessa adresser behöver toobe som uttryckligen tillåts åtkomst i hello NSG.  Eftersom anrop mellan Apptjänstmiljöer betraktas som ”Internet” anrop, innebär detta hello utgående IP-adress som tilldelats tooeach av hello tre överordnade Apptjänstmiljöer behov toobe tillåts åtkomst i hello NSG för undernätet för hello ”apiase”.   Mer information om hur du fastställer hello utgående IP-adress för appar som körs i en Apptjänst-miljö finns hello [nätverksarkitektur] [ NetworkArchitecture] översiktsartikel.
* **Hello backend-API-app måste toocall själva?**  En ibland förbises och diskret är hello scenario där hello backend-programmet måste toocall sig själv.  Om en backend-API-program på en Apptjänst-miljö måste toocall sig själv, detta också behandlas som ett anrop för ”Internet”.  I hello exempelarkitektur kräver detta att tillåta åtkomst från hello utgående IP-adressen för hello ”apiase” Apptjänstmiljö samt.

## <a name="setting-up-hello-network-security-group"></a>Konfigurera hello Nätverkssäkerhetsgrupp
När hello uppsättning utgående IP-adresser är kända, hello nästa steg är tooconstruct en nätverkssäkerhetsgrupp.  Du kan skapa nätverkssäkerhetsgrupper för båda Resource Manager-baserade virtuella nätverk, samt klassiska virtuella nätverk.  hello exemplen nedan visar hur du skapar och konfigurerar en NSG på ett klassiskt virtuellt nätverk med hjälp av Powershell.

För hello exempelarkitektur finns hello miljöer i södra centrala USA, så skapas en tom NSG i regionen:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Ett explicit Låt först lägga till regeln för hello Azure infrastruktur som anges i hello artikeln på [inkommande trafik] [ InboundTraffic] för Apptjänstmiljöer.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

Därefter två regler läggs tooallow HTTP och HTTPS-anrop från hello första överordnade Apptjänst-miljö (”fe1ase”).

    #Grant access toorequests from hello first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Skölj och upprepa för hello andra och tredje överordnade Apptjänstmiljöer (”fe2ase” och ”fe3ase”).

    #Grant access toorequests from hello second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access toorequests from hello third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Till sist ska bevilja åtkomst toohello utgående IP-adressen för hello backend-API: er Apptjänst-miljö så att den kan anropa tillbaka till sig själv.

    #Allow apps on hello apiase environment toocall back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Inga andra Nätverkssäkerhetsregler måste toobe konfigureras eftersom varje NSG innehåller en uppsättning standardregler som blockerar inkommande åtkomst från hello Internet som standard.

hello fullständig lista över regler i hello nätverkssäkerhetsgruppen visas nedan.  Observera hur hello senaste rule är markerat blockerar inkommande åtkomst från anropare än de som uttryckligen getts åtkomst.

![NSG-konfiguration][NSGConfiguration] 

hello sista steget är tooapply hello NSG toohello undernät som innehåller hello ”apiase” Apptjänst-miljö.  

     #Apply hello NSG toohello backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Med hello NSG tillämpas toohello undernät endast tillåts hello tre överordnade Apptjänstmiljöer och hello Apptjänst-miljö som innehåller hello API serverdel toocall till hello ”apiase”-miljö.

## <a name="additional-links-and-information"></a>Information och ytterligare länkar
Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

Information om [nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md). 

Förstå [utgående IP-adresser] [ NetworkArchitecture] och Apptjänstmiljöer.

[Nätverksportar] [ InboundTraffic] används av Apptjänstmiljöer.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
