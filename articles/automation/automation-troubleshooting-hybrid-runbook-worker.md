---
title: "Felsöka Azure Automation Hybrid Runbook Worker | Microsoft Docs"
description: "Beskriv symptom, orsaker och lösningar för de vanligaste Hybrid Runbook Worker-problem i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 02c6606e-8924-4328-a196-45630c2255e9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: magoedte
ms.openlocfilehash: 9d1ceda5a072f494651a751a25a8ccf66e4c72ef
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-tips-for-hybrid-runbook-worker"></a><span data-ttu-id="98853-103">Felsökningstips för Hybrid Runbook Worker</span><span class="sxs-lookup"><span data-stu-id="98853-103">Troubleshooting tips for Hybrid Runbook Worker</span></span>

<span data-ttu-id="98853-104">Du får hjälp med att felsöka fel som du kan uppleva med Automation Hybrid Runbook Worker och ger förslag på lösningar för att lösa dem.</span><span class="sxs-lookup"><span data-stu-id="98853-104">This article provides help troubleshooting errors you might experience with Automation Hybrid Runbook Workers and suggests possible solutions to resolve them.</span></span>

## <a name="a-runbook-job-terminates-with-a-status-of-suspended"></a><span data-ttu-id="98853-105">En runbook-jobbet avslutas med statusen Pausad</span><span class="sxs-lookup"><span data-stu-id="98853-105">A runbook job terminates with a status of Suspended</span></span>

<span data-ttu-id="98853-106">Din runbook har pausats strax efter att försök att köra den tre gånger.</span><span class="sxs-lookup"><span data-stu-id="98853-106">Your runbook is suspended shortly after attempting to execute it three times.</span></span> <span data-ttu-id="98853-107">Det finns villkor som kan avbryta runbook slutförs och relaterade felmeddelandet innehåller inte någon ytterligare information som anger varför.</span><span class="sxs-lookup"><span data-stu-id="98853-107">There are conditions which may interrupt the runbook from completing successfully and the related error message does not include any additional information indicating why.</span></span> <span data-ttu-id="98853-108">Den här artikeln innehåller felsökningssteg för problem relaterade till Runbook Worker-Hybrid runbook körning av fel.</span><span class="sxs-lookup"><span data-stu-id="98853-108">This article provides troubleshooting steps for issues related to the Hybrid Runbook Worker runbook execution failures.</span></span>

