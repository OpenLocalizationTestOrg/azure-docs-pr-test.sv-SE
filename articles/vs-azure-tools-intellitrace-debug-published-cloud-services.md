---
title: "Felsökning av en publicerad en Azure cloud service med Visual Studio och IntelliTrace | Microsoft Docs"
description: "Lär dig att felsöka en molntjänst med Visual Studio och IntelliTrace"
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
ms.openlocfilehash: 7905dfb97cbd7578a8422567fe674839d00c21ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a><span data-ttu-id="01222-103">Felsökning av en publicerad Azure cloud service med Visual Studio och IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="01222-103">Debugging a published Azure cloud service with Visual Studio and IntelliTrace</span></span>
<span data-ttu-id="01222-104">Med IntelliTrace, kan du logga omfattande felsökningsinformation för en rollinstans när den körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="01222-104">With IntelliTrace, you can log extensive debugging information for a role instance when it runs in Azure.</span></span> <span data-ttu-id="01222-105">Om du behöver ta reda på orsaken till ett problem kan du använda IntelliTrace-loggar för att stega igenom koden från Visual Studio som om den kördes i Azure.</span><span class="sxs-lookup"><span data-stu-id="01222-105">If you need to find the cause of a problem, you can use the IntelliTrace logs to step through your code from Visual Studio as if it were running in Azure.</span></span> <span data-ttu-id="01222-106">I praktiken key IntelliTrace poster kodkörning och miljödata när ditt Azure-program körs som en tjänst i molnet i Azure och du kan upprepa inspelade data från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01222-106">In effect, IntelliTrace records key code execution and environment data when your Azure application is running as a cloud service in Azure, and lets you replay the recorded data from Visual Studio.</span></span> 

<span data-ttu-id="01222-107">Du kan använda IntelliTrace om du har Visual Studio Enterprise installerad och din Azure-program mål .NET Framework 4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="01222-107">You can use IntelliTrace if you have Visual Studio Enterprise installed and your Azure application targets .NET Framework 4 or a later version.</span></span> <span data-ttu-id="01222-108">IntelliTrace samlar in information för din Azure-roller.</span><span class="sxs-lookup"><span data-stu-id="01222-108">IntelliTrace collects information for your Azure roles.</span></span> <span data-ttu-id="01222-109">De virtuella datorerna för dessa roller körs alltid 64-bitars operativsystem.</span><span class="sxs-lookup"><span data-stu-id="01222-109">The virtual machines for these roles always run 64-bit operating systems.</span></span>

