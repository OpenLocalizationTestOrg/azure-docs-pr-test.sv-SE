---
title: Azure automation aaaAdd runbooks toorecovery planer i hello klassiska portal | Microsoft Docs
description: "Den här artikeln beskrivs hur Azure Site Recovery kan du nu tooextend återställningsplaner med hjälp av Azure Automation toocomplete komplicerade uppgifter under återställning tooAzure"
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: f24eaa62-9dea-4fce-92e1-a72513ca0289
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 3bb7420911afbce289b656f28823b1923e8af0c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans-in-hello-classic-portal"></a><span data-ttu-id="2bb28-103">Lägg till Azure automation-runbooks toorecovery planer i hello klassiska portalen</span><span class="sxs-lookup"><span data-stu-id="2bb28-103">Add Azure automation runbooks toorecovery plans in hello classic portal</span></span>
<span data-ttu-id="2bb28-104">Den här självstudiekursen beskrivs hur Azure Site Recovery kan integreras med Azure Automation tooprovide utökningsbarhet toorecovery planer.</span><span class="sxs-lookup"><span data-stu-id="2bb28-104">This tutorial describes how Azure Site Recovery integrates with Azure Automation tooprovide extensibility toorecovery plans.</span></span> <span data-ttu-id="2bb28-105">Återställningsplaner kan samordna återställning av dina virtuella datorer som skyddas med Azure Site Recovery för replikering tooAzure scenarier och replikering toosecondary moln.</span><span class="sxs-lookup"><span data-stu-id="2bb28-105">Recovery plans can orchestrate recovery of your virtual machines protected using Azure Site Recovery for both replication toosecondary cloud and replication tooAzure scenarios.</span></span> <span data-ttu-id="2bb28-106">De hjälper också att hello recovery **konsekvent korrekt**, **repeterbara**, och **automatiserad**.</span><span class="sxs-lookup"><span data-stu-id="2bb28-106">They also help in making hello recovery **consistently accurate**, **repeatable**, and **automated**.</span></span> <span data-ttu-id="2bb28-107">Om du misslyckas över dina virtuella datorer tooAzure integrering med Azure Automation utökar återställningsplaner och ger dig kapaciteten tooexecute runbooks, vilket ger kraftfulla automation-aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="2bb28-107">If you are failing over your virtual machines tooAzure, integration with Azure Automation extends the recovery plans and gives you capability tooexecute runbooks, thus allowing powerful automation tasks.</span></span>

