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
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a><span data-ttu-id="fee63-103">Hur du avlänkar ditt Automation-konto från logganalys-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="fee63-103">How to unlink your Automation account from a Log Analytics workspace</span></span>

<span data-ttu-id="fee63-104">Azure Automation kan integreras med logganalys som inte bara stöd för Proaktiv övervakning av runbook-jobb över alla Automation-konton, men den krävs också när du importerar följande lösningar som är beroende av logganalys:</span><span class="sxs-lookup"><span data-stu-id="fee63-104">Azure Automation integrates with Log Analytics to not only support proactive monitoring of your runbook jobs across all of your Automation accounts, but is also required when you import the following solutions that are dependent on Log Analytics:</span></span>

* [<span data-ttu-id="fee63-105">Hantering av uppdateringar</span><span class="sxs-lookup"><span data-stu-id="fee63-105">Update Management</span></span>](../operations-management-suite/oms-solution-update-management.md)
* [<span data-ttu-id="fee63-106">Spårning av ändringar</span><span class="sxs-lookup"><span data-stu-id="fee63-106">Change Tracking</span></span>](../log-analytics/log-analytics-change-tracking.md)
* [<span data-ttu-id="fee63-107">Starta/Stoppa VMs kontorstid</span><span class="sxs-lookup"><span data-stu-id="fee63-107">Start/Stop VMs during off-hours</span></span>](automation-solution-vm-management.md)
 
<span data-ttu-id="fee63-108">Om du inte längre vill integrera ditt Automation-konto med logganalys Avlänka du ditt konto direkt från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fee63-108">If you decide you no longer wish to integrate your Automation account with Log Analytics, you can unlink your account directly from the Azure portal.</span></span>  <span data-ttu-id="fee63-109">Innan du fortsätter måste du först ta bort de lösningar som tidigare nämnts, annars kommer den här processen hindras från att du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="fee63-109">Before you proceed, you will first need to remove the solutions mentioned earlier, otherwise this process will be prevented from proceeding.</span></span>  <span data-ttu-id="fee63-110">Granska i avsnittet för en viss lösning som du har importerat för att förstå de steg som krävs för att ta bort den.</span><span class="sxs-lookup"><span data-stu-id="fee63-110">Review the topic for the particular solution you have imported to understand the steps required to remove it.</span></span>  

<span data-ttu-id="fee63-111">När du tar bort dessa lösningar kan du utföra följande steg om du vill Avlänka Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="fee63-111">After you remove these solutions you can perform the following steps to unlink your Automation account.</span></span>

## <a name="unlink-workspace"></a><span data-ttu-id="fee63-112">Avlänka arbetsytan</span><span class="sxs-lookup"><span data-stu-id="fee63-112">Unlink workspace</span></span>

1. <span data-ttu-id="fee63-113">Öppna ditt Automation-konto från Azure-portalen och markera i bladet Automation-konto i bladet konto **Avlänka arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="fee63-113">From the Azure portal, open your Automation account, and in the Automation account blade, in the account blade, select **Unlink workspace**.</span></span><br><br> <span data-ttu-id="fee63-114">![Avlänka arbetsyta](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span><span class="sxs-lookup"><span data-stu-id="fee63-114">![Unlink workspace option](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span></span><br><br>  
2. <span data-ttu-id="fee63-115">Klicka på bladet Avlänka från arbetsytan **Avlänka arbetsyta**.</span><span class="sxs-lookup"><span data-stu-id="fee63-115">On the Unlink workspace blade, click **Unlink worksapce**.</span></span><br><br> <span data-ttu-id="fee63-116">![Avlänka arbetsytan bladet](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span><span class="sxs-lookup"><span data-stu-id="fee63-116">![Unlink workspace blade](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span></span><br><br>  <span data-ttu-id="fee63-117">Ett meddelande visas där du bekräftar att du vill fortsätta.</span><span class="sxs-lookup"><span data-stu-id="fee63-117">You will receive a prompt verifying you wish to proceed.</span></span><br><br>
3. <span data-ttu-id="fee63-118">Medan Azure Automation försöker Avlänka kontot logganalys-arbetsytan, du kan följa förloppet under **meddelanden** på menyn.</span><span class="sxs-lookup"><span data-stu-id="fee63-118">While Azure Automation attempts to unlink the account your Log Analytics workspace, you can track the progress under **Notifications** from the menu.</span></span>

<span data-ttu-id="fee63-119">Om du har använt lösning för hantering av uppdateringar kanske (valfritt) du vill ta bort följande objekt som inte längre behövs när du tar bort lösningen.</span><span class="sxs-lookup"><span data-stu-id="fee63-119">If you used the Update Management solution, optionally you may want to remove the following items that are no longer needed after you remove the solution.</span></span>

* <span data-ttu-id="fee63-120">Uppdatera scheman.</span><span class="sxs-lookup"><span data-stu-id="fee63-120">Update schedules.</span></span>  <span data-ttu-id="fee63-121">Varje har namn som matchar uppdateringsdistributioner som du skapade)</span><span class="sxs-lookup"><span data-stu-id="fee63-121">Each will have names that match the update deployments you created)</span></span>

* <span data-ttu-id="fee63-122">Hybrid worker grupper som skapats för lösningen.</span><span class="sxs-lookup"><span data-stu-id="fee63-122">Hybrid worker groups created for the solution.</span></span>  <span data-ttu-id="fee63-123">Varje får namnet på samma sätt att machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).</span><span class="sxs-lookup"><span data-stu-id="fee63-123">Each will be named similarly to  machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8).</span></span>

<span data-ttu-id="fee63-124">Om du använde Starta/stoppa virtuella datorer under låg belastning på nätverket lösning kanske eventuellt du vill ta bort följande objekt som inte längre behövs när du tar bort lösningen.</span><span class="sxs-lookup"><span data-stu-id="fee63-124">If you used the Start/Stop VMs during off-hours solution, optionally you may want to remove the following items that are no longer needed after you remove the solution.</span></span>

* <span data-ttu-id="fee63-125">Starta och stoppa scheman för VM-runbook</span><span class="sxs-lookup"><span data-stu-id="fee63-125">Start and stop VM runbook schedules</span></span> 
* <span data-ttu-id="fee63-126">Starta och stoppa runbooks för VM</span><span class="sxs-lookup"><span data-stu-id="fee63-126">Start and stop VM runbooks</span></span>
* <span data-ttu-id="fee63-127">Variabler</span><span class="sxs-lookup"><span data-stu-id="fee63-127">Variables</span></span>   

## <a name="next-steps"></a><span data-ttu-id="fee63-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fee63-128">Next steps</span></span>

<span data-ttu-id="fee63-129">Om du vill konfigurera om ditt Automation-konto för att integrera med OMS Log Analytics finns [vidarebefordra jobbstatus och jobbet strömmar från Automation till logganalys (OMS)](automation-manage-send-joblogs-log-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="fee63-129">To reconfigure your Automation account to integrate with OMS Log Analytics, see [Forward job status and job streams from Automation to Log Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).</span></span> 