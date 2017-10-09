---
title: Azure Automation aaaAdd runbooks toorecovery planer i Azure Site Recovery | Microsoft Docs
description: "Lär dig hur Azure Site Recovery kan du utöka återställningsplaner med Azure Automation. Lär dig hur toocomplete komplexa uppgifter under återställning tooAzure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 90d517200cec5527e98a0d00da466bace587b70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans"></a><span data-ttu-id="c57a9-104">Lägg till Azure Automation-runbooks toorecovery planer</span><span class="sxs-lookup"><span data-stu-id="c57a9-104">Add Azure Automation runbooks toorecovery plans</span></span>
<span data-ttu-id="c57a9-105">I den här artikeln beskriver vi hur Azure Site Recovery kan integreras med Azure Automation toohelp du utöka din återställningsplaner.</span><span class="sxs-lookup"><span data-stu-id="c57a9-105">In this article, we describe how Azure Site Recovery integrates with Azure Automation toohelp you extend your recovery plans.</span></span> <span data-ttu-id="c57a9-106">Återställningsplaner kan samordna återställning av virtuella datorer som är skyddade med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="c57a9-106">Recovery plans can orchestrate recovery of VMs that are protected with Site Recovery.</span></span> <span data-ttu-id="c57a9-107">Återställningsplaner fungerar både för replikering tooa sekundära molnet och för replikering tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c57a9-107">Recovery plans work both for replication tooa secondary cloud, and for replication tooAzure.</span></span> <span data-ttu-id="c57a9-108">Återställningsplaner också hjälpa dig att göra hello recovery **konsekvent korrekt**, **repeterbara**, och **automatiserad**.</span><span class="sxs-lookup"><span data-stu-id="c57a9-108">Recovery plans also help make hello recovery **consistently accurate**, **repeatable**, and **automated**.</span></span> <span data-ttu-id="c57a9-109">Om du växlar över dina virtuella datorer tooAzure utökar integrering med Azure Automation din återställningsplaner.</span><span class="sxs-lookup"><span data-stu-id="c57a9-109">If you fail over your VMs tooAzure, integration with Azure Automation extends your recovery plans.</span></span> <span data-ttu-id="c57a9-110">Du kan använda den tooexecute runbooks, som ger kraftfulla automation-aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="c57a9-110">You can use it tooexecute runbooks, which offer powerful automation tasks.</span></span>

