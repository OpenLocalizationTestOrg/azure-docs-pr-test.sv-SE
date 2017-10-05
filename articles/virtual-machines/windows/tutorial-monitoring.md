---
title: "Azure övervaknings- och Windows-datorer | Microsoft Docs"
description: "Kursen - Övervakare för en virtuell dator i Windows med Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 42a475092bdd615c23154e5f813861b0acefcf29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a><span data-ttu-id="c785b-103">Övervaka en Windows-dator med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c785b-103">Monitor a Windows Virtual Machine with Azure PowerShell</span></span>

<span data-ttu-id="c785b-104">Azure övervakning använder agenter för att samla in start-och prestandadata från Azure virtuella datorer, lagra dessa data i Azure-lagring och göra det tillgängligt via portalen, Azure PowerShell-modulen och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c785b-104">Azure monitoring uses agents to collect boot and performance data from Azure VMs, store this data in Azure storage, and make it accessible through portal, the Azure PowerShell module, and the Azure CLI.</span></span> <span data-ttu-id="c785b-105">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="c785b-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c785b-106">Aktivera startdiagnostikinställningar på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c785b-106">Enable boot diagnostics on a VM</span></span>
> * <span data-ttu-id="c785b-107">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="c785b-107">View boot diagnostics</span></span>
> * <span data-ttu-id="c785b-108">Visa VM värden mått</span><span class="sxs-lookup"><span data-stu-id="c785b-108">View VM host metrics</span></span>
> * <span data-ttu-id="c785b-109">Installera tillägget diagnostik</span><span class="sxs-lookup"><span data-stu-id="c785b-109">Install the diagnostics extension</span></span>
> * <span data-ttu-id="c785b-110">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="c785b-110">View VM metrics</span></span>
> * <span data-ttu-id="c785b-111">Skapa en avisering</span><span class="sxs-lookup"><span data-stu-id="c785b-111">Create an alert</span></span>
> * <span data-ttu-id="c785b-112">Konfigurera avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="c785b-112">Set up advanced monitoring</span></span>

<span data-ttu-id="c785b-113">Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c785b-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="c785b-114">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="c785b-114">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="c785b-115">Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="c785b-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="c785b-116">Du måste ha en befintlig virtuell dator för att slutföra exemplet i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c785b-116">To complete the example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="c785b-117">Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="c785b-117">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="c785b-118">När du arbetar med kursen, Ersätt resursgrupp, namn och plats där det behövs.</span><span class="sxs-lookup"><span data-stu-id="c785b-118">When working through the tutorial, replace the resource group, VM name, and location where needed.</span></span>

## <a name="view-boot-diagnostics"></a><span data-ttu-id="c785b-119">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="c785b-119">View boot diagnostics</span></span>

<span data-ttu-id="c785b-120">Eftersom Windows-datorer starta avbildar Start diagnostikagenten utdata som kan användas för felsökning.</span><span class="sxs-lookup"><span data-stu-id="c785b-120">As Windows virtual machines boot up, the boot diagnostic agent captures screen output that can be used for troubleshooting purpose.</span></span> <span data-ttu-id="c785b-121">Den här funktionen är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="c785b-121">This capability is enabled by default.</span></span> <span data-ttu-id="c785b-122">Fångade skärmdumpar lagras i Azure storage-konto som skapas också som standard.</span><span class="sxs-lookup"><span data-stu-id="c785b-122">The captured screen shots are stored in an Azure storage account, which is also created by default.</span></span> 

<span data-ttu-id="c785b-123">Du kan hämta Start diagnostikdata med den [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) kommando.</span><span class="sxs-lookup"><span data-stu-id="c785b-123">You can get the boot diagnostic data with the [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) command.</span></span> <span data-ttu-id="c785b-124">I följande exempel hämtas startdiagnostikinställningar till roten av den * c:\* enhet.</span><span class="sxs-lookup"><span data-stu-id="c785b-124">In the following example, boot diagnostics are downloaded to the root of the *c:\* drive.</span></span> 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a><span data-ttu-id="c785b-125">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="c785b-125">View host metrics</span></span>

<span data-ttu-id="c785b-126">En virtuell Windows-dator har en dedikerad värd eller virtuell dator i Azure som den interagerar med.</span><span class="sxs-lookup"><span data-stu-id="c785b-126">A Windows VM has a dedicated Host VM in Azure that it interacts with.</span></span> <span data-ttu-id="c785b-127">Mått samlas in automatiskt för värden och kan visas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c785b-127">Metrics are automatically collected for the Host and can be viewed in the Azure portal.</span></span>

