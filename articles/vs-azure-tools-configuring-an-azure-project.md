---
title: Konfigurera ett Azure cloud service-projekt i Visual Studio | Microsoft Docs
description: "Lär dig hur du konfigurerar en Azure cloud service-projekt i Visual Studio, beroende på dina krav för projektet."
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
ms.openlocfilehash: 799715093bd1a8c8e77e6cdb7168670dc263dfa5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="af157-103">Konfigurera ett Azure cloud service-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af157-103">Configure an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="af157-104">Du kan konfigurera ett Azure cloud service-projekt, beroende på dina krav för projektet.</span><span class="sxs-lookup"><span data-stu-id="af157-104">You can configure an Azure cloud service project, depending on your requirements for that project.</span></span> <span data-ttu-id="af157-105">Du kan ange egenskaper för projektet i följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="af157-105">You can set properties for the project for the following categories:</span></span>

- <span data-ttu-id="af157-106">**Publicera en tjänst i molnet till Azure** -du kan ange en egenskap för att se till att en befintlig molntjänst som distribueras till Azure inte tas bort av misstag.</span><span class="sxs-lookup"><span data-stu-id="af157-106">**Publish a cloud service to Azure** - You can set a property to make sure that an existing cloud service deployed to Azure is not accidentally deleted.</span></span>
- <span data-ttu-id="af157-107">**Köra eller felsöka en molnbaserad tjänst på den lokala datorn** -du kan välja en tjänstkonfiguration och ange om du vill starta Azure storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="af157-107">**Run or debug a cloud service on the local computer** - You can select a service configuration to use and indicate whether you want to start the Azure storage emulator.</span></span>
- <span data-ttu-id="af157-108">**Validera en cloud service-paket när den skapas** -du kan välja att behandla alla varningar som fel så att du kan se till att cloud service-paketet distribueras utan problem.</span><span class="sxs-lookup"><span data-stu-id="af157-108">**Validate a cloud service package when it is created** - You can decide to treat any warnings as errors so that you can ensure that the cloud service package deploys without any issues.</span></span> 

## <a name="steps-to-configure-an-azure-cloud-service-project"></a><span data-ttu-id="af157-109">Steg för att konfigurera en Azure cloud service-projekt</span><span class="sxs-lookup"><span data-stu-id="af157-109">Steps to configure an Azure cloud service project</span></span>
1. <span data-ttu-id="af157-110">Öppna eller skapa ett molntjänstprojekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af157-110">Open or create a cloud service project in Visual Studio</span></span>

1. <span data-ttu-id="af157-111">I **Solution Explorer**, högerklicka på projektet och, från snabbmenyn, Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="af157-111">In **Solution Explorer**, right-click the project, and, from the context menu, select **Properties**.</span></span>
   
1. <span data-ttu-id="af157-112">Välj i projektets egenskapssida i **Development** fliken.</span><span class="sxs-lookup"><span data-stu-id="af157-112">In the project's properties page, select the **Development** tab.</span></span>

    ![Projekt egenskaper-menyn](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. <span data-ttu-id="af157-114">Ange **fråga innan du tar bort en befintlig distribution** till **SANT**.</span><span class="sxs-lookup"><span data-stu-id="af157-114">Set **Prompt before deleting an existing deployment** to **True**.</span></span> <span data-ttu-id="af157-115">Den här inställningen som hjälper dig för att kontrollera att du inte av misstag tar bort en befintlig distribution i Azure</span><span class="sxs-lookup"><span data-stu-id="af157-115">This setting helps to ensure you don't accidentally delete an existing deployment in Azure</span></span>

1. <span data-ttu-id="af157-116">Välj den önskade **tjänstkonfiguration** som anger vilken tjänstkonfiguration som du vill använda när du kör eller felsöka din molntjänst lokalt.</span><span class="sxs-lookup"><span data-stu-id="af157-116">Select the desired **Service configuration** to indicate which service configuration you want to use when you run or debug your cloud service locally.</span></span> <span data-ttu-id="af157-117">Mer information om hur du ändrar en tjänstkonfiguration för en roll finns [hur du konfigurerar roller för en Azure-molntjänst med Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="af157-117">For more information on how to modify a service configuration for a role, see [How to configure the roles for an Azure cloud service with Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

1. <span data-ttu-id="af157-118">Ange **starta Azure storage-emulatorn** till **SANT** att starta Azure storage-emulatorn när du kör eller felsöka din molntjänst lokalt.</span><span class="sxs-lookup"><span data-stu-id="af157-118">Set **Start Azure storage emulator** to **True** to start the Azure storage emulator when you run or debug your cloud service locally.</span></span>

1. <span data-ttu-id="af157-119">Ange **behandla varningar som fel** till **SANT** att se till att du inte kan publicera om det finns valideringsfel i paketet.</span><span class="sxs-lookup"><span data-stu-id="af157-119">Set **Treat warnings as errors** to **True** to make sure you cannot publish if there are package validation errors.</span></span>

1. <span data-ttu-id="af157-120">Ange **web project portar** till **SANT** se till att din webbroll använder samma port varje gång det startar lokalt i IIS Express.</span><span class="sxs-lookup"><span data-stu-id="af157-120">Set **Use web project ports** to **True** to make sure that your web role uses the same port each time it starts locally in IIS Express.</span></span>

1. <span data-ttu-id="af157-121">I verktygsfältet i Visual Studio väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="af157-121">From the Visual Studio toolbar, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af157-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af157-122">Next steps</span></span>
- [<span data-ttu-id="af157-123">Konfigurera en Azure-projekt med flera tjänstkonfiguration</span><span class="sxs-lookup"><span data-stu-id="af157-123">Configure an Azure project using multiple service configurations</span></span>](vs-azure-tools-multiple-services-project-configurations.md)

