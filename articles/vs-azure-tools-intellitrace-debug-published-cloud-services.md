---
title: aaaDebugging publicerade ett Azure cloud service med Visual Studio och IntelliTrace | Microsoft Docs
description: "Lär dig hur toodebug ett moln service med Visual Studio och IntelliTrace"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 60734a71d5499de72e1ca81a3ab0ab9f34bc303a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a><span data-ttu-id="696eb-103">Felsökning av en publicerad Azure cloud service med Visual Studio och IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="696eb-103">Debugging a published Azure cloud service with Visual Studio and IntelliTrace</span></span>
<span data-ttu-id="696eb-104">Med IntelliTrace, kan du logga omfattande felsökningsinformation för en rollinstans när den körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="696eb-104">With IntelliTrace, you can log extensive debugging information for a role instance when it runs in Azure.</span></span> <span data-ttu-id="696eb-105">Om du behöver toofind hello orsaken till ett problem kan använda du hello IntelliTrace-loggar toostep genom från Visual Studio-koden som om den kördes i Azure.</span><span class="sxs-lookup"><span data-stu-id="696eb-105">If you need toofind hello cause of a problem, you can use hello IntelliTrace logs toostep through your code from Visual Studio as if it were running in Azure.</span></span> <span data-ttu-id="696eb-106">I praktiken IntelliTrace viktiga kod körning och miljö postdata när ditt Azure-program körs som en tjänst i molnet i Azure och du kan spela upp hello registreras data från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="696eb-106">In effect, IntelliTrace records key code execution and environment data when your Azure application is running as a cloud service in Azure, and lets you replay hello recorded data from Visual Studio.</span></span> 

<span data-ttu-id="696eb-107">Du kan använda IntelliTrace om du har Visual Studio Enterprise installerad och din Azure-program mål .NET Framework 4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="696eb-107">You can use IntelliTrace if you have Visual Studio Enterprise installed and your Azure application targets .NET Framework 4 or a later version.</span></span> <span data-ttu-id="696eb-108">IntelliTrace samlar in information för din Azure-roller.</span><span class="sxs-lookup"><span data-stu-id="696eb-108">IntelliTrace collects information for your Azure roles.</span></span> <span data-ttu-id="696eb-109">hello virtuella datorer för dessa roller körs alltid 64-bitars operativsystem.</span><span class="sxs-lookup"><span data-stu-id="696eb-109">hello virtual machines for these roles always run 64-bit operating systems.</span></span>

