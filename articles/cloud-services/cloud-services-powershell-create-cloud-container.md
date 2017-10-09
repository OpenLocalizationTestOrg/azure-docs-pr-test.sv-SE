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
# <a name="use-an-azure-powershell-command-toocreate-an-empty-cloud-service-container"></a>Använda en Azure PowerShell-kommandot toocreate en tom cloud service-behållare
Den här artikeln förklarar hur tooquickly skapa en behållare för molntjänster med hjälp av Azure PowerShell-cmdlets. Följ hello stegen nedan:

1. Installera Microsoft Azure PowerShell-cmdlet för hello från hello [Azure PowerShell laddar ned](http://aka.ms/webpi-azps) sidan.
2. Öppna hello PowerShell-Kommandotolken.
3. Använd hello [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) toosign i.

   > [!NOTE]
   > För ytterligare information om att installera hello Azure PowerShell-cmdlet och ansluter tooyour Azure-prenumeration finns för[hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
   >
   >
4. Använd hello **New-AzureService** cmdlet toocreate en tom Azure cloud service-behållare.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Följ den här exempel tooinvoke hello-cmdlet:

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Mer information om hur du skapar hello Azure-molntjänst kör du:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Nästa steg
* toomanage hello cloud service-distributionen finns toohello [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), och [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) kommandon. Du kan också läsa för[hur tooconfigure molntjänster](cloud-services-how-to-configure.md) för ytterligare information.
* toopublish Molntjänsten projektet tooAzure finns toohello **PublishCloudService.ps1** kodexempel från [kontinuerlig leverans för Molntjänsten i Azure](cloud-services-dotnet-continuous-delivery.md).
