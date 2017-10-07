---
title: aaaMonitor Linux virtuella datorer i Azure | Microsoft Docs
description: "Lär dig hur toomonitor starta diagnostik- och prestandamått i en virtuell Linux-dator i Azure"
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
ms.openlocfilehash: 282da0f03ab0bf37bd44750c22813ca8d1c89560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="577ae-103">Hur toomonitor en Linux-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="577ae-103">How toomonitor a Linux virtual machine in Azure</span></span>

<span data-ttu-id="577ae-104">tooensure dina virtuella datorer (VM) i Azure körs på rätt sätt, kan du granska Start diagnostik- och prestandamått.</span><span class="sxs-lookup"><span data-stu-id="577ae-104">tooensure your virtual machines (VMs) in Azure are running correctly, you can review boot diagnostics and performance metrics.</span></span> <span data-ttu-id="577ae-105">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="577ae-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="577ae-106">Aktivera startdiagnostikinställningar på hello VM</span><span class="sxs-lookup"><span data-stu-id="577ae-106">Enable boot diagnostics on hello VM</span></span>
> * <span data-ttu-id="577ae-107">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="577ae-107">View boot diagnostics</span></span>
> * <span data-ttu-id="577ae-108">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="577ae-108">View host metrics</span></span>
> * <span data-ttu-id="577ae-109">Aktivera diagnostik hello VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="577ae-109">Enable diagnostics extension on hello VM</span></span>
> * <span data-ttu-id="577ae-110">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="577ae-110">View VM metrics</span></span>
> * <span data-ttu-id="577ae-111">Skapa aviseringar baserat på diagnostiska mått</span><span class="sxs-lookup"><span data-stu-id="577ae-111">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="577ae-112">Konfigurera avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="577ae-112">Set up advanced monitoring</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="577ae-113">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="577ae-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="577ae-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="577ae-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="577ae-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="577ae-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-vm"></a><span data-ttu-id="577ae-116">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="577ae-116">Create VM</span></span>

<span data-ttu-id="577ae-117">toosee diagnostik- och mått i åtgärden måste en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="577ae-117">toosee diagnostics and metrics in action, you need a VM.</span></span> <span data-ttu-id="577ae-118">Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="577ae-118">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="577ae-119">hello följande exempel skapar en resursgrupp med namnet *myResourceGroupMonitor* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="577ae-119">hello following example creates a resource group named *myResourceGroupMonitor* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

