---
title: "Med hjälp av Express-emulatorn att köra och felsöka en Azure-molntjänst på en lokal dator | Microsoft Docs"
description: "Med hjälp av Express-emulatorn att köra och felsöka en molnbaserad tjänst på en lokal dator"
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
ms.openlocfilehash: d7d87045569fdc473333deae05f95d1df343b68c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="using-emulator-express-to-run-and-debug-an-azure-cloud-service-on-a-local-machine"></a><span data-ttu-id="04ae8-103">Med hjälp av Express-emulatorn att köra och felsöka en Azure-molntjänst på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="04ae8-103">Using Emulator Express to run and debug an Azure cloud service on a local machine</span></span>
<span data-ttu-id="04ae8-104">Du kan testa och felsöka en molnbaserad tjänst utan att köra Visual Studio som administratör genom att använda emulatorn Express.</span><span class="sxs-lookup"><span data-stu-id="04ae8-104">By using Emulator Express, you can test and debug a cloud service without running Visual Studio as an administrator.</span></span> <span data-ttu-id="04ae8-105">Du kan ange dina Projektinställningar att använda emulatorn Express eller den fullständiga emulatorn, beroende på kraven för din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="04ae8-105">You can set your project settings to use either Emulator Express or the full emulator, depending on the requirements of your cloud service.</span></span> <span data-ttu-id="04ae8-106">Mer information om den fullständiga emulatorn finns [kör ett Azure-program i Compute Emulator](storage/common/storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="04ae8-106">For more information about the full emulator, see [Run an Azure Application in the Compute Emulator](storage/common/storage-use-emulator.md).</span></span>

## <a name="using-emulator-express-in-visual-studio"></a><span data-ttu-id="04ae8-107">Med hjälp av emulatorn i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04ae8-107">Using Emulator Express in Visual Studio</span></span>
<span data-ttu-id="04ae8-108">När du skapar ett Azure-projekt i Azure SDK 2.3 eller senare används automatiskt emulatorn Express.</span><span class="sxs-lookup"><span data-stu-id="04ae8-108">When you create an Azure project in Azure SDK 2.3 or later, Emulator Express is automatically used.</span></span> <span data-ttu-id="04ae8-109">För befintliga projekt som har skapats med en tidigare version av Azure SDK, använder du följande steg för att välja Express-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="04ae8-109">For existing projects that were created with an earlier version of the Azure SDK, use the following steps to select Emulator Express:</span></span>

1. <span data-ttu-id="04ae8-110">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04ae8-110">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="04ae8-111">I **Solution Explorer**, högerklicka på projektet och, från snabbmenyn, Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="04ae8-111">In **Solution Explorer**, right-click the project, and, from the context menu, select **Properties**.</span></span>

1. <span data-ttu-id="04ae8-112">Välj i egenskapssidor projekt i **Web** fliken.</span><span class="sxs-lookup"><span data-stu-id="04ae8-112">In the projects properties pages, select the **Web** tab.</span></span>

    ![Egenskaper för ett Azure cloud service-projekt](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. <span data-ttu-id="04ae8-114">Under **lokala utvecklingsserver**väljer **används IIS Express alternativet**.</span><span class="sxs-lookup"><span data-stu-id="04ae8-114">Under **Local Development Server**, select **Use IIS Express option**.</span></span>

1. <span data-ttu-id="04ae8-115">Under **emulatorn**väljer **Använd emulatorn Express**.</span><span class="sxs-lookup"><span data-stu-id="04ae8-115">Under **Emulator**, select **Use Emulator Express**.</span></span>
   
1. <span data-ttu-id="04ae8-116">Om du vill starta emulatorn Express, kör du följande kommando i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="04ae8-116">To launch the Emulator Express, run the following command at a command prompt:</span></span> 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a><span data-ttu-id="04ae8-117">Emulatorn Express begränsningar</span><span class="sxs-lookup"><span data-stu-id="04ae8-117">Emulator Express limitations</span></span>
<span data-ttu-id="04ae8-118">Följande är kända begränsningar i emulatorn Express:</span><span class="sxs-lookup"><span data-stu-id="04ae8-118">The following issues are known limitations of Emulator Express:</span></span> 

- <span data-ttu-id="04ae8-119">Emulatorn Express är inte kompatibel med IIS-webbserver.</span><span class="sxs-lookup"><span data-stu-id="04ae8-119">Emulator Express is not compatible with IIS Web Server.</span></span>
- <span data-ttu-id="04ae8-120">Din molntjänst kan innehålla flera roller, men varje roll är begränsad till en instans.</span><span class="sxs-lookup"><span data-stu-id="04ae8-120">Your cloud service can contain multiple roles, but each role is limited to one instance.</span></span>
- <span data-ttu-id="04ae8-121">Du kan inte komma åt portnummer nedan 1000.</span><span class="sxs-lookup"><span data-stu-id="04ae8-121">You can't access port numbers below 1000.</span></span> <span data-ttu-id="04ae8-122">Om du använder en autentiseringsprovider som normalt använder en port under 1000 kan behöva du ändra värdet till ett portnummer som är högre än 1000.</span><span class="sxs-lookup"><span data-stu-id="04ae8-122">If you use an authentication provider that normally uses a port below 1000, you might need to change this value to a port number that's above 1000.</span></span>
- <span data-ttu-id="04ae8-123">Eventuella begränsningar som gäller för Azure Compute Emulator gäller även för Express-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="04ae8-123">Any limitations that apply to the Azure Compute Emulator also apply to Emulator Express.</span></span> <span data-ttu-id="04ae8-124">Du kan till exempel inte fler än 50 rollinstanser per distribution.</span><span class="sxs-lookup"><span data-stu-id="04ae8-124">For example, you can't have more than 50 role instances per deployment.</span></span> <span data-ttu-id="04ae8-125">Läs mer om Azure Compute Emulator [kör ett Azure-program i Compute Emulator](http://go.microsoft.com/fwlink/p/?LinkId=623050).</span><span class="sxs-lookup"><span data-stu-id="04ae8-125">For more information about the Azure Compute Emulator, see [Run an Azure Application in the Compute Emulator](http://go.microsoft.com/fwlink/p/?LinkId=623050).</span></span>

## <a name="next-steps"></a><span data-ttu-id="04ae8-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="04ae8-126">Next steps</span></span>
[<span data-ttu-id="04ae8-127">Felsöka Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="04ae8-127">Debugging Azure cloud services</span></span>](https://msdn.microsoft.com/library/azure/ee405479.aspx)
