---
title: "aaaUse JSON-formaterad taggar tooschedule Azure VM tillstånd | Microsoft Docs"
description: "Den här artikeln visar hur toouse JSON strängar på taggar tooautomate hello schemaläggning av VM-start och stopp."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: f6bbf1dea1c193e5d1010f12f3b1ed63562f9daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-toocreate-a-schedule-for-azure-vm-startup-and-shutdown"></a><span data-ttu-id="f1887-103">Azure Automation-scenario: använda JSON-formaterad taggar toocreate ett schema för Virtuella Azure-start och stopp</span><span class="sxs-lookup"><span data-stu-id="f1887-103">Azure Automation scenario: Using JSON-formatted tags toocreate a schedule for Azure VM startup and shutdown</span></span>
<span data-ttu-id="f1887-104">Kunder vill ofta tooschedule hello start och stopp av virtuella datorer toohelp minska kostnaderna för prenumerationen eller stöder affärsmässiga och tekniska krav.</span><span class="sxs-lookup"><span data-stu-id="f1887-104">Customers often want tooschedule hello startup and shutdown of virtual machines toohelp reduce subscription costs or support business and technical requirements.</span></span>

<span data-ttu-id="f1887-105">hello kan följande scenario du tooset in automatisk start och stopp av dina virtuella datorer med hjälp av en tagg som kallas schemat på en resursgruppsnivå eller virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="f1887-105">hello following scenario enables you tooset up automated startup and shutdown of your VMs by using a tag called Schedule at a resource group level or virtual machine level in Azure.</span></span> <span data-ttu-id="f1887-106">Det här schemat kan konfigureras från söndag tooSaturday med ett starttiden och avstängning tid.</span><span class="sxs-lookup"><span data-stu-id="f1887-106">This schedule can be configured from Sunday tooSaturday with a startup time and shutdown time.</span></span>

<span data-ttu-id="f1887-107">Vi har några out box-alternativ.</span><span class="sxs-lookup"><span data-stu-id="f1887-107">We do have some out-of-the-box options.</span></span> <span data-ttu-id="f1887-108">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="f1887-108">These include:</span></span>

* <span data-ttu-id="f1887-109">[Skaluppsättningar för den virtuella datorn](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) med Autoskala inställningar som gör att du tooscale in eller ut.</span><span class="sxs-lookup"><span data-stu-id="f1887-109">[Virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) with autoscale settings that enable you tooscale in or out.</span></span>
* <span data-ttu-id="f1887-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) -tjänsten som har inbyggda hello-funktionen för schemaläggning av åtgärder för start och stopp.</span><span class="sxs-lookup"><span data-stu-id="f1887-110">[DevTest Labs](../devtest-lab/devtest-lab-overview.md) service, which has hello built-in capability of scheduling startup and shutdown operations.</span></span>

<span data-ttu-id="f1887-111">Dessa alternativ stöder dock endast specifika scenarier och går inte att använda tooinfrastructure-som en tjänst (IaaS) virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f1887-111">However, these options only support specific scenarios and cannot be applied tooinfrastructure-as-a-service (IaaS) VMs.</span></span>

<span data-ttu-id="f1887-112">När hello schema taggen är tillämpade tooa resursgrupp, är det också tillämpade tooall virtuella datorer i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f1887-112">When hello Schedule tag is applied tooa resource group, it's also applied tooall virtual machines inside that resource group.</span></span> <span data-ttu-id="f1887-113">Om ett schema är också direkt tooa VM, företräde hello senaste schema filer i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="f1887-113">If a schedule is also directly applied tooa VM, hello last schedule takes precedence in hello following order:</span></span>

1. <span data-ttu-id="f1887-114">Schemalägga tillämpade tooa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f1887-114">Schedule applied tooa resource group</span></span>
2. <span data-ttu-id="f1887-115">Schemalägga tillämpade tooa resursgrupp och den virtuella datorn i hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="f1887-115">Schedule applied tooa resource group and virtual machine in hello resource group</span></span>
3. <span data-ttu-id="f1887-116">Schemalägga tillämpade tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f1887-116">Schedule applied tooa virtual machine</span></span>

