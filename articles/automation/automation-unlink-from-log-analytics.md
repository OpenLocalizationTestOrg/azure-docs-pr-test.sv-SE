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
# <a name="how-toounlink-your-automation-account-from-a-log-analytics-workspace"></a><span data-ttu-id="e581b-103">Hur toounlink ditt Automation-konto från logganalys-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="e581b-103">How toounlink your Automation account from a Log Analytics workspace</span></span>

<span data-ttu-id="e581b-104">Azure Automation kan integreras med logganalys toonot endast stöd för Proaktiv övervakning av runbook-jobb för alla Automation-konton, men den krävs också när du importerar följande lösningar som är beroende av logganalys hello:</span><span class="sxs-lookup"><span data-stu-id="e581b-104">Azure Automation integrates with Log Analytics toonot only support proactive monitoring of your runbook jobs across all of your Automation accounts, but is also required when you import hello following solutions that are dependent on Log Analytics:</span></span>

* [<span data-ttu-id="e581b-105">Hantering av uppdateringar</span><span class="sxs-lookup"><span data-stu-id="e581b-105">Update Management</span></span>](../operations-management-suite/oms-solution-update-management.md)
* [<span data-ttu-id="e581b-106">Spårning av ändringar</span><span class="sxs-lookup"><span data-stu-id="e581b-106">Change Tracking</span></span>](../log-analytics/log-analytics-change-tracking.md)
* [<span data-ttu-id="e581b-107">Starta/Stoppa VMs kontorstid</span><span class="sxs-lookup"><span data-stu-id="e581b-107">Start/Stop VMs during off-hours</span></span>](automation-solution-vm-management.md)
 
<span data-ttu-id="e581b-108">Om du inte längre vill toointegrate ditt Automation-konto med logganalys Avlänka du ditt konto direkt från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e581b-108">If you decide you no longer wish toointegrate your Automation account with Log Analytics, you can unlink your account directly from hello Azure portal.</span></span>  <span data-ttu-id="e581b-109">Innan du fortsätter måste du först tooremove hello lösningar som tidigare nämnts, annars kommer den här processen hindras från att du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="e581b-109">Before you proceed, you will first need tooremove hello solutions mentioned earlier, otherwise this process will be prevented from proceeding.</span></span>  <span data-ttu-id="e581b-110">Granska hello avsnittet för hello lösning som du har importerat toounderstand hello steg som krävs för tooremove den.</span><span class="sxs-lookup"><span data-stu-id="e581b-110">Review hello topic for hello particular solution you have imported toounderstand hello steps required tooremove it.</span></span>  

<span data-ttu-id="e581b-111">När du tar bort dessa lösningar kan du utföra följande steg toounlink hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="e581b-111">After you remove these solutions you can perform hello following steps toounlink your Automation account.</span></span>

## <a name="unlink-workspace"></a><span data-ttu-id="e581b-112">Avlänka arbetsytan</span><span class="sxs-lookup"><span data-stu-id="e581b-112">Unlink workspace</span></span>

1. <span data-ttu-id="e581b-113">Öppna ditt Automation-konto från hello Azure-portalen, och markera i hello Automation-kontoblad i hello-kontoblad **Avlänka arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="e581b-113">From hello Azure portal, open your Automation account, and in hello Automation account blade, in hello account blade, select **Unlink workspace**.</span></span><br><br> <span data-ttu-id="e581b-114">![Avlänka arbetsyta](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span><span class="sxs-lookup"><span data-stu-id="e581b-114">![Unlink workspace option](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span></span><br><br>  
2. <span data-ttu-id="e581b-115">Klicka på hello Avlänka från arbetsytan bladet **Avlänka arbetsyta**.</span><span class="sxs-lookup"><span data-stu-id="e581b-115">On hello Unlink workspace blade, click **Unlink worksapce**.</span></span><br><br> <span data-ttu-id="e581b-116">![Avlänka arbetsytan bladet](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span><span class="sxs-lookup"><span data-stu-id="e581b-116">![Unlink workspace blade](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span></span><br><br>  <span data-ttu-id="e581b-117">Du får ett meddelande som verifierar du vill tooproceed.</span><span class="sxs-lookup"><span data-stu-id="e581b-117">You will receive a prompt verifying you wish tooproceed.</span></span><br><br>
3. <span data-ttu-id="e581b-118">Medan Azure Automation försöker toounlink hello konto logganalys-arbetsytan, du kan följa förloppet för hello under **meddelanden** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="e581b-118">While Azure Automation attempts toounlink hello account your Log Analytics workspace, you can track hello progress under **Notifications** from hello menu.</span></span>

<span data-ttu-id="e581b-119">Om du har använt hello uppdateringshantering lösning kanske om du vill du vill tooremove hello följande objekt som inte längre behövs när du tar bort hello lösning.</span><span class="sxs-lookup"><span data-stu-id="e581b-119">If you used hello Update Management solution, optionally you may want tooremove hello following items that are no longer needed after you remove hello solution.</span></span>

* <span data-ttu-id="e581b-120">Uppdatera scheman.</span><span class="sxs-lookup"><span data-stu-id="e581b-120">Update schedules.</span></span>  <span data-ttu-id="e581b-121">Varje har namn som matchar hello distributioner av du skapade)</span><span class="sxs-lookup"><span data-stu-id="e581b-121">Each will have names that match hello update deployments you created)</span></span>

* <span data-ttu-id="e581b-122">Hybrid worker grupper som skapats för hello lösningen.</span><span class="sxs-lookup"><span data-stu-id="e581b-122">Hybrid worker groups created for hello solution.</span></span>  <span data-ttu-id="e581b-123">Varje får namnet på samma sätt för machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).</span><span class="sxs-lookup"><span data-stu-id="e581b-123">Each will be named similarly too machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8).</span></span>

<span data-ttu-id="e581b-124">Om du använde hello Starta/stoppa virtuella datorer vid låg belastning på nätverket lösning kanske om du vill du vill tooremove hello följande objekt som inte längre behövs när du tar bort hello lösning.</span><span class="sxs-lookup"><span data-stu-id="e581b-124">If you used hello Start/Stop VMs during off-hours solution, optionally you may want tooremove hello following items that are no longer needed after you remove hello solution.</span></span>

* <span data-ttu-id="e581b-125">Starta och stoppa scheman för VM-runbook</span><span class="sxs-lookup"><span data-stu-id="e581b-125">Start and stop VM runbook schedules</span></span> 
* <span data-ttu-id="e581b-126">Starta och stoppa runbooks för VM</span><span class="sxs-lookup"><span data-stu-id="e581b-126">Start and stop VM runbooks</span></span>
* <span data-ttu-id="e581b-127">Variabler</span><span class="sxs-lookup"><span data-stu-id="e581b-127">Variables</span></span>   

## <a name="next-steps"></a><span data-ttu-id="e581b-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e581b-128">Next steps</span></span>

<span data-ttu-id="e581b-129">tooreconfigure toointegrate ditt Automation-konto med OMS Log Analytics finns [vidarebefordra jobbstatus och jobbet strömmar från Automation tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="e581b-129">tooreconfigure your Automation account toointegrate with OMS Log Analytics, see [Forward job status and job streams from Automation tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).</span></span> 
