---
title: "aaaVM omstart Underhåll för Linux virtuella datorer i Azure | Microsoft Docs"
description: "VM att starta om Underhåll för Linux virtuella datorer."
services: virtual-machines-linux
documentationcenter: 
author: zivr
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: eb4b92d8-be0f-44f6-a6c3-f8f7efab09fe
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: zivr
ms.openlocfilehash: 41caa2e56740cdfa95d2ea67278e5c1d15a174c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vm-restarting-maintenance"></a>VM att starta om Underhåll

Det finns några fall där dina virtuella datorer startas om på grund av tooplanned Underhåll toohello underliggande infrastruktur. Som effektfulla toothe tillgängligheten för din virtuella dator finns i Azure är hello följande nu tillgängliga för du toouse:

-   Meddelande skickat minst 30 dagar före hello påverkan.

-   Synlighet toohello underhållsfönster per varje virtuell dator.

-   Flexibilitet och kontroll i inställningen hello exakta tidpunkt för underhåll påverkan på din virtuella dator.

Schemalagt underhåll i Microsoft Azure i iterationer. Inledande iterationer har mindre omfattning i ordning tooreduce hello risken i lansera nya korrigeringar och funktioner. Senare iterationer kan sträcka sig över flera regioner (aldrig från hello samma region par). En virtuell dator ingår i en enda Underhåll iteration. Om en iteration avbryts med återstående virtuella datorer i en annan, framtida iteration.

hello planerat underhåll iteration har två faser: Pre-emptive underhållsfönstret och en schemalagda underhållsperiod.

Hej **Pre-emptive underhållsperiod** ger dig flexibilitet hello tooinitiate hello Underhåll på dina virtuella datorer. På så sätt, du fastställa när dina virtuella datorer som påverkas, hello sekvens av hello update och hello tiden mellan varje virtuell dator som underhålls. Du kan fråga varje VM-toosee om den planerade för underhåll och kontrollera hello resultatet av din senaste begäran som initierade underhåll.

Hej **schemalagda underhållsperiod** när Azure har schemalagt dina virtuella datorer för hello underhåll. Under detta tidsfönster som följer pre-emptive underhållsperiod, kan du fortfarande fråga efter hello underhållsperiod, men inte längre att kunna tooorchestrate hello underhåll.

## <a name="availability-considerations-during-planned-maintenance"></a>Tillgänglighet under planerat underhåll 

### <a name="paired-regions"></a>Parad regioner

