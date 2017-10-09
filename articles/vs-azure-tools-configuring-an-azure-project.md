---
title: aaaConfigure ett Azure cloud service-projekt i Visual Studio | Microsoft Docs
description: "Lär dig hur tooconfigure en Azure cloud service-projekt i Visual Studio, beroende på dina krav för projektet."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: 40eb5eedd6ea23bf03c8707431799be28beae701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="ac379-103">Konfigurera ett Azure cloud service-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac379-103">Configure an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="ac379-104">Du kan konfigurera ett Azure cloud service-projekt, beroende på dina krav för projektet.</span><span class="sxs-lookup"><span data-stu-id="ac379-104">You can configure an Azure cloud service project, depending on your requirements for that project.</span></span> <span data-ttu-id="ac379-105">Du kan ange egenskaper för hello projekt för hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="ac379-105">You can set properties for hello project for hello following categories:</span></span>

- <span data-ttu-id="ac379-106">**Publicera en cloud service tooAzure** -du kan ange en egenskap toomake till att en befintlig molnet tjänsten har distribuerats tooAzure inte tas bort av misstag.</span><span class="sxs-lookup"><span data-stu-id="ac379-106">**Publish a cloud service tooAzure** - You can set a property toomake sure that an existing cloud service deployed tooAzure is not accidentally deleted.</span></span>
- <span data-ttu-id="ac379-107">**Köra eller felsöka en tjänst i molnet på hello lokal dator** -du kan välja en service configuration toouse och ange om du vill toostart hello Azure storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="ac379-107">**Run or debug a cloud service on hello local computer** - You can select a service configuration toouse and indicate whether you want toostart hello Azure storage emulator.</span></span>
- <span data-ttu-id="ac379-108">**Validera en cloud service-paket när den skapas** -du kan välja tootreat alla varningar som fel så att du kan se till att hello cloud service-paketet distribueras utan problem.</span><span class="sxs-lookup"><span data-stu-id="ac379-108">**Validate a cloud service package when it is created** - You can decide tootreat any warnings as errors so that you can ensure that hello cloud service package deploys without any issues.</span></span> 

## <a name="steps-tooconfigure-an-azure-cloud-service-project"></a><span data-ttu-id="ac379-109">Steg tooconfigure ett Azure cloud service-projekt</span><span class="sxs-lookup"><span data-stu-id="ac379-109">Steps tooconfigure an Azure cloud service project</span></span>
1. <span data-ttu-id="ac379-110">Öppna eller skapa ett molntjänstprojekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac379-110">Open or create a cloud service project in Visual Studio</span></span>

1. <span data-ttu-id="ac379-111">I **Solution Explorer**, högerklicka på hello-projektet och, hello snabbmenyn väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ac379-111">In **Solution Explorer**, right-click hello project, and, from hello context menu, select **Properties**.</span></span>
   
1. <span data-ttu-id="ac379-112">Välj hello i hello projektets egenskapssida **Development** fliken.</span><span class="sxs-lookup"><span data-stu-id="ac379-112">In hello project's properties page, select hello **Development** tab.</span></span>

    ![Projekt egenskaper-menyn](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. <span data-ttu-id="ac379-114">Ange **fråga innan du tar bort en befintlig distribution** för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="ac379-114">Set **Prompt before deleting an existing deployment** too**True**.</span></span> <span data-ttu-id="ac379-115">Den här inställningen hjälper tooensure som du inte av misstag tar bort en befintlig distribution i Azure</span><span class="sxs-lookup"><span data-stu-id="ac379-115">This setting helps tooensure you don't accidentally delete an existing deployment in Azure</span></span>

1. <span data-ttu-id="ac379-116">Välj hello önskad **tjänstkonfiguration** tooindicate vilka tjänstkonfiguration önskade toouse när du kör eller felsöka din molntjänst lokalt.</span><span class="sxs-lookup"><span data-stu-id="ac379-116">Select hello desired **Service configuration** tooindicate which service configuration you want toouse when you run or debug your cloud service locally.</span></span> <span data-ttu-id="ac379-117">Mer information om hur toomodify en tjänstkonfiguration för en roll finns [hur tooconfigure hello roller för en Azure cloud service med Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="ac379-117">For more information on how toomodify a service configuration for a role, see [How tooconfigure hello roles for an Azure cloud service with Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

1. <span data-ttu-id="ac379-118">Ange **starta Azure storage-emulatorn** för**SANT** toostart hello Azure storage-emulatorn när du kör eller felsöka din molntjänst lokalt.</span><span class="sxs-lookup"><span data-stu-id="ac379-118">Set **Start Azure storage emulator** too**True** toostart hello Azure storage emulator when you run or debug your cloud service locally.</span></span>

1. <span data-ttu-id="ac379-119">Ange **behandla varningar som fel** för**SANT** toomake att du inte kan publicera om det finns valideringsfel i paketet.</span><span class="sxs-lookup"><span data-stu-id="ac379-119">Set **Treat warnings as errors** too**True** toomake sure you cannot publish if there are package validation errors.</span></span>

1. <span data-ttu-id="ac379-120">Ange **web project portar** för**SANT** toomake till att din webbroll använder hello samma port varje gång det startar lokalt i IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ac379-120">Set **Use web project ports** too**True** toomake sure that your web role uses hello same port each time it starts locally in IIS Express.</span></span>

1. <span data-ttu-id="ac379-121">Hello Visual Studio-verktygsfältet, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="ac379-121">From hello Visual Studio toolbar, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac379-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac379-122">Next steps</span></span>
- [<span data-ttu-id="ac379-123">Konfigurera en Azure-projekt med flera tjänstkonfiguration</span><span class="sxs-lookup"><span data-stu-id="ac379-123">Configure an Azure project using multiple service configurations</span></span>](vs-azure-tools-multiple-services-project-configurations.md)

