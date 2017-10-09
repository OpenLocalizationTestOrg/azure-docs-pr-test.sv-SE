---
title: "aaaHPC Pack kluster för Excel och SOA | Microsoft Docs"
description: "Kom igång storskaliga Excel- och SOA-arbetsbelastningar som körs i ett HPC Pack-kluster i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: cb6a9abe-caf3-44da-b911-849a50f6cfb3
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 55b4b2c25fe65d06b75025cc23c3c13b8b764238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="bc7fe-103">Kom igång Excel- och SOA-arbetsbelastningar som körs i ett HPC Pack-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="bc7fe-103">Get started running Excel and SOA workloads on an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="bc7fe-104">Den här artikeln visar hur toodeploy Microsoft HPC Pack 2012 R2-kluster på virtuella Azure-datorer med hjälp av en mall för Azure quickstart eller alternativt en Azure PowerShell-distributionsskriptet.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-104">This article shows you how toodeploy a Microsoft HPC Pack 2012 R2 cluster on Azure virtual machines by using an Azure quickstart template, or optionally an Azure PowerShell deployment script.</span></span> <span data-ttu-id="bc7fe-105">hello klustret använder Azure Marketplace VM bilder som har utformats för toorun Microsoft Excel eller tjänstorienterad arkitektur (SOA) arbetsbelastningar med HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-105">hello cluster uses Azure Marketplace VM images designed toorun Microsoft Excel or service-oriented architecture (SOA) workloads with HPC Pack.</span></span> <span data-ttu-id="bc7fe-106">Du kan använda hello klustret toorun Excel HPC och SOA-tjänster från en klientdator för lokalt.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-106">You can use hello cluster toorun Excel HPC and SOA services from an on-premises client computer.</span></span> <span data-ttu-id="bc7fe-107">hello Excel HPC tjänster omfattar arbetsboksavlastning i Excel och Excel användardefinierade funktioner eller UDF: er.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-107">hello Excel HPC services include Excel workbook offloading and Excel user-defined functions, or UDFs.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="bc7fe-108">Den här artikeln baseras på funktioner, mallar och skript för HPC Pack 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-108">This article is based on features, templates, and scripts for HPC Pack 2012 R2.</span></span> <span data-ttu-id="bc7fe-109">Det här scenariot stöds inte för närvarande i HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-109">This scenario is not currently supported in HPC Pack 2016.</span></span>
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="bc7fe-110">På en hög nivå hello följande diagram visar hello HPC Pack kluster du skapar.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-110">At a high level, hello following diagram shows hello HPC Pack cluster you create.</span></span>

![HPC-kluster med noder som kör Excel arbetsbelastningar][scenario]