<span data-ttu-id="c57a9-111">Om du är ny tooAzure Automation kan du [registrering](https://azure.microsoft.com/services/automation/) och [hämta skriptexempel](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="c57a9-111">If you are new tooAzure Automation, you can [sign up](https://azure.microsoft.com/services/automation/) and [download sample scripts](https://azure.microsoft.com/documentation/scripts/).</span></span> <span data-ttu-id="c57a9-112">Mer information och toolearn hur tooorchestrate recovery tooAzure med hjälp av [återställningsplaner](https://azure.microsoft.com/blog/?p=166264), se [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="c57a9-112">For more information, and toolearn how tooorchestrate recovery tooAzure by using [recovery plans](https://azure.microsoft.com/blog/?p=166264), see [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).</span></span>

<span data-ttu-id="c57a9-113">I den här artikeln beskriver vi hur du kan integrera Azure Automation-runbooks i din återställningsplaner.</span><span class="sxs-lookup"><span data-stu-id="c57a9-113">In this article, we describe how you can integrate Azure Automation runbooks into your recovery plans.</span></span> <span data-ttu-id="c57a9-114">Vi använder exempel tooautomate grundläggande uppgifter som tidigare krävde manuella åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c57a9-114">We use examples tooautomate basic tasks that previously required manual intervention.</span></span> <span data-ttu-id="c57a9-115">Vi beskriver också hur tooconvert en återställning av flera steg tooa enkelklickning återställningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="c57a9-115">We also describe how tooconvert a multi-step recovery tooa single-click recovery action.</span></span>

## <a name="customize-hello-recovery-plan"></a><span data-ttu-id="c57a9-116">Anpassa hello återställningsplan</span><span class="sxs-lookup"><span data-stu-id="c57a9-116">Customize hello recovery plan</span></span>
1. <span data-ttu-id="c57a9-117">Gå toohello **Site Recovery** resursbladet plan för återställning.</span><span class="sxs-lookup"><span data-stu-id="c57a9-117">Go toohello **Site Recovery** recovery plan resource blade.</span></span> <span data-ttu-id="c57a9-118">I det här exemplet återställningsplanens hello två virtuella datorer som lagts till tooit, för återställning.</span><span class="sxs-lookup"><span data-stu-id="c57a9-118">For this example, hello recovery plan has two VMs added tooit, for recovery.</span></span> <span data-ttu-id="c57a9-119">toobegin att lägga till en runbook, klicka på hello **anpassa** fliken.</span><span class="sxs-lookup"><span data-stu-id="c57a9-119">toobegin adding a runbook, click hello **Customize** tab.</span></span>

    ![Klicka hello anpassa](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. <span data-ttu-id="c57a9-121">Högerklicka på **grupp 1: starta**, och välj sedan **Lägg till post åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="c57a9-121">Right-click **Group 1: Start**, and then select **Add post action**.</span></span>

    ![Högerklicka på grupp 1: Starta och Lägg till post åtgärd](media/site-recovery-runbook-automation-new/customize-rp.png)

3. <span data-ttu-id="c57a9-123">Klicka på **väljer du ett skript**.</span><span class="sxs-lookup"><span data-stu-id="c57a9-123">Click **Choose a script**.</span></span>

4. <span data-ttu-id="c57a9-124">På hello **uppdatera åtgärden** bladet, hello skriptet **Hello World**.</span><span class="sxs-lookup"><span data-stu-id="c57a9-124">On hello **Update action** blade, name hello script **Hello World**.</span></span>

    ![hello Update åtgärd bladet](media/site-recovery-runbook-automation-new/update-rp.png)

5. <span data-ttu-id="c57a9-126">Ange namnet på ett Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="c57a9-126">Enter an Automation account name.</span></span>
    >[!NOTE]
    > <span data-ttu-id="c57a9-127">hello Automation-kontot kan vara i Azure-region.</span><span class="sxs-lookup"><span data-stu-id="c57a9-127">hello Automation account can be in any Azure region.</span></span> <span data-ttu-id="c57a9-128">hello Automation-kontot måste vara i hello samma prenumeration som hello Azure Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="c57a9-128">hello Automation account must be in hello same subscription as hello Azure Site Recovery vault.</span></span>

6. <span data-ttu-id="c57a9-129">Välj en runbook i Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="c57a9-129">In your Automation account, select a runbook.</span></span> <span data-ttu-id="c57a9-130">Denna runbook är hello-skript som körs under hello körningen av hello återställningsplan efter hello återställning av hello första gruppen.</span><span class="sxs-lookup"><span data-stu-id="c57a9-130">This runbook is hello script that runs during hello execution of hello recovery plan, after hello recovery of hello first group.</span></span>

7. <span data-ttu-id="c57a9-131">toosave hello skript klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c57a9-131">toosave hello script, click **OK**.</span></span> <span data-ttu-id="c57a9-132">hello skript har lagts till för**grupp 1: efter steg**.</span><span class="sxs-lookup"><span data-stu-id="c57a9-132">hello script is added too**Group 1: Post-steps**.</span></span>

    ![1:Start efter grupp](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a><span data-ttu-id="c57a9-134">Överväganden för att lägga till ett skript</span><span class="sxs-lookup"><span data-stu-id="c57a9-134">Considerations for adding a script</span></span>

* <span data-ttu-id="c57a9-135">Alternativ för**ta bort ett steg** eller **uppdateringsskript hello**, högerklicka på hello skript.</span><span class="sxs-lookup"><span data-stu-id="c57a9-135">For options too**delete a step** or **update hello script**, right-click hello script.</span></span>
* <span data-ttu-id="c57a9-136">Ett skript kan köras i Azure under växling vid fel från en lokal dator tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c57a9-136">A script can run on Azure during failover from an on-premises machine tooAzure.</span></span> <span data-ttu-id="c57a9-137">Den kan även köras på Azure som en primär plats skriptet innan avslutning, vid återställning från Azure tooan lokal dator.</span><span class="sxs-lookup"><span data-stu-id="c57a9-137">It also can run on Azure as a primary-site script before shutdown, during failback from Azure tooan on-premises machine.</span></span>
* <span data-ttu-id="c57a9-138">När ett skript körs den lägger in en återställning plan kontext.</span><span class="sxs-lookup"><span data-stu-id="c57a9-138">When a script runs, it injects a recovery plan context.</span></span> <span data-ttu-id="c57a9-139">hello som följande exempel visar en kontext variabel:</span><span class="sxs-lookup"><span data-stu-id="c57a9-139">hello following example shows a context variable:</span></span>

    ```
            {"RecoveryPlanName":"hrweb-recovery",

            "FailoverType":"Test",

            "FailoverDirection":"PrimaryToSecondary",

            "GroupId":"1",

            "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",

                    "ResourceGroupName":"ContosoRG",

                    "CloudServiceName":"pod02hrweb-Chicago-test",

                    "RoleName":"Fabrikam-Hrweb-frontend-test",

                    "RecoveryPointId":"TimeStamp"}

                    }

            }
    ```

    <span data-ttu-id="c57a9-140">hello i den följande tabellen listas hello namn och beskrivning för varje variabel i hello-kontexten.</span><span class="sxs-lookup"><span data-stu-id="c57a9-140">hello following table lists hello name and description of each variable in hello context.</span></span>

    | <span data-ttu-id="c57a9-141">**Variabelnamn**</span><span class="sxs-lookup"><span data-stu-id="c57a9-141">**Variable name**</span></span> | <span data-ttu-id="c57a9-142">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="c57a9-142">**Description**</span></span> |
    | --- | --- |
    | <span data-ttu-id="c57a9-143">RecoveryPlanName</span><span class="sxs-lookup"><span data-stu-id="c57a9-143">RecoveryPlanName</span></span> |<span data-ttu-id="c57a9-144">hello namnet på hello plan som körs.</span><span class="sxs-lookup"><span data-stu-id="c57a9-144">hello name of hello plan being run.</span></span> <span data-ttu-id="c57a9-145">Den här variabeln hjälper dig att vidta olika åtgärder baserat på hello schemanamn för återställning.</span><span class="sxs-lookup"><span data-stu-id="c57a9-145">This variable helps you take different actions based on hello recovery plan name.</span></span> <span data-ttu-id="c57a9-146">Du kan också återanvända hello skript.</span><span class="sxs-lookup"><span data-stu-id="c57a9-146">You also can reuse hello script.</span></span> |
    | <span data-ttu-id="c57a9-147">FailoverType</span><span class="sxs-lookup"><span data-stu-id="c57a9-147">FailoverType</span></span> |<span data-ttu-id="c57a9-148">Anger om hello redundans är ett test, planerad eller oplanerad.</span><span class="sxs-lookup"><span data-stu-id="c57a9-148">Specifies whether hello failover is a test, planned, or unplanned.</span></span> |
    | <span data-ttu-id="c57a9-149">FailoverDirection</span><span class="sxs-lookup"><span data-stu-id="c57a9-149">FailoverDirection</span></span> |<span data-ttu-id="c57a9-150">Anger om återställning tooa primär eller sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="c57a9-150">Specifies whether recovery is tooa primary or secondary site.</span></span> |
    | <span data-ttu-id="c57a9-151">Grupp-ID</span><span class="sxs-lookup"><span data-stu-id="c57a9-151">GroupID</span></span> |<span data-ttu-id="c57a9-152">Identifierar hello gruppnumret i hello återställningsplan när hello plan körs.</span><span class="sxs-lookup"><span data-stu-id="c57a9-152">Identifies hello group number in hello recovery plan when hello plan is running.</span></span> |
    | <span data-ttu-id="c57a9-153">VmMap</span><span class="sxs-lookup"><span data-stu-id="c57a9-153">VmMap</span></span> |<span data-ttu-id="c57a9-154">En matris med alla virtuella datorer i hello grupp.</span><span class="sxs-lookup"><span data-stu-id="c57a9-154">An array of all VMs in hello group.</span></span> |
    | <span data-ttu-id="c57a9-155">VMMap nyckel</span><span class="sxs-lookup"><span data-stu-id="c57a9-155">VMMap key</span></span> |<span data-ttu-id="c57a9-156">En unik nyckel (GUID) för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c57a9-156">A unique key (GUID) for each VM.</span></span> <span data-ttu-id="c57a9-157">Hello är samma som hello Azure Virtual Machine Manager (VMM) ID hello VM, i tillämpliga fall.</span><span class="sxs-lookup"><span data-stu-id="c57a9-157">It's hello same as hello Azure Virtual Machine Manager (VMM) ID of hello VM, where applicable.</span></span> |
    | <span data-ttu-id="c57a9-158">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="c57a9-158">SubscriptionId</span></span> |<span data-ttu-id="c57a9-159">hello Azure prenumerations-ID som hello VM skapades.</span><span class="sxs-lookup"><span data-stu-id="c57a9-159">hello Azure subscription ID in which hello VM was created.</span></span> |
    | <span data-ttu-id="c57a9-160">RoleName</span><span class="sxs-lookup"><span data-stu-id="c57a9-160">RoleName</span></span> |<span data-ttu-id="c57a9-161">hello namnet på hello Azure VM som återställs.</span><span class="sxs-lookup"><span data-stu-id="c57a9-161">hello name of hello Azure VM that's being recovered.</span></span> |
    | <span data-ttu-id="c57a9-162">CloudServiceName</span><span class="sxs-lookup"><span data-stu-id="c57a9-162">CloudServiceName</span></span> |<span data-ttu-id="c57a9-163">hello Azure molntjänstnamnet hello VM som har skapats.</span><span class="sxs-lookup"><span data-stu-id="c57a9-163">hello Azure cloud service name under which hello VM was created.</span></span> |
    | <span data-ttu-id="c57a9-164">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="c57a9-164">ResourceGroupName</span></span>|<span data-ttu-id="c57a9-165">hello Azure resursgruppens namn hello VM som har skapats.</span><span class="sxs-lookup"><span data-stu-id="c57a9-165">hello Azure resource group name under which hello VM was created.</span></span> |
    | <span data-ttu-id="c57a9-166">RecoveryPointId</span><span class="sxs-lookup"><span data-stu-id="c57a9-166">RecoveryPointId</span></span>|<span data-ttu-id="c57a9-167">hello tidsstämpel för när hello VM återställs.</span><span class="sxs-lookup"><span data-stu-id="c57a9-167">hello timestamp for when hello VM is recovered.</span></span> |

* <span data-ttu-id="c57a9-168">Kontrollera att hello Automation-konto har hello följande moduler:</span><span class="sxs-lookup"><span data-stu-id="c57a9-168">Ensure that hello Automation account has hello following modules:</span></span>
    * <span data-ttu-id="c57a9-169">AzureRM.profile</span><span class="sxs-lookup"><span data-stu-id="c57a9-169">AzureRM.profile</span></span>
    * <span data-ttu-id="c57a9-170">AzureRM.Resources</span><span class="sxs-lookup"><span data-stu-id="c57a9-170">AzureRM.Resources</span></span>
    * <span data-ttu-id="c57a9-171">AzureRM.Automation</span><span class="sxs-lookup"><span data-stu-id="c57a9-171">AzureRM.Automation</span></span>
    * <span data-ttu-id="c57a9-172">AzureRM.Network</span><span class="sxs-lookup"><span data-stu-id="c57a9-172">AzureRM.Network</span></span>
    * <span data-ttu-id="c57a9-173">AzureRM.Compute</span><span class="sxs-lookup"><span data-stu-id="c57a9-173">AzureRM.Compute</span></span>

<span data-ttu-id="c57a9-174">Alla moduler som anges i kompatibla versioner.</span><span class="sxs-lookup"><span data-stu-id="c57a9-174">All modules should be of compatible versions.</span></span> <span data-ttu-id="c57a9-175">Ett enkelt sätt tooensure att alla moduler som är kompatibla är toouse hello senaste versionerna av alla hello-moduler.</span><span class="sxs-lookup"><span data-stu-id="c57a9-175">An easy way tooensure that all modules are compatible is toouse hello latest versions of all hello modules.</span></span>

### <a name="access-all-vms-of-hello-vmmap-in-a-loop"></a><span data-ttu-id="c57a9-176">Åtkomst till alla virtuella datorer av hello VMMap i en loop</span><span class="sxs-lookup"><span data-stu-id="c57a9-176">Access all VMs of hello VMMap in a loop</span></span>
<span data-ttu-id="c57a9-177">Använd följande kod tooloop över alla virtuella datorer av hello Microsoft VMMap hello:</span><span class="sxs-lookup"><span data-stu-id="c57a9-177">Use hello following code tooloop across all VMs of hello Microsoft VMMap:</span></span>

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is tooensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> <span data-ttu-id="c57a9-178">hello resurs grupp namn och rollen namnvärden är tomt när hello skript är en före åtgärden tooa Start-grupp.</span><span class="sxs-lookup"><span data-stu-id="c57a9-178">hello resource group name and role name values are empty when hello script is a pre-action tooa boot group.</span></span> <span data-ttu-id="c57a9-179">hello värden fylls i om hello VM i gruppen lyckas växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c57a9-179">hello values are populated only if hello VM of that group succeeds in failover.</span></span> <span data-ttu-id="c57a9-180">hello skript är en efter hello Start grupp-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c57a9-180">hello script is a post-action of hello boot group.</span></span>

## <a name="use-hello-same-automation-runbook-in-multiple-recovery-plans"></a><span data-ttu-id="c57a9-181">Använd hello samma Automation-runbook i flera återställningsplaner</span><span class="sxs-lookup"><span data-stu-id="c57a9-181">Use hello same Automation runbook in multiple recovery plans</span></span>

<span data-ttu-id="c57a9-182">Du kan använda ett enda skript i flera återställningsplaner med externa variabler.</span><span class="sxs-lookup"><span data-stu-id="c57a9-182">You can use a single script in multiple recovery plans by using external variables.</span></span> <span data-ttu-id="c57a9-183">Du kan använda [Azure Automation-variabler](../automation/automation-variables.md) toostore parametrar som du kan skicka för en återställning planera körningen.</span><span class="sxs-lookup"><span data-stu-id="c57a9-183">You can use [Azure Automation variables](../automation/automation-variables.md) toostore parameters that you can pass for a recovery plan execution.</span></span> <span data-ttu-id="c57a9-184">Du kan skapa enskilda variabler för varje återställningsplan genom att lägga till hello recovery schemanamn som ett prefix toohello variabel.</span><span class="sxs-lookup"><span data-stu-id="c57a9-184">By adding hello recovery plan name as a prefix toohello variable, you can create individual variables for each recovery plan.</span></span> <span data-ttu-id="c57a9-185">Använd sedan hello variabler som parametrar.</span><span class="sxs-lookup"><span data-stu-id="c57a9-185">Then, use hello variables as parameters.</span></span> <span data-ttu-id="c57a9-186">Du kan ändra en parameter utan att ändra hello skript, men ändå ändra hello hello sätt skript.</span><span class="sxs-lookup"><span data-stu-id="c57a9-186">You can change a parameter without changing hello script, but still change hello way hello script works.</span></span>

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a><span data-ttu-id="c57a9-187">Använda en enkel string-variabel i en runbook-skript</span><span class="sxs-lookup"><span data-stu-id="c57a9-187">Use a simple string variable in a runbook script</span></span>

<span data-ttu-id="c57a9-188">I det här exemplet ett skript som tar hello indata för en Nätverkssäkerhetsgrupp (NSG) och tillämpar den toohello virtuella datorer i en återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="c57a9-188">In this example, a script takes hello input of a Network Security Group (NSG) and applies it toohello VMs of a recovery plan.</span></span>

<span data-ttu-id="c57a9-189">Använd hello recovery plan kontext för hello skriptet toodetect vilken återställningsplan körs:</span><span class="sxs-lookup"><span data-stu-id="c57a9-189">For hello script toodetect which recovery plan is running, use hello recovery plan context:</span></span>

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

<span data-ttu-id="c57a9-190">tooapply en befintlig NSG, måste du veta hello NSG namn och hello NSG resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="c57a9-190">tooapply an existing NSG, you must know hello NSG name and hello NSG resource group name.</span></span> <span data-ttu-id="c57a9-191">Använd dessa variabler som indata för återställning plan skript.</span><span class="sxs-lookup"><span data-stu-id="c57a9-191">Use these variables as inputs for recovery plan scripts.</span></span> <span data-ttu-id="c57a9-192">toodo kan skapa två variabler i hello Automation-konto tillgångar.</span><span class="sxs-lookup"><span data-stu-id="c57a9-192">toodo this, create two variables in hello Automation account assets.</span></span> <span data-ttu-id="c57a9-193">Lägg till hello namnet på hello återställningsplan som du skapar hello parametrar för som ett prefix toohello variabelnamn.</span><span class="sxs-lookup"><span data-stu-id="c57a9-193">Add hello name of hello recovery plan that you are creating hello parameters for as a prefix toohello variable name.</span></span>

1. <span data-ttu-id="c57a9-194">Skapa en variabel toostore hello NSG.</span><span class="sxs-lookup"><span data-stu-id="c57a9-194">Create a variable toostore hello NSG name.</span></span> <span data-ttu-id="c57a9-195">Lägga till ett prefix toohello variabelnamn med hello namnet på hello återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="c57a9-195">Add a prefix toohello variable name by using hello name of hello recovery plan.</span></span>

    ![Skapa en NSG-variabel](media/site-recovery-runbook-automation-new/var1.png)

2. <span data-ttu-id="c57a9-197">Skapa en variabel toostore hello NSG resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="c57a9-197">Create a variable toostore hello NSG's resource group name.</span></span> <span data-ttu-id="c57a9-198">Lägga till ett prefix toohello variabelnamn med hello namnet på hello återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="c57a9-198">Add a prefix toohello variable name by using hello name of hello recovery plan.</span></span>

    ![Skapa en NSG resursgruppens namn](media/site-recovery-runbook-automation-new/var2.png)


3.  <span data-ttu-id="c57a9-200">Använd hello följande referens kod tooget hello variabelvärden i hello skript:</span><span class="sxs-lookup"><span data-stu-id="c57a9-200">In hello script, use hello following reference code tooget hello variable values:</span></span>

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  <span data-ttu-id="c57a9-201">Använd hello variabler i hello runbook tooapply hello NSG toohello nätverksgränssnittet för hello misslyckades över VM:</span><span class="sxs-lookup"><span data-stu-id="c57a9-201">Use hello variables in hello runbook tooapply hello NSG toohello network interface of hello failed-over VM:</span></span>

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply hello NSG tooa network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

<span data-ttu-id="c57a9-202">Skapa oberoende variabler för varje återställningsplanen så att du kan återanvända hello skript.</span><span class="sxs-lookup"><span data-stu-id="c57a9-202">For each recovery plan, create independent variables so that you can reuse hello script.</span></span> <span data-ttu-id="c57a9-203">Lägga till ett prefix med hello schemanamn för återställning.</span><span class="sxs-lookup"><span data-stu-id="c57a9-203">Add a prefix by using hello recovery plan name.</span></span> <span data-ttu-id="c57a9-204">En fullständig, slutpunkt-till-slutpunkt skript i det här scenariot finns [lägga till en offentlig IP-adress och NSG tooVMs under redundanstestningen av en återställningsplan för Site Recovery](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span><span class="sxs-lookup"><span data-stu-id="c57a9-204">For a complete, end-to-end script for this scenario, see [Add a public IP and NSG tooVMs during test failover of a Site Recovery recovery plan](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).</span></span>


### <a name="use-a-complex-variable-toostore-more-information"></a><span data-ttu-id="c57a9-205">Använd en komplex variabeln toostore mer information</span><span class="sxs-lookup"><span data-stu-id="c57a9-205">Use a complex variable toostore more information</span></span>

<span data-ttu-id="c57a9-206">Föreställ dig ett scenario som du vill använda ett enda skript tooturn på en offentlig IP-adress på specifika virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c57a9-206">Consider a scenario in which you want a single script tooturn on a public IP on specific VMs.</span></span> <span data-ttu-id="c57a9-207">Du kanske vill tooapply i ett annat scenario olika NSG: er på olika virtuella datorer (inte på alla virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="c57a9-207">In another scenario, you might want tooapply different NSGs on different VMs (not on all VMs).</span></span> <span data-ttu-id="c57a9-208">Du kan göra ett skript som är återanvändbara för en återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="c57a9-208">You can make a script that is reusable for any recovery plan.</span></span> <span data-ttu-id="c57a9-209">Varje återställningsplanen kan ha en variabel antal virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c57a9-209">Each recovery plan can have a variable number of VMs.</span></span> <span data-ttu-id="c57a9-210">Till exempel har en SharePoint-återställning två frontwebbservrarna.</span><span class="sxs-lookup"><span data-stu-id="c57a9-210">For example, a SharePoint recovery has two front ends.</span></span> <span data-ttu-id="c57a9-211">En grundläggande line-of-business (LOB)-programmet har bara en klientdel.</span><span class="sxs-lookup"><span data-stu-id="c57a9-211">A basic line-of-business (LOB) application has only one front end.</span></span> <span data-ttu-id="c57a9-212">Du kan skapa olika variabler för varje återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="c57a9-212">You cannot create separate variables for each recovery plan.</span></span> 

<span data-ttu-id="c57a9-213">I följande exempel hello, vi en ny teknik och skapa en [komplex variabeln](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) i hello Azure Automation-konto tillgångar.</span><span class="sxs-lookup"><span data-stu-id="c57a9-213">In hello following example, we use a new technique and create a [complex variable](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) in hello Azure Automation account assets.</span></span> <span data-ttu-id="c57a9-214">Det gör du genom att ange flera värden.</span><span class="sxs-lookup"><span data-stu-id="c57a9-214">You do this by specifying multiple values.</span></span> <span data-ttu-id="c57a9-215">Du måste använda Azure PowerShell toocomplete hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c57a9-215">You must use Azure PowerShell toocomplete hello following steps:</span></span>

1. <span data-ttu-id="c57a9-216">Logga in tooyour Azure-prenumeration i PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c57a9-216">In PowerShell, sign in tooyour Azure subscription:</span></span>

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. <span data-ttu-id="c57a9-217">toostore hello parametrar, skapa hello komplex variabeln med hello namnet på hello återställningsplan:</span><span class="sxs-lookup"><span data-stu-id="c57a9-217">toostore hello parameters, create hello complex variable by using hello name of hello recovery plan:</span></span>

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. <span data-ttu-id="c57a9-218">I den här komplex variabeln **VMDetails** är hello VM-ID för hello skyddade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c57a9-218">In this complex variable, **VMDetails** is hello VM ID for hello protected VM.</span></span> <span data-ttu-id="c57a9-219">tooget hello VM-ID i hello Azure-portalen visa hello VM egenskaper.</span><span class="sxs-lookup"><span data-stu-id="c57a9-219">tooget hello VM ID, in hello Azure portal, view hello VM properties.</span></span> <span data-ttu-id="c57a9-220">hello visar följande skärmbild en variabel som lagrar hello information om två virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="c57a9-220">hello following screenshot shows a variable that stores hello details of two VMs:</span></span>

    ![Använd hello VM-ID som hello GUID](media/site-recovery-runbook-automation-new/vmguid.png)

4. <span data-ttu-id="c57a9-222">Använd den här variabeln i din runbook.</span><span class="sxs-lookup"><span data-stu-id="c57a9-222">Use this variable in your runbook.</span></span> <span data-ttu-id="c57a9-223">Om hello anges VM-GUID kan hittas i kontexten för hello recovery planera, installera hello NSG på hello VM:</span><span class="sxs-lookup"><span data-stu-id="c57a9-223">If hello indicated VM GUID is found in hello recovery plan context, apply hello NSG on hello VM:</span></span>

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. <span data-ttu-id="c57a9-224">Gå igenom hello virtuella datorer i hello recovery plan kontexten i din runbook.</span><span class="sxs-lookup"><span data-stu-id="c57a9-224">In your runbook, loop through hello VMs of hello recovery plan context.</span></span> <span data-ttu-id="c57a9-225">Kontrollera om hello VM finns i **$VMDetailsObj**.</span><span class="sxs-lookup"><span data-stu-id="c57a9-225">Check whether hello VM exists in **$VMDetailsObj**.</span></span> <span data-ttu-id="c57a9-226">Om det finns öppna hello egenskaper för hello variabeln tooapply hello NSG:</span><span class="sxs-lookup"><span data-stu-id="c57a9-226">If it exists, access hello properties of hello variable tooapply hello NSG:</span></span>

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If hello VM exists in hello context, this will not b Null
                $VM = $vmMap.$VMID
                # Access hello properties of hello variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code tooapply hello NSG properties toohello VM
            }
        }
    ```

<span data-ttu-id="c57a9-227">Du kan använda samma skript hello för olika återställningsplaner.</span><span class="sxs-lookup"><span data-stu-id="c57a9-227">You can use hello same script for different recovery plans.</span></span> <span data-ttu-id="c57a9-228">Ange olika parametrar genom att lagra hello-värde som motsvarar tooa återställningsplan i olika variabler.</span><span class="sxs-lookup"><span data-stu-id="c57a9-228">Enter different parameters by storing hello value that corresponds tooa recovery plan in different variables.</span></span>

## <a name="sample-scripts"></a><span data-ttu-id="c57a9-229">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="c57a9-229">Sample scripts</span></span>

<span data-ttu-id="c57a9-230">toodeploy exempel skript tooyour Automation-konto, klickar du på hello **distribuera tooAzure** knappen.</span><span class="sxs-lookup"><span data-stu-id="c57a9-230">toodeploy sample scripts tooyour Automation account, click hello **Deploy tooAzure** button.</span></span>

<span data-ttu-id="c57a9-231">[![Distribuera tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span><span class="sxs-lookup"><span data-stu-id="c57a9-231">[![Deploy tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)</span></span>

<span data-ttu-id="c57a9-232">Ett annat exempel finns i följande video hello.</span><span class="sxs-lookup"><span data-stu-id="c57a9-232">For another example, see hello following video.</span></span> <span data-ttu-id="c57a9-233">Den visar hur toorecover en två-lagers WordPress programmet tooAzure:</span><span class="sxs-lookup"><span data-stu-id="c57a9-233">It demonstrates how toorecover a two-tier WordPress application tooAzure:</span></span>


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a><span data-ttu-id="c57a9-234">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c57a9-234">Additional resources</span></span>
* [<span data-ttu-id="c57a9-235">Azure Automation-tjänsten kör som-konto</span><span class="sxs-lookup"><span data-stu-id="c57a9-235">Azure Automation service Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)
* [<span data-ttu-id="c57a9-236">Översikt över Azure Automation</span><span class="sxs-lookup"><span data-stu-id="c57a9-236">Azure Automation overview</span></span>](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation-översikt")
* [<span data-ttu-id="c57a9-237">Azure Automation-exempelskript</span><span class="sxs-lookup"><span data-stu-id="c57a9-237">Azure Automation sample scripts</span></span>](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure Automation-exempelskript")