1. <span data-ttu-id="c785b-128">I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="c785b-128">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="c785b-129">Klicka på **mått** på VM-bladet och välj sedan någon av mätvärdena som är värd under **tillgängliga mått** att se hur den Virtuella värden utförs.</span><span class="sxs-lookup"><span data-stu-id="c785b-129">Click **Metrics** on the VM blade, and then select any of the Host metrics under **Available metrics** to see how the Host VM is performing.</span></span>

    ![Visa värden mått](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a><span data-ttu-id="c785b-131">Installera tillägg för diagnostik</span><span class="sxs-lookup"><span data-stu-id="c785b-131">Install diagnostics extension</span></span>

<span data-ttu-id="c785b-132">De grundläggande värden är tillgänglig, men att se mer detaljerad och VM-specifika statistik, och du behöver installera Azure-diagnostik-tillägget på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c785b-132">The basic host metrics are available, but to see more granular and VM-specific metrics, you to need to install the Azure diagnostics extension on the VM.</span></span> <span data-ttu-id="c785b-133">Azure diagnostics-tillägget kan ytterligare övervakning och diagnostikdata som ska hämtas från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c785b-133">The Azure diagnostics extension allows additional monitoring and diagnostics data to be retrieved from the VM.</span></span> <span data-ttu-id="c785b-134">Du kan visa dessa prestandamått och skapa varningar baserat på hur den virtuella datorn utför.</span><span class="sxs-lookup"><span data-stu-id="c785b-134">You can view these performance metrics and create alerts based on how the VM performs.</span></span> <span data-ttu-id="c785b-135">Diagnostiska tillägget installeras via Azure portal på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c785b-135">The diagnostic extension is installed through the Azure portal as follows:</span></span>

1. <span data-ttu-id="c785b-136">I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="c785b-136">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="c785b-137">Klicka på **diagnos inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c785b-137">Click **Diagnosis settings**.</span></span> <span data-ttu-id="c785b-138">I listan visas som *starta diagnostik* redan aktiverat från föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c785b-138">The list shows that *Boot diagnostics* are already enabled from the previous section.</span></span> <span data-ttu-id="c785b-139">Klicka på kryssrutan för *grundläggande mått*.</span><span class="sxs-lookup"><span data-stu-id="c785b-139">Click the check box for *Basic metrics*.</span></span>
3. <span data-ttu-id="c785b-140">Klicka på den **aktivera övervakning av gästnivå** knappen.</span><span class="sxs-lookup"><span data-stu-id="c785b-140">Click the **Enable guest-level monitoring** button.</span></span>

    ![Visa diagnostik mått](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a><span data-ttu-id="c785b-142">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="c785b-142">View VM metrics</span></span>

<span data-ttu-id="c785b-143">Du kan visa VM mått på samma sätt som du granskat värden VM mått:</span><span class="sxs-lookup"><span data-stu-id="c785b-143">You can view the VM metrics in the same way that you viewed the host VM metrics:</span></span>

1. <span data-ttu-id="c785b-144">I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="c785b-144">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="c785b-145">Klicka för att se hur den virtuella datorn utför **mått** på VM-bladet och välj sedan någon av diagnostik mått under **tillgängliga mått**.</span><span class="sxs-lookup"><span data-stu-id="c785b-145">To see how the VM is performing, click **Metrics** on the VM blade, and then select any of the diagnostics metrics under **Available metrics**.</span></span>

    ![Visa VM mått](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a><span data-ttu-id="c785b-147">Skapa aviseringar</span><span class="sxs-lookup"><span data-stu-id="c785b-147">Create alerts</span></span>

<span data-ttu-id="c785b-148">Du kan skapa varningar baserat på specifika prestandamått.</span><span class="sxs-lookup"><span data-stu-id="c785b-148">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="c785b-149">Aviseringar kan användas för att meddela dig när Genomsnittlig CPU-användning överskrider ett visst tröskelvärde eller tillgängligt diskutrymme sjunker under ett visst belopp, till exempel.</span><span class="sxs-lookup"><span data-stu-id="c785b-149">Alerts can be used to notify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="c785b-150">Aviseringar visas i Azure-portalen eller kan skickas via e-post.</span><span class="sxs-lookup"><span data-stu-id="c785b-150">Alerts are displayed in the Azure portal or can be sent via email.</span></span> <span data-ttu-id="c785b-151">Du kan också utlösa Azure Automation-runbooks eller Logikappar i Azure som svar på varningar genereras.</span><span class="sxs-lookup"><span data-stu-id="c785b-151">You can also trigger Azure Automation runbooks or Azure Logic Apps in response to alerts being generated.</span></span>

<span data-ttu-id="c785b-152">I följande exempel skapas en avisering för Genomsnittlig CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="c785b-152">The following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="c785b-153">I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="c785b-153">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="c785b-154">Klicka på **Varna regler** på VM-bladet Klicka **Lägg till mått avisering** överst på bladet aviseringar.</span><span class="sxs-lookup"><span data-stu-id="c785b-154">Click **Alert rules** on the VM blade, then click **Add metric alert** across the top of the alerts blade.</span></span>
4. <span data-ttu-id="c785b-155">Ange en **namn** om varningen som *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="c785b-155">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="c785b-156">För att utlösa en avisering när CPU-procent överskrider 1.0 5 minuter, lämnar du alla andra standardinställningar som valts.</span><span class="sxs-lookup"><span data-stu-id="c785b-156">To trigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all the other defaults selected.</span></span>
6. <span data-ttu-id="c785b-157">Du kan också markera kryssrutan för *e-ägare, deltagare och läsare* att skicka e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="c785b-157">Optionally, check the box for *Email owners, contributors, and readers* to send email notification.</span></span> <span data-ttu-id="c785b-158">Standardåtgärden är att visa ett meddelande i portalen.</span><span class="sxs-lookup"><span data-stu-id="c785b-158">The default action is to present a notification in the portal.</span></span>
7. <span data-ttu-id="c785b-159">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c785b-159">Click the **OK** button.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="c785b-160">Avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="c785b-160">Advanced monitoring</span></span> 

<span data-ttu-id="c785b-161">Du kan göra mer avancerad övervakning av den virtuella datorn med hjälp av [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="c785b-161">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="c785b-162">Om du inte redan gjort det. Du kan registrera dig för en [kostnadsfri utvärderingsversion](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) av Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="c785b-162">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="c785b-163">När du har åtkomst till OMS-portalen hittar du den arbetsyta och arbetsytan identifierare på bladet inställningar.</span><span class="sxs-lookup"><span data-stu-id="c785b-163">When you have access to the OMS portal, you can find the workspace key and workspace identifier on the Settings blade.</span></span> <span data-ttu-id="c785b-164">Använd den [Set AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand för att lägga till OMS-tillägget på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c785b-164">Use the [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand to to add the OMS extension to the VM.</span></span> <span data-ttu-id="c785b-165">Uppdatera variabelvärden i den nedan exempel för att återspegla du OMS arbetsytenyckel och arbetsytan Id.</span><span class="sxs-lookup"><span data-stu-id="c785b-165">Update the variable values in the below sample to reflect you OMS workspace key and workspace Id.</span></span>  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

<span data-ttu-id="c785b-166">Du bör se den nya virtuella datorn i OMS-arbetsytan efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="c785b-166">After a few minutes, you should see the new VM in the OMS workspace.</span></span> 

![OMS-bladet](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="c785b-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c785b-168">Next steps</span></span>
<span data-ttu-id="c785b-169">I kursen får du konfigurerade och granskas virtuella datorer med Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="c785b-169">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="c785b-170">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="c785b-170">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c785b-171">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="c785b-171">Create a virtual network</span></span>
> * <span data-ttu-id="c785b-172">Skapa en resursgrupp och en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c785b-172">Create a resource group and VM</span></span> 
> * <span data-ttu-id="c785b-173">Aktivera startdiagnostikinställningar på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c785b-173">Enable boot diagnostics on the VM</span></span>
> * <span data-ttu-id="c785b-174">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="c785b-174">View boot diagnostics</span></span>
> * <span data-ttu-id="c785b-175">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="c785b-175">View host metrics</span></span>
> * <span data-ttu-id="c785b-176">Installera tillägget diagnostik</span><span class="sxs-lookup"><span data-stu-id="c785b-176">Install the diagnostics extension</span></span>
> * <span data-ttu-id="c785b-177">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="c785b-177">View VM metrics</span></span>
> * <span data-ttu-id="c785b-178">Skapa en avisering</span><span class="sxs-lookup"><span data-stu-id="c785b-178">Create an alert</span></span>
> * <span data-ttu-id="c785b-179">Konfigurera avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="c785b-179">Set up advanced monitoring</span></span>

<span data-ttu-id="c785b-180">Gå vidare till nästa kurs vill veta mer om Azure security center.</span><span class="sxs-lookup"><span data-stu-id="c785b-180">Advance to the next tutorial to learn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c785b-181">Hantera säkerheten för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c785b-181">Manage VM security</span></span>](./tutorial-azure-security.md)