<span data-ttu-id="98853-109">Om din Azure problemet inte åtgärdas i den här artikeln, besöker du Azure-forum på [MSDN och Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="98853-109">If your Azure issue is not addressed in this article, visit the Azure forums on [MSDN and the Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="98853-110">Du kan publicera problemet på dessa forum eller [ @AzureSupport på Twitter](https://twitter.com/AzureSupport).</span><span class="sxs-lookup"><span data-stu-id="98853-110">You can post your issue on these forums or to [@AzureSupport on Twitter](https://twitter.com/AzureSupport).</span></span> <span data-ttu-id="98853-111">Du kan också filen en Azure-supportbegäran genom att välja **få support** på den [Azure-supporten](https://azure.microsoft.com/support/options/) plats.</span><span class="sxs-lookup"><span data-stu-id="98853-111">Also, you can file an Azure support request by selecting **Get support** on the [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

### <a name="symptom"></a><span data-ttu-id="98853-112">Symtom</span><span class="sxs-lookup"><span data-stu-id="98853-112">Symptom</span></span>
<span data-ttu-id="98853-113">Runbook-körningen misslyckas och felet som returnerades är ”Jobbåtgärden 'Aktivera' inte kan köras eftersom processen oväntat stoppades.</span><span class="sxs-lookup"><span data-stu-id="98853-113">Runbook execution fails and the error returned is, "The job action 'Activate' cannot be run, because the process stopped unexpectedly.</span></span> <span data-ttu-id="98853-114">Jobbåtgärden gjordes 3 gånger ”.</span><span class="sxs-lookup"><span data-stu-id="98853-114">The job action was attempted 3 times."</span></span>

<span data-ttu-id="98853-115">Det finns flera möjliga orsaker till felet:</span><span class="sxs-lookup"><span data-stu-id="98853-115">There are several possible causes for the error:</span></span> 

1. <span data-ttu-id="98853-116">Worker-hybriden finns bakom en proxyserver eller brandvägg</span><span class="sxs-lookup"><span data-stu-id="98853-116">The hybrid worker is behind a proxy or firewall</span></span>
2. <span data-ttu-id="98853-117">Worker-hybriden körs på datorn har mindre än minst [maskinvarukrav](automation-offering-get-started.md#hybrid-runbook-worker)</span><span class="sxs-lookup"><span data-stu-id="98853-117">The computer the hybrid worker is running on has less than the minimum [hardware  requirements](automation-offering-get-started.md#hybrid-runbook-worker)</span></span>  
3. <span data-ttu-id="98853-118">Runbooks kan inte autentisera med lokala resurser</span><span class="sxs-lookup"><span data-stu-id="98853-118">The runbooks cannot authenticate with local resources</span></span>

#### <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a><span data-ttu-id="98853-119">Orsak 1: Runbook Worker-Hybrid är bakom en proxyserver eller brandvägg</span><span class="sxs-lookup"><span data-stu-id="98853-119">Cause 1: Hybrid Runbook Worker is behind proxy or firewall</span></span>
<span data-ttu-id="98853-120">Runbook Worker-hybriden körs på datorn finns bakom en brandvägg eller proxyserver server och utgående nätverksåtkomst kan inte beviljas eller konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="98853-120">The computer the Hybrid Runbook Worker is running on is behind a firewall or proxy server and outbound network access may not be permitted or configured correctly.</span></span>

#### <a name="solution"></a><span data-ttu-id="98853-121">Lösning</span><span class="sxs-lookup"><span data-stu-id="98853-121">Solution</span></span>
<span data-ttu-id="98853-122">Kontrollera att datorn är utgående åtkomst till *.azure automation.net på port 443.</span><span class="sxs-lookup"><span data-stu-id="98853-122">Verify the computer has outbound access to *.azure-automation.net on port 443.</span></span> 

#### <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a><span data-ttu-id="98853-123">Orsak 2: Datorn har mindre än minimikraven för maskinvara</span><span class="sxs-lookup"><span data-stu-id="98853-123">Cause 2: Computer has less than minimum hardware requirements</span></span>
<span data-ttu-id="98853-124">Datorer som kör Runbook Worker-hybriden ska uppfylla minimikraven för maskinvara innan du utser den som värd för den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="98853-124">Computers running the Hybrid Runbook Worker should meet the minimum hardware requirements before designating it to host this feature.</span></span> <span data-ttu-id="98853-125">Annars kommer att bli överbelastad datorn beroende på resursutnyttjande andra bakgrundsprocesser och konkurrens på grund av runbooks under körning och orsaka fördröjningar för runbook-jobb eller timeout.</span><span class="sxs-lookup"><span data-stu-id="98853-125">Otherwise, depending on the resource utilization of other background processes and contention caused by runbooks during execution, the computer will become over utilized and cause runbook job delays or timeouts.</span></span> 

#### <a name="solution"></a><span data-ttu-id="98853-126">Lösning</span><span class="sxs-lookup"><span data-stu-id="98853-126">Solution</span></span>
<span data-ttu-id="98853-127">Kontrollera först tilldelad att köra funktionen Hybrid Runbook Worker datorn uppfyller minimikraven på maskinvara.</span><span class="sxs-lookup"><span data-stu-id="98853-127">First confirm the computer designated to run the Hybrid Runbook Worker feature meets the minimum hardware requirements.</span></span>  <span data-ttu-id="98853-128">Om den finns, kan du övervaka användningen av CPU och minne för att fastställa alla korrelation mellan prestanda för Hybrid Runbook Worker-processer och Windows.</span><span class="sxs-lookup"><span data-stu-id="98853-128">If it does, monitor CPU and memory utilization to determine any correlation between the performance of Hybrid Runbook Worker processes and Windows.</span></span>  <span data-ttu-id="98853-129">Om det finns minne eller CPU-belastning, kan detta tyda på att behöva uppgradera eller lägga till fler processorer eller öka minne för att adressera resurs flaskhalsar och åtgärda felet.</span><span class="sxs-lookup"><span data-stu-id="98853-129">If there is memory or CPU pressure, this may indicate the need to upgrade or add additional processors, or increase memory to address the resource bottleneck and resolve the error.</span></span> <span data-ttu-id="98853-130">Du kan också markera en annan beräkningsresurser som stöder de minimikrav och skala när arbetsbelastning indikera att öka krävs.</span><span class="sxs-lookup"><span data-stu-id="98853-130">Alternatively, select a different compute resource that can support the minimum requirements and scale when workload demands indicate an increase is necessary.</span></span>         

#### <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a><span data-ttu-id="98853-131">Orsak 3: Runbooks inte autentisera med lokala resurser</span><span class="sxs-lookup"><span data-stu-id="98853-131">Cause 3: Runbooks cannot authenticate with local resources</span></span>

#### <a name="solution"></a><span data-ttu-id="98853-132">Lösning</span><span class="sxs-lookup"><span data-stu-id="98853-132">Solution</span></span>
<span data-ttu-id="98853-133">Kontrollera den **Microsoft SMA** händelseloggen för motsvarande händelse med beskrivning *Win32 processen avslutades med koden [4294967295]*.</span><span class="sxs-lookup"><span data-stu-id="98853-133">Check the **Microsoft-SMA** event log for a corresponding event with description *Win32 Process Exited with code [4294967295]*.</span></span>  <span data-ttu-id="98853-134">Orsaken till felet är att du inte har konfigurerat autentisering i dina runbooks eller angivna kör som-autentiseringsuppgifter för Hybrid worker-gruppen.</span><span class="sxs-lookup"><span data-stu-id="98853-134">The cause of this error is you haven't configured authentication in your runbooks or specified the Run As credentials for the Hybrid worker group.</span></span>  <span data-ttu-id="98853-135">Granska [Runbook-behörigheter](automation-hrw-run-runbooks.md#runbook-permissions) att bekräfta att du har konfigurerat autentisering korrekt för dina runbooks.</span><span class="sxs-lookup"><span data-stu-id="98853-135">Please review [Runbook permissions](automation-hrw-run-runbooks.md#runbook-permissions) to confirm you have correctly configured authentication for your runbooks.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="98853-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="98853-136">Next steps</span></span>

<span data-ttu-id="98853-137">Hjälp med felsökning av andra problem i Automation finns [felsökning av vanliga problem med Azure Automation](automation-troubleshooting-automation-errors.md)</span><span class="sxs-lookup"><span data-stu-id="98853-137">For help troubleshooting other issues in Automation, see [Troubleshooting common Azure Automation issues](automation-troubleshooting-automation-errors.md)</span></span> 