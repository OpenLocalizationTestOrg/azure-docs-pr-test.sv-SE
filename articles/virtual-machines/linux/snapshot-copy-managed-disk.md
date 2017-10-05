---
title: "Kopiera en Azure-hanterade disken för tillbaka in | Microsoft Docs"
description: "Lär dig hur du skapar en kopia av en Azure hanteras Disk som ska använda för tillbaka eller felsökning av problem med disken."
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
ms.openlocfilehash: c91367ef11c9d531bebac7c069d2df586607ec29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="73c43-103">Skapa en kopia av en virtuell Hårddisk lagras som en hanterad Azure-disken med hjälp av hanterade ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="73c43-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="73c43-104">Ta en ögonblicksbild av en hanterad disk för säkerhetskopiering eller skapa en Disk som hanteras av ögonblicksbilden och koppla den till en virtuell testdator för att felsöka.</span><span class="sxs-lookup"><span data-stu-id="73c43-104">Take a snapshot of a Managed disk for backup or create a Managed Disk from the snapshot and attach it to a test virtual machine to troubleshoot.</span></span> <span data-ttu-id="73c43-105">En ögonblicksbild av en hanterad är en fullständig i tidpunkt kopia av en virtuell dator hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="73c43-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="73c43-106">Den skapar en skrivskyddad kopia av den virtuella Hårddisken och som standard lagras som en Standard hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="73c43-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> 

<span data-ttu-id="73c43-107">Information om priser finns i [priser för Azure Storage](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="73c43-107">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> <!--Add link to topic or blog post that explains managed disks. -->

<span data-ttu-id="73c43-108">Använda Azure-portalen eller Azure CLI 2.0 för att ta en ögonblicksbild av den hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="73c43-108">Use either the Azure portal or the Azure CLI 2.0 to take a snapshot of the Managed Disk.</span></span>

## <a name="use-azure-cli-20-to-take-a-snapshot"></a><span data-ttu-id="73c43-109">Använda Azure CLI 2.0 för att ta en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="73c43-109">Use Azure CLI 2.0 to take a snapshot</span></span>

> [!NOTE] 
> <span data-ttu-id="73c43-110">I följande exempel kräver Azure CLI 2.0 installerat och loggat in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="73c43-110">The following example requires the Azure CLI 2.0 installed and logged into your Azure account.</span></span>

<span data-ttu-id="73c43-111">Följande steg visar hur du hämtar och ta en ögonblicksbild av en hanterad OS disk med den `az snapshot create` kommandot med de `--source-disk` parameter.</span><span class="sxs-lookup"><span data-stu-id="73c43-111">The following steps show how to obtain and take a snapshot of a managed OS disk using the `az snapshot create` command with the `--source-disk` parameter.</span></span> <span data-ttu-id="73c43-112">Följande exempel förutsätter att det finns en virtuell dator kallas `myVM` skapats med en hanterad OS-disk i den `myResourceGroup` resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="73c43-112">The following example assumes that there is a VM called `myVM` created with a managed OS disk in the `myResourceGroup` resource group.</span></span>

```azure-cli
# take the disk id with which to create a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

<span data-ttu-id="73c43-113">Utdata ska se ut ungefär:</span><span class="sxs-lookup"><span data-stu-id="73c43-113">The output should look something like:</span></span>

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

## <a name="use-azure-portal-to-take-a-snapshot"></a><span data-ttu-id="73c43-114">Använd Azure-portalen för att ta en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="73c43-114">Use Azure portal to take a snapshot</span></span> 

1. <span data-ttu-id="73c43-115">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="73c43-115">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="73c43-116">Klicka på Start i det övre vänstra **ny** och Sök efter **ögonblicksbild**.</span><span class="sxs-lookup"><span data-stu-id="73c43-116">Starting in the upper-left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="73c43-117">I bladet ögonblicksbild klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="73c43-117">In the Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="73c43-118">Ange en **namn** för ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="73c43-118">Enter a **Name** for the snapshot.</span></span>
5. <span data-ttu-id="73c43-119">Välj en befintlig [resursgrupp](../../azure-resource-manager/resource-group-overview.md#resource-groups) eller skriv namnet på en ny.</span><span class="sxs-lookup"><span data-stu-id="73c43-119">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type the name for a new one.</span></span> 
6. <span data-ttu-id="73c43-120">Välj ett Azure-datacenter plats.</span><span class="sxs-lookup"><span data-stu-id="73c43-120">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="73c43-121">För **källdisken**, Välj den hanterade disken till ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="73c43-121">For **Source disk**, select the Managed Disk to snapshot.</span></span>
8. <span data-ttu-id="73c43-122">Välj den **kontotyp** du vill lagra ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="73c43-122">Select the **Account type** to use to store the snapshot.</span></span> <span data-ttu-id="73c43-123">Vi rekommenderar **Standard_LRS** om du inte behöver den lagras på en disk med hög prestanda.</span><span class="sxs-lookup"><span data-stu-id="73c43-123">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="73c43-124">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="73c43-124">Click **Create**.</span></span>

<span data-ttu-id="73c43-125">Om du planerar att använda ögonblicksbilden för att skapa en hanterad Disk och koppla den till en virtuell dator som måste vara höga prestanda, använder du parametern `--sku Premium_LRS` med den `az snapshot create` kommando.</span><span class="sxs-lookup"><span data-stu-id="73c43-125">If you plan to use the snapshot to create a Managed Disk and attach it a VM that needs to be high performing, use the parameter `--sku Premium_LRS` with the `az snapshot create` command.</span></span> <span data-ttu-id="73c43-126">Detta skapar ögonblicksbilden så att den lagras som en Premium hanteras Disk.</span><span class="sxs-lookup"><span data-stu-id="73c43-126">This creates the snapshot so that it is stored as a Premium Managed Disk.</span></span> <span data-ttu-id="73c43-127">Hanterade Premiumdiskar bättre eftersom de SSD-enheter (SSD), men kostar mer än standarddiskar (HDD).</span><span class="sxs-lookup"><span data-stu-id="73c43-127">Premium Managed Disks perform better because they are solid-state drives (SSDs), but cost more than Standard disks (HDDs).</span></span>