<span data-ttu-id="f1887-117">Det här scenariot huvudsakligen tar en JSON-sträng med ett bestämt format och lägger till den som hello värde för en tagg som kallas schema.</span><span class="sxs-lookup"><span data-stu-id="f1887-117">This scenario essentially takes a JSON string with a specified format and adds it as hello value for a tag called Schedule.</span></span> <span data-ttu-id="f1887-118">Sedan en runbook visas alla resursgrupper och virtuella datorer och identifierar hello scheman för varje virtuell dator baserat på hello scenarier som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="f1887-118">Then a runbook lists all resource groups and virtual machines and identifies hello schedules for each VM based on hello scenarios listed earlier.</span></span> <span data-ttu-id="f1887-119">Därefter loop genom hello virtuella datorer som har scheman som är anslutna och utvärderar vilka måste åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="f1887-119">Next it loops through hello VMs that have schedules attached and evaluates what action should be taken.</span></span> <span data-ttu-id="f1887-120">Till exempel avgör vilka virtuella datorer måste toobe Stoppad, avstängd eller ignoreras.</span><span class="sxs-lookup"><span data-stu-id="f1887-120">For example, it determines which VMs need toobe stopped, shut down, or ignored.</span></span>

<span data-ttu-id="f1887-121">Dessa runbooks autentisera med hjälp av hello [Azure kör som-konto](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="f1887-121">These runbooks authenticate by using hello [Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>

## <a name="download-hello-runbooks-for-hello-scenario"></a><span data-ttu-id="f1887-122">Hämta hello runbooks för hello scenario</span><span class="sxs-lookup"><span data-stu-id="f1887-122">Download hello runbooks for hello scenario</span></span>
<span data-ttu-id="f1887-123">Det här scenariot består av fyra runbooks i PowerShell-arbetsflöde som du kan hämta från hello [TechNet-galleriet](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) eller hello [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) databasen för det här projektet.</span><span class="sxs-lookup"><span data-stu-id="f1887-123">This scenario consists of four PowerShell Workflow runbooks that you can download from hello [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) or hello [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) repository for this project.</span></span>

| <span data-ttu-id="f1887-124">Runbook</span><span class="sxs-lookup"><span data-stu-id="f1887-124">Runbook</span></span> | <span data-ttu-id="f1887-125">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1887-125">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f1887-126">Testa ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="f1887-126">Test-ResourceSchedule</span></span> |<span data-ttu-id="f1887-127">Kontrollerar schema för varje virtuell dator och utför stängs av eller startas beroende på hello schema.</span><span class="sxs-lookup"><span data-stu-id="f1887-127">Checks each virtual machine schedule and performs shutdown or startup depending on hello schedule.</span></span> |
| <span data-ttu-id="f1887-128">Lägg till ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="f1887-128">Add-ResourceSchedule</span></span> |<span data-ttu-id="f1887-129">Lägger till hello schema taggen tooa virtuell dator eller resurs grupp.</span><span class="sxs-lookup"><span data-stu-id="f1887-129">Adds hello Schedule tag tooa VM or resource group.</span></span> |
| <span data-ttu-id="f1887-130">Uppdatera ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="f1887-130">Update-ResourceSchedule</span></span> |<span data-ttu-id="f1887-131">Ändrar hello befintlig schema tagg genom att ersätta den med en ny.</span><span class="sxs-lookup"><span data-stu-id="f1887-131">Modifies hello existing Schedule tag by replacing it with a new one.</span></span> |
| <span data-ttu-id="f1887-132">Ta bort ResourceSchedule</span><span class="sxs-lookup"><span data-stu-id="f1887-132">Remove-ResourceSchedule</span></span> |<span data-ttu-id="f1887-133">Tar bort hello schema taggen från en virtuell dator eller resurs-grupp.</span><span class="sxs-lookup"><span data-stu-id="f1887-133">Removes hello Schedule tag from a VM or resource group.</span></span> |

## <a name="install-and-configure-this-scenario"></a><span data-ttu-id="f1887-134">Installera och konfigurera det här scenariot</span><span class="sxs-lookup"><span data-stu-id="f1887-134">Install and configure this scenario</span></span>
### <a name="install-and-publish-hello-runbooks"></a><span data-ttu-id="f1887-135">Installera och publicera hello runbooks</span><span class="sxs-lookup"><span data-stu-id="f1887-135">Install and publish hello runbooks</span></span>
<span data-ttu-id="f1887-136">När du har hämtat hello runbooks kan du importera dem med hello proceduren i [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span><span class="sxs-lookup"><span data-stu-id="f1887-136">After downloading hello runbooks, you can import them by using hello procedure in [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).</span></span>  <span data-ttu-id="f1887-137">Publicera varje runbook när den har importerats till ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f1887-137">Publish each runbook after it has been successfully imported into your Automation account.</span></span>

### <a name="add-a-schedule-toohello-test-resourceschedule-runbook"></a><span data-ttu-id="f1887-138">Lägga till en schema toohello ResourceSchedule Test-runbook</span><span class="sxs-lookup"><span data-stu-id="f1887-138">Add a schedule toohello Test-ResourceSchedule runbook</span></span>
<span data-ttu-id="f1887-139">Följ dessa steg tooenable hello schema för hello Test ResourceSchedule runbook.</span><span class="sxs-lookup"><span data-stu-id="f1887-139">Follow these steps tooenable hello schedule for hello Test-ResourceSchedule runbook.</span></span> <span data-ttu-id="f1887-140">Detta är hello-runbook som kontrollerar vilka virtuella datorer ska startas, stänga av eller vänster är.</span><span class="sxs-lookup"><span data-stu-id="f1887-140">This is hello runbook that verifies which virtual machines should be started, shut down, or left as is.</span></span>

1. <span data-ttu-id="f1887-141">Hello Azure-portalen, öppna ditt Automation-konto och klicka sedan på hello **Runbooks** panelen.</span><span class="sxs-lookup"><span data-stu-id="f1887-141">From hello Azure portal, open your Automation account, and then click hello **Runbooks** tile.</span></span>
2. <span data-ttu-id="f1887-142">På hello **Test ResourceSchedule** bladet, klickar du på hello **scheman** panelen.</span><span class="sxs-lookup"><span data-stu-id="f1887-142">On hello **Test-ResourceSchedule** blade, click hello **Schedules** tile.</span></span>
3. <span data-ttu-id="f1887-143">På hello **scheman** bladet, klickar du på **lägga till ett schema**.</span><span class="sxs-lookup"><span data-stu-id="f1887-143">On hello **Schedules** blade, click **Add a schedule**.</span></span>
4. <span data-ttu-id="f1887-144">På hello **scheman** bladet väljer **länka ett schema tooyour runbook**.</span><span class="sxs-lookup"><span data-stu-id="f1887-144">On hello **Schedules** blade, select **Link a schedule tooyour runbook**.</span></span> <span data-ttu-id="f1887-145">Välj sedan **skapa ett nytt schema**.</span><span class="sxs-lookup"><span data-stu-id="f1887-145">Then select **Create a new schedule**.</span></span>
5. <span data-ttu-id="f1887-146">På hello **nytt schema** bladet anger hello namn för det här schemat, till exempel: *HourlyExecution*.</span><span class="sxs-lookup"><span data-stu-id="f1887-146">On hello **New schedule** blade, type in hello name of this schedule, for example: *HourlyExecution*.</span></span>
6. <span data-ttu-id="f1887-147">För hello schema **starta**, ange hello start tid tooan timme ökning.</span><span class="sxs-lookup"><span data-stu-id="f1887-147">For hello schedule **Start**, set hello start time tooan hour increment.</span></span>
7. <span data-ttu-id="f1887-148">Välj **återkommande**, och sedan för **upprepas varje intervall**väljer **1 timme**.</span><span class="sxs-lookup"><span data-stu-id="f1887-148">Select **Recurrence**, and then for **Recur every interval**, select **1 hour**.</span></span>
8. <span data-ttu-id="f1887-149">Kontrollera att **ställa in ett utgångsdatum** har angetts för**nr**, och klicka sedan på **skapa** toosave nya schemat.</span><span class="sxs-lookup"><span data-stu-id="f1887-149">Verify that **Set expiration** is set too**No**, and then click **Create** toosave your new schedule.</span></span>
9. <span data-ttu-id="f1887-150">På hello **schema Runbook** alternativ bladet väljer **parametrar och körinställningar**.</span><span class="sxs-lookup"><span data-stu-id="f1887-150">On hello **Schedule Runbook** options blade, select **Parameters and run settings**.</span></span> <span data-ttu-id="f1887-151">I hello Test ResourceSchedule **parametrar** bladet ange hello namn för din prenumeration i hello **SubscriptionName** fältet.</span><span class="sxs-lookup"><span data-stu-id="f1887-151">In hello Test-ResourceSchedule **Parameters** blade, enter hello name of your subscription in hello **SubscriptionName** field.</span></span>  <span data-ttu-id="f1887-152">Detta är endast hello-parameter som krävs för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="f1887-152">This is hello only parameter that's required for hello runbook.</span></span>  <span data-ttu-id="f1887-153">När du är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1887-153">When you're finished, click **OK**.</span></span>

<span data-ttu-id="f1887-154">hello schemat för runbook bör se ut som följande hello när den har slutförts:</span><span class="sxs-lookup"><span data-stu-id="f1887-154">hello runbook schedule should look like hello following when it's completed:</span></span>

![Konfigurerade ResourceSchedule Test-runbook](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-hello-json-string"></a><span data-ttu-id="f1887-156">Formatet hello JSON-strängen</span><span class="sxs-lookup"><span data-stu-id="f1887-156">Format hello JSON string</span></span>
<span data-ttu-id="f1887-157">Den här lösningen i grunden tar en JSON sträng med ett bestämt format och lägger till den som hello värde för en tagg kallas du schema.</span><span class="sxs-lookup"><span data-stu-id="f1887-157">This solution basically takes a JSON string with a specified format and adds it as hello value for a tag called Schedule.</span></span> <span data-ttu-id="f1887-158">En runbook visas alla resursgrupper och virtuella datorer och identifierar hello scheman för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f1887-158">Then a runbook lists all resource groups and virtual machines and identifies hello schedules for each virtual machine.</span></span>

<span data-ttu-id="f1887-159">hello slinga över hello virtuella datorer som har kopplade scheman och kontrollerar du vilka åtgärder som ska vidtas.</span><span class="sxs-lookup"><span data-stu-id="f1887-159">hello runbook loops over hello virtual machines that have schedules attached and checks what actions should be taken.</span></span> <span data-ttu-id="f1887-160">hello följande är ett exempel på hur hello lösningar ska formateras:</span><span class="sxs-lookup"><span data-stu-id="f1887-160">hello following is an example of how hello solutions should be formatted:</span></span>

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

<span data-ttu-id="f1887-161">Här är några detaljerad information om den här strukturen:</span><span class="sxs-lookup"><span data-stu-id="f1887-161">Here is some detailed information about this structure:</span></span>

1. <span data-ttu-id="f1887-162">hello formatet för den här JSON-strukturen är optimerad toowork runt hello 256 tecken begränsning i ett enda Taggvärde i Azure.</span><span class="sxs-lookup"><span data-stu-id="f1887-162">hello format of this JSON structure is optimized toowork around hello 256-character limitation of a single tag value in Azure.</span></span>
2. <span data-ttu-id="f1887-163">*TzId* representerar hello tidszon hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="f1887-163">*TzId* represents hello time zone of hello virtual machine.</span></span> <span data-ttu-id="f1887-164">Detta ID kan erhållas med hjälp av hello TimeZoneInfo .NET-klass i ett PowerShell-session--**[System.TimeZoneInfo]:: GetSystemTimeZones()**.</span><span class="sxs-lookup"><span data-stu-id="f1887-164">This ID can be obtained by using hello TimeZoneInfo .NET class in a PowerShell session--**[System.TimeZoneInfo]::GetSystemTimeZones()**.</span></span>

   ![GetSystemTimeZones i PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * <span data-ttu-id="f1887-166">Veckodagar representeras med ett numeriskt värde på noll toosix.</span><span class="sxs-lookup"><span data-stu-id="f1887-166">Weekdays are represented with a numeric value of zero toosix.</span></span> <span data-ttu-id="f1887-167">hello värdet noll är lika med söndag.</span><span class="sxs-lookup"><span data-stu-id="f1887-167">hello value zero equals Sunday.</span></span>
   * <span data-ttu-id="f1887-168">hello starttiden representeras med hello **S** attributet och dess värde är i ett 24-timmarsformat.</span><span class="sxs-lookup"><span data-stu-id="f1887-168">hello start time is represented with hello **S** attribute, and its value is in a 24-hour format.</span></span>
   * <span data-ttu-id="f1887-169">hello slutet eller avstängning representeras med hello **E** attributet och dess värde är i ett 24-timmarsformat.</span><span class="sxs-lookup"><span data-stu-id="f1887-169">hello end or shutdown time is represented with hello **E** attribute, and its value is in a 24-hour format.</span></span>

     <span data-ttu-id="f1887-170">Om hello **S** och **E** attribut varje har värdet noll (0), hello virtuella datorn ska vara kvar i det nuvarande tillståndet hello tiden för utvärderingen.</span><span class="sxs-lookup"><span data-stu-id="f1887-170">If hello **S** and **E** attributes each have a value of zero (0), hello virtual machine will be left in its present state at hello time of evaluation.</span></span>
3. <span data-ttu-id="f1887-171">Om du vill tooskip utvärderingen för en viss dag i veckan hello Lägg inte till ett avsnitt för att hello veckodag.</span><span class="sxs-lookup"><span data-stu-id="f1887-171">If you want tooskip evaluation for a specific day of hello week, don’t add a section for that day of hello week.</span></span> <span data-ttu-id="f1887-172">Följande exempel endast måndag utvärderas i hello och hello andra hello veckodagar ignoreras:</span><span class="sxs-lookup"><span data-stu-id="f1887-172">In hello following example, only Monday is evaluated, and hello other days of hello week are ignored:</span></span>

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a><span data-ttu-id="f1887-173">Taggen resursgrupper eller virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f1887-173">Tag resource groups or VMs</span></span>
<span data-ttu-id="f1887-174">tooshut av virtuella datorer måste tootag hello virtuella datorer eller hello resursgrupper där de är placerade.</span><span class="sxs-lookup"><span data-stu-id="f1887-174">tooshut down VMs, you need tootag either hello VMs or hello resource groups in which they're located.</span></span> <span data-ttu-id="f1887-175">Virtuella datorer som inte har en schema-tagg utvärderas inte.</span><span class="sxs-lookup"><span data-stu-id="f1887-175">Virtual machines that don't have a Schedule tag are not evaluated.</span></span> <span data-ttu-id="f1887-176">Därför är inte de starta eller stänga av.</span><span class="sxs-lookup"><span data-stu-id="f1887-176">Therefore, they aren't started or shut down.</span></span>

<span data-ttu-id="f1887-177">Det finns två sätt tootag resursgrupper eller virtuella datorer med den här lösningen.</span><span class="sxs-lookup"><span data-stu-id="f1887-177">There are two ways tootag resource groups or VMs with this solution.</span></span> <span data-ttu-id="f1887-178">Du kan göra det direkt från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f1887-178">You can do it directly from hello portal.</span></span> <span data-ttu-id="f1887-179">Eller så kan du använda hello Lägg till ResourceSchedule, Update-ResourceSchedule och ta bort ResourceSchedule runbooks.</span><span class="sxs-lookup"><span data-stu-id="f1887-179">Or you can use hello Add-ResourceSchedule, Update-ResourceSchedule, and Remove-ResourceSchedule runbooks.</span></span>

### <a name="tag-through-hello-portal"></a><span data-ttu-id="f1887-180">Tagga hello-portalen</span><span class="sxs-lookup"><span data-stu-id="f1887-180">Tag through hello portal</span></span>
<span data-ttu-id="f1887-181">Följ dessa steg tootag en virtuell dator eller resursgrupp i hello portal:</span><span class="sxs-lookup"><span data-stu-id="f1887-181">Follow these steps tootag a virtual machine or resource group in hello portal:</span></span>

1. <span data-ttu-id="f1887-182">Förenkla hello JSON-strängen och kontrollera att det inte finns några blanksteg.</span><span class="sxs-lookup"><span data-stu-id="f1887-182">Flatten hello JSON string and verify that there aren't any spaces.</span></span>  <span data-ttu-id="f1887-183">JSON-strängen ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="f1887-183">Your JSON string should look like this:</span></span>

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. <span data-ttu-id="f1887-184">Välj hello **taggen** ikon för en virtuell dator eller resurs gruppera tooapply schemat.</span><span class="sxs-lookup"><span data-stu-id="f1887-184">Select hello **Tag** icon for a VM or resource group tooapply this schedule.</span></span>

   ![Alternativet för VM-tagg](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. <span data-ttu-id="f1887-186">Taggar har definierats efter ett nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="f1887-186">Tags are defined following a key/value pair.</span></span> <span data-ttu-id="f1887-187">Typen **schema** i hello **nyckeln** fältet och sedan klistra in JSON-strängen för hello i hello **värdet** fältet.</span><span class="sxs-lookup"><span data-stu-id="f1887-187">Type **Schedule** in hello **Key** field, and then paste hello JSON string into hello **Value** field.</span></span> <span data-ttu-id="f1887-188">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f1887-188">Click **Save**.</span></span> <span data-ttu-id="f1887-189">Din nya taggen bör nu visas i hello lista med taggar för din resurs.</span><span class="sxs-lookup"><span data-stu-id="f1887-189">Your new tag should now appear in hello list of tags for your resource.</span></span>

   ![Taggen för VM-schema](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a><span data-ttu-id="f1887-191">Taggen från PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1887-191">Tag from PowerShell</span></span>
<span data-ttu-id="f1887-192">Alla importerade runbooks innehåller hjälpinformation hello början på hello-skript som beskriver hur tooexecute hello runbooks direkt från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f1887-192">All imported runbooks contain help information at hello beginning of hello script that describes how tooexecute hello runbooks directly from PowerShell.</span></span> <span data-ttu-id="f1887-193">Du kan anropa hello Lägg till ScheduleResource och uppdatera ScheduleResource runbooks från PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f1887-193">You can call hello Add-ScheduleResource and Update-ScheduleResource runbooks from PowerShell.</span></span> <span data-ttu-id="f1887-194">Det gör du genom att skicka parametrar som gör att du toocreate eller uppdatera hello schema tagg på en virtuell dator eller resurs utanför hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f1887-194">You do this by passing required parameters that enable you toocreate or update hello Schedule tag on a VM or resource group outside of hello portal.</span></span>

<span data-ttu-id="f1887-195">toocreate, lägga till och ta bort taggar via PowerShell, måste du först för[konfigurera din PowerShell-miljö för Azure](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f1887-195">toocreate, add, and delete tags through PowerShell, you first need too[set up your PowerShell environment for Azure](/powershell/azure/overview).</span></span> <span data-ttu-id="f1887-196">Du kan fortsätta med hello följande steg när du har slutfört hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="f1887-196">After you complete hello setup, you can proceed with hello following steps.</span></span>

### <a name="create-a-schedule-tag-with-powershell"></a><span data-ttu-id="f1887-197">Skapa en schema-tagg med PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1887-197">Create a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="f1887-198">Öppna ett PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="f1887-198">Open a PowerShell session.</span></span> <span data-ttu-id="f1887-199">Använd sedan följande exempel tooauthenticate med Kör som-konto och toospecify en prenumeration hello:</span><span class="sxs-lookup"><span data-stu-id="f1887-199">Then use hello following example tooauthenticate with your Run As account and toospecify a subscription:</span></span>

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="f1887-200">Definiera ett schema för hash-tabell.</span><span class="sxs-lookup"><span data-stu-id="f1887-200">Define a schedule hash table.</span></span> <span data-ttu-id="f1887-201">Här är ett exempel på hur det ska konstrueras:</span><span class="sxs-lookup"><span data-stu-id="f1887-201">Here is an example of how it should be constructed:</span></span>

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. <span data-ttu-id="f1887-202">Definiera hello-parametrar som krävs av hello runbook.</span><span class="sxs-lookup"><span data-stu-id="f1887-202">Define hello parameters that are required by hello runbook.</span></span> <span data-ttu-id="f1887-203">I följande exempel hello, utvecklar vi en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="f1887-203">In hello following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    <span data-ttu-id="f1887-204">Om du ange en resursgrupp, ta bort hello *VMName* parametern från hello $params hash-tabell enligt följande:</span><span class="sxs-lookup"><span data-stu-id="f1887-204">If you’re tagging a resource group, remove hello *VMName* parameter from hello $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. <span data-ttu-id="f1887-205">Kör hello Lägg till ResourceSchedule runbook med följande parametrar toocreate hello schema taggen hello:</span><span class="sxs-lookup"><span data-stu-id="f1887-205">Run hello Add-ResourceSchedule runbook with hello following parameters toocreate hello Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. <span data-ttu-id="f1887-206">tooupdate en virtuell dator eller grupp Resurstagg, köra hello **uppdatering ResourceSchedule** runbook med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="f1887-206">tooupdate a resource group or virtual machine tag, execute hello **Update-ResourceSchedule** runbook with hello following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a><span data-ttu-id="f1887-207">Ta bort en schema-tagg med PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1887-207">Remove a schedule tag with PowerShell</span></span>
1. <span data-ttu-id="f1887-208">Öppna ett PowerShell-session och kör följande tooauthenticate med Kör som-konto och tooselect hello och ange en prenumeration:</span><span class="sxs-lookup"><span data-stu-id="f1887-208">Open a PowerShell session and run hello following tooauthenticate with your Run As account and tooselect and specify a subscription:</span></span>

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. <span data-ttu-id="f1887-209">Definiera hello-parametrar som krävs av hello runbook.</span><span class="sxs-lookup"><span data-stu-id="f1887-209">Define hello parameters that are required by hello runbook.</span></span> <span data-ttu-id="f1887-210">I följande exempel hello, utvecklar vi en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="f1887-210">In hello following example, we are targeting a VM:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    <span data-ttu-id="f1887-211">Om du tar bort en tagg från en resursgrupp, ta bort hello *VMName* parametern från hello $params hash-tabell enligt följande:</span><span class="sxs-lookup"><span data-stu-id="f1887-211">If you’re removing a tag from a resource group, remove hello *VMName* parameter from hello $params hash table as follows:</span></span>

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. <span data-ttu-id="f1887-212">Kör hello ta bort ResourceSchedule runbook tooremove hello schema tagg:</span><span class="sxs-lookup"><span data-stu-id="f1887-212">Execute hello Remove-ResourceSchedule runbook tooremove hello Schedule tag:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. <span data-ttu-id="f1887-213">tooupdate en virtuell dator eller grupp Resurstagg, köra hello ta bort ResourceSchedule runbook med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="f1887-213">tooupdate a resource group or virtual machine tag, execute hello Remove-ResourceSchedule runbook with hello following parameters:</span></span>

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> <span data-ttu-id="f1887-214">Vi rekommenderar att du proaktivt övervaka dessa runbooks (och hello tillstånd för virtuell dator) tooverify dina virtuella datorer som ska stängas av och startas om i enlighet därmed.</span><span class="sxs-lookup"><span data-stu-id="f1887-214">We recommend that you proactively monitor these runbooks (and hello virtual machine states) tooverify that your virtual machines are being shut down and started accordingly.</span></span>
>

<span data-ttu-id="f1887-215">tooview hello information om hello Test ResourceSchedule runbook jobbet i hello Azure-portalen väljer hello **jobb** för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="f1887-215">tooview hello details of hello Test-ResourceSchedule runbook job in hello Azure portal, select hello **Jobs** tile of hello runbook.</span></span> <span data-ttu-id="f1887-216">hello jobbet sammanfattning visar hello indataparametrar och hello utdata strömma, dessutom toogeneral information om hello jobbet och eventuella undantag om de inträffade.</span><span class="sxs-lookup"><span data-stu-id="f1887-216">hello job summary displays hello input parameters and hello output stream, in addition toogeneral information about hello job and any exceptions if they occurred.</span></span>

<span data-ttu-id="f1887-217">Hej **jobbsammanfattning** innehåller meddelanden från hello utdata, varningar och fel dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="f1887-217">hello **Job Summary** includes messages from hello output, warning, and error streams.</span></span> <span data-ttu-id="f1887-218">Välj hello **utdata** panelen tooview detaljerade resultat från hello runbook-körningen.</span><span class="sxs-lookup"><span data-stu-id="f1887-218">Select hello **Output** tile tooview detailed results from hello runbook execution.</span></span>

![Testa ResourceSchedule utdata](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a><span data-ttu-id="f1887-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1887-220">Next steps</span></span>
* <span data-ttu-id="f1887-221">tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md).</span><span class="sxs-lookup"><span data-stu-id="f1887-221">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md).</span></span>
* <span data-ttu-id="f1887-222">toolearn mer om runbook-typer och fördelar och begränsningar finns [Azure Automation runbook-typer](automation-runbook-types.md).</span><span class="sxs-lookup"><span data-stu-id="f1887-222">toolearn more about runbook types, and their advantages and limitations, see [Azure Automation runbook types](automation-runbook-types.md).</span></span>
* <span data-ttu-id="f1887-223">Mer information om PowerShell-skript stödfunktioner finns [stöd för intern PowerShell-skriptet i Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span><span class="sxs-lookup"><span data-stu-id="f1887-223">For more information about PowerShell script support features, see [Native PowerShell script support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).</span></span>
* <span data-ttu-id="f1887-224">toolearn mer om runbook-loggning och utdata, se [Runbook-utdata och meddelanden i Azure Automation](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="f1887-224">toolearn more about runbook logging and output, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md).</span></span>
* <span data-ttu-id="f1887-225">Mer om Azure kör som-konto och hur tooauthenticate runbooks med hjälp av det, se toolearn [autentisera runbooks med Kör som-kontot Azure](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="f1887-225">toolearn more about an Azure Run As account and how tooauthenticate your runbooks by using it, see [Authenticate runbooks with Azure Run As account](automation-sec-configure-azure-runas-account.md).</span></span>
