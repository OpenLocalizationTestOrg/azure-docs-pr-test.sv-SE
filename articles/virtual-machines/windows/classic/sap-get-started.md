---
title: "aaaUsing SAP på virtuella Windows-datorer | Microsoft Docs"
description: "Radera om hur du använder SAP på Windows-datorer (VM) i Microsoft Azure"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: 1b455be4-c02f-43e3-8d39-c2d5f216e646
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 6c4d8a066a4a8805668e78e67fd2110f2000ee75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Med hjälp av SAP på Windows-datorer i Azure
Molntjänster är en term som används mycket som har blivit mer och mer betydelse inom hello IT-branschen, från små företag upp toolarge och multinationella företag. Microsoft Azure är hello Molnplattform tjänster från Microsoft, vilket ger ett brett spektrum av nya möjligheter. Nu är kunder kan toorapidly etablera och avinstallation etablera program som molntjänster, så att de inte är begränsad tootechnical eller budgetering begränsningar. I stället för investera tid och budget i infrastrukturen för maskinvara, kan företag fokuserar på hello program, processer och dess fördelar för kunder och användare.

Microsoft erbjuder ett omfattande infrastruktur som en tjänst (IaaS)-plattform med Microsoft Azure-datorer. SAP NetWeaver-baserade program stöds på virtuella Azure-datorer (IaaS). hello whitepapers nedan beskrivs hur tooplan och implementera SAP NetWeaver baserade program på Windows-datorer i Azure. Du kan också implementeras SAP NetWeaver baserat program på [virtuella Linux-datorer](../../linux/classic/sap-get-started.md).

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver på Azure - hög tillgänglighet
Rubrik: aaaSAP NetWeaver på Azure - klustring SAP ASCS/SCS instanser som använder Windows Server Failover-kluster på Azure med SIOS DataKeeper

Sammanfattning ”: det här dokumentet beskriver hur toouse SIOS DataKeeper tooset installerar en högtillgänglig SAP ASCS/SCS-konfiguration på Azure. SAP skyddar sina enda åtkomstpunkt för fel komponenter som SAP ASCS/SCS eller sätta replikering Services med redundanskluster i Windows Server-konfigurationer som kräver delade diskar. Dessa SAP-komponenter är viktiga för hello funktionerna i en SAP-system. Funktioner för hög tillgänglighet måste toobe placeras i placera därför toomake till att dessa komponenter kan klara av fel på en server eller en virtuell dator som görs med Windows-kluster konfigurationer för bare metal- och Hyper-V-miljöer. Från och med augusti 2015 tillhandahålla inte Azure på själva delade diskar som krävs för Windows hello baserat högtillgängliga konfigurationer som krävs för dessa viktiga SAP-komponenter. Med hello hello produkten DataKeeper av SIOS, men Windows Server Failover Cluster konfigurationerna som behövs för SAP ASCS/SCS byggas på hello Azure IaaS-plattformen. Det här dokumentet beskriver en metod för steg för steg hur tooinstall en konfiguration för Windows Server Failover-kluster med delad disk som tillhandahålls av SIOS Datakeeper i Azure. hello dokumentet förklarar information i konfigurationer på hello Azure, Windows och SAP sida som gör hello konfiguration för hög tillgänglighet att fungera på ett optimalt sätt. hello papper kompletterar hello SAP dokumentationen och SAP anteckningar som representerar hello primära resurser för installation och distribution av program på anges plattformar.

Uppdaterad: Augusti 2015

[Hämta denna guide nu](http://go.microsoft.com/fwlink/?LinkId=613056)

