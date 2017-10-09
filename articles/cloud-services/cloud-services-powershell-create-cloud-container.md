---
title: "aaaCreate en cloud service-behållare med PowerShell | Microsoft Docs"
description: "Den här artikeln förklarar hur toocreate ett moln tjänsten behållare med PowerShell. hello-behållaren är värd för webb-och arbetsroller."
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
ms.openlocfilehash: 4c47b10b5ba1f9cc39594a45cd869bf04fcaadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-an-azure-powershell-command-toocreate-an-empty-cloud-service-container"></a><span data-ttu-id="74d6f-104">Använda en Azure PowerShell-kommandot toocreate en tom cloud service-behållare</span><span class="sxs-lookup"><span data-stu-id="74d6f-104">Use an Azure PowerShell command toocreate an empty cloud service container</span></span>
<span data-ttu-id="74d6f-105">Den här artikeln förklarar hur tooquickly skapa en behållare för molntjänster med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="74d6f-105">This article explains how tooquickly create a Cloud Services container using Azure PowerShell cmdlets.</span></span> <span data-ttu-id="74d6f-106">Följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="74d6f-106">Please follow hello steps below:</span></span>

1. <span data-ttu-id="74d6f-107">Installera Microsoft Azure PowerShell-cmdlet för hello från hello [Azure PowerShell laddar ned](http://aka.ms/webpi-azps) sidan.</span><span class="sxs-lookup"><span data-stu-id="74d6f-107">Install hello Microsoft Azure PowerShell cmdlet from hello [Azure PowerShell downloads](http://aka.ms/webpi-azps) page.</span></span>
2. <span data-ttu-id="74d6f-108">Öppna hello PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="74d6f-108">Open hello PowerShell command prompt.</span></span>
3. <span data-ttu-id="74d6f-109">Använd hello [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) toosign i.</span><span class="sxs-lookup"><span data-stu-id="74d6f-109">Use hello [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) toosign in.</span></span>

   > [!NOTE]
   > <span data-ttu-id="74d6f-110">För ytterligare information om att installera hello Azure PowerShell-cmdlet och ansluter tooyour Azure-prenumeration finns för[hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="74d6f-110">For further instruction on installing hello Azure PowerShell cmdlet and connecting tooyour Azure subscription, refer too[How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
   >
   >
4. <span data-ttu-id="74d6f-111">Använd hello **New-AzureService** cmdlet toocreate en tom Azure cloud service-behållare.</span><span class="sxs-lookup"><span data-stu-id="74d6f-111">Use hello **New-AzureService** cmdlet toocreate an empty Azure cloud service container.</span></span>

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. <span data-ttu-id="74d6f-112">Följ den här exempel tooinvoke hello-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="74d6f-112">Follow this example tooinvoke hello cmdlet:</span></span>

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

<span data-ttu-id="74d6f-113">Mer information om hur du skapar hello Azure-molntjänst kör du:</span><span class="sxs-lookup"><span data-stu-id="74d6f-113">For more information about creating hello Azure cloud service, run:</span></span>

```
Get-help New-AzureService
```

### <a name="next-steps"></a><span data-ttu-id="74d6f-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74d6f-114">Next steps</span></span>
* <span data-ttu-id="74d6f-115">toomanage hello cloud service-distributionen finns toohello [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), och [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) kommandon.</span><span class="sxs-lookup"><span data-stu-id="74d6f-115">toomanage hello cloud service deployment, refer toohello [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), and [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) commands.</span></span> <span data-ttu-id="74d6f-116">Du kan också läsa för[hur tooconfigure molntjänster](cloud-services-how-to-configure.md) för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="74d6f-116">You may also refer too[How tooconfigure cloud services](cloud-services-how-to-configure.md) for further information.</span></span>
* <span data-ttu-id="74d6f-117">toopublish Molntjänsten projektet tooAzure finns toohello **PublishCloudService.ps1** kodexempel från [kontinuerlig leverans för Molntjänsten i Azure](cloud-services-dotnet-continuous-delivery.md).</span><span class="sxs-lookup"><span data-stu-id="74d6f-117">toopublish your cloud service project tooAzure, refer toohello  **PublishCloudService.ps1** code sample from [Continuous delivery for cloud service in Azure](cloud-services-dotnet-continuous-delivery.md).</span></span>
