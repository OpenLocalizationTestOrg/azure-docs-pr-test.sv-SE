---
title: "Avlänka Azure Automation-konto från logganalys | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över hur du avlänkar Azure Automation-konto från en OMS-arbetsyta."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to-article
ms.date: 02/07/2017
ms.author: magoedte
ms.openlocfilehash: 56b09c2cfc14813b5efcb364c580787fec1bf639
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Hur du avlänkar ditt Automation-konto från logganalys-arbetsytan

Azure Automation kan integreras med logganalys som inte bara stöd för Proaktiv övervakning av runbook-jobb över alla Automation-konton, men den krävs också när du importerar följande lösningar som är beroende av logganalys:

* [Hantering av uppdateringar](../operations-management-suite/oms-solution-update-management.md)
* [Spårning av ändringar](../log-analytics/log-analytics-change-tracking.md)
* [Starta/Stoppa VMs kontorstid](automation-solution-vm-management.md)
 
Om du inte längre vill integrera ditt Automation-konto med logganalys Avlänka du ditt konto direkt från Azure-portalen.  Innan du fortsätter måste du först ta bort de lösningar som tidigare nämnts, annars kommer den här processen hindras från att du fortsätter.  Granska i avsnittet för en viss lösning som du har importerat för att förstå de steg som krävs för att ta bort den.  

När du tar bort dessa lösningar kan du utföra följande steg om du vill Avlänka Automation-konto.

## <a name="unlink-workspace"></a>Avlänka arbetsytan

1. Öppna ditt Automation-konto från Azure-portalen och markera i bladet Automation-konto i bladet konto **Avlänka arbetsytan**.<br><br> ![Avlänka arbetsyta](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. Klicka på bladet Avlänka från arbetsytan **Avlänka arbetsyta**.<br><br> ![Avlänka arbetsytan bladet](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  Ett meddelande visas där du bekräftar att du vill fortsätta.<br><br>
3. Medan Azure Automation försöker Avlänka kontot logganalys-arbetsytan, du kan följa förloppet under **meddelanden** på menyn.

Om du har använt lösning för hantering av uppdateringar kanske (valfritt) du vill ta bort följande objekt som inte längre behövs när du tar bort lösningen.

* Uppdatera scheman.  Varje har namn som matchar uppdateringsdistributioner som du skapade)

* Hybrid worker grupper som skapats för lösningen.  Varje får namnet på samma sätt att machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Om du använde Starta/stoppa virtuella datorer under låg belastning på nätverket lösning kanske eventuellt du vill ta bort följande objekt som inte längre behövs när du tar bort lösningen.

* Starta och stoppa scheman för VM-runbook 
* Starta och stoppa runbooks för VM
* Variabler   

## <a name="next-steps"></a>Nästa steg

Om du vill konfigurera om ditt Automation-konto för att integrera med OMS Log Analytics finns [vidarebefordra jobbstatus och jobbet strömmar från Automation till logganalys (OMS)](automation-manage-send-joblogs-log-analytics.md). 