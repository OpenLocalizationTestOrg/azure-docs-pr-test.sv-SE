---
title: "aaaAzure övervaknings- och Windows-datorer | Microsoft Docs"
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
ms.openlocfilehash: 191dc5a30d41c25a9e38f8ec2a32efdc05e03015
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a><span data-ttu-id="6bd97-103">Övervaka en Windows-dator med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6bd97-103">Monitor a Windows Virtual Machine with Azure PowerShell</span></span>

<span data-ttu-id="6bd97-104">Azure-övervakning används agenter toocollect start och prestandadata från Azure virtuella datorer kan lagra dessa data i Azure-lagring och gör den tillgänglig via portalen, hello Azure PowerShell-modulen och hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6bd97-104">Azure monitoring uses agents toocollect boot and performance data from Azure VMs, store this data in Azure storage, and make it accessible through portal, hello Azure PowerShell module, and hello Azure CLI.</span></span> <span data-ttu-id="6bd97-105">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="6bd97-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6bd97-106">Aktivera startdiagnostikinställningar på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="6bd97-106">Enable boot diagnostics on a VM</span></span>
> * <span data-ttu-id="6bd97-107">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="6bd97-107">View boot diagnostics</span></span>
> * <span data-ttu-id="6bd97-108">Visa VM värden mått</span><span class="sxs-lookup"><span data-stu-id="6bd97-108">View VM host metrics</span></span>
> * <span data-ttu-id="6bd97-109">Installera tillägg för hello diagnostik</span><span class="sxs-lookup"><span data-stu-id="6bd97-109">Install hello diagnostics extension</span></span>
> * <span data-ttu-id="6bd97-110">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="6bd97-110">View VM metrics</span></span>
> * <span data-ttu-id="6bd97-111">Skapa en avisering</span><span class="sxs-lookup"><span data-stu-id="6bd97-111">Create an alert</span></span>
> * <span data-ttu-id="6bd97-112">Konfigurera avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="6bd97-112">Set up advanced monitoring</span></span>

<span data-ttu-id="6bd97-113">Den här kursen kräver hello Azure PowerShell module 3,6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="6bd97-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="6bd97-114">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="6bd97-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="6bd97-115">Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="6bd97-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="6bd97-116">toocomplete hello exempel i den här självstudiekursen, måste du ha en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6bd97-116">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="6bd97-117">Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="6bd97-117">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="6bd97-118">När du arbetar via hello kursen, Ersätt hello resursgrupp, namn och plats där det behövs.</span><span class="sxs-lookup"><span data-stu-id="6bd97-118">When working through hello tutorial, replace hello resource group, VM name, and location where needed.</span></span>

## <a name="view-boot-diagnostics"></a><span data-ttu-id="6bd97-119">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="6bd97-119">View boot diagnostics</span></span>

<span data-ttu-id="6bd97-120">Eftersom Windows-datorer starta avbildar hello Start diagnostikagenten utdata som kan användas för felsökning.</span><span class="sxs-lookup"><span data-stu-id="6bd97-120">As Windows virtual machines boot up, hello boot diagnostic agent captures screen output that can be used for troubleshooting purpose.</span></span> <span data-ttu-id="6bd97-121">Den här funktionen är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="6bd97-121">This capability is enabled by default.</span></span> <span data-ttu-id="6bd97-122">hello avbildas skärmen bilderna lagras i Azure storage-konto som skapas också som standard.</span><span class="sxs-lookup"><span data-stu-id="6bd97-122">hello captured screen shots are stored in an Azure storage account, which is also created by default.</span></span> 

<span data-ttu-id="6bd97-123">Du kan hämta hello Start diagnostikdata med hello [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) kommando.</span><span class="sxs-lookup"><span data-stu-id="6bd97-123">You can get hello boot diagnostic data with hello [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) command.</span></span> <span data-ttu-id="6bd97-124">I följande exempel hello, startdiagnostikinställningar är hämtade toohello rot hello * c:\* enhet.</span><span class="sxs-lookup"><span data-stu-id="6bd97-124">In hello following example, boot diagnostics are downloaded toohello root of hello *c:\* drive.</span></span> 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a><span data-ttu-id="6bd97-125">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="6bd97-125">View host metrics</span></span>

<span data-ttu-id="6bd97-126">En virtuell Windows-dator har en dedikerad värd eller virtuell dator i Azure som den interagerar med.</span><span class="sxs-lookup"><span data-stu-id="6bd97-126">A Windows VM has a dedicated Host VM in Azure that it interacts with.</span></span> <span data-ttu-id="6bd97-127">Mått är automatiskt samlas in för hello värd och kan visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6bd97-127">Metrics are automatically collected for hello Host and can be viewed in hello Azure portal.</span></span>

