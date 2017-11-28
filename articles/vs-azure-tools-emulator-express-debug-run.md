---
title: "aaaUsing emulatorn Express toorun och felsöka en Azure cloud service på en lokal dator | Microsoft Docs"
description: "Med hjälp av emulatorn Express toorun och felsöka en molnbaserad tjänst på en lokal dator"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: d89a0fc2dce42b4a2d2f93f9c4ff0482af924ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-emulator-express-toorun-and-debug-an-azure-cloud-service-on-a-local-machine"></a><span data-ttu-id="95945-103">Med emulatorn Express cloud toorun och felsöka en Azure service på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="95945-103">Using Emulator Express toorun and debug an Azure cloud service on a local machine</span></span>
<span data-ttu-id="95945-104">Du kan testa och felsöka en molnbaserad tjänst utan att köra Visual Studio som administratör genom att använda emulatorn Express.</span><span class="sxs-lookup"><span data-stu-id="95945-104">By using Emulator Express, you can test and debug a cloud service without running Visual Studio as an administrator.</span></span> <span data-ttu-id="95945-105">Du kan ange ditt projekt inställningar toouse antingen emulatorn Express eller hello fullständiga emulatorn, beroende på hello kraven för din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="95945-105">You can set your project settings toouse either Emulator Express or hello full emulator, depending on hello requirements of your cloud service.</span></span> <span data-ttu-id="95945-106">Läs mer om hello fullständiga emulatorn [kör ett Azure-program i hello Compute Emulator](storage/common/storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="95945-106">For more information about hello full emulator, see [Run an Azure Application in hello Compute Emulator](storage/common/storage-use-emulator.md).</span></span>

## <a name="using-emulator-express-in-visual-studio"></a><span data-ttu-id="95945-107">Med hjälp av emulatorn i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95945-107">Using Emulator Express in Visual Studio</span></span>
<span data-ttu-id="95945-108">När du skapar ett Azure-projekt i Azure SDK 2.3 eller senare används automatiskt emulatorn Express.</span><span class="sxs-lookup"><span data-stu-id="95945-108">When you create an Azure project in Azure SDK 2.3 or later, Emulator Express is automatically used.</span></span> <span data-ttu-id="95945-109">För befintliga projekt som har skapats med en tidigare version av hello Azure SDK använder du följande steg tooselect emulatorn Express hello:</span><span class="sxs-lookup"><span data-stu-id="95945-109">For existing projects that were created with an earlier version of hello Azure SDK, use hello following steps tooselect Emulator Express:</span></span>

1. <span data-ttu-id="95945-110">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="95945-110">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="95945-111">I **Solution Explorer**, högerklicka på hello-projektet och, hello snabbmenyn väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="95945-111">In **Solution Explorer**, right-click hello project, and, from hello context menu, select **Properties**.</span></span>

1. <span data-ttu-id="95945-112">Välj hello i hello projekt egenskapssidor **Web** fliken.</span><span class="sxs-lookup"><span data-stu-id="95945-112">In hello projects properties pages, select hello **Web** tab.</span></span>

    ![Egenskaper för ett Azure cloud service-projekt](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. <span data-ttu-id="95945-114">Under **lokala utvecklingsserver**väljer **används IIS Express alternativet**.</span><span class="sxs-lookup"><span data-stu-id="95945-114">Under **Local Development Server**, select **Use IIS Express option**.</span></span>

1. <span data-ttu-id="95945-115">Under **emulatorn**väljer **Använd emulatorn Express**.</span><span class="sxs-lookup"><span data-stu-id="95945-115">Under **Emulator**, select **Use Emulator Express**.</span></span>
   
1. <span data-ttu-id="95945-116">toolaunch hello Express-emulatorn kör hello följande kommando i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="95945-116">toolaunch hello Emulator Express, run hello following command at a command prompt:</span></span> 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a><span data-ttu-id="95945-117">Emulatorn Express begränsningar</span><span class="sxs-lookup"><span data-stu-id="95945-117">Emulator Express limitations</span></span>
<span data-ttu-id="95945-118">hello följande problem är kända begränsningar i emulatorn Express:</span><span class="sxs-lookup"><span data-stu-id="95945-118">hello following issues are known limitations of Emulator Express:</span></span> 

- <span data-ttu-id="95945-119">Emulatorn Express är inte kompatibel med IIS-webbserver.</span><span class="sxs-lookup"><span data-stu-id="95945-119">Emulator Express is not compatible with IIS Web Server.</span></span>
- <span data-ttu-id="95945-120">Din molntjänst kan innehålla flera roller, men varje roll är begränsad tooone instans.</span><span class="sxs-lookup"><span data-stu-id="95945-120">Your cloud service can contain multiple roles, but each role is limited tooone instance.</span></span>
- <span data-ttu-id="95945-121">Du kan inte komma åt portnummer nedan 1000.</span><span class="sxs-lookup"><span data-stu-id="95945-121">You can't access port numbers below 1000.</span></span> <span data-ttu-id="95945-122">Om du använder en autentiseringsprovider som normalt använder en port under 1000 kan behöva du toochange det här värdet tooa-portnummer som är högre än 1000.</span><span class="sxs-lookup"><span data-stu-id="95945-122">If you use an authentication provider that normally uses a port below 1000, you might need toochange this value tooa port number that's above 1000.</span></span>
- <span data-ttu-id="95945-123">Eventuella begränsningar som gäller toohello Azure Compute Emulator gäller även tooEmulator Express.</span><span class="sxs-lookup"><span data-stu-id="95945-123">Any limitations that apply toohello Azure Compute Emulator also apply tooEmulator Express.</span></span> <span data-ttu-id="95945-124">Du kan till exempel inte fler än 50 rollinstanser per distribution.</span><span class="sxs-lookup"><span data-stu-id="95945-124">For example, you can't have more than 50 role instances per deployment.</span></span> <span data-ttu-id="95945-125">Läs mer om hello Azure Compute Emulator [kör ett Azure-program i hello Compute Emulator](http://go.microsoft.com/fwlink/p/?LinkId=623050).</span><span class="sxs-lookup"><span data-stu-id="95945-125">For more information about hello Azure Compute Emulator, see [Run an Azure Application in hello Compute Emulator](http://go.microsoft.com/fwlink/p/?LinkId=623050).</span></span>

## <a name="next-steps"></a><span data-ttu-id="95945-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95945-126">Next steps</span></span>
[<span data-ttu-id="95945-127">Felsöka Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="95945-127">Debugging Azure cloud services</span></span>](https://msdn.microsoft.com/library/azure/ee405479.aspx)
