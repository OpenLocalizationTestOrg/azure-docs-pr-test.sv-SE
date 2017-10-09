---
title: "aaaTroubleshooting Molntjänsten Allokeringsfel | Microsoft Docs"
description: "Felsök allokeringsfel när du distribuerar Cloud Services i Azure"
services: azure-service-management, cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 529157eb-e4a1-4388-aa2b-09e8b923af74
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: dfd5cc4663ccc6ed1b27ca9df579182737363b0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Felsök allokeringsfel när du distribuerar Cloud Services i Azure
## <a name="summary"></a>Sammanfattning
När du distribuerar instanser tooa Molntjänsten eller lägga till nya webb eller arbetare rollinstanser allokerar Microsoft Azure beräkningsresurser. Ibland kan du få felmeddelanden när du utför dessa åtgärder innan du når hello Azure-prenumerationsbegränsningar. Den här artikeln förklarar hello orsakerna till vissa av hello vanliga Allokeringsfel och föreslår möjliga reparation. hello informationen kan också vara användbar när du planerar distributionen av hello-tjänster.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Bakgrund – hur allokering fungerar
hello-servrar i Azure-Datacenter partitioneras i kluster. En ny molnet allokering tjänstbegäran görs i flera kluster. När hello första instansen är distribuerade tooa molntjänst (i Förproduktion eller produktion), som molntjänst hämtar Fäst tooa klustret. Någon ytterligare distributioner för hello Molntjänsten sker i hello samma kluster. I den här artikeln ska vi refererar toothis som ”Fäst tooa cluster”. Diagram 1 nedan illustrerar hello skiftläget för en normal allokering som görs i flera kluster; Diagram 2 visar hello fallet för en allokering som Fäst tooCluster 2 eftersom som är där hello befintliga Cloud Service CS_1 finns.

![Allokering av Diagram](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Varför Allokeringsfel händer
När en begäran om minnesallokering är fäst tooa klustret, finns det en högre risken för felaktiga toofind lediga resurser eftersom hello tillgängliga resurspool är begränsad tooa kluster. Dessutom misslyckas din begäran även om hello klustret har fri resurs om din begäran om minnesallokering är fäst tooa kluster men hello typ av resurs som du har begärt stöds inte av klustret. Bild 3 nedan visar hello fall där en fast allokering misslyckas eftersom hello endast candidate klustret inte har lediga resurser. Diagram 4 illustrerar hello fall där en fast allokering misslyckas eftersom hello endast kandidat kluster inte stöder hello begärda VM-storlek, även om hello klustret har frigöra resurser.

![Fäst Allokeringsfel](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Felsök tilldelningsproblem för molntjänster
### <a name="error-message"></a>Felmeddelande
Du kan se hello följande felmeddelande:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable toosatisfy constraints in request. hello requested new service deployment is bound tooan Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains hello new deployment toospecific Azure resources. Please retry later or try reducing hello VM size or number of role instances. Alternatively, if possible, remove hello aforementioned constraints or try deploying tooa different region."

### <a name="common-issues"></a>Vanliga problem
Här följer hello vanliga allokering scenarier som orsakar en allokering begäran toobe Fäst tooa kluster.

* Distribuerar tooStaging fack - en molnbaserad tjänst har en distribution på antingen platsen, sedan hello hela molntjänst är fäst tooa specifikt kluster.  Det innebär att om en distribution som redan finns i hello produktionsplatsen sedan en ny fristående distribution kan endast allokeras i samma kluster som hello produktionsplatsen hello. Om klustret hello närmar sig kapacitetsgränsen, kan hello förfrågan misslyckas.
* Skalning – att lägga till nya instanser tooan befintlig molntjänst måste tilldelas i hello samma kluster.  Liten skala begäranden kan vanligen allokeras, men inte alltid. Om klustret hello närmar sig kapacitetsgränsen, kan hello förfrågan misslyckas.
* Tillhörighetsgrupp - en ny distribution tooan tom molntjänst kan allokeras av hello fabric i ett kluster i den regionen, såvida inte hello molntjänst är fäst tooan tillhörighetsgrupp. Distributioner toohello samma tillhörighetsgrupp görs på hello samma kluster. Om klustret hello närmar sig kapacitetsgränsen, kan hello förfrågan misslyckas.
* Tillhörighetsgruppen vNet - äldre virtuella nätverk har bundet tooaffinity grupper i stället för regioner och molntjänster i dessa virtuella nätverk är fäst toohello tillhörighetsgrupp klustret. Distributioner toothis typen av virtuellt nätverk görs på hello Fäst klustret. Om klustret hello närmar sig kapacitetsgränsen, kan hello förfrågan misslyckas.

## <a name="solutions"></a>Lösningar
1. Omdistributionen tooa ny molntjänst - den här lösningen är sannolikt toobe mest framgångsrika som tillåter hello plattform toochoose från alla kluster i den regionen.

   * Distribuera hello arbetsbelastning tooa ny molntjänst  
   * Uppdatera hello CNAME-post eller en post toopoint trafik toohello ny molntjänst
   * När noll-trafik skickas toohello gamla platsen, kan du ta bort hello gamla Molntjänsten. Den här lösningen ska innebära ett driftstopp.
2. Ta bort produktion och mellanlagring platser – den här lösningen kommer att behålla din befintliga DNS-namn, men gör driftstopp tooyour program.

   * Ta bort hello produktions- och mellanlagring fack för en befintlig molntjänst så att hello molntjänst är tomt, och sedan
   * Skapa en ny distribution i hello befintlig molntjänst. Tooallocation på alla kluster i hello region försök igen. Kontrollera hello Molntjänsten inte bundet tooan tillhörighetsgrupp.
3. Reserverad IP - den här lösningen kommer att behålla din befintliga IP-adress, men gör driftstopp tooyour program.  

   * Skapa en ReservedIP för den befintliga distributionen med Powershell

     ```
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```
   * Följ #2 ovan och gör att toospecify hello nya ReservedIP i hello tjänsten CSCFG.
4. Ta bort tillhörighetsgruppen för nya distributioner - Tillhörighetsgrupper rekommenderas inte längre. Följ stegen för #1 ovan toodeploy en ny molntjänst. Kontrollera att Molntjänsten inte har en tillhörighetsgrupp.
5. Konvertera tooa regionalt virtuellt nätverk – Se [hur toomigrate från Tillhörighetsgrupper tooa regionalt virtuellt nätverk (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