<span data-ttu-id="577ae-120">Nu skapa en virtuell dator med [az vm skapa](https://docs.microsoft.com/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="577ae-120">Now create a VM with [az vm create](https://docs.microsoft.com/cli/azure/vm#create).</span></span> <span data-ttu-id="577ae-121">hello följande exempel skapas en virtuell dator med namnet *myVM*:</span><span class="sxs-lookup"><span data-stu-id="577ae-121">hello following example creates a VM named *myVM*:</span></span>

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a><span data-ttu-id="577ae-122">Aktivera startdiagnostikinställningar</span><span class="sxs-lookup"><span data-stu-id="577ae-122">Enable boot diagnostics</span></span>

<span data-ttu-id="577ae-123">Som virtuella Linux-datorer startar hello Start diagnostiska tillägget avbildas Start utdata och lagrar den i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="577ae-123">As Linux VMs boot, hello boot diagnostic extension captures boot output and stores it in Azure storage.</span></span> <span data-ttu-id="577ae-124">Dessa data kan vara används tootroubleshoot VM startproblem.</span><span class="sxs-lookup"><span data-stu-id="577ae-124">This data can be used tootroubleshoot VM boot issues.</span></span> <span data-ttu-id="577ae-125">Startdiagnostikinställningar aktiveras inte automatiskt när du skapar en Linux VM som använder hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="577ae-125">Boot diagnostics are not automatically enabled when you create a Linux VM using hello Azure CLI.</span></span>

<span data-ttu-id="577ae-126">Innan du aktiverar startdiagnostikinställningar måste ett lagringskonto toobe som skapats för att lagra start loggar.</span><span class="sxs-lookup"><span data-stu-id="577ae-126">Before enabling boot diagnostics, a storage account needs toobe created for storing boot logs.</span></span> <span data-ttu-id="577ae-127">Storage-konton måste ha ett globalt unikt namn vara mellan 3 och 24 tecken och får enbart innehålla siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="577ae-127">Storage accounts must have a globally unique name, be between 3 and 24 characters, and must contain only numbers and lowercase letters.</span></span> <span data-ttu-id="577ae-128">Skapa ett lagringskonto med hello [az storage-konto skapar](/cli/azure/storage/account#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="577ae-128">Create a storage account with hello [az storage account create](/cli/azure/storage/account#create) command.</span></span> <span data-ttu-id="577ae-129">I det här exemplet är en slumpmässig sträng används toocreate en unik lagringskontonamnet.</span><span class="sxs-lookup"><span data-stu-id="577ae-129">In this example, a random string is used toocreate a unique storage account name.</span></span> 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

<span data-ttu-id="577ae-130">När du aktiverar startdiagnostikinställningar krävs hello URI toohello blob storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="577ae-130">When enabling boot diagnostics, hello URI toohello blob storage container is needed.</span></span> <span data-ttu-id="577ae-131">hello hello följande kommandofrågor storage-konto tooreturn URI: N.</span><span class="sxs-lookup"><span data-stu-id="577ae-131">hello following command queries hello storage account tooreturn this URI.</span></span> <span data-ttu-id="577ae-132">hello URI-värdet lagras i ett variabelnamn *bloburi*, som används i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="577ae-132">hello URI value is stored in a variable names *bloburi*, which is used in hello next step.</span></span>

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

<span data-ttu-id="577ae-133">Nu aktivera startdiagnostikinställningar med [az vm-startdiagnostikinställningar aktivera](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="577ae-133">Now enable boot diagnostics with [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="577ae-134">Hej `--storage` värdet är hello blob-URI som samlas in i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="577ae-134">hello `--storage` value is hello blob URI collected in hello previous step.</span></span>

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a><span data-ttu-id="577ae-135">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="577ae-135">View boot diagnostics</span></span>

<span data-ttu-id="577ae-136">När startdiagnostikinställningar aktiveras skrivs varje gång du stoppa och starta hello VM, information om hello startprocessen tooa loggfilen.</span><span class="sxs-lookup"><span data-stu-id="577ae-136">When boot diagnostics are enabled, each time you stop and start hello VM, information about hello boot process is written tooa log file.</span></span> <span data-ttu-id="577ae-137">I det här exemplet först frigöra hello VM med hello [az vm frigöra](/cli/azure/vm#deallocate) kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="577ae-137">For this example, first deallocate hello VM with hello [az vm deallocate](/cli/azure/vm#deallocate) command as follows:</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="577ae-138">Nu starta hello VM med hello [az vm start]( /cli/azure/vm#stop) kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="577ae-138">Now start hello VM with hello [az vm start]( /cli/azure/vm#stop) command as follows:</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="577ae-139">Du kan hämta hello Start diagnostikdata för *myVM* med hello [az vm-startdiagnostikinställningar get-Start-loggen](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="577ae-139">You can get hello boot diagnostic data for *myVM* with hello [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) command as follows:</span></span>

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a><span data-ttu-id="577ae-140">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="577ae-140">View host metrics</span></span>

<span data-ttu-id="577ae-141">En Linux-VM med en dedikerad värd i Azure som den interagerar med.</span><span class="sxs-lookup"><span data-stu-id="577ae-141">A Linux VM has a dedicated host in Azure that it interacts with.</span></span> <span data-ttu-id="577ae-142">Mått är automatiskt samlas in för hello värd och kan visas i hello Azure-portalen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="577ae-142">Metrics are automatically collected for hello host and can be viewed in hello Azure portal as follows:</span></span>

1. <span data-ttu-id="577ae-143">I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroupMonitor**, och välj sedan **myVM** i hello resurslistan.</span><span class="sxs-lookup"><span data-stu-id="577ae-143">In hello Azure portal, click **Resource Groups**, select **myResourceGroupMonitor**, and then select **myVM** in hello resource list.</span></span>
1. <span data-ttu-id="577ae-144">toosee hur hello värden VM utförs klickar du på **mått** hello VM-bladet, välj sedan någon av hello *[värd]* mått under **tillgängliga mått**.</span><span class="sxs-lookup"><span data-stu-id="577ae-144">toosee how hello host VM is performing, click **Metrics** on hello VM blade, then select any of hello *[Host]* metrics under **Available metrics**.</span></span>

    ![Visa värden mått](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a><span data-ttu-id="577ae-146">Installera tillägg för diagnostik</span><span class="sxs-lookup"><span data-stu-id="577ae-146">Install diagnostics extension</span></span>

> [!IMPORTANT]
> <span data-ttu-id="577ae-147">Det här dokumentet beskriver version 2.3 av hello diagnostiska Linux-tillägg, som har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="577ae-147">This document describes version 2.3 of hello Linux Diagnostic Extension, which has been deprecated.</span></span> <span data-ttu-id="577ae-148">Version 2.3 kommer att stödjas tills 30 juni 2018.</span><span class="sxs-lookup"><span data-stu-id="577ae-148">Version 2.3 will be supported until June 30, 2018.</span></span>
>
> <span data-ttu-id="577ae-149">Version 3.0 av hello Linux diagnostiska tillägget kan aktiveras i stället.</span><span class="sxs-lookup"><span data-stu-id="577ae-149">Version 3.0 of hello Linux Diagnostic Extension can be enabled instead.</span></span> <span data-ttu-id="577ae-150">Mer information finns i [hello dokumentationen](./diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="577ae-150">For more information, see [hello documentation](./diagnostic-extension.md).</span></span>

<span data-ttu-id="577ae-151">hello grundläggande värden är tillgänglig, men toosee mer detaljerade och VM-specifika statistik, och du tooneed tooinstall hello Azure diagnostics tillägg på hello VM.</span><span class="sxs-lookup"><span data-stu-id="577ae-151">hello basic host metrics are available, but toosee more granular and VM-specific metrics, you tooneed tooinstall hello Azure diagnostics extension on hello VM.</span></span> <span data-ttu-id="577ae-152">hello Azure diagnostics tillägget kan ytterligare övervakning och diagnostik data toobe hämtas från hello VM.</span><span class="sxs-lookup"><span data-stu-id="577ae-152">hello Azure diagnostics extension allows additional monitoring and diagnostics data toobe retrieved from hello VM.</span></span> <span data-ttu-id="577ae-153">Du kan visa dessa prestandamått och skapa varningar baserat på hur hello VM utför.</span><span class="sxs-lookup"><span data-stu-id="577ae-153">You can view these performance metrics and create alerts based on how hello VM performs.</span></span> <span data-ttu-id="577ae-154">hello diagnostiska tillägg har installerats via hello Azure-portalen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="577ae-154">hello diagnostic extension is installed through hello Azure portal as follows:</span></span>

1. <span data-ttu-id="577ae-155">I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.</span><span class="sxs-lookup"><span data-stu-id="577ae-155">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
1. <span data-ttu-id="577ae-156">Klicka på **diagnos inställningar**.</span><span class="sxs-lookup"><span data-stu-id="577ae-156">Click **Diagnosis settings**.</span></span> <span data-ttu-id="577ae-157">hello lista visar att *starta diagnostik* redan aktiverat från hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="577ae-157">hello list shows that *Boot diagnostics* are already enabled from hello previous section.</span></span> <span data-ttu-id="577ae-158">Klicka på hello kryssrutan för *grundläggande mått*.</span><span class="sxs-lookup"><span data-stu-id="577ae-158">Click hello check box for *Basic metrics*.</span></span>
1. <span data-ttu-id="577ae-159">I hello *lagringskonto* bläddrar tooand väljer hello *mydiagdata [1234]* konto hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="577ae-159">In hello *Storage account* section, browse tooand select hello *mydiagdata[1234]* account created in hello previous section.</span></span>
1. <span data-ttu-id="577ae-160">Klicka på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="577ae-160">Click hello **Save** button.</span></span>

    ![Visa diagnostik mått](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a><span data-ttu-id="577ae-162">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="577ae-162">View VM metrics</span></span>

<span data-ttu-id="577ae-163">Du kan visa hello VM mått i hello samma sätt som du granskat hello värden VM mått:</span><span class="sxs-lookup"><span data-stu-id="577ae-163">You can view hello VM metrics in hello same way that you viewed hello host VM metrics:</span></span>

1. <span data-ttu-id="577ae-164">I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.</span><span class="sxs-lookup"><span data-stu-id="577ae-164">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
1. <span data-ttu-id="577ae-165">toosee hur hello VM utförs klickar du på **mått** på hello VM-bladet och välj sedan någon av hello diagnostik mått under **tillgängliga mått**.</span><span class="sxs-lookup"><span data-stu-id="577ae-165">toosee how hello VM is performing, click **Metrics** on hello VM blade, and then select any of hello diagnostics metrics under **Available metrics**.</span></span>

    ![Visa VM mått](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a><span data-ttu-id="577ae-167">Skapa aviseringar</span><span class="sxs-lookup"><span data-stu-id="577ae-167">Create alerts</span></span>

<span data-ttu-id="577ae-168">Du kan skapa varningar baserat på specifika prestandamått.</span><span class="sxs-lookup"><span data-stu-id="577ae-168">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="577ae-169">Aviseringar kan till exempel vara används toonotify när Genomsnittlig CPU-användning överskrider ett visst tröskelvärde eller tillgängligt diskutrymme sjunker under en viss mängd.</span><span class="sxs-lookup"><span data-stu-id="577ae-169">Alerts can be used toonotify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="577ae-170">Aviseringar visas i hello Azure-portalen eller kan skickas via e-post.</span><span class="sxs-lookup"><span data-stu-id="577ae-170">Alerts are displayed in hello Azure portal or can be sent via email.</span></span> <span data-ttu-id="577ae-171">Du kan även utlösa Azure Automation-runbooks eller Azure Logikappar i svaret tooalerts som genereras.</span><span class="sxs-lookup"><span data-stu-id="577ae-171">You can also trigger Azure Automation runbooks or Azure Logic Apps in response tooalerts being generated.</span></span>

<span data-ttu-id="577ae-172">hello skapas följande exempel en avisering för Genomsnittlig CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="577ae-172">hello following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="577ae-173">I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.</span><span class="sxs-lookup"><span data-stu-id="577ae-173">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="577ae-174">Klicka på **Varna regler** på hello VM-bladet, klickar sedan på **Lägg till mått avisering** hello överst på bladet med säkerhetsaviseringar hello.</span><span class="sxs-lookup"><span data-stu-id="577ae-174">Click **Alert rules** on hello VM blade, then click **Add metric alert** across hello top of hello alerts blade.</span></span>
4. <span data-ttu-id="577ae-175">Ange en **namn** om varningen som *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="577ae-175">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="577ae-176">tootrigger en avisering när CPU-procent överskrider 1.0 i fem minuter lämnar du alla hello standardvärden som valts.</span><span class="sxs-lookup"><span data-stu-id="577ae-176">tootrigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all hello other defaults selected.</span></span>
6. <span data-ttu-id="577ae-177">Du kan också markera kryssrutan hello för *e-ägare, deltagare och läsare* toosend e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="577ae-177">Optionally, check hello box for *Email owners, contributors, and readers* toosend email notification.</span></span> <span data-ttu-id="577ae-178">hello standardåtgärden är toopresent ett meddelande i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="577ae-178">hello default action is toopresent a notification in hello portal.</span></span>
7. <span data-ttu-id="577ae-179">Klicka på hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="577ae-179">Click hello **OK** button.</span></span>


## <a name="advanced-monitoring"></a><span data-ttu-id="577ae-180">Avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="577ae-180">Advanced monitoring</span></span> 

<span data-ttu-id="577ae-181">Du kan göra mer avancerad övervakning av den virtuella datorn med hjälp av [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="577ae-181">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="577ae-182">Om du inte redan gjort det. Du kan registrera dig för en [kostnadsfri utvärderingsversion](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) av Operations Management Suite.</span><span class="sxs-lookup"><span data-stu-id="577ae-182">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="577ae-183">När du har åtkomst toohello OMS-portalen hittar hello arbetsytan arbetsytan och nyckeln identifierare på inställningsbladet för hello.</span><span class="sxs-lookup"><span data-stu-id="577ae-183">When you have access toohello OMS portal, you can find hello workspace key and workspace identifier on hello Settings blade.</span></span> <span data-ttu-id="577ae-184">Ersätt < arbetsytenyckel > och < arbetsytan-id > med hello värden för från din OMS arbetsyta och du sedan kan använda **az vm-tillägget set** tooadd hello OMS tillägget toohello VM:</span><span class="sxs-lookup"><span data-stu-id="577ae-184">Replace <workspace-key> and <workspace-id> with hello values for from your OMS workspace and then you can use **az vm extension set** tooadd hello OMS extension toohello VM:</span></span>

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

<span data-ttu-id="577ae-185">På hello loggen Sök bladet för hello OMS-portalen, bör du se *myVM* , till exempel vad som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="577ae-185">On hello Log Search blade of hello OMS portal, you should see *myVM* such as what is shown in hello following picture:</span></span>

![OMS-bladet](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="577ae-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="577ae-187">Next steps</span></span>

<span data-ttu-id="577ae-188">I kursen får du konfigurerade och granskas virtuella datorer med Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="577ae-188">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="577ae-189">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="577ae-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="577ae-190">Aktivera startdiagnostikinställningar på hello VM</span><span class="sxs-lookup"><span data-stu-id="577ae-190">Enable boot diagnostics on hello VM</span></span>
> * <span data-ttu-id="577ae-191">Visa startdiagnostik</span><span class="sxs-lookup"><span data-stu-id="577ae-191">View boot diagnostics</span></span>
> * <span data-ttu-id="577ae-192">Visa värden mått</span><span class="sxs-lookup"><span data-stu-id="577ae-192">View host metrics</span></span>
> * <span data-ttu-id="577ae-193">Aktivera diagnostik hello VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="577ae-193">Enable diagnostics extension on hello VM</span></span>
> * <span data-ttu-id="577ae-194">Visa VM mått</span><span class="sxs-lookup"><span data-stu-id="577ae-194">View VM metrics</span></span>
> * <span data-ttu-id="577ae-195">Skapa aviseringar baserat på diagnostiska mått</span><span class="sxs-lookup"><span data-stu-id="577ae-195">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="577ae-196">Konfigurera avancerad övervakning</span><span class="sxs-lookup"><span data-stu-id="577ae-196">Set up advanced monitoring</span></span>

<span data-ttu-id="577ae-197">Avancera toohello nästa självstudiekurs toolearn om Azure security center.</span><span class="sxs-lookup"><span data-stu-id="577ae-197">Advance toohello next tutorial toolearn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="577ae-198">Hantera säkerheten för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="577ae-198">Manage VM security</span></span>](./tutorial-azure-security.md)