1. <span data-ttu-id="6bd97-128">I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.</span><span class="sxs-lookup"><span data-stu-id="6bd97-128">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="6bd97-129">Klicka på **mått** på hello VM-bladet och välj sedan någon av hello värden mått under **tillgängliga mått** toosee hur hello värden VM utförs.</span><span class="sxs-lookup"><span data-stu-id="6bd97-129">Click **Metrics** on hello VM blade, and then select any of hello Host metrics under **Available metrics** toosee how hello Host VM is performing.</span></span>

    ![Visa värden mått](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a><span data-ttu-id="6bd97-131">Installera tillägg för diagnostik</span><span class="sxs-lookup"><span data-stu-id="6bd97-131">Install diagnostics extension</span></span>

<span data-ttu-id="6bd97-132">hello grundläggande värden är tillgänglig, men toosee mer detaljerade och VM-specifika statistik, och du tooneed tooinstall hello Azure diagnostics tillägg på hello VM.</span><span class="sxs-lookup"><span data-stu-id="6bd97-132">hello basic host metrics are available, but toosee more granular and VM-specific metrics, you tooneed tooinstall hello Azure diagnostics extension on hello VM.</span></span> <span data-ttu-id="6bd97-133">hello Azure diagnostics tillägget kan ytterligare övervakning och diagnostik data toobe hämtas från hello VM.</span><span class="sxs-lookup"><span data-stu-id="6bd97-133">hello Azure diagnostics extension allows additional monitoring and diagnostics data toobe retrieved from hello VM.</span></span> <span data-ttu-id="6bd97-134">Du kan visa dessa prestandamått och skapa varningar baserat på hur hello VM utför.</span><span class="sxs-lookup"><span data-stu-id="6bd97-134">You can view these performance metrics and create alerts based on how hello VM performs.</span></span> <span data-ttu-id="6bd97-135">hello diagnostiska tillägg har installerats via hello Azure-portalen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="6bd97-135">hello diagnostic extension is installed through hello Azure portal as follows:</span></span>

1. <span data-ttu-id="6bd97-136">I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.</span><span class="sxs-lookup"><span data-stu-id="6bd97-136">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="6bd97-137">Klicka på **diagnos inställningar**.</span><span class="sxs-lookup"><span data-stu-id="6bd97-137">Click **Diagnosis settings**.</span></span> <span data-ttu-id="6bd97-138">hello lista visar att *starta diagnostik* redan aktiverat från hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6bd97-138">hello list shows that *Boot diagnostics* are already enabled from hello previous section.</span></span> <span data-ttu-id="6bd97-139">Klicka på hello kryssrutan för *grundläggande mått*.</span><span class="sxs-lookup"><span data-stu-id="6bd97-139">Click hello check box for *Basic metrics*.</span></span>
3. <span data-ttu-id="6bd97-140">Klicka på hello **aktivera övervakning av gästnivå** knappen.</span><span class="sxs-lookup"><span data-stu-id="6bd97-140">Click hello **Enable guest-level monitoring** button.</span></span>

    ![Visa diagnostik mått](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a><span data-ttu-id="6bd97-142">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="6bd97-142">View VM metrics</span></span>

<span data-ttu-id="6bd97-143">Du kan visa hello VM mått i hello samma sätt som du granskat hello värden VM mått:</span><span class="sxs-lookup"><span data-stu-id="6bd97-143">You can view hello VM metrics in hello same way that you viewed hello host VM metrics:</span></span>

1. <span data-ttu-id="6bd97-144">I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.</span><span class="sxs-lookup"><span data-stu-id="6bd97-144">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="6bd97-145">toosee hur hello VM utförs klickar du på **mått** på hello VM-bladet och välj sedan någon av hello diagnostik mått under **tillgängliga mått**.</span><span class="sxs-lookup"><span data-stu-id="6bd97-145">toosee how hello VM is performing, click **Metrics** on hello VM blade, and then select any of hello diagnostics metrics under **Available metrics**.</span></span>

    ![Visa VM mått](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a><span data-ttu-id="6bd97-147">Skapa aviseringar</span><span class="sxs-lookup"><span data-stu-id="6bd97-147">Create alerts</span></span>

<span data-ttu-id="6bd97-148">Du kan skapa varningar baserat på specifika prestandamått.</span><span class="sxs-lookup"><span data-stu-id="6bd97-148">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="6bd97-149">Aviseringar kan till exempel vara används toonotify när Genomsnittlig CPU-användning överskrider ett visst tröskelvärde eller tillgängligt diskutrymme sjunker under en viss mängd.</span><span class="sxs-lookup"><span data-stu-id="6bd97-149">Alerts can be used toonotify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="6bd97-150">Aviseringar visas i hello Azure-portalen eller kan skickas via e-post.</span><span class="sxs-lookup"><span data-stu-id="6bd97-150">Alerts are displayed in hello Azure portal or can be sent via email.</span></span> <span data-ttu-id="6bd97-151">Du kan även utlösa Azure Automation-runbooks eller Azure Logikappar i svaret tooalerts som genereras.</span><span class="sxs-lookup"><span data-stu-id="6bd97-151">You can also trigger Azure Automation runbooks or Azure Logic Apps in response tooalerts being generated.</span></span>

<span data-ttu-id="6bd97-152">hello skapas följande exempel en avisering för Genomsnittlig CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="6bd97-152">hello following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="6bd97-153">I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.</span><span class="sxs-lookup"><span data-stu-id="6bd97-153">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="6bd97-154">Klicka på **Varna regler** på hello VM-bladet, klickar sedan på **Lägg till mått avisering** hello överst på bladet med säkerhetsaviseringar hello.</span><span class="sxs-lookup"><span data-stu-id="6bd97-154">Click **Alert rules** on hello VM blade, then click **Add metric alert** across hello top of hello alerts blade.</span></span>
4. <span data-ttu-id="6bd97-155">Ange en **namn** om varningen som *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="6bd97-155">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="6bd97-156">tootrigger en avisering när CPU-procent överskrider 1.0 i fem minuter lämnar du alla hello standardvärden som valts.</span><span class="sxs-lookup"><span data-stu-id="6bd97-156">tootrigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all hello other defaults selected.</span></span>
6. <span data-ttu-id="6bd97-157">Du kan också markera kryssrutan hello för *e-ägare, deltagare och läsare* toosend e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="6bd97-157">Optionally, check hello box for *Email owners, contributors, and readers* toosend email notification.</span></span> <span data-ttu-id="6bd97-158">hello standardåtgärden är toopresent ett meddelande i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="6bd97-158">hello default action is toopresent a notification in hello portal.</span></span>
7. <span data-ttu-id="6bd97-159">Klicka på hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="6bd97-159">Click hello **OK** button.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="6bd97-160">Avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="6bd97-160">Advanced monitoring</span></span> 

<span data-ttu-id="6bd97-161">Du kan göra mer avancerad övervakning av den virtuella datorn med hjälp av [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="6bd97-161">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="6bd97-162">Om du inte redan gjort det. Du kan registrera dig för en [kostnadsfri utvärderingsversion](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) av Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="6bd97-162">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="6bd97-163">När du har åtkomst toohello OMS-portalen hittar hello arbetsytan arbetsytan och nyckeln identifierare på inställningsbladet för hello.</span><span class="sxs-lookup"><span data-stu-id="6bd97-163">When you have access toohello OMS portal, you can find hello workspace key and workspace identifier on hello Settings blade.</span></span> <span data-ttu-id="6bd97-164">Använd hello [Set AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand tootooadd hello OMS tillägget toohello VM.</span><span class="sxs-lookup"><span data-stu-id="6bd97-164">Use hello [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand tootooadd hello OMS extension toohello VM.</span></span> <span data-ttu-id="6bd97-165">Uppdatera hello variabeln värden i hello nedan exempel tooreflect du arbetsytan ID och arbetsytenyckel OMS</span><span class="sxs-lookup"><span data-stu-id="6bd97-165">Update hello variable values in hello below sample tooreflect you OMS workspace key and workspace Id.</span></span>  

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

<span data-ttu-id="6bd97-166">Efter några minuter, bör du se hello ny virtuell dator i hello OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="6bd97-166">After a few minutes, you should see hello new VM in hello OMS workspace.</span></span> 

![OMS-bladet](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="6bd97-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6bd97-168">Next steps</span></span>
<span data-ttu-id="6bd97-169">I kursen får du konfigurerade och granskas virtuella datorer med Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="6bd97-169">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="6bd97-170">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="6bd97-170">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6bd97-171">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="6bd97-171">Create a virtual network</span></span>
> * <span data-ttu-id="6bd97-172">Skapa en resursgrupp och en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="6bd97-172">Create a resource group and VM</span></span> 
> * <span data-ttu-id="6bd97-173">Aktivera startdiagnostikinställningar på hello VM</span><span class="sxs-lookup"><span data-stu-id="6bd97-173">Enable boot diagnostics on hello VM</span></span>
> * <span data-ttu-id="6bd97-174">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="6bd97-174">View boot diagnostics</span></span>
> * <span data-ttu-id="6bd97-175">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="6bd97-175">View host metrics</span></span>
> * <span data-ttu-id="6bd97-176">Installera tillägg för hello diagnostik</span><span class="sxs-lookup"><span data-stu-id="6bd97-176">Install hello diagnostics extension</span></span>
> * <span data-ttu-id="6bd97-177">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="6bd97-177">View VM metrics</span></span>
> * <span data-ttu-id="6bd97-178">Skapa en avisering</span><span class="sxs-lookup"><span data-stu-id="6bd97-178">Create an alert</span></span>
> * <span data-ttu-id="6bd97-179">Konfigurera avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="6bd97-179">Set up advanced monitoring</span></span>

<span data-ttu-id="6bd97-180">Avancera toohello nästa självstudiekurs toolearn om Azure security center.</span><span class="sxs-lookup"><span data-stu-id="6bd97-180">Advance toohello next tutorial toolearn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6bd97-181">Hantera säkerheten för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="6bd97-181">Manage VM security</span></span>](./tutorial-azure-security.md)