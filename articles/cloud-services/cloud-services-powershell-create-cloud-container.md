---
title: "Skapa en cloud service-behållare med PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du skapar en cloud service-behållare med PowerShell. Behållaren är värd för webb-och arbetsroller."
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 2023fa7b318f9f76ce1e1ea0a46110297be9a001
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a><span data-ttu-id="0023a-104">Använd en Azure PowerShell-kommando för att skapa en tom cloud service-behållare</span><span class="sxs-lookup"><span data-stu-id="0023a-104">Use an Azure PowerShell command to create an empty cloud service container</span></span>
<span data-ttu-id="0023a-105">Den här artikeln beskriver hur du snabbt skapa en behållare för molntjänster med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0023a-105">This article explains how to quickly create a Cloud Services container using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="0023a-106">Följ stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="0023a-106">Please follow the steps below:</span></span>

1. <span data-ttu-id="0023a-107">Installera Microsoft Azure PowerShell-cmdlet från den [Azure PowerShell laddar ned](http://aka.ms/webpi-azps) sidan.</span><span class="sxs-lookup"><span data-stu-id="0023a-107">Install the Microsoft Azure PowerShell cmdlet from the [Azure PowerShell downloads](http://aka.ms/webpi-azps) page.</span></span>
2. <span data-ttu-id="0023a-108">Öppna PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="0023a-108">Open the PowerShell command prompt.</span></span>
3. <span data-ttu-id="0023a-109">Använd den [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) att logga in.</span><span class="sxs-lookup"><span data-stu-id="0023a-109">Use the [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) to sign in.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0023a-110">För mer information om att installera Azure PowerShell-cmdlet och ansluter till din Azure-prenumeration, referera till [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0023a-110">For further instruction on installing the Azure PowerShell cmdlet and connecting to your Azure subscription, refer to [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
   >
   >
4. <span data-ttu-id="0023a-111">Använd den **New-AzureService** för att skapa en tom Azure cloud service-behållare.</span><span class="sxs-lookup"><span data-stu-id="0023a-111">Use the **New-AzureService** cmdlet to create an empty Azure cloud service container.</span></span>

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. <span data-ttu-id="0023a-112">Så här gör att anropa cmdlet:</span><span class="sxs-lookup"><span data-stu-id="0023a-112">Follow this example to invoke the cmdlet:</span></span>

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

<span data-ttu-id="0023a-113">Mer information om hur du skapar Azure-Molntjänsten, kör du:</span><span class="sxs-lookup"><span data-stu-id="0023a-113">For more information about creating the Azure cloud service, run:</span></span>

```
Get-help New-AzureService
```

### <a name="next-steps"></a><span data-ttu-id="0023a-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0023a-114">Next steps</span></span>
* <span data-ttu-id="0023a-115">För att hantera molndistribution för tjänsten, referera till den [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), och [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) kommandon.</span><span class="sxs-lookup"><span data-stu-id="0023a-115">To manage the cloud service deployment, refer to the [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), and [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) commands.</span></span> <span data-ttu-id="0023a-116">Du kan också läsa [hur du konfigurerar molntjänster](cloud-services-how-to-configure.md) för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="0023a-116">You may also refer to [How to configure cloud services](cloud-services-how-to-configure.md) for further information.</span></span>
* <span data-ttu-id="0023a-117">Om du vill publicera ditt molntjänstprojekt till Azure, referera till den **PublishCloudService.ps1** kodexempel från [kontinuerlig leverans för Molntjänsten i Azure](cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="0023a-117">To publish your cloud service project to Azure, refer to the  **PublishCloudService.ps1** code sample from [Continuous delivery for cloud service in Azure](cloud-services-dotnet-continuous-delivery.md).</span></span>