<span data-ttu-id="696eb-110">Alternativt kan du använda [fjärrfelsökning](http://go.microsoft.com/fwlink/p/?LinkId=623041) tooattach direkt tooa molntjänst som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="696eb-110">As an alternative, you can use [remote debugging](http://go.microsoft.com/fwlink/p/?LinkId=623041) tooattach directly tooa cloud service that's running in Azure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="696eb-111">IntelliTrace är avsedd för debug-scenarier och ska inte användas för en Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="696eb-111">IntelliTrace is intended for debug scenarios only, and should not be used for a production deployment.</span></span>
> 

## <a name="configure-an-azure-application-for-intellitrace"></a><span data-ttu-id="696eb-112">Konfigurera ett Azure-program för IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="696eb-112">Configure an Azure application for IntelliTrace</span></span>
<span data-ttu-id="696eb-113">tooenable IntelliTrace för ett Azure-program måste du skapa och publicera programmet hello från ett Azure för Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="696eb-113">tooenable IntelliTrace for an Azure application, you must create and publish hello application from a Visual Studio Azure project.</span></span> <span data-ttu-id="696eb-114">Du måste konfigurera IntelliTrace för ditt Azure-program innan du publicerar den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="696eb-114">You must configure IntelliTrace for your Azure application before you publish it tooAzure.</span></span> <span data-ttu-id="696eb-115">Om du publicerar ditt program utan att konfigurera IntelliTrace måste toorepublish hello projektet.</span><span class="sxs-lookup"><span data-stu-id="696eb-115">If you publish your application without configuring IntelliTrace, you need toorepublish hello project.</span></span> <span data-ttu-id="696eb-116">Mer information finns i [publicera ett Azure cloud services-projekt med hjälp av Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span><span class="sxs-lookup"><span data-stu-id="696eb-116">For more information, see [Publishing an Azure cloud services projects using Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span></span>

1. <span data-ttu-id="696eb-117">När du är klar toodeploy ditt Azure-program, kontrollerar du att projektet build målen är inställda för**felsöka**.</span><span class="sxs-lookup"><span data-stu-id="696eb-117">When you are ready toodeploy your Azure application, verify that your project build targets are set too**Debug**.</span></span>

1. <span data-ttu-id="696eb-118">I **Solution Explorer**, högerklicka på projektet och, hello snabbmenyn väljer **publicera**.</span><span class="sxs-lookup"><span data-stu-id="696eb-118">In **Solution Explorer**, right-click project, and, from hello context menu, select **Publish**.</span></span>
   
1. <span data-ttu-id="696eb-119">I hello **publicera Azure-programmet** dialogrutan, Välj hello Azure-prenumeration och välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="696eb-119">In hello **Publish Azure Application** dialog, select hello Azure subscription, and select **Next**.</span></span>

1. <span data-ttu-id="696eb-120">I hello **inställningar** sidan, Välj hello **avancerade inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="696eb-120">In hello **Settings** page, select hello **Advanced Settings** tab.</span></span>

1. <span data-ttu-id="696eb-121">Aktivera hello **aktivera IntelliTrace** alternativet toocollect IntelliTrace-loggar för ditt program när den publiceras i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="696eb-121">Turn on hello **Enable IntelliTrace** option toocollect IntelliTrace logs for your application when it is published in hello cloud.</span></span>
   
1. <span data-ttu-id="696eb-122">toocustomize hello IntelliTrace grundkonfiguration, Välj **inställningar** nästa för**aktivera IntelliTrace**.</span><span class="sxs-lookup"><span data-stu-id="696eb-122">toocustomize hello basic IntelliTrace configuration, select **Settings** next too**Enable IntelliTrace**.</span></span>

    ![Länk för IntelliTrace-inställningar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. <span data-ttu-id="696eb-124">I hello **IntelliTrace inställningar** dialogrutan, du kan ange vilka händelser toolog, om toocollect anropa information, vilka moduler och processer toocollect loggar för och hur mycket utrymme tooallocate toohello registrering.</span><span class="sxs-lookup"><span data-stu-id="696eb-124">In hello **IntelliTrace Settings** dialog, you can specify which events toolog, whether toocollect call information, which modules and processes toocollect logs for, and how much space tooallocate toohello recording.</span></span> <span data-ttu-id="696eb-125">Mer information om IntelliTrace finns [felsökning med IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span><span class="sxs-lookup"><span data-stu-id="696eb-125">For more information about IntelliTrace, see [Debugging with IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span></span>
   
    ![IntelliTrace-inställningar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

<span data-ttu-id="696eb-127">Hej IntelliTrace-logg är en cirkulär loggfil över hello maximala storleken som angavs i hello IntelliTrace-inställningar (hello standardstorlek är 250 MB).</span><span class="sxs-lookup"><span data-stu-id="696eb-127">hello IntelliTrace log is a circular log file of hello maximum size specified in hello IntelliTrace settings (hello default size is 250 MB).</span></span> <span data-ttu-id="696eb-128">IntelliTrace-loggar är insamlade tooa filen i hello filsystem hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="696eb-128">IntelliTrace logs are collected tooa file in hello file system of hello virtual machine.</span></span> <span data-ttu-id="696eb-129">När du begär hello loggar en ögonblicksbild tas då i gång och hämtas tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="696eb-129">When you request hello logs, a snapshot is taken at that point in time and downloaded tooyour local computer.</span></span>

<span data-ttu-id="696eb-130">När hello Azure-Molntjänsten har publicerade tooAzure kan du bestämma om IntelliTrace har aktiverats från hello Azure nod i **Server Explorer**som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="696eb-130">After hello Azure cloud service has been published tooAzure, you can determine if IntelliTrace has been enabled from hello Azure node in **Server Explorer**, as shown in hello following image:</span></span>

![Server Explorer - IntelliTrace aktiverad](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a><span data-ttu-id="696eb-132">Hämta IntelliTrace-loggar för en rollinstans</span><span class="sxs-lookup"><span data-stu-id="696eb-132">Download IntelliTrace logs for a role instance</span></span>
<span data-ttu-id="696eb-133">Med Visual Studio kan hämta du IntelliTrace-loggar för en rollinstans genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="696eb-133">Using Visual Studio, you can download IntelliTrace logs for a role instance by following these steps:</span></span>

1. <span data-ttu-id="696eb-134">I **Server Explorer**, expandera hello **molntjänster** nod, och leta upp rollinstansen vars loggar som du vill toodownload.</span><span class="sxs-lookup"><span data-stu-id="696eb-134">In **Server Explorer**, expand hello **Cloud Services** node, and locate role instance whose logs you wish toodownload.</span></span> 

1. <span data-ttu-id="696eb-135">Högerklicka på hälsningspaket rollinstansen och snabbmenyn hello s, Välj **visa IntelliTrace-loggar**.</span><span class="sxs-lookup"><span data-stu-id="696eb-135">Right-click hello role instance, and from hello s context menu, select **View IntelliTrace Logs**.</span></span> 

    ![Visa menyalternativet för IntelliTrace-loggar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. <span data-ttu-id="696eb-137">Hej IntelliTrace-loggar är hämtade tooa fil i en katalog på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="696eb-137">hello IntelliTrace logs are downloaded tooa file in a directory on your local computer.</span></span> <span data-ttu-id="696eb-138">Varje gång du begär hello IntelliTrace-loggar, skapas en ny ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="696eb-138">Each time that you request hello IntelliTrace logs, a new snapshot is created.</span></span> <span data-ttu-id="696eb-139">Medan hello loggar hämtas, Visual Studio visar hello hello åtgärdens förlopp i hello **Azure-aktivitetsloggen** fönster.</span><span class="sxs-lookup"><span data-stu-id="696eb-139">While hello logs are being downloaded, Visual Studio displays hello progress of hello operation in hello **Azure Activity Log** window.</span></span> <span data-ttu-id="696eb-140">Som visas i följande bild hello, kan du expandera hello radobjekt för hello åtgärden toosee detalj.</span><span class="sxs-lookup"><span data-stu-id="696eb-140">As shown in hello following figure, you can expand hello line item for hello operation toosee more detail.</span></span>

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

<span data-ttu-id="696eb-142">Du kan fortsätta toowork i Visual Studio medan hämtar hello IntelliTrace-loggar.</span><span class="sxs-lookup"><span data-stu-id="696eb-142">You can continue toowork in Visual Studio while hello IntelliTrace logs are downloading.</span></span> <span data-ttu-id="696eb-143">När hello loggen har hämtats, öppnas i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="696eb-143">When hello log has finished downloading, it opens in Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="696eb-144">Hej IntelliTrace-loggar kan innehålla undantag hello ramverket genererar och hanterar efteråt.</span><span class="sxs-lookup"><span data-stu-id="696eb-144">hello IntelliTrace logs might contain exceptions that hello framework generates and handles afterwards.</span></span> <span data-ttu-id="696eb-145">Internt framework kod genereras undantagen som för att starta en roll, så du kan ignorera dem..</span><span class="sxs-lookup"><span data-stu-id="696eb-145">Internal framework code generates these exceptions as a normal part of starting up a role, so you may safely ignore them.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="696eb-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="696eb-146">Next steps</span></span>
- [<span data-ttu-id="696eb-147">Alternativ för felsökning av Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="696eb-147">Options for debugging Azure cloud services</span></span>](vs-azure-tools-debugging-cloud-services-overview.md)
- [<span data-ttu-id="696eb-148">Publicera ett Azure cloud service med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="696eb-148">Publishing an Azure cloud service using Visual Studio</span></span>](vs-azure-tools-publishing-a-cloud-service.md)