## <a name="prerequisites"></a><span data-ttu-id="bc7fe-112">Krav</span><span class="sxs-lookup"><span data-stu-id="bc7fe-112">Prerequisites</span></span>
* <span data-ttu-id="bc7fe-113">**Klientdatorn** -du behöver ett Windows-baserad klient datorn toosubmit exempel Excel- och SOA-jobb toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-113">**Client computer** - You need a Windows-based client computer toosubmit sample Excel and SOA jobs toohello cluster.</span></span> <span data-ttu-id="bc7fe-114">Du måste också en Windows datorn toorun hello Azure PowerShell distributionsskriptet för klustret (om du väljer att distributionsmetoden).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-114">You also need a Windows computer toorun hello Azure PowerShell cluster deployment script (if you choose that deployment method).</span></span>
* <span data-ttu-id="bc7fe-115">**Azure-prenumeration** -om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/) på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-115">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="bc7fe-116">**Kärnor kvoten** – du kan behöva tooincrease hello kvoten för kärnor, särskilt om du distribuerar flera klusternoder med flera kärnor VM-storlekar.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-116">**Cores quota** - You might need tooincrease hello quota of cores, especially if you deploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="bc7fe-117">Om du använder en mall för Azure quickstart är hello kärnor kvot i Resource Manager per Azure-region.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-117">If you are using an Azure quickstart template, hello cores quota in Resource Manager is per Azure region.</span></span> <span data-ttu-id="bc7fe-118">I så fall kan behöva du tooincrease hello kvot i en viss region.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-118">In that case, you might need tooincrease hello quota in a specific region.</span></span> <span data-ttu-id="bc7fe-119">Se [Azure-prenumeration gränser, kvoter och begränsningar](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-119">See [Azure subscription limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span> <span data-ttu-id="bc7fe-120">tooincrease en kvot [öppna en supportbegäran online customer](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-120">tooincrease a quota, [open an online customer support request](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) at no charge.</span></span>
* <span data-ttu-id="bc7fe-121">**Microsoft Office-licens** - om du distribuerar compute-noder som använder en Marketplace HPC Pack 2012 R2 VM-avbildning med Microsoft Excel, en 30-dagars utvärderingsversion av Microsoft Excel Professional Plus 2013 har installerats.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-121">**Microsoft Office license** - If you deploy compute nodes using a Marketplace HPC Pack 2012 R2 VM image with Microsoft Excel, a 30-day evaluation version of Microsoft Excel Professional Plus 2013 is installed.</span></span> <span data-ttu-id="bc7fe-122">När du hello utvärderingsperioden måste tooprovide en giltig Microsoft Office-licens tooactivate Excel toocontinue toorun arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-122">After hello evaluation period, you need tooprovide a valid Microsoft Office license tooactivate Excel toocontinue toorun workloads.</span></span> <span data-ttu-id="bc7fe-123">Se [Excel aktivering](#excel-activation) senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-123">See [Excel activation](#excel-activation) later in this article.</span></span> 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="bc7fe-124">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-124">Step 1.</span></span> <span data-ttu-id="bc7fe-125">Konfigurera ett HPC Pack kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="bc7fe-125">Set up an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="bc7fe-126">Visar vi två alternativ tooset in hello HPC Pack 2012 R2-kluster: första, med hjälp av en mall för Azure quickstart och hello Azure-portalen; och andra som använder ett skript för distribution av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-126">We show two options tooset up hello HPC Pack 2012 R2 cluster: first, using an Azure quickstart template and hello Azure portal; and second, using an Azure PowerShell deployment script.</span></span>

### <a name="option-1-use-a-quickstart-template"></a><span data-ttu-id="bc7fe-127">Alternativ 1.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-127">Option 1.</span></span> <span data-ttu-id="bc7fe-128">Använd en mall för Snabbstart</span><span class="sxs-lookup"><span data-stu-id="bc7fe-128">Use a quickstart template</span></span>
<span data-ttu-id="bc7fe-129">Använd en mall för Azure quickstart-tooquickly distribuera ett HPC Pack kluster i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-129">Use an Azure quickstart template tooquickly deploy an HPC Pack cluster in hello Azure portal.</span></span> <span data-ttu-id="bc7fe-130">När du öppnar hello mallen i hello-portalen kan du få ett enkelt gränssnitt där du kan ange hello inställningar för klustret.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-130">When you open hello template in hello portal, you get a simple UI where you enter hello settings for your cluster.</span></span> <span data-ttu-id="bc7fe-131">Här är hello åtgärder.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-131">Here are hello steps.</span></span> 

> [!TIP]
> <span data-ttu-id="bc7fe-132">Om du vill använda en [Azure Marketplace mallen](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) som skapar ett liknande kluster specifikt för Excel-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-132">If you want, use an [Azure Marketplace template](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) that creates a similar cluster specifically for Excel workloads.</span></span> <span data-ttu-id="bc7fe-133">hello stegen skiljer sig från hello följande.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-133">hello steps differ slightly from hello following.</span></span>
> 
> 

1. <span data-ttu-id="bc7fe-134">Besök hello [skapa HPC-kluster mallsida på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-134">Visit hello [Create HPC Cluster template page on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster).</span></span> <span data-ttu-id="bc7fe-135">Om du vill granska information om hello mallen och hello källkoden.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-135">If you want, review information about hello template and hello source code.</span></span>
2. <span data-ttu-id="bc7fe-136">Klicka på **distribuera tooAzure** toostart en distribution med hello mallen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-136">Click **Deploy tooAzure** toostart a deployment with hello template in hello Azure portal.</span></span>
   
   ![Distribuera mallen tooAzure][github]
3. <span data-ttu-id="bc7fe-138">Följ dessa steg tooenter hello parametrar för hello HPC-kluster mallen i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-138">In hello portal, follow these steps tooenter hello parameters for hello HPC cluster template.</span></span>
   
   <span data-ttu-id="bc7fe-139">a.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-139">a.</span></span> <span data-ttu-id="bc7fe-140">På hello **parametrar** anger eller ändrar värdena för hello mallparametrar.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-140">On hello **Parameters** page, enter or modify values for hello template parameters.</span></span> <span data-ttu-id="bc7fe-141">(Klicka på hello ikonen nästa tooeach inställning för hjälp.) I hello följande skärm visas exempelvärden.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-141">(Click hello icon next tooeach setting for help information.) Sample values are shown in hello following screen.</span></span> <span data-ttu-id="bc7fe-142">Det här exemplet skapar ett kluster med namnet *hpc01* i hello *hpc.local* domän som består av en huvudnod och 2 compute-noder.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-142">This example creates a cluster named *hpc01* in hello *hpc.local* domain consisting of a head node and 2 compute nodes.</span></span> <span data-ttu-id="bc7fe-143">hello compute-noder har skapats från en HPC Pack VM-avbildning som omfattar Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-143">hello compute nodes are created from an HPC Pack VM image that includes Microsoft Excel.</span></span>
   
   ![Ange parametrar][parameters-new-portal]
   
   > [!NOTE]
   > <span data-ttu-id="bc7fe-145">hello huvudnod VM skapas automatiskt från hello [senaste Marketplace-avbildning](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) av HPC Pack 2012 R2 på Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-145">hello head node VM is created automatically from hello [latest Marketplace image](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) of HPC Pack 2012 R2 on Windows Server 2012 R2.</span></span> <span data-ttu-id="bc7fe-146">För närvarande baseras hello på HPC Pack 2012 R2 uppdatering 3.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-146">Currently hello image is based on HPC Pack 2012 R2 Update 3.</span></span>
   > 
   > <span data-ttu-id="bc7fe-147">Compute-nod virtuella datorer skapas från hello senaste bild av hello valt beräkning nod familj.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-147">Compute node VMs are created from hello latest image of hello selected compute node family.</span></span> <span data-ttu-id="bc7fe-148">Välj hello **ComputeNodeWithExcel** alternativ för hello senaste HPC Pack compute-nod avbildning som omfattar en utvärderingsversion av Microsoft Excel Professional Plus 2013.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-148">Select hello **ComputeNodeWithExcel** option for hello latest HPC Pack compute node image that includes an evaluation version of Microsoft Excel Professional Plus 2013.</span></span> <span data-ttu-id="bc7fe-149">toodeploy ett kluster för allmän SOA-sessioner eller Excel-UDF-avlastning, Välj hello **ComputeNode** alternativ (utan Excel installerat).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-149">toodeploy a cluster for general SOA sessions or for Excel UDF offloading, choose hello **ComputeNode** option (without Excel installed).</span></span>
   > 
   > 
   
   <span data-ttu-id="bc7fe-150">b.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-150">b.</span></span> <span data-ttu-id="bc7fe-151">Välj hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-151">Choose hello subscription.</span></span>
   
   <span data-ttu-id="bc7fe-152">c.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-152">c.</span></span> <span data-ttu-id="bc7fe-153">Skapa en resursgrupp för hello-kluster som *hpc01RG*.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-153">Create a resource group for hello cluster, such as *hpc01RG*.</span></span>
   
   <span data-ttu-id="bc7fe-154">d.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-154">d.</span></span> <span data-ttu-id="bc7fe-155">Välj en plats för hello resursgrupp, till exempel centrala USA.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-155">Choose a location for hello resource group, such as Central US.</span></span>
   
   <span data-ttu-id="bc7fe-156">e.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-156">e.</span></span> <span data-ttu-id="bc7fe-157">På hello **juridiska villkor** granskar hello villkoren.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-157">On hello **Legal terms** page, review hello terms.</span></span> <span data-ttu-id="bc7fe-158">Om du godkänner villkoren klickar du på **inköp**.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-158">If you agree, click **Purchase**.</span></span> <span data-ttu-id="bc7fe-159">När du är klar inställningsvärden hello hello mallen Klicka **skapa**.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-159">Then, when you are finished setting hello values for hello template, click **Create**.</span></span>
4. <span data-ttu-id="bc7fe-160">När hello distribution har slutförts (det tar vanligtvis cirka 30 minuter), exportera hello klustret certifikatfilen från hello klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-160">When hello deployment completes (it typically takes around 30 minutes), export hello cluster certificate file from hello cluster head node.</span></span> <span data-ttu-id="bc7fe-161">I ett senare steg kan du importera den här offentliga certifikat på hello tooprovide hello serversidan klientdatorautentisering för säker HTTP-bindning.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-161">In a later step, you import this public certificate on hello client computer tooprovide hello server-side authentication for secure HTTP binding.</span></span>
   
   <span data-ttu-id="bc7fe-162">a.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-162">a.</span></span> <span data-ttu-id="bc7fe-163">I hello Azure-portalen, går toohello instrumentpanelen, väljer hello huvudnod, och klickar på **Anslut** hello överst i hello sidan tooconnect med hjälp av fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-163">In hello Azure portal, go toohello dashboard, select hello head node, and click **Connect** at hello top of hello page tooconnect using Remote Desktop.</span></span>
   
    <!-- ![Connect toohello head node][connect] -->
   
   <span data-ttu-id="bc7fe-164">b.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-164">b.</span></span> <span data-ttu-id="bc7fe-165">Använd standard procedurer i Certifikathanteraren tooexport hello huvudnod certifikat (finns under Cert: \LocalMachine\My) utan hello privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-165">Use standard procedures in Certificate Manager tooexport hello head node certificate (located under Cert:\LocalMachine\My) without hello private key.</span></span> <span data-ttu-id="bc7fe-166">Exportera i det här exemplet *CN = hpc01.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-166">In this example, export *CN = hpc01.eastus.cloudapp.azure.com*.</span></span>
   
   ![Exportera hello certifikat][cert]

### <a name="option-2-use-hello-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="bc7fe-168">Alternativ 2.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-168">Option 2.</span></span> <span data-ttu-id="bc7fe-169">Använd hello HPC Pack IaaS distributionsskriptet</span><span class="sxs-lookup"><span data-stu-id="bc7fe-169">Use hello HPC Pack IaaS Deployment script</span></span>
<span data-ttu-id="bc7fe-170">hello HPC Pack IaaS distributionsskriptet innehåller en annan flexibel sätt toodeploy ett HPC Pack-kluster.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-170">hello HPC Pack IaaS deployment script provides another versatile way toodeploy an HPC Pack cluster.</span></span> <span data-ttu-id="bc7fe-171">Den skapar ett kluster i hello klassiska distributionsmodellen medan hello mallen använder hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-171">It creates a cluster in hello classic deployment model, whereas hello template uses hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="bc7fe-172">Hello skript är också kompatibla med en prenumeration i Azure globala hello eller Azure Kina service.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-172">Also, hello script is compatible with a subscription in either hello Azure Global or Azure China service.</span></span>

<span data-ttu-id="bc7fe-173">**Ytterligare krav**</span><span class="sxs-lookup"><span data-stu-id="bc7fe-173">**Additional prerequisites**</span></span>

* <span data-ttu-id="bc7fe-174">**Azure PowerShell** - [installera och konfigurera Azure PowerShell](/powershell/azure/overview) (version 0.8.10 eller senare) på din klientdator.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-174">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="bc7fe-175">**HPC Pack IaaS distributionsskriptet** – hämta och packa upp hello senaste versionen av hello skriptet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-175">**HPC Pack IaaS deployment script** - Download and unpack hello latest version of hello script from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="bc7fe-176">Kontrollera hello version av hello skriptet genom att köra `New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-176">Check hello version of hello script by running `New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="bc7fe-177">Den här artikeln är baserat på version 4.5.0 eller senare av hello skript.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-177">This article is based on version 4.5.0 or later of hello script.</span></span>

<span data-ttu-id="bc7fe-178">**Skapa hello-konfigurationsfil**</span><span class="sxs-lookup"><span data-stu-id="bc7fe-178">**Create hello configuration file**</span></span>

 <span data-ttu-id="bc7fe-179">hello HPC Pack IaaS distributionsskriptet använder en XML-konfigurationsfil som indata som beskriver hello infrastruktur hello HPC-kluster.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-179">hello HPC Pack IaaS deployment script uses an XML configuration file as input that describes hello infrastructure of hello HPC cluster.</span></span> <span data-ttu-id="bc7fe-180">toodeploy ett kluster som består av en huvudnod och 18 compute-noder som skapas från hello compute-nod avbildning som omfattar Microsoft Excel Ersätt värdena för din miljö till hello följande exempel konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-180">toodeploy a cluster consisting of a head node and 18 compute nodes created from hello compute node image that includes Microsoft Excel, substitute values for your environment into hello following sample configuration file.</span></span> <span data-ttu-id="bc7fe-181">Mer information om hello-konfigurationsfilen finns hello Manual.rtf fil i mappen för hello-skript och [skapa ett HPC-kluster med hello HPC Pack IaaS distributionsskriptet](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-181">For more information about hello configuration file, see hello Manual.rtf file in hello script folder and [Create an HPC cluster with hello HPC Pack IaaS deployment script](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

<span data-ttu-id="bc7fe-182">**Information om hello-konfigurationsfil**</span><span class="sxs-lookup"><span data-stu-id="bc7fe-182">**Notes about hello configuration file**</span></span>

* <span data-ttu-id="bc7fe-183">Hej **VMName** av hello huvudnod **måste** hello vara samma som hello **ServiceName**, SOA-jobb misslyckas toorun.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-183">hello **VMName** of hello head node **MUST** be hello same as hello **ServiceName**, or SOA jobs fail toorun.</span></span>
* <span data-ttu-id="bc7fe-184">Kontrollera att du anger **EnableWebPortal** så att hello huvudnod certifikatet genereras och exporteras.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-184">Make sure you specify **EnableWebPortal** so that hello head node certificate is generated and exported.</span></span>
* <span data-ttu-id="bc7fe-185">hello-filen anger ett efterkonfiguration PowerShell-skript PostConfig.ps1 som körs på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-185">hello file specifies a post-configuration PowerShell script PostConfig.ps1 that runs on hello head node.</span></span> <span data-ttu-id="bc7fe-186">hello följande exempelskript konfigurerar hello anslutningssträngen för Azure storage, tar bort hello beräkningsrollen noden från hello huvudnod och ger alla noder online när de distribueras.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-186">hello following sample script configures hello Azure storage connection string, removes hello compute node role from hello head node, and brings all nodes online when they are deployed.</span></span> 

```
    # add hello HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set hello Azure storage connection string for hello cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove hello compute node role for head node toomake sure hello Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in hello deployment including hello head node and compute nodes, which should match hello number specified in hello XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

<span data-ttu-id="bc7fe-187">**Kör hello-skript**</span><span class="sxs-lookup"><span data-stu-id="bc7fe-187">**Run hello script**</span></span>

1. <span data-ttu-id="bc7fe-188">Öppna hello PowerShell-konsol på hello klientdatorn som administratör.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-188">Open hello PowerShell console on hello client computer as an administrator.</span></span>
2. <span data-ttu-id="bc7fe-189">Ändra katalogmapp toohello skript (E:\IaaSClusterScript i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-189">Change directory toohello script folder (E:\IaaSClusterScript in this example).</span></span>
   
   ```
   cd E:\IaaSClusterScript
   ```
3. <span data-ttu-id="bc7fe-190">toodeploy hello HPC Pack klustret, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-190">toodeploy hello HPC Pack cluster, run hello following command.</span></span> <span data-ttu-id="bc7fe-191">Det här exemplet förutsätter att hello-konfigurationsfilen finns i E:\HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-191">This example assumes that hello configuration file is located in E:\HPCDemoConfig.xml.</span></span>
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

<span data-ttu-id="bc7fe-192">hello HPC Pack distributionsskriptet körs under en viss tid.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-192">hello HPC Pack deployment script runs for some time.</span></span> <span data-ttu-id="bc7fe-193">En sak hello skriptet gör är tooexport och hämta hello klustret certifikatet och spara det i hello den aktuella användarens dokument-mappen på hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-193">One thing hello script does is tooexport and download hello cluster certificate and save it in hello current user’s Documents folder on hello client computer.</span></span> <span data-ttu-id="bc7fe-194">hello skriptet genererar ett meddelande liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-194">hello script generates a message similar toohello following.</span></span> <span data-ttu-id="bc7fe-195">I följande steg ska importera du hello certifikat i certifikatarkivet för hello lämplig.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-195">In a following step, you import hello certificate in hello appropriate certificate store.</span></span>    

    You have enabled REST API or web portal on HPC Pack head node. Please import hello following certificate in hello Trusted Root Certification Authorities certificate store on hello computer where you are submitting job or accessing hello HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a><span data-ttu-id="bc7fe-196">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-196">Step 2.</span></span> <span data-ttu-id="bc7fe-197">Avlastning Excel-arbetsböcker och köra UDF: er från en lokal klient</span><span class="sxs-lookup"><span data-stu-id="bc7fe-197">Offload Excel workbooks and run UDFs from an on-premises client</span></span>
### <a name="excel-activation"></a><span data-ttu-id="bc7fe-198">Excel-aktivering</span><span class="sxs-lookup"><span data-stu-id="bc7fe-198">Excel activation</span></span>
<span data-ttu-id="bc7fe-199">När du använder hello ComputeNodeWithExcel VM-avbildning för produktionsarbetsbelastningar måste tooprovide en giltig Microsoft Office-licens viktiga tooactivate Excel på hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-199">When using hello ComputeNodeWithExcel VM image for production workloads, you need tooprovide a valid Microsoft Office license key tooactivate Excel on hello compute nodes.</span></span> <span data-ttu-id="bc7fe-200">Annars hello utvärderingsversionen av Microsoft Excel upphör att gälla efter 30 dagar och kör Excel-arbetsböcker misslyckas med hello COMException (0x800AC472).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-200">Otherwise, hello evaluation version of Excel expires after 30 days, and running Excel workbooks will fail with hello COMException (0x800AC472).</span></span> 

<span data-ttu-id="bc7fe-201">Du kan återställa Excel med ytterligare 30 dagar utvärdering tid: Logga in toohello huvudnod och clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` på alla Excel datornoder via HPC Cluster Manager.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-201">You can rearm Excel for another 30 days of evaluation time: Log on toohello head node and clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` on all Excel compute nodes via HPC Cluster Manager.</span></span> <span data-ttu-id="bc7fe-202">Du kan återställa högst två gånger.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-202">You can rearm a maximum of two times.</span></span> <span data-ttu-id="bc7fe-203">Därefter måste du ange en giltig licensnyckel Office.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-203">After that, you must provide a valid Office license key.</span></span>

<span data-ttu-id="bc7fe-204">hello Office Professional Plus 2013 har installerats på hello VM-avbildning är en volymutgåva med en allmän nyckel GVLK (Volume License).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-204">hello Office Professional Plus 2013 installed on hello VM image is a volume edition with a Generic Volume License Key (GVLK).</span></span> <span data-ttu-id="bc7fe-205">Du kan aktivera den via nyckelhanteringstjänsten (KMS) / Active Directory-Based aktivering (AD BA) eller fleraktiveringsnyckel (MAK).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-205">You can activate it via Key Management Service (KMS)/Active Directory-Based Activation (AD-BA) or Multiple Activation Key (MAK).</span></span> 

    * <span data-ttu-id="bc7fe-206">toouse KMS/AD-BA, Använd en befintlig KMS-server eller Ställ in en ny med hjälp av hello licenspaketet för Microsoft Office 2013 volym.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-206">toouse KMS/AD-BA, use an existing KMS server or set up a new one by using hello Microsoft Office 2013 Volume License Pack.</span></span> <span data-ttu-id="bc7fe-207">(Om du vill ställa in hello server i hello huvudnod.) Aktivera sedan hello KMS-värdnyckeln via hello Internet eller per telefon.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-207">(If you want to, set up hello server on hello head node.) Then, activate hello KMS host key via hello Internet or telephone.</span></span> <span data-ttu-id="bc7fe-208">Sedan clusrun `ospp.vbs` tooset hello KMS-servern och porten och aktivera Office på alla hello Excel compute-noder.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-208">Then clusrun `ospp.vbs` tooset hello KMS server and port and activate Office on all hello Excel compute nodes.</span></span> 

    * <span data-ttu-id="bc7fe-209">toouse MAK första clusrun `ospp.vbs` tooinput hello nyckeln och sedan aktivera alla hello Excel datornoderna via hello Internet eller per telefon.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-209">toouse MAK, first clusrun `ospp.vbs` tooinput hello key and then activate all hello Excel compute nodes via hello Internet or telephone.</span></span> 

> [!NOTE]
> <span data-ttu-id="bc7fe-210">Retail produktnycklar för Office Professional Plus 2013 kan inte användas med den här Virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-210">Retail product keys for Office Professional Plus 2013 cannot be used with this VM image.</span></span> <span data-ttu-id="bc7fe-211">Om du har giltiga nycklar och installationsmediet för Office eller Excel-versioner än den här Office Professional Plus 2013 volym-versionen kan använda du dem i stället.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-211">If you have valid keys and installation media for Office or Excel editions other than this Office Professional Plus 2013 volume edition, you can use them instead.</span></span> <span data-ttu-id="bc7fe-212">Först avinstallera den här volymen-versionen och installera hello-version som du har.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-212">First uninstall this volume edition and install hello edition that you have.</span></span> <span data-ttu-id="bc7fe-213">hello ominstalleras Excel compute-nod kan sparas som en anpassad VM avbildningen toouse i en distribution i stor skala.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-213">hello reinstalled Excel compute node can be captured as a customized VM image toouse in a deployment at scale.</span></span>
> 
> 

### <a name="offload-excel-workbooks"></a><span data-ttu-id="bc7fe-214">Avlastning Excel-arbetsböcker</span><span class="sxs-lookup"><span data-stu-id="bc7fe-214">Offload Excel workbooks</span></span>
<span data-ttu-id="bc7fe-215">Följ dessa steg toooffload en Excel-arbetsbok så att den körs på hello HPC Pack klustret i Azure.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-215">Follow these steps toooffload an Excel workbook so that it runs on hello HPC Pack cluster in Azure.</span></span> <span data-ttu-id="bc7fe-216">toodo, måste du ha Excel 2010 eller 2013 redan installerad på hello klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-216">toodo this, you must have Excel 2010 or 2013 already installed on hello client computer.</span></span>

1. <span data-ttu-id="bc7fe-217">Använd en av hello alternativ i steg 1 toodeploy ett HPC Pack kluster med hello Excel compute-nod avbildningen.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-217">Use one of hello options in Step 1 toodeploy an HPC Pack cluster with hello Excel compute node image.</span></span> <span data-ttu-id="bc7fe-218">Hämta hello klustret certifikatfil (.cer) och klustret användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-218">Obtain hello cluster certificate file (.cer) and cluster username and password.</span></span>
2. <span data-ttu-id="bc7fe-219">Importera hello klustret certifikat under Cert: \CurrentUser\Root på hello-klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-219">On hello client computer, import hello cluster certificate under Cert:\CurrentUser\Root.</span></span>
3. <span data-ttu-id="bc7fe-220">Kontrollera att Excel har installerats.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-220">Make sure Excel is installed.</span></span> <span data-ttu-id="bc7fe-221">Skapa en Excel.exe.config-fil med följande innehåll i hello hello samma mapp som Excel.exe på hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-221">Create an Excel.exe.config file with hello following contents in hello same folder as Excel.exe on hello client computer.</span></span> <span data-ttu-id="bc7fe-222">Det här steget säkerställer att hello HPC Pack 2012 R2 Excel COM-tillägg har lästs in.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-222">This step ensures that hello HPC Pack 2012 R2 Excel COM add-in loads successfully.</span></span>
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. <span data-ttu-id="bc7fe-223">Ställ in hello klient toosubmit jobb toohello HPC Pack kluster.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-223">Set up hello client toosubmit jobs toohello HPC Pack cluster.</span></span> <span data-ttu-id="bc7fe-224">Ett alternativ är toodownload hello fullständig [HPC Pack 2012 R2 uppdatering 3 installation](http://www.microsoft.com/download/details.aspx?id=49922) och installera hello HPC Pack-klienten.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-224">One option is toodownload hello full [HPC Pack 2012 R2 Update 3 installation](http://www.microsoft.com/download/details.aspx?id=49922) and install hello HPC Pack client.</span></span> <span data-ttu-id="bc7fe-225">Du kan också hämta och installera hello [HPC Pack 2012 R2 uppdatering 3 klientverktyg](https://www.microsoft.com/download/details.aspx?id=49923) och hello lämpligt Visual C++ 2010 redistributable för datorn ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-225">Alternatively, download and install hello [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923) and hello appropriate Visual C++ 2010 redistributable for your computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).</span></span>
5. <span data-ttu-id="bc7fe-226">Vi använder en exempel Excel-arbetsbok med namnet ConvertiblePricing_Complete.xlsb i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-226">In this example, we use a sample Excel workbook named ConvertiblePricing_Complete.xlsb.</span></span> <span data-ttu-id="bc7fe-227">Du kan ladda ned det [här](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-227">You can download it [here](https://www.microsoft.com/en-us/download/details.aspx?id=2939).</span></span>
6. <span data-ttu-id="bc7fe-228">Kopiera arbetsmappen för hello Excel-arbetsboken tooa, till exempel D:\Excel\Run.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-228">Copy hello Excel workbook tooa working folder such as D:\Excel\Run.</span></span>
7. <span data-ttu-id="bc7fe-229">Öppna hello Excel-arbetsbok.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-229">Open hello Excel workbook.</span></span> <span data-ttu-id="bc7fe-230">På hello **utveckla** band klickar du på **COM-tillägg** och bekräfta att hello HPC Pack Excel COM-tillägg har lästs in.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-230">On hello **Develop** ribbon, click **COM Add-Ins** and confirm that hello HPC Pack Excel COM add-in is loaded successfully.</span></span>
   
   ![Excel-tillägg för HPC Pack][addin]
8. <span data-ttu-id="bc7fe-232">Redigera hello VBA makrot HPCControlMacros i Excel genom att ändra hello kommenterade rader som visas i hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-232">Edit hello VBA macro HPCControlMacros in Excel by changing hello commented lines as shown in hello following script.</span></span> <span data-ttu-id="bc7fe-233">Ersätt lämpliga värden för din miljö.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-233">Substitute appropriate values for your environment.</span></span>
   
   ![Excel-makro för HPC Pack][macro]
   
   ```
   'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
   Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
   
   'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
   Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.Initialize ActiveWorkbook
   HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
   
   'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
   HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
   HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
   ```
9. <span data-ttu-id="bc7fe-235">Kopiera hello Excel-arbetsboken tooan överför katalog, t.ex D:\Excel\Upload.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-235">Copy hello Excel workbook tooan upload directory such as D:\Excel\Upload.</span></span> <span data-ttu-id="bc7fe-236">Den här katalogen har angetts i hello HPC_DependsFiles konstant i hello VBA makro.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-236">This directory is specified in hello HPC_DependsFiles constant in hello VBA macro.</span></span>
10. <span data-ttu-id="bc7fe-237">toorun hello arbetsboken på hello klustret i Azure, klicka på hello **klustret** knappen hello kalkylblad.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-237">toorun hello workbook on hello cluster in Azure, click hello **Cluster** button on hello worksheet.</span></span>

### <a name="run-excel-udfs"></a><span data-ttu-id="bc7fe-238">Kör Excel-UDF: er</span><span class="sxs-lookup"><span data-stu-id="bc7fe-238">Run Excel UDFs</span></span>
<span data-ttu-id="bc7fe-239">toorun Excel-UDF: er, följ hello föregående steg 1 – 3 tooset in hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-239">toorun Excel UDFs, follow hello preceding steps 1 – 3 tooset up hello client computer.</span></span> <span data-ttu-id="bc7fe-240">För Excel-UDF: er behöver du inte toohave hello Excel installerat program på compute-noder.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-240">For Excel UDFs, you don't need toohave hello Excel application installed on compute nodes.</span></span> <span data-ttu-id="bc7fe-241">Så när du skapar klustret compute-noder, kan du välja en normal beräkning nod avbildning i stället för hello compute-nod avbildningen med Excel.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-241">So, when creating your cluster compute nodes, you could choose a normal compute node image instead of hello compute node image with Excel.</span></span>

> [!NOTE]
> <span data-ttu-id="bc7fe-242">Det finns en 34 tecken i hello Excel 2010 och 2013 klustret connector dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-242">There is a 34 character limit in hello Excel 2010 and 2013 cluster connector dialog box.</span></span> <span data-ttu-id="bc7fe-243">Du använder den här dialogrutan rutan toospecify hello-kluster som kör hello UDF: er.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-243">You use this dialog box toospecify hello cluster that runs hello UDFs.</span></span> <span data-ttu-id="bc7fe-244">Om hello fullständig klustrets namn är längre (till exempel hpcexcelhn01.southeastasia.cloudapp.azure.com) passar inte hello i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-244">If hello full cluster name is longer (for example, hpcexcelhn01.southeastasia.cloudapp.azure.com), it does not fit in hello dialog box.</span></span> <span data-ttu-id="bc7fe-245">hello lösning är tooset en datoromfattande variabel som *CCP_IAASHN* med hello värdet hello lång klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-245">hello workaround is tooset a machine-wide variable such as *CCP_IAASHN* with hello value of hello long cluster name.</span></span> <span data-ttu-id="bc7fe-246">Ange sedan *CCP_IAASHN %* hello i dialogrutan som hello klustrets huvudnod namn.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-246">Then, enter *%CCP_IAASHN%* in hello dialog box as hello cluster head node name.</span></span> 
> 
> 

<span data-ttu-id="bc7fe-247">När hello klustret har distribuerats, fortsätter du med följande steg toorun hello exempel inbyggda Excel-UDF.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-247">After hello cluster is successfully deployed, continue with hello following steps toorun a sample built-in Excel UDF.</span></span> <span data-ttu-id="bc7fe-248">Anpassade Excel-UDF finns dessa [resurser](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) toobuild hello XLL och distribuera dem på hello IaaS-klustret.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-248">For customized Excel UDFs, see these [resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) toobuild hello XLLs and deploy them on hello IaaS cluster.</span></span>

1. <span data-ttu-id="bc7fe-249">Öppna en ny Excel-arbetsbok.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-249">Open a new Excel workbook.</span></span> <span data-ttu-id="bc7fe-250">På hello **utveckla** band klickar du på **tillägg**. Klicka i dialogrutan hello **Bläddra**, navigera toohello %CCP_HOME%Bin\XLL32 mapp och välj hello exempel ClusterUDF32.xll.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-250">On hello **Develop** ribbon, click **Add-Ins**. Then, in hello dialog box, click **Browse**, navigate toohello %CCP_HOME%Bin\XLL32 folder, and select hello sample ClusterUDF32.xll.</span></span> <span data-ttu-id="bc7fe-251">Om hello ClusterUDF32 inte finns på hello klientdatorn måste du kopiera den från hello %CCP_HOME%Bin\XLL32 mapp på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-251">If hello ClusterUDF32 doesn't exist on hello client machine, copy it from hello %CCP_HOME%Bin\XLL32 folder on hello head node.</span></span>
   
   ![Välj hello UDF][udf]
2. <span data-ttu-id="bc7fe-253">Klicka på **filen** > **alternativ** > **avancerade**.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-253">Click **File** > **Options** > **Advanced**.</span></span> <span data-ttu-id="bc7fe-254">Under **formler**, kontrollera **Tillåt användardefinierade XLL funktioner toorun ett beräkningskluster**.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-254">Under **Formulas**, check **Allow user-defined XLL functions toorun a compute cluster**.</span></span> <span data-ttu-id="bc7fe-255">Klicka på **alternativ** och ange hello fullständig klusternamnet i **klustrets huvudnod namn**.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-255">Then click **Options** and enter hello full cluster name in **Cluster head node name**.</span></span> <span data-ttu-id="bc7fe-256">(Som anges tidigare inkommande rutan är begränsad too34 tecken så långt klusternamnet inte kanske får plats.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-256">(As noted previously this input box is limited too34 characters, so a long cluster name may not fit.</span></span> <span data-ttu-id="bc7fe-257">Du kan använda den datoromfattande här för ett långt klusternamn.)</span><span class="sxs-lookup"><span data-stu-id="bc7fe-257">You may use a machine-wide variable here for a long cluster name.)</span></span>
   
   ![Konfigurera hello UDF][options]
3. <span data-ttu-id="bc7fe-259">toorun hello UDF beräkning på hello kluster, klicka på hello cellen med värdet =XllGetComputerNameC() och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-259">toorun hello UDF calculation on hello cluster, click hello cell with value =XllGetComputerNameC() and press Enter.</span></span> <span data-ttu-id="bc7fe-260">hello funktionen hämtar bara hello namnet på hello compute-nod på vilka hello UDF körs.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-260">hello function simply retrieves hello name of hello compute node on which hello UDF runs.</span></span> <span data-ttu-id="bc7fe-261">För hello först köra, en autentiseringsuppgifter i dialogrutan för hello användarnamn och lösenord tooconnect toohello IaaS-kluster.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-261">For hello first run, a credentials dialog box prompts for hello username and password tooconnect toohello IaaS cluster.</span></span>
   
   ![Kör UDF][run]
   
   <span data-ttu-id="bc7fe-263">När det finns många celler toocalculate, trycker du på Alt-Skift-Ctrl + F9 toorun hello beräkningen på alla celler.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-263">When there are many cells toocalculate, press Alt-Shift-Ctrl + F9 toorun hello calculation on all cells.</span></span>

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a><span data-ttu-id="bc7fe-264">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-264">Step 3.</span></span> <span data-ttu-id="bc7fe-265">Kör en SOA-arbetsbelastning från en lokal klient</span><span class="sxs-lookup"><span data-stu-id="bc7fe-265">Run a SOA workload from an on-premises client</span></span>
<span data-ttu-id="bc7fe-266">toorun Allmänt SOA-program på hello HPC Pack IaaS-kluster med någon av hello metoder i steg 1 toodeploy hello kluster först.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-266">toorun general SOA applications on hello HPC Pack IaaS cluster, first use one of hello methods in Step 1 toodeploy hello cluster.</span></span> <span data-ttu-id="bc7fe-267">Ange en allmän compute-nod avbildning i det här fallet, eftersom du inte behöver Excel på hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-267">Specify a generic compute node image in this case, because you do not need Excel on hello compute nodes.</span></span> <span data-ttu-id="bc7fe-268">Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-268">Then follow these steps.</span></span>

1. <span data-ttu-id="bc7fe-269">När du har hämtat hello klustret certifikat att importera den på hello-klientdatorn under Cert: \CurrentUser\Root.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-269">After retrieving hello cluster certificate, import it on hello client computer under Cert:\CurrentUser\Root.</span></span>
2. <span data-ttu-id="bc7fe-270">Installera hello [HPC Pack 2012 R2 uppdatering 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) och [HPC Pack 2012 R2 uppdatering 3 klientverktyg](https://www.microsoft.com/download/details.aspx?id=49923).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-270">Install hello [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) and [HPC Pack 2012 R2 Update 3 client utilities](https://www.microsoft.com/download/details.aspx?id=49923).</span></span> <span data-ttu-id="bc7fe-271">Dessa verktyg kan du toodevelop och kör SOA-klientprogram.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-271">These tools enable you toodevelop and run SOA client applications.</span></span>
3. <span data-ttu-id="bc7fe-272">Hämta hello HelloWorldR2 [exempelkoden](https://www.microsoft.com/download/details.aspx?id=41633).</span><span class="sxs-lookup"><span data-stu-id="bc7fe-272">Download hello HelloWorldR2 [sample code](https://www.microsoft.com/download/details.aspx?id=41633).</span></span> <span data-ttu-id="bc7fe-273">Öppna hello HelloWorldR2.sln i Visual Studio 2010 eller 2012.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-273">Open hello HelloWorldR2.sln in Visual Studio 2010 or 2012.</span></span> <span data-ttu-id="bc7fe-274">(Det här exemplet är inte kompatibelt med nyare versioner av Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="bc7fe-274">(This sample is not currently compatible with more recent versions of Visual Studio.)</span></span>
4. <span data-ttu-id="bc7fe-275">Skapa hello EchoService projekt först.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-275">Build hello EchoService project first.</span></span> <span data-ttu-id="bc7fe-276">Sedan kan du distribuera hello service toohello IaaS-kluster i hello samma sätt som du distribuerar tooan lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-276">Then, deploy hello service toohello IaaS cluster in hello same way you deploy tooan on-premises cluster.</span></span> <span data-ttu-id="bc7fe-277">Detaljerade anvisningar finns i hello viktigt.doc i HelloWordR2.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-277">For detailed steps, see hello Readme.doc in HelloWordR2.</span></span> <span data-ttu-id="bc7fe-278">Ändra och skapa hello HellWorldR2 och andra projekt som beskrivs i hello följande avsnitt toogenerate hello SOA-klientprogram som körs på ett Azure IaaS-kluster.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-278">Modify and build hello HellWorldR2 and other projects as described in hello following section toogenerate hello SOA client applications that run on an Azure IaaS cluster.</span></span>

### <a name="use-http-binding-with-azure-storage-queue"></a><span data-ttu-id="bc7fe-279">Använd Http-bindning med Azure storage-kö</span><span class="sxs-lookup"><span data-stu-id="bc7fe-279">Use Http binding with Azure storage queue</span></span>
<span data-ttu-id="bc7fe-280">toouse Http-bindning med ett Azure storage-kö gör några ändringar toohello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-280">toouse Http binding with an Azure storage queue, make a few changes toohello sample code.</span></span>

* <span data-ttu-id="bc7fe-281">Uppdatera hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-281">Update hello cluster name.</span></span>
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* <span data-ttu-id="bc7fe-282">Du kan också använda hello standard TransportScheme i SessionStartInfo eller uttryckligen ange tooHttp.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-282">Optionally, use hello default TransportScheme in SessionStartInfo or explicitly set it tooHttp.</span></span>

```
    info.TransportScheme = TransportScheme.Http;
```

* <span data-ttu-id="bc7fe-283">Använd standard bindning för hello BrokerClient.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-283">Use default binding for hello BrokerClient.</span></span>
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    <span data-ttu-id="bc7fe-284">Eller ange uttryckligen med hjälp av basicHttpBinding hello.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-284">Or set explicitly using hello basicHttpBinding.</span></span>
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* <span data-ttu-id="bc7fe-285">Du kan också ange hello UseAzureQueue flaggan tootrue i SessionStartInfo.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-285">Optionally, set hello UseAzureQueue flag tootrue in SessionStartInfo.</span></span> <span data-ttu-id="bc7fe-286">Om inte mängd sätts tootrue som standard när hello klusternamnet har Azure domänsuffix och hello TransportScheme är Http.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-286">If not set, it will be set tootrue by default when hello cluster name has Azure domain suffixes and hello TransportScheme is Http.</span></span>
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a><span data-ttu-id="bc7fe-287">Använd Http-bindning utan Azure storage-kö</span><span class="sxs-lookup"><span data-stu-id="bc7fe-287">Use Http binding without Azure storage queue</span></span>
<span data-ttu-id="bc7fe-288">toouse Http-bindning utan att explicit ange hello UseAzureQueue flaggan toofalse i hello SessionStartInfo en Azure storage-kö.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-288">toouse Http binding without an Azure storage queue, explicitly set hello UseAzureQueue flag toofalse in hello SessionStartInfo.</span></span>

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a><span data-ttu-id="bc7fe-289">Använda NetTcp bindning</span><span class="sxs-lookup"><span data-stu-id="bc7fe-289">Use NetTcp binding</span></span>
<span data-ttu-id="bc7fe-290">toouse NetTcp bindning, hello konfigurationen är liknande tooconnecting tooan lokala kluster.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-290">toouse NetTcp binding, hello configuration is similar tooconnecting tooan on-premises cluster.</span></span> <span data-ttu-id="bc7fe-291">Du måste tooopen några slutpunkter i hello huvudnod VM.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-291">You need tooopen a few endpoints on hello head node VM.</span></span> <span data-ttu-id="bc7fe-292">Om du använde hello HPC Pack IaaS distribution skriptet toocreate hello klustret, till exempel hello set hello slutpunkter i Azure-portalen på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-292">If you used hello HPC Pack IaaS deployment script toocreate hello cluster, for example, set hello endpoints in hello Azure portal as follows.</span></span>

1. <span data-ttu-id="bc7fe-293">Stoppa hello VM.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-293">Stop hello VM.</span></span>
2. <span data-ttu-id="bc7fe-294">Lägg till hello TCP-portar 9090, 9087, 9091, 9094 för hello Session Broker, respektive Broker worker och Data services</span><span class="sxs-lookup"><span data-stu-id="bc7fe-294">Add hello TCP ports 9090, 9087, 9091, 9094 for hello Session, Broker, Broker worker, and Data services, respectively</span></span>
   
    ![Konfigurera slutpunkter][endpoint-new-portal]
3. <span data-ttu-id="bc7fe-296">Starta hello VM.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-296">Start hello VM.</span></span>

<span data-ttu-id="bc7fe-297">hello SOA-klientprogrammet kräver inga ändringar förutom ändra hello head toohello IaaS klustret fullständiga namn.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-297">hello SOA client application requires no changes except altering hello head name toohello IaaS cluster full name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc7fe-298">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc7fe-298">Next steps</span></span>
* <span data-ttu-id="bc7fe-299">Se [resurserna](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) för mer information om hur du kör Excel arbetsbelastningar med HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-299">See [these resources](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) for more information about running Excel workloads with HPC Pack.</span></span>
* <span data-ttu-id="bc7fe-300">Se [hantera SOA-tjänster i Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) mer information om att distribuera och hantera SOA-tjänster med HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="bc7fe-300">See [Managing SOA Services in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) for more about deploying and managing SOA services with HPC Pack.</span></span>

<!--Image references-->
[scenario]: ./media/excel-cluster-hpcpack/scenario.png
[github]: ./media/excel-cluster-hpcpack/github.png
[template]: ./media/excel-cluster-hpcpack/template.png
[parameters]: ./media/excel-cluster-hpcpack/parameters.png
[parameters-new-portal]: ./media/excel-cluster-hpcpack/parameters-new-portal.png
[create]: ./media/excel-cluster-hpcpack/create.png
[connect]: ./media/excel-cluster-hpcpack/connect.png
[cert]: ./media/excel-cluster-hpcpack/cert.png
[addin]: ./media/excel-cluster-hpcpack/addin.png
[macro]: ./media/excel-cluster-hpcpack/macro.png
[options]: ./media/excel-cluster-hpcpack/options.png
[run]: ./media/excel-cluster-hpcpack/run.png
[endpoint]: ./media/excel-cluster-hpcpack/endpoint.png
[endpoint-new-portal]: ./media/excel-cluster-hpcpack/endpoint-new-portal.png
[udf]: ./media/excel-cluster-hpcpack/udf.png
