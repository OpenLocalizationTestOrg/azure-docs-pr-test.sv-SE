---
title: "Övervaka Linux-datorer i Azure | Microsoft Docs"
description: "Lär dig hur du övervakar Start diagnostik- och prestandamått i en virtuell Linux-dator i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 3fe8390e88e609b57a462e066f972346f8e8730e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-monitor-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="9973e-103">Så här övervakar du en virtuell Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="9973e-103">How to monitor a Linux virtual machine in Azure</span></span>

<span data-ttu-id="9973e-104">Du kan granska Start diagnostik- och prestandamått för att se till att dina virtuella datorer (VM) i Azure körs på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="9973e-104">To ensure your virtual machines (VMs) in Azure are running correctly, you can review boot diagnostics and performance metrics.</span></span> <span data-ttu-id="9973e-105">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="9973e-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9973e-106">Aktivera startdiagnostikinställningar på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9973e-106">Enable boot diagnostics on the VM</span></span>
> * <span data-ttu-id="9973e-107">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="9973e-107">View boot diagnostics</span></span>
> * <span data-ttu-id="9973e-108">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="9973e-108">View host metrics</span></span>
> * <span data-ttu-id="9973e-109">Aktivera diagnostik tillägget på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9973e-109">Enable diagnostics extension on the VM</span></span>
> * <span data-ttu-id="9973e-110">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="9973e-110">View VM metrics</span></span>
> * <span data-ttu-id="9973e-111">Skapa aviseringar baserat på diagnostiska mått</span><span class="sxs-lookup"><span data-stu-id="9973e-111">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="9973e-112">Konfigurera avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="9973e-112">Set up advanced monitoring</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="9973e-113">Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="9973e-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="9973e-114">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="9973e-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="9973e-115">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9973e-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-vm"></a><span data-ttu-id="9973e-116">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9973e-116">Create VM</span></span>

<span data-ttu-id="9973e-117">Om du vill visa diagnostik- och mått i praktiken, behöver du en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="9973e-117">To see diagnostics and metrics in action, you need a VM.</span></span> <span data-ttu-id="9973e-118">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="9973e-118">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="9973e-119">I följande exempel skapas en resursgrupp med namnet *myResourceGroupMonitor* i den *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="9973e-119">The following example creates a resource group named *myResourceGroupMonitor* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