<span data-ttu-id="2bb28-108">Registrera dig om du inte har lyssnat på om Azure Automation ännu, [här](https://azure.microsoft.com/services/automation/) och hämta sina exempelskript [här](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="2bb28-108">If you have not heard about Azure Automation yet, sign up [here](https://azure.microsoft.com/services/automation/) and download their sample scripts [here](https://azure.microsoft.com/documentation/scripts/).</span></span> <span data-ttu-id="2bb28-109">Läs mer om [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) och hur tooorchestrate recovery tooAzure med hjälp av recovery planerar [här](https://azure.microsoft.com/blog/?p=166264).</span><span class="sxs-lookup"><span data-stu-id="2bb28-109">Read more about [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) and how tooorchestrate recovery tooAzure using recovery plans [here](https://azure.microsoft.com/blog/?p=166264).</span></span>

<span data-ttu-id="2bb28-110">I den här korta kursen ska vi titta på hur du kan integrera Azure Automation-runbooks i återställningsplaner.</span><span class="sxs-lookup"><span data-stu-id="2bb28-110">In this short tutorial, we will look at how you can integrate Azure Automation runbooks into recovery plans.</span></span> <span data-ttu-id="2bb28-111">Kommer att automatisera enkla uppgifter som tidigare krävde manuella åtgärder och se hur tooconvert en flera steg återställning till en återställningsåtgärd med enkelklickning.</span><span class="sxs-lookup"><span data-stu-id="2bb28-111">We will automate simple tasks that earlier required manual intervention and see how tooconvert a multi step recovery into a single-click recovery action.</span></span> <span data-ttu-id="2bb28-112">Vi kommer också titta på hur du felsöker ett enkelt skript om det händer.</span><span class="sxs-lookup"><span data-stu-id="2bb28-112">We will also look at how you can troubleshoot a simple script if it goes wrong.</span></span>

## <a name="protect-hello-application-tooazure"></a><span data-ttu-id="2bb28-113">Skydda hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="2bb28-113">Protect hello application tooAzure</span></span>
<span data-ttu-id="2bb28-114">Låt oss börja med ett enkelt program som består av två virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2bb28-114">Let us begin with a simple application consisting of two virtual machines.</span></span> <span data-ttu-id="2bb28-115">Här, har vi HRweb-programmet för Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="2bb28-115">Here, we have a HRweb application of Fabrikam.</span></span> <span data-ttu-id="2bb28-116">Fabrikam-HRweb-klient- och Fabrikam-Hrweb-servergränssnitten är hello två virtuella datorer skyddas tooAzure med hjälp av Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="2bb28-116">Fabrikam-HRweb-frontend and Fabrikam-Hrweb-backend are hello two virtual machines protected tooAzure using Azure Site Recovery.</span></span> <span data-ttu-id="2bb28-117">tooprotect hello virtuella datorer med Azure Site Recovery gör hello nedan.</span><span class="sxs-lookup"><span data-stu-id="2bb28-117">tooprotect hello virtual machines using Azure Site Recovery, follow hello steps below.</span></span>

1. <span data-ttu-id="2bb28-118">Aktivera skydd för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2bb28-118">Enable protection for your virtual machines.</span></span>
2. <span data-ttu-id="2bb28-119">Kontrollera att hello virtuella datorer har slutfört den inledande replikeringen och replikerar.</span><span class="sxs-lookup"><span data-stu-id="2bb28-119">Ensure that hello virtual machines have completed initial replication and are replicating.</span></span>
3. <span data-ttu-id="2bb28-120">Vänta tills hello den inledande replikeringen har slutförts och hello replikeringsstatus står skyddade.</span><span class="sxs-lookup"><span data-stu-id="2bb28-120">Wait till hello initial replication completes and hello Replication status says Protected.</span></span>

## ![](media/site-recovery-runbook-automation/01.png)
<span data-ttu-id="2bb28-121">I den här självstudiekursen skapar vi en återställningsplan för hello Fabrikam HRweb programmet toofailover hello programmet tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2bb28-121">In this tutorial, we will create a recovery plan for hello Fabrikam HRweb application toofailover hello application tooAzure.</span></span> <span data-ttu-id="2bb28-122">Vi kommer sedan integrera den med en runbook som skapar en slutpunkt på hello redundansväxlats virtuella Azure-datorn tooserve webbsidor på port 80.</span><span class="sxs-lookup"><span data-stu-id="2bb28-122">Then we will integrate it with a runbook that will create an endpoint on hello failed over Azure virtual machine tooserve web pages at port 80.</span></span>

<span data-ttu-id="2bb28-123">Först ska vi skapa en återställningsplan för vårt program.</span><span class="sxs-lookup"><span data-stu-id="2bb28-123">First, let's create a recovery plan for our application.</span></span>

## <a name="create-hello-recovery-plan"></a><span data-ttu-id="2bb28-124">Skapa hello återställningsplan</span><span class="sxs-lookup"><span data-stu-id="2bb28-124">Create hello recovery plan</span></span>
<span data-ttu-id="2bb28-125">toorecover hello programmet tooAzure måste toocreate en återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="2bb28-125">toorecover hello application tooAzure, you need toocreate a recovery plan.</span></span>
<span data-ttu-id="2bb28-126">Med hjälp av en återställningsplan som du kan ange hello ordning för återställning av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2bb28-126">Using a recovery plan you can specify hello order of recovery of the virtual machines.</span></span> <span data-ttu-id="2bb28-127">hello virtuella datorn placeras i grupp 1 ska återställa och starta först och sedan hello virtuell dator i grupp 2 följer.</span><span class="sxs-lookup"><span data-stu-id="2bb28-127">hello virtual machine placed in group 1 will recover and start first, and then hello virtual machine in group 2 will follow.</span></span>

<span data-ttu-id="2bb28-128">Skapa en återställningspunkt planera som ser ut som nedan.</span><span class="sxs-lookup"><span data-stu-id="2bb28-128">Create a Recovery Plan that looks like below.</span></span>

![](media/site-recovery-runbook-automation/12.png)

<span data-ttu-id="2bb28-129">Mer om återställningsplaner dokumentationen tooread [här](https://msdn.microsoft.com/library/azure/dn788799.aspx "här").</span><span class="sxs-lookup"><span data-stu-id="2bb28-129">tooread more about recovery plans, read documentation [here](https://msdn.microsoft.com/library/azure/dn788799.aspx "here").</span></span>

<span data-ttu-id="2bb28-130">Därefter skapar vi hello nödvändiga artefakter i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="2bb28-130">Next, let's create hello necessary artifacts in Azure Automation.</span></span>

## <a name="create-hello-automation-account-and-its-assets"></a><span data-ttu-id="2bb28-131">Skapa hello automation-kontot och dess tillgångar</span><span class="sxs-lookup"><span data-stu-id="2bb28-131">Create hello automation account and its assets</span></span>
<span data-ttu-id="2bb28-132">Du behöver ett Azure Automation-konto toocreate runbooks.</span><span class="sxs-lookup"><span data-stu-id="2bb28-132">You need an Azure Automation account toocreate runbooks.</span></span> <span data-ttu-id="2bb28-133">Om du inte redan har ett konto navigerar tooAzure Automation fliken med ![](media/site-recovery-runbook-automation/02.png)och skapa ett nytt konto.</span><span class="sxs-lookup"><span data-stu-id="2bb28-133">If you do not already have an account, navigate tooAzure Automation tab denoted by ![](media/site-recovery-runbook-automation/02.png)and create a new account.</span></span>

1. <span data-ttu-id="2bb28-134">Ge hello konto en namnet tooidentify med.</span><span class="sxs-lookup"><span data-stu-id="2bb28-134">Give hello account a name tooidentify with.</span></span>
2. <span data-ttu-id="2bb28-135">Ange en geografisk region där du vill att tooplace hello-konto.</span><span class="sxs-lookup"><span data-stu-id="2bb28-135">Specify a geographical region where you want tooplace hello account.</span></span>

<span data-ttu-id="2bb28-136">Det rekommenderas tooplace hello konto i hello samma region som hello ASR valvet.</span><span class="sxs-lookup"><span data-stu-id="2bb28-136">It is recommended tooplace hello account in hello same region as hello ASR vault.</span></span>

![](media/site-recovery-runbook-automation/03.png)

<span data-ttu-id="2bb28-137">Skapa sedan hello följande tillgångar i hello konto.</span><span class="sxs-lookup"><span data-stu-id="2bb28-137">Next, create hello following assets in hello Account.</span></span>

### <a name="add-a-subscription-name-as-asset"></a><span data-ttu-id="2bb28-138">Lägg till ett prenumerationsnamn som tillgång</span><span class="sxs-lookup"><span data-stu-id="2bb28-138">Add a subscription name as asset</span></span>
1. <span data-ttu-id="2bb28-139">Lägg till en ny inställning ![](media/site-recovery-runbook-automation/04.png) i hello Azure Automation-tillgångar och välj för![](media/site-recovery-runbook-automation/05.png)</span><span class="sxs-lookup"><span data-stu-id="2bb28-139">Add a new setting ![](media/site-recovery-runbook-automation/04.png) in hello Azure Automation Assets and select too![](media/site-recovery-runbook-automation/05.png)</span></span>
2. <span data-ttu-id="2bb28-140">Välj hello variabeltyp som **sträng**</span><span class="sxs-lookup"><span data-stu-id="2bb28-140">Select hello variable type as **String**</span></span>
3. <span data-ttu-id="2bb28-141">Ange variabelnamnet som **AzureSubscriptionName**</span><span class="sxs-lookup"><span data-stu-id="2bb28-141">Specify variable name as **AzureSubscriptionName**</span></span>

   ![](media/site-recovery-runbook-automation/06.png)
4. <span data-ttu-id="2bb28-142">Ange ditt faktiska namn i Azure-prenumeration som hello variabelvärdet.</span><span class="sxs-lookup"><span data-stu-id="2bb28-142">Specify your actual Azure Subscription name as hello variable value.</span></span>

   ![](media/site-recovery-runbook-automation/07_1.png)

<span data-ttu-id="2bb28-143">Du kan identifiera hello namnet på din prenumeration från hello inställningssidan för ditt konto på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2bb28-143">You can identify hello name of your subscription from hello settings page of your account on hello Azure portal.</span></span>

### <a name="add-an-azure-login-credential-as-asset"></a><span data-ttu-id="2bb28-144">Lägg till en Azure-inloggning autentiseringsuppgift som tillgång</span><span class="sxs-lookup"><span data-stu-id="2bb28-144">Add an Azure login credential as asset</span></span>
<span data-ttu-id="2bb28-145">Azure Automation använder Azure PowerShell tooconnect toothe prenumeration och fungerar på det hello-artefakter.</span><span class="sxs-lookup"><span data-stu-id="2bb28-145">Azure Automation uses Azure PowerShell tooconnect toothe subscription and operates on hello artifacts there.</span></span> <span data-ttu-id="2bb28-146">För att göra detta måste autentiseras med ditt Microsoft-konto eller ett arbets- eller skolkonto.</span><span class="sxs-lookup"><span data-stu-id="2bb28-146">For this, you need to authenticate using your Microsoft account or a work or school account.</span></span>
<span data-ttu-id="2bb28-147">Du kan lagra hello autentiseringsuppgifter i en tillgång toobe används på ett säkert sätt av hello runbook.</span><span class="sxs-lookup"><span data-stu-id="2bb28-147">You can store hello account credentials in an asset toobe used securely by hello runbook.</span></span>

1. <span data-ttu-id="2bb28-148">Lägg till en ny inställning ![](media/site-recovery-runbook-automation/04.png) i hello Azure Automation-tillgångar och välj![](media/site-recovery-runbook-automation/09.png)</span><span class="sxs-lookup"><span data-stu-id="2bb28-148">Add a new setting ![](media/site-recovery-runbook-automation/04.png) in hello Azure Automation Assets and select ![](media/site-recovery-runbook-automation/09.png)</span></span>
2. <span data-ttu-id="2bb28-149">Välj hello autentiseringstyp som **Windows PowerShell-autentiseringsuppgift**</span><span class="sxs-lookup"><span data-stu-id="2bb28-149">Select hello Credential type as **Windows PowerShell Credential**</span></span>
3. <span data-ttu-id="2bb28-150">Ange hello namn som **AzureCredential**</span><span class="sxs-lookup"><span data-stu-id="2bb28-150">Specify hello name as **AzureCredential**</span></span>

   ![](media/site-recovery-runbook-automation/10.png)
4. <span data-ttu-id="2bb28-151">Ange hello användarnamn och lösenord toosign i med.</span><span class="sxs-lookup"><span data-stu-id="2bb28-151">Specify hello username and password toosign-in with.</span></span>

<span data-ttu-id="2bb28-152">Båda dessa inställningar är nu tillgänglig i dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="2bb28-152">Now both these settings are available in your assets.</span></span>

![](media/site-recovery-runbook-automation/11.png)

<span data-ttu-id="2bb28-153">Mer information om hur tooconnect tooyour prenumeration via PowerShell ges [här](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2bb28-153">More information about how tooconnect tooyour subscription via PowerShell is given [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="2bb28-154">Därefter skapar du en runbook i Azure Automation kan lägga till en slutpunkt för hello frontend virtuella datorn efter redundans.</span><span class="sxs-lookup"><span data-stu-id="2bb28-154">Next, you will create a runbook in Azure Automation that can add an endpoint for hello front-end virtual machine after failover.</span></span>

## <a name="azure-automation-context"></a><span data-ttu-id="2bb28-155">Azure automation-kontexten</span><span class="sxs-lookup"><span data-stu-id="2bb28-155">Azure automation context</span></span>
<span data-ttu-id="2bb28-156">ASR skickar en kontext variabeln toohello runbook toohelp du skriva deterministiska skript.</span><span class="sxs-lookup"><span data-stu-id="2bb28-156">ASR passes a context variable toohello runbook toohelp you write deterministic scripts.</span></span> <span data-ttu-id="2bb28-157">Något gick argumentera att hello namnen på hello Molntjänsten och hello virtuell dator är förutsägbar, men händer att det inte är alltid hello fall på grund av toocertain scenarier, till exempel hello en där hello namnet på hello virtuell dator kan ha ändrats på grund av toounsupported tecken i Azure.</span><span class="sxs-lookup"><span data-stu-id="2bb28-157">One could argue that hello names of hello Cloud Service and hello Virtual Machine are predictable, but happens that it is not always hello case owing toocertain scenarios such as hello one where hello name of hello virtual machine name might have changed due toounsupported characters in Azure.</span></span> <span data-ttu-id="2bb28-158">Därför informationen skickas toohello ASR återställningsplan som en del av hello *kontexten*.</span><span class="sxs-lookup"><span data-stu-id="2bb28-158">Hence this information is passed toohello ASR recovery plan as part of hello *context*.</span></span>

<span data-ttu-id="2bb28-159">Nedan visas ett exempel på hur hello kontexten variabeln ser ut.</span><span class="sxs-lookup"><span data-stu-id="2bb28-159">Below is an example of how hello context variable looks.</span></span>

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


<span data-ttu-id="2bb28-160">hello tabellen nedan innehåller namn och beskrivning för varje variabel i hello-kontexten.</span><span class="sxs-lookup"><span data-stu-id="2bb28-160">hello table below contains name and description for each variable in hello context.</span></span>

| <span data-ttu-id="2bb28-161">**Variabelnamn**</span><span class="sxs-lookup"><span data-stu-id="2bb28-161">**Variable name**</span></span> | <span data-ttu-id="2bb28-162">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="2bb28-162">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="2bb28-163">RecoveryPlanName</span><span class="sxs-lookup"><span data-stu-id="2bb28-163">RecoveryPlanName</span></span> |<span data-ttu-id="2bb28-164">Namnet på planen som körs.</span><span class="sxs-lookup"><span data-stu-id="2bb28-164">Name of plan being run.</span></span> <span data-ttu-id="2bb28-165">Hjälper till att du vidtar åtgärder baserat på namn med hello samma skript</span><span class="sxs-lookup"><span data-stu-id="2bb28-165">Helps you take action based on name using hello same script</span></span> |
| <span data-ttu-id="2bb28-166">FailoverType</span><span class="sxs-lookup"><span data-stu-id="2bb28-166">FailoverType</span></span> |<span data-ttu-id="2bb28-167">Anger om hello växling vid fel är testa planerad eller oplanerad.</span><span class="sxs-lookup"><span data-stu-id="2bb28-167">Specifies whether hello failover is test, planned, or unplanned.</span></span> |
| <span data-ttu-id="2bb28-168">FailoverDirection</span><span class="sxs-lookup"><span data-stu-id="2bb28-168">FailoverDirection</span></span> |<span data-ttu-id="2bb28-169">Ange om återställning är tooprimary eller sekundär</span><span class="sxs-lookup"><span data-stu-id="2bb28-169">Specify whether recovery is tooprimary or secondary</span></span> |
| <span data-ttu-id="2bb28-170">Grupp-ID</span><span class="sxs-lookup"><span data-stu-id="2bb28-170">GroupID</span></span> |<span data-ttu-id="2bb28-171">Identifiera hello gruppnumret inom hello återställningsplan när hello plan körs</span><span class="sxs-lookup"><span data-stu-id="2bb28-171">Identify hello group number within hello recovery plan when hello plan is running</span></span> |
| <span data-ttu-id="2bb28-172">VmMap</span><span class="sxs-lookup"><span data-stu-id="2bb28-172">VmMap</span></span> |<span data-ttu-id="2bb28-173">Matris med alla hello virtuella datorer i gruppen hello</span><span class="sxs-lookup"><span data-stu-id="2bb28-173">Array of all hello virtual machines in hello group</span></span> |
| <span data-ttu-id="2bb28-174">VMMap nyckel</span><span class="sxs-lookup"><span data-stu-id="2bb28-174">VMMap key</span></span> |<span data-ttu-id="2bb28-175">Unik nyckel (GUID) för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2bb28-175">Unique key (GUID) for each VM.</span></span> <span data-ttu-id="2bb28-176">Det har hello samtidigt som hello-ID för VMM för hello virtuell dator i tillämpliga fall.</span><span class="sxs-lookup"><span data-stu-id="2bb28-176">It's hello same as hello VMM ID of hello virtual machine where applicable.</span></span> |
| <span data-ttu-id="2bb28-177">RoleName</span><span class="sxs-lookup"><span data-stu-id="2bb28-177">RoleName</span></span> |<span data-ttu-id="2bb28-178">Namnet på hello Azure VM som återställs</span><span class="sxs-lookup"><span data-stu-id="2bb28-178">Name of hello Azure VM that's being recovered</span></span> |
| <span data-ttu-id="2bb28-179">CloudServiceName</span><span class="sxs-lookup"><span data-stu-id="2bb28-179">CloudServiceName</span></span> |<span data-ttu-id="2bb28-180">Azure Cloud Service namn under vilka hello virtuell dator skapas.</span><span class="sxs-lookup"><span data-stu-id="2bb28-180">Azure Cloud Service name under which hello virtual machine is created.</span></span> |

<span data-ttu-id="2bb28-181">tooidentify hello VmMap Key i hello kontexten du kan också gå toohello VM egenskapssidan i ASR och titta på hello VM-GUID-egenskap.</span><span class="sxs-lookup"><span data-stu-id="2bb28-181">tooidentify hello VmMap Key in hello context you could also go toohello VM properties page in ASR and look at hello VM GUID property.</span></span>

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a><span data-ttu-id="2bb28-182">Skapa en Automation-runbook</span><span class="sxs-lookup"><span data-stu-id="2bb28-182">Author an Automation runbook</span></span>
<span data-ttu-id="2bb28-183">Nu skapa hello runbook tooopen port 80 på hello frontend virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2bb28-183">Now create hello runbook tooopen port 80 on hello front-end virtual machine.</span></span>

1. <span data-ttu-id="2bb28-184">Skapa en ny runbook i hello Azure Automation-konto med namnet hello **OpenPort80**</span><span class="sxs-lookup"><span data-stu-id="2bb28-184">Create a new runbook in hello Azure Automation account with hello name **OpenPort80**</span></span>

   ![](media/site-recovery-runbook-automation/14.png)
2. <span data-ttu-id="2bb28-185">Navigera toohello författare vy över hello runbook och ange hello utkastläge.</span><span class="sxs-lookup"><span data-stu-id="2bb28-185">Navigate toohello Author view of hello runbook and enter hello draft mode.</span></span>
3. <span data-ttu-id="2bb28-186">Ange först hello variabeln toouse som hello recovery plan kontext</span><span class="sxs-lookup"><span data-stu-id="2bb28-186">First specify hello variable toouse as hello recovery plan context</span></span>

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. <span data-ttu-id="2bb28-187">Sedan Anslut toohello prenumeration med hjälp av hello autentiseringsuppgifter och prenumeration namn</span><span class="sxs-lookup"><span data-stu-id="2bb28-187">Next connect toohello subscription using hello credential and subscription name</span></span>

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect tooAzure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   <span data-ttu-id="2bb28-188">Observera att du använder hello Azure tillgångar – **AzureCredential** och **AzureSubscriptionName** här.</span><span class="sxs-lookup"><span data-stu-id="2bb28-188">Note that you use hello Azure assets – **AzureCredential** and **AzureSubscriptionName** here.</span></span>
5. <span data-ttu-id="2bb28-189">Nu ange information om hello och hello GUID för hello virtuell dator som du vill tooexpose hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2bb28-189">Now specify hello endpoint details and hello GUID of hello virtual machine for which you want tooexpose hello endpoint.</span></span> <span data-ttu-id="2bb28-190">I det här fallet hello frontend virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2bb28-190">In this case hello front-end virtual machine.</span></span>

   ```
       # Specify hello parameters toobe used by hello script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   <span data-ttu-id="2bb28-191">Anger hello Azure endpoint-protokollet, lokal port på hello VM och dess mappade offentlig port.</span><span class="sxs-lookup"><span data-stu-id="2bb28-191">This specifies hello Azure endpoint protocol, local port on hello VM and its mapped public port.</span></span> <span data-ttu-id="2bb28-192">Dessa variabler är parametrar som krävs av hello Azure kommandon som lägga till slutpunkter tooVMs.</span><span class="sxs-lookup"><span data-stu-id="2bb28-192">These variables are parameters     required by hello Azure commands that add endpoints tooVMs.</span></span> <span data-ttu-id="2bb28-193">Hej VMGUID innehåller hello hello virtuell dator som du behöver toooperate på GUID.</span><span class="sxs-lookup"><span data-stu-id="2bb28-193">hello VMGUID holds hello GUID of hello virtual machine you need toooperate on.</span></span>
6. <span data-ttu-id="2bb28-194">hello-skriptet kommer nu extrahera hello kontext för hello angivna VM-GUID och skapa en slutpunkt på hello virtuell dator som den refererar till.</span><span class="sxs-lookup"><span data-stu-id="2bb28-194">hello script will now extract hello context for hello given VM GUID and create an endpoint on hello virtual machine referenced by it.</span></span>

   ```
       #Read hello VM GUID from hello context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke hello necessary pipeline commands tooadd a Azure Endpoint tooa specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. <span data-ttu-id="2bb28-195">När den är klar, klicka på Publicera ![](media/site-recovery-runbook-automation/20.png) tooallow din toobe för skript som är tillgänglig för körning.</span><span class="sxs-lookup"><span data-stu-id="2bb28-195">Once this is complete, hit Publish ![](media/site-recovery-runbook-automation/20.png) tooallow your script toobe available for execution.</span></span>

<span data-ttu-id="2bb28-196">hello fullständiga skriptet anges nedan som referens</span><span class="sxs-lookup"><span data-stu-id="2bb28-196">hello complete script is given below for your reference</span></span>

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect tooAzure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify hello parameters toobe used by hello script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read hello VM GUID from hello context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke hello necessary pipeline commands tooadd an Azure Endpoint tooa specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-hello-script-toohello-recovery-plan"></a><span data-ttu-id="2bb28-197">Lägg till hello skriptet toohello återställningsplan</span><span class="sxs-lookup"><span data-stu-id="2bb28-197">Add hello script toohello recovery plan</span></span>
<span data-ttu-id="2bb28-198">När hello skriptet är klar kan lägga du toohello återställningsplan som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="2bb28-198">Once hello script is ready, you can add it toohello recovery plan that you created earlier.</span></span>

1. <span data-ttu-id="2bb28-199">Välj tooadd ett skript i hello återställningsplan som du har skapat, efter grupp 2.</span><span class="sxs-lookup"><span data-stu-id="2bb28-199">In hello recovery plan you created, choose tooadd a script after the group 2.</span></span> ![](media/site-recovery-runbook-automation/15.png)
2. <span data-ttu-id="2bb28-200">Ange ett skriptnamn.</span><span class="sxs-lookup"><span data-stu-id="2bb28-200">Specify a script name.</span></span> <span data-ttu-id="2bb28-201">Detta är ett eget namn för det här skriptet för visar inom hello återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="2bb28-201">This is just a friendly name for this script for showing within hello Recovery plan.</span></span>
3. <span data-ttu-id="2bb28-202">Hello redundans tooAzure skript – Välj i hello Azure Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="2bb28-202">In hello failover tooAzure script – Select hello Azure Automation Account name.</span></span>
4. <span data-ttu-id="2bb28-203">Hello Azure Runbooks, Välj hello runbook som du skapade.</span><span class="sxs-lookup"><span data-stu-id="2bb28-203">In hello Azure Runbooks, select hello runbook you authored.</span></span>

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a><span data-ttu-id="2bb28-204">Primär skript</span><span class="sxs-lookup"><span data-stu-id="2bb28-204">Primary side scripts</span></span>
<span data-ttu-id="2bb28-205">När du kör en redundansväxling tooAzure, kan du också välja tooexecute primära skript.</span><span class="sxs-lookup"><span data-stu-id="2bb28-205">When you are executing a failover tooAzure, you can also choose tooexecute primary side scripts.</span></span> <span data-ttu-id="2bb28-206">Dessa skript körs på hello VMM-servern under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="2bb28-206">These scripts will run on hello VMM server during failover.</span></span>
<span data-ttu-id="2bb28-207">Primär klientsidans skript finns bara tillgänglig enbart för före avstängning och efter avstängning steg.</span><span class="sxs-lookup"><span data-stu-id="2bb28-207">Primary side scripts are only available only for pre-shutdown and post shutdown stages.</span></span> <span data-ttu-id="2bb28-208">Det beror på att vi räknar med hello primär plats toobe vanligtvis inte tillgängligt när en katastrof inträffar.</span><span class="sxs-lookup"><span data-stu-id="2bb28-208">This is because we expect hello primary site toobe typically unavailable when a disaster strikes.</span></span>
<span data-ttu-id="2bb28-209">Under en oplanerad redundans endast om du väljer i för primär plats, görs ett försök toorun hello primära skript.</span><span class="sxs-lookup"><span data-stu-id="2bb28-209">During an unplanned failover, only if you opt in for primary site operations, it will attempt toorun hello primary side scripts.</span></span> <span data-ttu-id="2bb28-210">Om de inte kan nås eller timeout, hello redundans fortsätter hello toorecover virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2bb28-210">If they are not reachable or timeout, hello failover will continue toorecover hello virtual machines.</span></span>
<span data-ttu-id="2bb28-211">Primär skript finns icke för VMware/fysisk/Hyper-v-platser utan VMM skyddade tooAzure - under växling vid fel tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2bb28-211">Primary side scripts are un-available for VMware/Physical/Hyper-v Sites without VMM protected tooAzure - while you failover tooAzure.</span></span>
<span data-ttu-id="2bb28-212">Men när återställning från Azure tooon lokal primär skript (Runbooks) kan användas för alla mål förutom VMware.</span><span class="sxs-lookup"><span data-stu-id="2bb28-212">However, when you failback from Azure tooon-premises, primary side scripts (Runbooks) can be used for all targets except VMware.</span></span>

## <a name="test-hello-recovery-plan"></a><span data-ttu-id="2bb28-213">Testplan hello återställning</span><span class="sxs-lookup"><span data-stu-id="2bb28-213">Test hello recovery plan</span></span>
<span data-ttu-id="2bb28-214">När du har lagt till hello runbook toohello planen kan du initiera ett redundanstest och se hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="2bb28-214">Once you have added hello runbook toohello plan you can initiate a test failover and see it in action.</span></span> <span data-ttu-id="2bb28-215">Det är alltid rekommenderas toorun en testa redundans tootest dina program och hello recovery plan tooensure att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="2bb28-215">It is always recommended toorun a test failover tootest your application and hello recovery plan tooensure that there are no errors.</span></span>

1. <span data-ttu-id="2bb28-216">Välj hello återställningsplan och initiera testa redundans.</span><span class="sxs-lookup"><span data-stu-id="2bb28-216">Select hello recovery plan and initiate a test failover.</span></span>
2. <span data-ttu-id="2bb28-217">Under hello plan körning kan du se om hello runbook har körts eller inte via dess status.</span><span class="sxs-lookup"><span data-stu-id="2bb28-217">During hello plan execution, you can see whether hello runbook has executed or not via its status.</span></span>

   ![](media/site-recovery-runbook-automation/17.png)
3. <span data-ttu-id="2bb28-218">Du kan också se hello detaljerad status för runbook-körningen på hello Azure Automation-jobb sidan för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="2bb28-218">You can also see hello detailed runbook execution status on hello Azure Automation jobs page for hello runbook.</span></span>

   ![](media/site-recovery-runbook-automation/18.png)
4. <span data-ttu-id="2bb28-219">När hello växling vid fel är klar, förutom hello runbook Körningsresultat, kan du se om hello körningen har lyckats eller inte genom att besöka hello Azure virtuella sidan och titta på hello slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="2bb28-219">After hello failover completes, apart from hello runbook execution result, you can see whether hello execution is successful or not by visiting hello Azure virtual machine page and looking at hello endpoints.</span></span>

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a><span data-ttu-id="2bb28-220">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="2bb28-220">Sample scripts</span></span>
<span data-ttu-id="2bb28-221">När vi har gått igenom automatisera en ofta används uppgiften att lägga till en slutpunkt tooan virtuella Azure-datorn i den här självstudiekursen, kan du göra ett antal andra kraftfulla automation-aktiviteter med hjälp av Azure automation.</span><span class="sxs-lookup"><span data-stu-id="2bb28-221">While we walked through automating one commonly used task of adding an endpoint tooan Azure virtual machine in this tutorial, you could do a number of other powerful automation tasks using Azure automation.</span></span> <span data-ttu-id="2bb28-222">Microsoft och hello Azure Automation-gruppen ger du runbook-exempel som kan hjälpa dig komma igång med dina egna lösningar och verktyg för runbooks som du kan använda som byggblock för större automation-aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="2bb28-222">Microsoft and hello Azure Automation community provide sample runbooks which can help you get started creating your own solutions, and utility runbooks, which you can use as building blocks for larger automation tasks.</span></span> <span data-ttu-id="2bb28-223">Börja använda dem från hello-galleriet och skapa kraftfulla enkelklickning återställningsplaner för dina program med Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="2bb28-223">Start using them from hello gallery and build  powerful one-click recovery plans for your applications using Azure Site Recovery.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2bb28-224">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2bb28-224">Additional Resources</span></span>
[<span data-ttu-id="2bb28-225">Översikt över Azure Automation</span><span class="sxs-lookup"><span data-stu-id="2bb28-225">Azure Automation Overview</span></span>](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation-översikt")

[<span data-ttu-id="2bb28-226">Exempel på Azure automatiseringsskript</span><span class="sxs-lookup"><span data-stu-id="2bb28-226">Sample Azure Automation Scripts</span></span>](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "exempel Azure Automation-skript")