<span data-ttu-id="01222-110">Alternativt kan du använda [fjärrfelsökning](http://go.microsoft.com/fwlink/p/?LinkId=623041) att ansluta direkt till en molnbaserad tjänst som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="01222-110">As an alternative, you can use [remote debugging](http://go.microsoft.com/fwlink/p/?LinkId=623041) to attach directly to a cloud service that's running in Azure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="01222-111">IntelliTrace är avsedd för debug-scenarier och ska inte användas för en Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="01222-111">IntelliTrace is intended for debug scenarios only, and should not be used for a production deployment.</span></span>
> 

## <a name="configure-an-azure-application-for-intellitrace"></a><span data-ttu-id="01222-112">Konfigurera ett Azure-program för IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="01222-112">Configure an Azure application for IntelliTrace</span></span>
<span data-ttu-id="01222-113">Du måste skapa och publicera programmet från Visual Studio Azure-projekt för att aktivera IntelliTrace för ett Azure-program.</span><span class="sxs-lookup"><span data-stu-id="01222-113">To enable IntelliTrace for an Azure application, you must create and publish the application from a Visual Studio Azure project.</span></span> <span data-ttu-id="01222-114">Du måste konfigurera IntelliTrace för ditt Azure-program innan du publicerar den till Azure.</span><span class="sxs-lookup"><span data-stu-id="01222-114">You must configure IntelliTrace for your Azure application before you publish it to Azure.</span></span> <span data-ttu-id="01222-115">Om du publicerar ditt program utan att konfigurera IntelliTrace måste du publicera projektet.</span><span class="sxs-lookup"><span data-stu-id="01222-115">If you publish your application without configuring IntelliTrace, you need to republish the project.</span></span> <span data-ttu-id="01222-116">Mer information finns i [publicera ett Azure cloud services-projekt med hjälp av Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span><span class="sxs-lookup"><span data-stu-id="01222-116">For more information, see [Publishing an Azure cloud services projects using Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).</span></span>

1. <span data-ttu-id="01222-117">När du är redo att distribuera din Azure-program, kontrollera att bygga projektet målen är **felsöka**.</span><span class="sxs-lookup"><span data-stu-id="01222-117">When you are ready to deploy your Azure application, verify that your project build targets are set to **Debug**.</span></span>

1. <span data-ttu-id="01222-118">I **Solution Explorer**, högerklicka på projektet och, från snabbmenyn, Välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="01222-118">In **Solution Explorer**, right-click project, and, from the context menu, select **Publish**.</span></span>
   
1. <span data-ttu-id="01222-119">I den **publicera Azure-programmet** dialogrutan, Välj den Azure-prenumerationen och välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="01222-119">In the **Publish Azure Application** dialog, select the Azure subscription, and select **Next**.</span></span>

1. <span data-ttu-id="01222-120">I den **inställningar** väljer den **avancerade inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="01222-120">In the **Settings** page, select the **Advanced Settings** tab.</span></span>

1. <span data-ttu-id="01222-121">Aktivera den **aktivera IntelliTrace** alternativet för att samla in IntelliTrace-loggar för ditt program när den publiceras i molnet.</span><span class="sxs-lookup"><span data-stu-id="01222-121">Turn on the **Enable IntelliTrace** option to collect IntelliTrace logs for your application when it is published in the cloud.</span></span>
   
1. <span data-ttu-id="01222-122">Om du vill anpassa den grundläggande konfigurationen för IntelliTrace Välj **inställningar** bredvid **aktivera IntelliTrace**.</span><span class="sxs-lookup"><span data-stu-id="01222-122">To customize the basic IntelliTrace configuration, select **Settings** next to **Enable IntelliTrace**.</span></span>

    ![Länk för IntelliTrace-inställningar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. <span data-ttu-id="01222-124">I den **IntelliTrace inställningar** dialogrutan kan du ange vilka händelser som ska logga, om du vill samla in information om anropet, vilka moduler och processer för att samla in loggar för och hur mycket utrymme som ska allokeras till inspelningen.</span><span class="sxs-lookup"><span data-stu-id="01222-124">In the **IntelliTrace Settings** dialog, you can specify which events to log, whether to collect call information, which modules and processes to collect logs for, and how much space to allocate to the recording.</span></span> <span data-ttu-id="01222-125">Mer information om IntelliTrace finns [felsökning med IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span><span class="sxs-lookup"><span data-stu-id="01222-125">For more information about IntelliTrace, see [Debugging with IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).</span></span>
   
    ![IntelliTrace-inställningar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

<span data-ttu-id="01222-127">IntelliTrace-logg är en cirkulär loggfil med den maximala storleken som anges i inställningarna för IntelliTrace (standardstorleken är 250 MB).</span><span class="sxs-lookup"><span data-stu-id="01222-127">The IntelliTrace log is a circular log file of the maximum size specified in the IntelliTrace settings (the default size is 250 MB).</span></span> <span data-ttu-id="01222-128">IntelliTrace-loggar samlas in till en fil i filsystemet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="01222-128">IntelliTrace logs are collected to a file in the file system of the virtual machine.</span></span> <span data-ttu-id="01222-129">När du begär loggarna en ögonblicksbild tas då i gång och hämtas till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="01222-129">When you request the logs, a snapshot is taken at that point in time and downloaded to your local computer.</span></span>

<span data-ttu-id="01222-130">När Azure-Molntjänsten har publicerats till Azure kan du bestämma om IntelliTrace har aktiverats på noden Azure i **Server Explorer**, enligt följande bild:</span><span class="sxs-lookup"><span data-stu-id="01222-130">After the Azure cloud service has been published to Azure, you can determine if IntelliTrace has been enabled from the Azure node in **Server Explorer**, as shown in the following image:</span></span>

![Server Explorer - IntelliTrace aktiverad](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a><span data-ttu-id="01222-132">Hämta IntelliTrace-loggar för en rollinstans</span><span class="sxs-lookup"><span data-stu-id="01222-132">Download IntelliTrace logs for a role instance</span></span>
<span data-ttu-id="01222-133">Med Visual Studio kan hämta du IntelliTrace-loggar för en rollinstans genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="01222-133">Using Visual Studio, you can download IntelliTrace logs for a role instance by following these steps:</span></span>

1. <span data-ttu-id="01222-134">I **Server Explorer**, expandera den **molntjänster** nod, och leta upp rollinstansen vars loggar som du vill hämta.</span><span class="sxs-lookup"><span data-stu-id="01222-134">In **Server Explorer**, expand the **Cloud Services** node, and locate role instance whose logs you wish to download.</span></span> 

1. <span data-ttu-id="01222-135">Högerklicka på instansen och på snabbmenyn s, Välj **visa IntelliTrace-loggar**.</span><span class="sxs-lookup"><span data-stu-id="01222-135">Right-click the role instance, and from the s context menu, select **View IntelliTrace Logs**.</span></span> 

    ![Visa menyalternativet för IntelliTrace-loggar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. <span data-ttu-id="01222-137">IntelliTrace-loggar hämtas till en fil i en katalog på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="01222-137">The IntelliTrace logs are downloaded to a file in a directory on your local computer.</span></span> <span data-ttu-id="01222-138">Varje gång du begär IntelliTrace-loggar, skapas en ny ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="01222-138">Each time that you request the IntelliTrace logs, a new snapshot is created.</span></span> <span data-ttu-id="01222-139">När loggarna hämtas Visual Studio visar förloppet för åtgärden i den **Azure-aktivitetsloggen** fönster.</span><span class="sxs-lookup"><span data-stu-id="01222-139">While the logs are being downloaded, Visual Studio displays the progress of the operation in the **Azure Activity Log** window.</span></span> <span data-ttu-id="01222-140">Du kan expandera radobjekt för åtgärden att se mer information som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="01222-140">As shown in the following figure, you can expand the line item for the operation to see more detail.</span></span>

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

<span data-ttu-id="01222-142">Du kan fortsätta att arbeta i Visual Studio när hämtar IntelliTrace-loggar.</span><span class="sxs-lookup"><span data-stu-id="01222-142">You can continue to work in Visual Studio while the IntelliTrace logs are downloading.</span></span> <span data-ttu-id="01222-143">När loggen har hämtats, öppnas i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01222-143">When the log has finished downloading, it opens in Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="01222-144">IntelliTrace-loggar kan innehålla undantag som ramen genererar och hanterar efteråt.</span><span class="sxs-lookup"><span data-stu-id="01222-144">The IntelliTrace logs might contain exceptions that the framework generates and handles afterwards.</span></span> <span data-ttu-id="01222-145">Internt framework kod genereras undantagen som för att starta en roll, så du kan ignorera dem..</span><span class="sxs-lookup"><span data-stu-id="01222-145">Internal framework code generates these exceptions as a normal part of starting up a role, so you may safely ignore them.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="01222-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="01222-146">Next steps</span></span>
- [<span data-ttu-id="01222-147">Alternativ för felsökning av Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="01222-147">Options for debugging Azure cloud services</span></span>](vs-azure-tools-debugging-cloud-services-overview.md)
- [<span data-ttu-id="01222-148">Publicera ett Azure cloud service med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01222-148">Publishing an Azure cloud service using Visual Studio</span></span>](vs-azure-tools-publishing-a-cloud-service.md)