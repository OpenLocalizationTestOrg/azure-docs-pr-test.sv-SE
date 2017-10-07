---
title: "aaaUnlink Azure Automation-konto från logganalys | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över hur toounlink Azure Automation-konto från en OMS-arbetsyta."
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
ms.openlocfilehash: 3ec0745673f6a872538d33023a7a1d2bb1d18ec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toounlink-your-automation-account-from-a-log-analytics-workspace"></a>Hur toounlink ditt Automation-konto från logganalys-arbetsytan

Azure Automation kan integreras med logganalys toonot endast stöd för Proaktiv övervakning av runbook-jobb för alla Automation-konton, men den krävs också när du importerar följande lösningar som är beroende av logganalys hello:

* [Hantering av uppdateringar](../operations-management-suite/oms-solution-update-management.md)
* [Spårning av ändringar](../log-analytics/log-analytics-change-tracking.md)
* [Starta/Stoppa VMs kontorstid](automation-solution-vm-management.md)
 
Om du inte längre vill toointegrate ditt Automation-konto med logganalys Avlänka du ditt konto direkt från hello Azure-portalen.  Innan du fortsätter måste du först tooremove hello lösningar som tidigare nämnts, annars kommer den här processen hindras från att du fortsätter.  Granska hello avsnittet för hello lösning som du har importerat toounderstand hello steg som krävs för tooremove den.  

När du tar bort dessa lösningar kan du utföra följande steg toounlink hello Automation-konto.

## <a name="unlink-workspace"></a>Avlänka arbetsytan

1. Öppna ditt Automation-konto från hello Azure-portalen, och markera i hello Automation-kontoblad i hello-kontoblad **Avlänka arbetsytan**.<br><br> ![Avlänka arbetsyta](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. Klicka på hello Avlänka från arbetsytan bladet **Avlänka arbetsyta**.<br><br> ![Avlänka arbetsytan bladet](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  Du får ett meddelande som verifierar du vill tooproceed.<br><br>
3. Medan Azure Automation försöker toounlink hello konto logganalys-arbetsytan, du kan följa förloppet för hello under **meddelanden** hello-menyn.

Om du har använt hello uppdateringshantering lösning kanske om du vill du vill tooremove hello följande objekt som inte längre behövs när du tar bort hello lösning.

* Uppdatera scheman.  Varje har namn som matchar hello distributioner av du skapade)

* Hybrid worker grupper som skapats för hello lösningen.  Varje får namnet på samma sätt för machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Om du använde hello Starta/stoppa virtuella datorer vid låg belastning på nätverket lösning kanske om du vill du vill tooremove hello följande objekt som inte längre behövs när du tar bort hello lösning.

* Starta och stoppa scheman för VM-runbook 
* Starta och stoppa runbooks för VM
* Variabler   

## <a name="next-steps"></a>Nästa steg

tooreconfigure toointegrate ditt Automation-konto med OMS Log Analytics finns [vidarebefordra jobbstatus och jobbet strömmar från Automation tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md). 
