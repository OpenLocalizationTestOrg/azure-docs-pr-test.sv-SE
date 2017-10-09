---
title: "aaaDownload hello mall för en virtuell dator i Azure | Microsoft Docs"
description: "Hämta hello templatefor en VM toohelp automatisera distributioner i hello Resource Manager-distributionsmodellen"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 86fd05f67409019b5e5c9023881745047860eee1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="download-hello-template-for-a-vm"></a><span data-ttu-id="f38dd-103">Hämta hello mall för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f38dd-103">Download hello template for a VM</span></span>
<span data-ttu-id="f38dd-104">När du skapar en virtuell dator i Azure skapas med hello portal eller PowerShell, en resurshanterare mall automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="f38dd-104">When you create a VM in Azure using hello portal or PowerShell, a Resource Manager template is automatically created for you.</span></span> <span data-ttu-id="f38dd-105">Du kan använda den här mallen tooquickly dubblett en distribution.</span><span class="sxs-lookup"><span data-stu-id="f38dd-105">You can use this template tooquickly duplicate a deployment.</span></span> <span data-ttu-id="f38dd-106">hello mallen innehåller information om alla hello resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f38dd-106">hello template contains information about all of hello resources in a resource group.</span></span> <span data-ttu-id="f38dd-107">För en virtuell dator, innebär detta att hello mallen innehåller allt som har skapats för att stödja hello VM i den resursgrupp, inklusive hello nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="f38dd-107">For a virtual machine, this means hello template contains everything that is created in support of hello VM in that resource group, including hello networking resources.</span></span>

## <a name="download-hello-template-using-hello-portal"></a><span data-ttu-id="f38dd-108">Hämta hello mallen med hjälp av hello portal</span><span class="sxs-lookup"><span data-stu-id="f38dd-108">Download hello template using hello portal</span></span>
1. <span data-ttu-id="f38dd-109">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f38dd-109">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="f38dd-110">En hello navmenyn väljer **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="f38dd-110">One hello hub menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="f38dd-111">Välj hello virtuella hello-listan.</span><span class="sxs-lookup"><span data-stu-id="f38dd-111">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="f38dd-112">Välj **automatiseringsskriptet**.</span><span class="sxs-lookup"><span data-stu-id="f38dd-112">Select **Automation script**.</span></span>
5. <span data-ttu-id="f38dd-113">Välj **hämta** och spara hello ZIP-filen tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="f38dd-113">Select **Download** and save hello .zip file tooyour local computer.</span></span>
6. <span data-ttu-id="f38dd-114">Öppna hello ZIP-filen och extrahera hello tooa-mappen.</span><span class="sxs-lookup"><span data-stu-id="f38dd-114">Open hello .zip file and extract hello files tooa folder.</span></span> <span data-ttu-id="f38dd-115">hello ZIP-filen innehåller:</span><span class="sxs-lookup"><span data-stu-id="f38dd-115">hello .zip file will contain:</span></span>
   
   * <span data-ttu-id="f38dd-116">Deploy.ps1</span><span class="sxs-lookup"><span data-stu-id="f38dd-116">deploy.ps1</span></span>
   * <span data-ttu-id="f38dd-117">Deploy.SH</span><span class="sxs-lookup"><span data-stu-id="f38dd-117">deploy.sh</span></span> 
   * <span data-ttu-id="f38dd-118">deployer.RB</span><span class="sxs-lookup"><span data-stu-id="f38dd-118">deployer.rb</span></span>
   * <span data-ttu-id="f38dd-119">DeploymentHelper.cs</span><span class="sxs-lookup"><span data-stu-id="f38dd-119">DeploymentHelper.cs</span></span>
   * <span data-ttu-id="f38dd-120">parameters.JSON</span><span class="sxs-lookup"><span data-stu-id="f38dd-120">parameters.json</span></span>
   * <span data-ttu-id="f38dd-121">Template.JSON</span><span class="sxs-lookup"><span data-stu-id="f38dd-121">template.json</span></span>

<span data-ttu-id="f38dd-122">Hej template.json filen är hello mallen.</span><span class="sxs-lookup"><span data-stu-id="f38dd-122">hello template.json file is hello template.</span></span>

## <a name="download-hello-template-using-powershell"></a><span data-ttu-id="f38dd-123">Hämta hello mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="f38dd-123">Download hello template using PowerShell</span></span>
<span data-ttu-id="f38dd-124">Du kan också hämta hello JSON mallfil med hjälp av hello [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f38dd-124">You can also download hello .json template file using hello [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) cmdlet.</span></span> <span data-ttu-id="f38dd-125">Du kan använda hello `-path` parametern tooprovide hello filnamnet och sökvägen för hello JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="f38dd-125">You can use hello `-path` parameter tooprovide hello filename and path for hello .json file.</span></span> <span data-ttu-id="f38dd-126">Det här exemplet illustrerar hur toodownload hello mallen för resursgruppen hello namnet **myResourceGroup** toohello **C:\users\public\downloads** mapp på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="f38dd-126">This example shows how toodownload hello template for hello resource group named **myResourceGroup** toohello **C:\users\public\downloads** folder on your local computer.</span></span>

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a><span data-ttu-id="f38dd-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f38dd-127">Next steps</span></span>
<span data-ttu-id="f38dd-128">toolearn mer information om hur du distribuerar resurser med hjälp av mallar, se [genomgång av Resource Manager-mall](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="f38dd-128">toolearn more about deploying resources using templates, see [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

