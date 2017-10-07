---
title: "ett Azure hanterade disken för tillbaka in aaaCopy | Microsoft Docs"
description: "Lär dig hur toocreate en kopia av en Azure-hanterade disken toouse för tillbaka dig eller felsökning disken utfärdar."
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/6/2017
ms.author: rasquill
ms.openlocfilehash: 41b91c2d68eb5be9c493a66be5f7d085a70450d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="01d19-103">Skapa en kopia av en virtuell Hårddisk lagras som en hanterad Azure-disken med hjälp av hanterade ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="01d19-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="01d19-104">Ta en ögonblicksbild av en hanterad disk för säkerhetskopiering eller skapa en Disk som hanteras av hello ögonblicksbild och koppla den tooa test virtuella tootroubleshoot.</span><span class="sxs-lookup"><span data-stu-id="01d19-104">Take a snapshot of a Managed disk for backup or create a Managed Disk from hello snapshot and attach it tooa test virtual machine tootroubleshoot.</span></span> <span data-ttu-id="01d19-105">En ögonblicksbild av en hanterad är en fullständig i tidpunkt kopia av en virtuell dator hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="01d19-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="01d19-106">Den skapar en skrivskyddad kopia av den virtuella Hårddisken och som standard lagras som en Standard hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="01d19-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> 

<span data-ttu-id="01d19-107">Information om priser finns i [priser för Azure Storage](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="01d19-107">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> <!--Add link tootopic or blog post that explains managed disks. -->

<span data-ttu-id="01d19-108">Använd antingen hello Azure-portalen eller hello Azure CLI 2.0 tootake en ögonblicksbild av hello hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="01d19-108">Use either hello Azure portal or hello Azure CLI 2.0 tootake a snapshot of hello Managed Disk.</span></span>

## <a name="use-azure-cli-20-tootake-a-snapshot"></a><span data-ttu-id="01d19-109">Använda Azure CLI 2.0 tootake en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="01d19-109">Use Azure CLI 2.0 tootake a snapshot</span></span>

> [!NOTE] 
> <span data-ttu-id="01d19-110">hello kräver följande exempel hello Azure CLI 2.0 installerat och loggat in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="01d19-110">hello following example requires hello Azure CLI 2.0 installed and logged into your Azure account.</span></span>

<span data-ttu-id="01d19-111">hello följande steg visar hur tooobtain och ta en ögonblicksbild av en hanterad OS-disken med hello `az snapshot create` med hello `--source-disk` parameter.</span><span class="sxs-lookup"><span data-stu-id="01d19-111">hello following steps show how tooobtain and take a snapshot of a managed OS disk using hello `az snapshot create` command with hello `--source-disk` parameter.</span></span> <span data-ttu-id="01d19-112">hello följande exempel förutsätter att det finns en virtuell dator kallas `myVM` skapats med en hanterad OS-disk i hello `myResourceGroup` resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="01d19-112">hello following example assumes that there is a VM called `myVM` created with a managed OS disk in hello `myResourceGroup` resource group.</span></span>

```azure-cli
# take hello disk id with which toocreate a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

<span data-ttu-id="01d19-113">hello utdata ska se ut ungefär:</span><span class="sxs-lookup"><span data-stu-id="01d19-113">hello output should look something like:</span></span>

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-tootake-a-snapshot"></a><span data-ttu-id="01d19-114">Använda Azure portal tootake en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="01d19-114">Use Azure portal tootake a snapshot</span></span> 

1. <span data-ttu-id="01d19-115">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01d19-115">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01d19-116">Från och med hello längst upp till vänster, klicka på **ny** och Sök efter **ögonblicksbild**.</span><span class="sxs-lookup"><span data-stu-id="01d19-116">Starting in hello upper-left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="01d19-117">I hello Snapshot-bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="01d19-117">In hello Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="01d19-118">Ange en **namn** för hello ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="01d19-118">Enter a **Name** for hello snapshot.</span></span>
5. <span data-ttu-id="01d19-119">Välj en befintlig [resursgruppen](../../azure-resource-manager/resource-group-overview.md#resource-groups) eller hello-typnamn för en ny.</span><span class="sxs-lookup"><span data-stu-id="01d19-119">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span> 
6. <span data-ttu-id="01d19-120">Välj ett Azure-datacenter plats.</span><span class="sxs-lookup"><span data-stu-id="01d19-120">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="01d19-121">För **källdisken**, Välj hello hanteras Disk toosnapshot.</span><span class="sxs-lookup"><span data-stu-id="01d19-121">For **Source disk**, select hello Managed Disk toosnapshot.</span></span>
8. <span data-ttu-id="01d19-122">Välj hello **kontotyp** toouse toostore hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="01d19-122">Select hello **Account type** toouse toostore hello snapshot.</span></span> <span data-ttu-id="01d19-123">Vi rekommenderar **Standard_LRS** om du inte behöver den lagras på en disk med hög prestanda.</span><span class="sxs-lookup"><span data-stu-id="01d19-123">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="01d19-124">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="01d19-124">Click **Create**.</span></span>

<span data-ttu-id="01d19-125">Om du planerar toouse hello ögonblicksbild toocreate en Disk som hanteras och koppla den till en virtuell dator som behöver toobe högpresterande, använda hello parametern `--sku Premium_LRS` med hello `az snapshot create` kommando.</span><span class="sxs-lookup"><span data-stu-id="01d19-125">If you plan toouse hello snapshot toocreate a Managed Disk and attach it a VM that needs toobe high performing, use hello parameter `--sku Premium_LRS` with hello `az snapshot create` command.</span></span> <span data-ttu-id="01d19-126">Detta skapar hello ögonblicksbild så att den lagras som en Premium hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="01d19-126">This creates hello snapshot so that it is stored as a Premium Managed Disk.</span></span> <span data-ttu-id="01d19-127">Hanterade Premiumdiskar bättre eftersom de SSD-enheter (SSD), men kostar mer än standarddiskar (HDD).</span><span class="sxs-lookup"><span data-stu-id="01d19-127">Premium Managed Disks perform better because they are solid-state drives (SSDs), but cost more than Standard disks (HDDs).</span></span>