Varje Azure-region paras ihop med en annan region inom hello samma geografisk plats, gör ett regionalt par tillsammans. När du kör Underhåll Azure bara att uppdatera hello virtuell datorinstans i en region för dess par. Till exempel när du uppdaterar hello virtuella datorer i norra centrala USA, Azure kommer inte att uppdatera alla virtuella datorer i södra centrala USA på hello samtidigt. Detta schemaläggs vid en separat tid för att möjliggöra redundans eller belastningsutjämning mellan regioner. Men andra regioner som Nordeuropa kan vara under Underhåll på hello samma tid som östra USA.
Läs mer om [Azure-region par](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="single-instance-vms-vs-availability-set-or-vm-scale-set"></a>Ange instans VMs jämfört med tillgänglighet eller skaluppsättning för virtuell dator

När du distribuerar en arbetsbelastning med virtuella datorer i Azure kan skapa du hello virtuella datorer i en tillgänglighetsuppsättning i ordning tooprovide hög tillgänglighet tooyour program. Den här konfigurationen garanterar att minst en virtuell dator vid ett strömavbrott eller underhåll händelser är tillgänglig.

I en tillgänglighetsuppsättning sprids enskilda virtuella datorer installerar too20 uppdatering domäner. Endast en enskild uppdateringsdomän påverkas vid en given tidpunkt under planerat underhåll. hello ordningen för update-domäner som påverkas kan inte fortsätta sekventiellt under planerat underhåll. En instans i virtuella datorer (inte ingår i tillgänglighetsuppsättningen), finns det inget sätt toopredict eller avgöra vilka och hur många virtuella datorer som påverkas tillsammans.

Skaluppsättningar för den virtuella datorn är en Azure compute-resurs som du kan använda toodeploy och hantera en uppsättning identiska virtuella datorer som en enskild resurs.
hello innehåller skala liknande garantier tooan tillgänglighetsuppsättning i form av uppdatering domäner. 

Mer information om hur du konfigurerar dina virtuella datorer för hög tillgänglighet finns [ *hantera hello tillgängligheten för din virtuella Windows-datorer*](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="scheduled-events"></a>Schemalagda händelser

Azure Metadata Service kan du toodiscover information om den virtuella datorn finns på Azure. Schemalagda händelser i hello exponeras kategorier, hämtar information om kommande händelser (till exempel omstart) så att programmet kan förbereda för dem och begränsa avbrott.

Mer information om schemalagda händelser finns för[Azure Metadata Service - schemalagda händelser](../virtual-machines-scheduled-events.md).

## <a name="maintenance-discovery-and-notifications"></a>Identifiering av underhåll och meddelanden

Underhållsschema är synliga toocustomers på hello nivå av enskilda virtuella datorer. Du kan använda Azure portal, API: et, PowerShell eller CLI tooquery för windows pre-emptive och schemalagt underhåll. Dessutom förväntar dig att få ett meddelande (e-post) i hello fall där en (eller flera) för dina virtuella datorer som påverkas under hello-processen.

Både pre-emptive underhåll och schemalagt underhåll börja med ett meddelande. Förvänta dig tooreceive ett enda meddelande per Azure-prenumeration. hello-meddelande skickas toohello prenumeration admin och medadministratör som standard. Du kan också konfigurera hello målgruppen för underhåll meddelandet.

### <a name="view-hello-maintenance-window-in-hello-portal"></a>Visa hello underhållsperiod hello-portalen 

Du kan använda hello Azure-portalen och leta efter virtuella datorer som är schemalagda för underhåll.

1.  Logga in toohello Azure-portalen.

2.  Klicka på och öppna hello **virtuella datorer** bladet.

3.  Klicka på hello **kolumner** knappen tooopen hello lista över tillgängliga kolumner toochoose från

4.  Välj och Lägg till hello **underhållsperiod** kolumner. Virtuella datorer som är schemalagda för underhåll har hello underhållsfönster som är anslutna. När underhållsläge har slutförts eller avbröts för finns inte längre ett underhållsfönster.

### <a name="query-maintenance-details-using-hello-azure-api"></a>Underhåll Frågedetaljer med hello Azure API

Använd hello [få information om Virtuellt API](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get) och leta efter hello instans visa toodiscover hello Underhåll information om en enskild VM. hello svaret innehåller hello följande element:

  - isCustomerInitiatedMaintenanceAllowed: Anger om du kan nu starta pre-emptive Omdistributionen på hello VM.

  - preMaintenanceWindowStartTime: hello starttiden för hello pre-emptive underhållsperiod.

  - preMaintenanceWindowEndTime: hello sluttid för hello pre-emptive underhållsperiod. Därefter kan längre du inte kan tooinitiate Underhåll på den här virtuella datorn.
    
  - maintenanceWindowStartTime: hello starttid för hello schemalagda underhållsperiod, när den virtuella datorn påverkas.

  - maintenanceWindowEndTime: hello sluttid för hello schemalagda underhållsperiod.
  
  - lastOperationResultCode: hello resultatet av din senaste Underhåll Omdistributionen-åtgärden.
 
  - lastOperationMessage: meddelande som beskriver hello resultatet av din senaste Underhåll Omdistributionen-åtgärden.


## <a name="pre-emptive-redeploy"></a>Pre-emptive Omdistributionen

Pre-emptive Omdistributionen åtgärden innehåller hello flexibilitet toocontrol hello tidpunkt då Underhåll är tillämpade tooyour virtuella datorer i Azure. Planerat underhåll börjar med en pre-emptive underhållsperiod där du kan bestämma hello exakta tidpunkt för var och en av dina virtuella datorer toobe startas om. Följande är användningsområden där sådan funktion är användbar:

-   Underhåll måste toobe samordnas med hello slutkunden.

-   Det räcker inte hello schemalagda underhållsperiod som Azure erbjuder.
    Det kan vara att hello fönstret händer toobe på hello mest använda gång i veckan eller den är för lång.

-   För flera instanser eller program på flera nivåer behöver du tillräckligt med tid mellan två virtuella datorer eller en viss sekvens toofollow.

När du anropar för pre-emptive Omdistributionen på en virtuell dator, flyttar hello VM tooan redan uppdaterats noden och sedan aktiveras den tillbaka, behåller alla konfigurationsalternativ och associerade resurser. När du gör det måste hello diskutrymme går förlorad och dynamiska IP-adresser som är kopplade till virtuella nätverksgränssnittet har uppdaterats.

Pre-emptive Omdistributionen aktiveras en gång per virtuell dator. Om ett fel vid hello processen hello åtgärden avbryts, hello VM inte påverkas och det ingår inte i hello planerat underhåll iteration. Du kontakta i ett senare tillfälle med ett nytt schema och erbjuder en ny affärsmöjlighet tooschedule och sekvens hello inverkan på din virtuella dator.
