---
title: aaaCreating ett Azure cloud service-projekt i Visual Studio | Microsoft Docs
description: "Lär dig nu toocreate ett Azure cloud service-projekt i Visual Studio"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 3c357016aa423688199a7ab3a670115e33a98fe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="b8912-103">Skapa ett Azure cloud service-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8912-103">Creating an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="b8912-104">hello Azure Tools för Visual Studio tillhandahåller en projektmall som låter dig skapa ett Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="b8912-104">hello Azure Tools for Visual Studio provides a project template that lets you create an Azure cloud service.</span></span> <span data-ttu-id="b8912-105">När hello projektet har skapats, Visual Studio kan du tooconfigure, felsöka och distribuera hello cloud service tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b8912-105">Once hello project has been created, Visual Studio enables you tooconfigure, debug, and deploy hello cloud service tooAzure.</span></span>

## <a name="steps-toocreate-an-azure-cloud-service-project-in-visual-studio"></a><span data-ttu-id="b8912-106">Steg toocreate ett Azure cloud service-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8912-106">Steps toocreate an Azure cloud service project in Visual Studio</span></span>
<span data-ttu-id="b8912-107">Det här avsnittet vägleder dig genom att skapa ett Azure cloud service-projekt i Visual Studio med en eller flera webbroller.</span><span class="sxs-lookup"><span data-stu-id="b8912-107">This section walks you through creating an Azure cloud service project in Visual Studio with one or more web roles.</span></span>  

1. <span data-ttu-id="b8912-108">Starta Visual Studio som administratör.</span><span class="sxs-lookup"><span data-stu-id="b8912-108">Start Visual Studio as an administrator.</span></span>

1. <span data-ttu-id="b8912-109">Markera på huvudmenyn hello **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="b8912-109">On hello main menu, select **File** > **New** > **Project**.</span></span>

1. <span data-ttu-id="b8912-110">Välj **moln** projektet mallen noder från hello Visual C# eller Visual Basic och välj **Azure Cloud Service** hello listan över mallar.</span><span class="sxs-lookup"><span data-stu-id="b8912-110">Select **Cloud** from hello Visual C# or Visual Basic project template nodes, and select **Azure Cloud Service** from hello list of templates.</span></span>

    ![Nya Azure-molntjänst](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. <span data-ttu-id="b8912-112">Ange vilken version av hello .NET Framework som du vill toouse toodevelop projektet.</span><span class="sxs-lookup"><span data-stu-id="b8912-112">Specify which version of hello .NET Framework you want toouse toodevelop your project.</span></span>

1. <span data-ttu-id="b8912-113">Ange ett namn och plats för ditt projekt och ett namn för hello lösning.</span><span class="sxs-lookup"><span data-stu-id="b8912-113">Enter a name and location for your project and a name for hello solution.</span></span> 

1. <span data-ttu-id="b8912-114">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8912-114">Select **OK**.</span></span>

1. <span data-ttu-id="b8912-115">I hello **nya Microsoft Azure Cloud Service** dialogrutan, Välj hello roller som du vill tooadd och välj hello högerpilen knappen tooadd dem tooyour lösning.</span><span class="sxs-lookup"><span data-stu-id="b8912-115">In hello **New Microsoft Azure Cloud Service** dialog, select hello roles that you want tooadd, and choose hello right arrow button tooadd them tooyour solution.</span></span>

    ![Välj Azure cloud service-roller](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. <span data-ttu-id="b8912-117">toorename en roll som du har lagt till hovra på hello roll i hello **ny Microsoft Azure-molntjänst** dialogrutan hello snabbmenyn, Välj **Byt namn på**.</span><span class="sxs-lookup"><span data-stu-id="b8912-117">toorename a role that you've added, hover on hello role in hello **New Microsoft Azure Cloud Service** dialog, and, from hello context menu, select **Rename**.</span></span> <span data-ttu-id="b8912-118">Du kan också byta namn på en roll i din lösning (i hello **Solution Explorer**) när den har lagts till.</span><span class="sxs-lookup"><span data-stu-id="b8912-118">You can also rename a role within your solution (in hello **Solution Explorer**) after it has been added.</span></span>

    ![Byt namn på Azure-molnet rolltjänst](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

<span data-ttu-id="b8912-120">hello Azure för Visual Studio-projekt har kopplingarna toohello roll projekt i hello lösning.</span><span class="sxs-lookup"><span data-stu-id="b8912-120">hello Visual Studio Azure project has associations toohello role projects in hello solution.</span></span> <span data-ttu-id="b8912-121">hello projektet innehåller också hello *tjänstdefinitionsfilen* och *tjänstkonfigurationsfilen*:</span><span class="sxs-lookup"><span data-stu-id="b8912-121">hello project also includes hello *service definition file* and *service configuration file*:</span></span>

- <span data-ttu-id="b8912-122">**Tjänstdefinitionsfilen** -definierar hello körningsinställningar för programmet, inklusive vilka roller är obligatoriska, slutpunkter och storlek på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b8912-122">**Service definition file** - Defines hello runtime settings for your application, including what roles are required, endpoints, and virtual machine size.</span></span> 
- <span data-ttu-id="b8912-123">**Tjänstkonfigurationsfilen** -konfigurerar hur många instanser av en roll som körs och hello värdena för hello inställningarna för en roll.</span><span class="sxs-lookup"><span data-stu-id="b8912-123">**Service configuration file** - Configures how many instances of a role are run and hello values of hello settings defined for a role.</span></span> 

<span data-ttu-id="b8912-124">Mer information om dessa filer finns [konfigurerar hello roller för en Azure-molntjänst med Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="b8912-124">For more information about these files, see [Configure hello Roles for an Azure cloud service with Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8912-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b8912-125">Next steps</span></span>
- [<span data-ttu-id="b8912-126">Hantera roller i Azure cloud service-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8912-126">Managing roles in Azure cloud service projects with Visual Studio</span></span>](./vs-azure-tools-cloud-service-project-managing-roles.md)