<span data-ttu-id="9973e-120">Nu skapa en virtuell dator med [az vm skapa](https://docs.microsoft.com/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="9973e-120">Now create a VM with [az vm create](https://docs.microsoft.com/cli/azure/vm#create).</span></span> <span data-ttu-id="9973e-121">I följande exempel skapas en virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="9973e-121">The following example creates a VM named *myVM*:</span></span>

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a><span data-ttu-id="9973e-122">Aktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="9973e-122">Enable boot diagnostics</span></span>

<span data-ttu-id="9973e-123">Eftersom virtuella Linux-datorer startar Start diagnostiska tillägget avbildas Start utdata och lagrar den i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="9973e-123">As Linux VMs boot, the boot diagnostic extension captures boot output and stores it in Azure storage.</span></span> <span data-ttu-id="9973e-124">Dessa data kan användas för att felsöka problem med VM-start.</span><span class="sxs-lookup"><span data-stu-id="9973e-124">This data can be used to troubleshoot VM boot issues.</span></span> <span data-ttu-id="9973e-125">Startdiagnostikinställningar aktiveras inte automatiskt när du skapar en Linux VM som använder Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9973e-125">Boot diagnostics are not automatically enabled when you create a Linux VM using the Azure CLI.</span></span>

<span data-ttu-id="9973e-126">Innan du aktiverar startdiagnostikinställningar behöver ett lagringskonto skapas för att lagra start loggar.</span><span class="sxs-lookup"><span data-stu-id="9973e-126">Before enabling boot diagnostics, a storage account needs to be created for storing boot logs.</span></span> <span data-ttu-id="9973e-127">Storage-konton måste ha ett globalt unikt namn vara mellan 3 och 24 tecken och får enbart innehålla siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="9973e-127">Storage accounts must have a globally unique name, be between 3 and 24 characters, and must contain only numbers and lowercase letters.</span></span> <span data-ttu-id="9973e-128">Skapa ett lagringskonto med den [az storage-konto skapar](/cli/azure/storage/account#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="9973e-128">Create a storage account with the [az storage account create](/cli/azure/storage/account#create) command.</span></span> <span data-ttu-id="9973e-129">I det här exemplet används en slumpmässig sträng för att skapa en unik lagringskontonamnet.</span><span class="sxs-lookup"><span data-stu-id="9973e-129">In this example, a random string is used to create a unique storage account name.</span></span> 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

<span data-ttu-id="9973e-130">När du aktiverar startdiagnostikinställningar krävs URI till blob storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="9973e-130">When enabling boot diagnostics, the URI to the blob storage container is needed.</span></span> <span data-ttu-id="9973e-131">Följande kommando frågar storage-konto för att returnera URI: N.</span><span class="sxs-lookup"><span data-stu-id="9973e-131">The following command queries the storage account to return this URI.</span></span> <span data-ttu-id="9973e-132">URI-värdet lagras i ett variabelnamn *bloburi*, som används i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="9973e-132">The URI value is stored in a variable names *bloburi*, which is used in the next step.</span></span>

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

<span data-ttu-id="9973e-133">Nu aktivera startdiagnostikinställningar med [az vm-startdiagnostikinställningar aktivera](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="9973e-133">Now enable boot diagnostics with [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="9973e-134">Den `--storage` värdet är blob-URI som samlas in i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="9973e-134">The `--storage` value is the blob URI collected in the previous step.</span></span>

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a><span data-ttu-id="9973e-135">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="9973e-135">View boot diagnostics</span></span>

<span data-ttu-id="9973e-136">När startdiagnostikinställningar aktiveras skrivs varje gång du stoppa och starta den virtuella datorn, information om startprocessen till en loggfil.</span><span class="sxs-lookup"><span data-stu-id="9973e-136">When boot diagnostics are enabled, each time you stop and start the VM, information about the boot process is written to a log file.</span></span> <span data-ttu-id="9973e-137">I det här exemplet först frigöra den virtuella datorn med den [az vm frigöra](/cli/azure/vm#deallocate) kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9973e-137">For this example, first deallocate the VM with the [az vm deallocate](/cli/azure/vm#deallocate) command as follows:</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="9973e-138">Nu starta den virtuella datorn med den [az vm start]( /cli/azure/vm#stop) kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9973e-138">Now start the VM with the [az vm start]( /cli/azure/vm#stop) command as follows:</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="9973e-139">Du kan hämta Start diagnostikdata för *myVM* med den [az vm-startdiagnostikinställningar get-Start-loggen](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9973e-139">You can get the boot diagnostic data for *myVM* with the [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) command as follows:</span></span>

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a><span data-ttu-id="9973e-140">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="9973e-140">View host metrics</span></span>

<span data-ttu-id="9973e-141">En Linux-VM med en dedikerad värd i Azure som den interagerar med.</span><span class="sxs-lookup"><span data-stu-id="9973e-141">A Linux VM has a dedicated host in Azure that it interacts with.</span></span> <span data-ttu-id="9973e-142">Mått är automatiskt samlas in för värden och kan visas i Azure-portalen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9973e-142">Metrics are automatically collected for the host and can be viewed in the Azure portal as follows:</span></span>

1. <span data-ttu-id="9973e-143">I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroupMonitor**, och välj sedan **myVM** i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="9973e-143">In the Azure portal, click **Resource Groups**, select **myResourceGroupMonitor**, and then select **myVM** in the resource list.</span></span>
1. <span data-ttu-id="9973e-144">Klicka för att se hur den Virtuella värddatorn utförs **mått** på VM-bladet välj sedan någon av de *[värd]* mått under **tillgängliga mått**.</span><span class="sxs-lookup"><span data-stu-id="9973e-144">To see how the host VM is performing, click **Metrics** on the VM blade, then select any of the *[Host]* metrics under **Available metrics**.</span></span>

    ![Visa värden mått](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a><span data-ttu-id="9973e-146">Installera tillägg för diagnostik</span><span class="sxs-lookup"><span data-stu-id="9973e-146">Install diagnostics extension</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9973e-147">Det här dokumentet beskriver version 2.3 av Linux diagnostiska tillägget, som har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="9973e-147">This document describes version 2.3 of the Linux Diagnostic Extension, which has been deprecated.</span></span> <span data-ttu-id="9973e-148">Version 2.3 kommer att stödjas tills 30 juni 2018.</span><span class="sxs-lookup"><span data-stu-id="9973e-148">Version 2.3 will be supported until June 30, 2018.</span></span>
>
> <span data-ttu-id="9973e-149">Version 3.0 av Linux diagnostiska tillägget kan aktiveras i stället.</span><span class="sxs-lookup"><span data-stu-id="9973e-149">Version 3.0 of the Linux Diagnostic Extension can be enabled instead.</span></span> <span data-ttu-id="9973e-150">Mer information finns i [dokumentationen](./diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="9973e-150">For more information, see [the documentation](./diagnostic-extension.md).</span></span>

<span data-ttu-id="9973e-151">De grundläggande värden är tillgänglig, men att se mer detaljerad och VM-specifika statistik, och du behöver installera Azure-diagnostik-tillägget på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9973e-151">The basic host metrics are available, but to see more granular and VM-specific metrics, you to need to install the Azure diagnostics extension on the VM.</span></span> <span data-ttu-id="9973e-152">Azure diagnostics-tillägget kan ytterligare övervakning och diagnostikdata som ska hämtas från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9973e-152">The Azure diagnostics extension allows additional monitoring and diagnostics data to be retrieved from the VM.</span></span> <span data-ttu-id="9973e-153">Du kan visa dessa prestandamått och skapa varningar baserat på hur den virtuella datorn utför.</span><span class="sxs-lookup"><span data-stu-id="9973e-153">You can view these performance metrics and create alerts based on how the VM performs.</span></span> <span data-ttu-id="9973e-154">Diagnostiska tillägget installeras via Azure portal på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9973e-154">The diagnostic extension is installed through the Azure portal as follows:</span></span>

1. <span data-ttu-id="9973e-155">I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="9973e-155">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
1. <span data-ttu-id="9973e-156">Klicka på **diagnos inställningar**.</span><span class="sxs-lookup"><span data-stu-id="9973e-156">Click **Diagnosis settings**.</span></span> <span data-ttu-id="9973e-157">I listan visas som *starta diagnostik* redan aktiverat från föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9973e-157">The list shows that *Boot diagnostics* are already enabled from the previous section.</span></span> <span data-ttu-id="9973e-158">Klicka på kryssrutan för *grundläggande mått*.</span><span class="sxs-lookup"><span data-stu-id="9973e-158">Click the check box for *Basic metrics*.</span></span>
1. <span data-ttu-id="9973e-159">I den *lagringskonto* , bläddra till och välj den *mydiagdata [1234]* konto som har skapats i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9973e-159">In the *Storage account* section, browse to and select the *mydiagdata[1234]* account created in the previous section.</span></span>
1. <span data-ttu-id="9973e-160">Klicka på knappen **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9973e-160">Click the **Save** button.</span></span>

    ![Visa diagnostik mått](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a><span data-ttu-id="9973e-162">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="9973e-162">View VM metrics</span></span>

<span data-ttu-id="9973e-163">Du kan visa VM mått på samma sätt som du granskat värden VM mått:</span><span class="sxs-lookup"><span data-stu-id="9973e-163">You can view the VM metrics in the same way that you viewed the host VM metrics:</span></span>

1. <span data-ttu-id="9973e-164">I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="9973e-164">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
1. <span data-ttu-id="9973e-165">Klicka för att se hur den virtuella datorn utför **mått** på VM-bladet och välj sedan någon av diagnostik mått under **tillgängliga mått**.</span><span class="sxs-lookup"><span data-stu-id="9973e-165">To see how the VM is performing, click **Metrics** on the VM blade, and then select any of the diagnostics metrics under **Available metrics**.</span></span>

    ![Visa VM mått](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a><span data-ttu-id="9973e-167">Skapa aviseringar</span><span class="sxs-lookup"><span data-stu-id="9973e-167">Create alerts</span></span>

<span data-ttu-id="9973e-168">Du kan skapa varningar baserat på specifika prestandamått.</span><span class="sxs-lookup"><span data-stu-id="9973e-168">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="9973e-169">Aviseringar kan användas för att meddela dig när Genomsnittlig CPU-användning överskrider ett visst tröskelvärde eller tillgängligt diskutrymme sjunker under ett visst belopp, till exempel.</span><span class="sxs-lookup"><span data-stu-id="9973e-169">Alerts can be used to notify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="9973e-170">Aviseringar visas i Azure-portalen eller kan skickas via e-post.</span><span class="sxs-lookup"><span data-stu-id="9973e-170">Alerts are displayed in the Azure portal or can be sent via email.</span></span> <span data-ttu-id="9973e-171">Du kan också utlösa Azure Automation-runbooks eller Logikappar i Azure som svar på varningar genereras.</span><span class="sxs-lookup"><span data-stu-id="9973e-171">You can also trigger Azure Automation runbooks or Azure Logic Apps in response to alerts being generated.</span></span>

<span data-ttu-id="9973e-172">I följande exempel skapas en avisering för Genomsnittlig CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="9973e-172">The following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="9973e-173">I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.</span><span class="sxs-lookup"><span data-stu-id="9973e-173">In the Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in the resource list.</span></span>
2. <span data-ttu-id="9973e-174">Klicka på **Varna regler** på VM-bladet Klicka **Lägg till mått avisering** överst på bladet aviseringar.</span><span class="sxs-lookup"><span data-stu-id="9973e-174">Click **Alert rules** on the VM blade, then click **Add metric alert** across the top of the alerts blade.</span></span>
4. <span data-ttu-id="9973e-175">Ange en **namn** om varningen som *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="9973e-175">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="9973e-176">För att utlösa en avisering när CPU-procent överskrider 1.0 5 minuter, lämnar du alla andra standardinställningar som valts.</span><span class="sxs-lookup"><span data-stu-id="9973e-176">To trigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all the other defaults selected.</span></span>
6. <span data-ttu-id="9973e-177">Du kan också markera kryssrutan för *e-ägare, deltagare och läsare* att skicka e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="9973e-177">Optionally, check the box for *Email owners, contributors, and readers* to send email notification.</span></span> <span data-ttu-id="9973e-178">Standardåtgärden är att visa ett meddelande i portalen.</span><span class="sxs-lookup"><span data-stu-id="9973e-178">The default action is to present a notification in the portal.</span></span>
7. <span data-ttu-id="9973e-179">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9973e-179">Click the **OK** button.</span></span>


## <a name="advanced-monitoring"></a><span data-ttu-id="9973e-180">Avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="9973e-180">Advanced monitoring</span></span> 

<span data-ttu-id="9973e-181">Du kan göra mer avancerad övervakning av den virtuella datorn med hjälp av [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="9973e-181">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="9973e-182">Om du inte redan gjort det. Du kan registrera dig för en [kostnadsfri utvärderingsversion](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) av Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="9973e-182">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="9973e-183">När du har åtkomst till OMS-portalen hittar du den arbetsyta och arbetsytan identifierare på bladet inställningar.</span><span class="sxs-lookup"><span data-stu-id="9973e-183">When you have access to the OMS portal, you can find the workspace key and workspace identifier on the Settings blade.</span></span> <span data-ttu-id="9973e-184">Ersätt < arbetsytenyckel > och < arbetsytan-id > med värden från din OMS för arbetsytan och du sedan kan använda **az vm-tillägget set** lägga till OMS-tillägget till den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="9973e-184">Replace <workspace-key> and <workspace-id> with the values for from your OMS workspace and then you can use **az vm extension set** to add the OMS extension to the VM:</span></span>

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

<span data-ttu-id="9973e-185">I bladet loggen Sök i OMS-portalen kan du se *myVM* , till exempel vad som ska visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="9973e-185">On the Log Search blade of the OMS portal, you should see *myVM* such as what is shown in the following picture:</span></span>

![OMS-bladet](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="9973e-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9973e-187">Next steps</span></span>

<span data-ttu-id="9973e-188">I kursen får du konfigurerade och granskas virtuella datorer med Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="9973e-188">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="9973e-189">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="9973e-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9973e-190">Aktivera startdiagnostikinställningar på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9973e-190">Enable boot diagnostics on the VM</span></span>
> * <span data-ttu-id="9973e-191">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="9973e-191">View boot diagnostics</span></span>
> * <span data-ttu-id="9973e-192">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="9973e-192">View host metrics</span></span>
> * <span data-ttu-id="9973e-193">Aktivera diagnostik tillägget på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9973e-193">Enable diagnostics extension on the VM</span></span>
> * <span data-ttu-id="9973e-194">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="9973e-194">View VM metrics</span></span>
> * <span data-ttu-id="9973e-195">Skapa aviseringar baserat på diagnostiska mått</span><span class="sxs-lookup"><span data-stu-id="9973e-195">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="9973e-196">Konfigurera avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="9973e-196">Set up advanced monitoring</span></span>

<span data-ttu-id="9973e-197">Gå vidare till nästa kurs vill veta mer om Azure security center.</span><span class="sxs-lookup"><span data-stu-id="9973e-197">Advance to the next tutorial to learn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9973e-198">Hantera säkerheten för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="9973e-198">Manage VM security</span></span>](./tutorial-azure-security.md)