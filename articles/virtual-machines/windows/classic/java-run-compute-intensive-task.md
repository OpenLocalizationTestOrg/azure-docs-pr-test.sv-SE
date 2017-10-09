---
title: "aaaCompute-intensiva Java-program på en virtuell dator | Microsoft Docs"
description: "Lär dig hur toocreate en Azure-dator som kör ett beräkningsintensiva Java-program som kan övervakas av ett annat Java-program."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: ae6f2737-94c7-4569-9913-d871450c2827
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 02a198802a8d78bd444cd5a9197a78cb94f48e3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-compute-intensive-task-in-java-on-a-virtual-machine"></a><span data-ttu-id="b4a32-103">Hur toorun en beräkningsintensiva uppgift i Java på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b4a32-103">How toorun a compute-intensive task in Java on a virtual machine</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="b4a32-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b4a32-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b4a32-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b4a32-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="b4a32-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="b4a32-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="b4a32-107">Med Azure, kan du använda en virtuell dator toohandle beräkningsintensiva aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="b4a32-107">With Azure, you can use a virtual machine toohandle compute-intensive tasks.</span></span> <span data-ttu-id="b4a32-108">En virtuell dator kan hantera aktiviteter och leverera resultat tooclient datorer eller mobila program.</span><span class="sxs-lookup"><span data-stu-id="b4a32-108">For example, a virtual machine can handle tasks and deliver results tooclient machines or mobile applications.</span></span> <span data-ttu-id="b4a32-109">När du har läst den här artikeln kommer du ha en förståelse av hur toocreate en virtuell dator som kör ett beräkningsintensiva Java-program som kan övervakas av ett annat Java-program.</span><span class="sxs-lookup"><span data-stu-id="b4a32-109">After reading this article, you will have an understanding of how toocreate a virtual machine that runs a compute-intensive Java application that can be monitored by another Java application.</span></span>

<span data-ttu-id="b4a32-110">Den här kursen förutsätter att du vet hur toocreate Java konsolprogram kan importera bibliotek tooyour Java-program och kan generera ett Java-Arkiv (JAR).</span><span class="sxs-lookup"><span data-stu-id="b4a32-110">This tutorial assumes you know how toocreate Java console applications, can import libraries tooyour Java application, and can generate a Java archive (JAR).</span></span> <span data-ttu-id="b4a32-111">Ingen kunskap om Microsoft Azure antas.</span><span class="sxs-lookup"><span data-stu-id="b4a32-111">No knowledge of Microsoft Azure is assumed.</span></span>

<span data-ttu-id="b4a32-112">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="b4a32-112">You will learn:</span></span>

* <span data-ttu-id="b4a32-113">Hur toocreate en virtuell dator med en Java Development Kit (JDK) redan installerats.</span><span class="sxs-lookup"><span data-stu-id="b4a32-113">How toocreate a virtual machine with a Java Development Kit (JDK) already installed.</span></span>
* <span data-ttu-id="b4a32-114">Hur tooremotely logga in tooyour virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-114">How tooremotely log in tooyour virtual machine.</span></span>
* <span data-ttu-id="b4a32-115">Hur toocreate en service bus namnområde.</span><span class="sxs-lookup"><span data-stu-id="b4a32-115">How toocreate a service bus namespace.</span></span>
* <span data-ttu-id="b4a32-116">Hur toocreate ett Java-program som utför en krävande uppgift.</span><span class="sxs-lookup"><span data-stu-id="b4a32-116">How toocreate a Java application that performs a compute-intensive task.</span></span>
* <span data-ttu-id="b4a32-117">Hur hello toocreate ett Java-program som övervakar förloppet för hello beräkningsintensiva aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="b4a32-117">How toocreate a Java application that monitors hello progress of hello compute-intensive task.</span></span>
* <span data-ttu-id="b4a32-118">Hur toorun hello Java-program.</span><span class="sxs-lookup"><span data-stu-id="b4a32-118">How toorun hello Java applications.</span></span>
* <span data-ttu-id="b4a32-119">Hur toostop hello Java-program.</span><span class="sxs-lookup"><span data-stu-id="b4a32-119">How toostop hello Java applications.</span></span>

<span data-ttu-id="b4a32-120">Den här kursen använder hello Traveling säljare Problem för hello beräkningsintensiva aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="b4a32-120">This tutorial will use hello Traveling Salesman Problem for hello compute-intensive task.</span></span> <span data-ttu-id="b4a32-121">hello följande är ett exempel på hello Java programmet hello beräkningsintensiva aktivitet som körs.</span><span class="sxs-lookup"><span data-stu-id="b4a32-121">hello following is an example of hello Java application running hello compute-intensive task.</span></span>

![Traveling säljare problemet solver][solver_output]

<span data-ttu-id="b4a32-123">hello följande är ett exempel på hello Java programmet hello beräkningsintensiva övervakningsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="b4a32-123">hello following is an example of hello Java application monitoring hello compute-intensive task.</span></span>

![Klienten Traveling säljare Problem][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a><span data-ttu-id="b4a32-125">toocreate en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b4a32-125">toocreate a virtual machine</span></span>
1. <span data-ttu-id="b4a32-126">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b4a32-126">Log in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="b4a32-127">Klicka på **ny**, klickar du på **Compute**, klickar du på **virtuella**, och klicka sedan på **från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-127">Click **New**, click **Compute**, click **Virtual machine**, and then click **From Gallery**.</span></span>
3. <span data-ttu-id="b4a32-128">I hello **virtuella avbildning väljer** dialogrutan **JDK 7 Windows Server 2012**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-128">In hello **Virtual machine image select** dialog box, select **JDK 7 Windows Server 2012**.</span></span>
   <span data-ttu-id="b4a32-129">Observera att **JDK 6 Windows Server 2012** är tillgänglig om du har äldre program som inte ännu är klar toorun i JDK 7.</span><span class="sxs-lookup"><span data-stu-id="b4a32-129">Note that **JDK 6 Windows Server 2012** is available in case you have legacy applications that are not yet ready toorun in JDK 7.</span></span>
4. <span data-ttu-id="b4a32-130">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-130">Click **Next**.</span></span>
5. <span data-ttu-id="b4a32-131">I hello **konfiguration av virtuell dator** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="b4a32-131">In hello **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="b4a32-132">Ange ett namn för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-132">Specify a name for hello virtual machine.</span></span>
   2. <span data-ttu-id="b4a32-133">Ange hello storlek toouse för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-133">Specify hello size toouse for hello virtual machine.</span></span>
   3. <span data-ttu-id="b4a32-134">Ange ett namn för Hej administratör i hello **användarnamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-134">Enter a name for hello administrator in hello **User Name** field.</span></span> <span data-ttu-id="b4a32-135">Kom ihåg detta namn och hello lösenord skriver du sedan kan du använda dem när du loggar via fjärranslutning på toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-135">Remember this name and hello password you will enter next, you will use them when you remotely log in toohello virtual machine.</span></span>
   4. <span data-ttu-id="b4a32-136">Ange ett lösenord i hello **nytt lösenord** fältet och ange den igen i hello **Bekräfta** fältet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-136">Enter a password in hello **New password** field, and re-enter it in hello **Confirm** field.</span></span> <span data-ttu-id="b4a32-137">Detta är hello lösenordet för administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="b4a32-137">This is hello Administrator account password.</span></span>
   5. <span data-ttu-id="b4a32-138">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-138">Click **Next**.</span></span>
6. <span data-ttu-id="b4a32-139">I hello nästa **konfiguration av virtuell dator** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="b4a32-139">In hello next **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="b4a32-140">För **Molntjänsten**, Använd hello standard **skapa en ny molntjänst**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-140">For **Cloud service**, use hello default **Create a new cloud service**.</span></span>
   2. <span data-ttu-id="b4a32-141">Hej värde för **DNS molntjänstnamnet** måste vara unikt inom cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="b4a32-141">hello value for **Cloud service DNS name** must be unique across cloudapp.net.</span></span> <span data-ttu-id="b4a32-142">Om det behövs, ändra värdet så att Azure anger att det är unikt.</span><span class="sxs-lookup"><span data-stu-id="b4a32-142">If needed, modify this value so that Azure indicates it is unique.</span></span>
   3. <span data-ttu-id="b4a32-143">Ange en region, en tillhörighetsgrupp eller ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b4a32-143">Specify a region, affinity group, or virtual network.</span></span> <span data-ttu-id="b4a32-144">För den här kursen är att ange en region som **västra USA**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-144">For purposes of this tutorial, specify a region such as **West US**.</span></span>
   4. <span data-ttu-id="b4a32-145">För **Lagringskonto**väljer **använda ett automatiskt genererat lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-145">For **Storage Account**, select **Use an automatically generated storage account**.</span></span>
   5. <span data-ttu-id="b4a32-146">För **Tillgänglighetsuppsättning**väljer **(ingen)**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-146">For **Availability Set**, select **(None)**.</span></span>
   6. <span data-ttu-id="b4a32-147">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-147">Click **Next**.</span></span>
7. <span data-ttu-id="b4a32-148">I sista hello **konfiguration av virtuell dator** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="b4a32-148">In hello final **Virtual machine configuration** dialog box:</span></span>
   1. <span data-ttu-id="b4a32-149">Acceptera hello standardvärden för slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="b4a32-149">Accept hello default endpoint entries.</span></span>
   2. <span data-ttu-id="b4a32-150">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="b4a32-150">Click **Complete**.</span></span>

## <a name="tooremotely-log-in-tooyour-virtual-machine"></a><span data-ttu-id="b4a32-151">tooremotely inloggningen tooyour virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b4a32-151">tooremotely log in tooyour virtual machine</span></span>
1. <span data-ttu-id="b4a32-152">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b4a32-152">Log on toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="b4a32-153">Klicka på **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-153">Click **Virtual machines**.</span></span>
3. <span data-ttu-id="b4a32-154">Klicka på hello virtuell dator som du vill toolog i hello namn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-154">Click hello name of hello virtual machine that you want toolog in to.</span></span>
4. <span data-ttu-id="b4a32-155">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-155">Click **Connect**.</span></span>
5. <span data-ttu-id="b4a32-156">Svara toohello prompter som behövs tooconnect toohello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b4a32-156">Respond toohello prompts as needed tooconnect toohello virtual machine.</span></span> <span data-ttu-id="b4a32-157">När du uppmanas Hej administratör användarnamn och lösenord för använda hello-värden som du angav när du skapade hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-157">When prompted for hello administrator name and password, use hello values that you provided when you created hello virtual machine.</span></span>

<span data-ttu-id="b4a32-158">Observera att hello Azure Service Bus-funktioner kräver hello Baltimore CyberTrust Root certificate toobe installeras som en del av din JRE **cacerts** lagras.</span><span class="sxs-lookup"><span data-stu-id="b4a32-158">Note that hello Azure Service Bus functionality requires hello Baltimore CyberTrust Root certificate toobe installed as part of your JRE's **cacerts** store.</span></span> <span data-ttu-id="b4a32-159">Det här certifikatet ingår automatiskt i hello miljö JRE (Java Runtime) används av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="b4a32-159">This certificate is automatically included in hello Java Runtime Environment (JRE) used by this tutorial.</span></span> <span data-ttu-id="b4a32-160">Om du inte har det här certifikatet i dina JRE **cacerts** finns i [att lägga till ett certifikat toohello certifikatarkiv för Java CA] [ add_ca_cert] information om att lägga till den (samt information om hur du visar hello certifikat i din cacerts store).</span><span class="sxs-lookup"><span data-stu-id="b4a32-160">If you do not have this certificate in your JRE **cacerts** store, see [Adding a Certificate toohello Java CA Certificate Store][add_ca_cert] for information on adding it (as well as information on viewing hello certificates in your cacerts store).</span></span>

## <a name="how-toocreate-a-service-bus-namespace"></a><span data-ttu-id="b4a32-161">Hur toocreate en service bus namnområde</span><span class="sxs-lookup"><span data-stu-id="b4a32-161">How toocreate a service bus namespace</span></span>
<span data-ttu-id="b4a32-162">toobegin med hjälp av Service Bus-köer i Azure, måste du först skapa ett namnområde för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b4a32-162">toobegin using Service Bus queues in Azure, you must first create a service namespace.</span></span> <span data-ttu-id="b4a32-163">Ett namnområde för tjänsten innehåller en omfattningsbehållare för adressering av Service Bus-resurser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="b4a32-163">A service namespace provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="b4a32-164">toocreate ett namnområde för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="b4a32-164">toocreate a service namespace:</span></span>

1. <span data-ttu-id="b4a32-165">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b4a32-165">Log on toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="b4a32-166">Hello nedre vänstra navigeringsfönstret för hello klassiska Azure-portalen, klicka på **Service Bus, Access Control och cachelagring**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-166">In hello lower-left navigation pane of hello Azure classic portal, click **Service Bus, Access Control & Caching**.</span></span>
3. <span data-ttu-id="b4a32-167">Klicka på hello i hello övre vänstra rutan hello klassiska Azure-portalen, **Service Bus** nod, och klicka sedan på hello **ny** knappen.</span><span class="sxs-lookup"><span data-stu-id="b4a32-167">In hello upper-left pane of hello Azure classic portal, click hello **Service Bus** node, and then click hello **New** button.</span></span>  
   <span data-ttu-id="b4a32-168">![Skärmbild för Service Bus-nod][svc_bus_node]</span><span class="sxs-lookup"><span data-stu-id="b4a32-168">![Service Bus Node screenshot][svc_bus_node]</span></span>
4. <span data-ttu-id="b4a32-169">I hello **skapa en ny Service Namespace** dialogrutan Ange en **Namespace**, och klicka på toomake till att det är unikt, den **Kontrollera tillgänglighet** knappen.</span><span class="sxs-lookup"><span data-stu-id="b4a32-169">In hello **Create a new Service Namespace** dialog box, enter a **Namespace**, and then toomake sure that it is unique, click the **Check Availability** button.</span></span>  
   <span data-ttu-id="b4a32-170">![Skapa en ny Namespace skärmbild][create_namespace]</span><span class="sxs-lookup"><span data-stu-id="b4a32-170">![Create a New Namespace screenshot][create_namespace]</span></span>
5. <span data-ttu-id="b4a32-171">Kontrollerar hello namnområdesnamnet som är tillgänglig, väljer du land eller region där namnområdet ska finnas och klicka sedan på hello **skapa Namespace** knappen.</span><span class="sxs-lookup"><span data-stu-id="b4a32-171">After making sure hello namespace name is available, choose the country or region in which your namespace should be hosted, and then click hello **Create Namespace** button.</span></span>  
   
   <span data-ttu-id="b4a32-172">hello-namnområde som du skapat visas sedan i hello klassiska Azure-portalen och tar en stund tooactivate.</span><span class="sxs-lookup"><span data-stu-id="b4a32-172">hello namespace you created will then appear in hello Azure classic portal and takes a moment tooactivate.</span></span> <span data-ttu-id="b4a32-173">Vänta tills hello status är **Active** innan du fortsätter med hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="b4a32-173">Wait until hello status is **Active** before continuing with hello next step.</span></span>

## <a name="obtain-hello-default-management-credentials-for-hello-namespace"></a><span data-ttu-id="b4a32-174">Hämta hello standard autentiseringsuppgifter för hantering hello namnområdet</span><span class="sxs-lookup"><span data-stu-id="b4a32-174">Obtain hello Default Management Credentials for hello namespace</span></span>
<span data-ttu-id="b4a32-175">I ordning hanteringsåtgärder tooperform, till exempel skapa en kö på hello nya namnområdet, behöver du tooobtain hello hantering av autentiseringsuppgifter för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-175">In order tooperform management operations, such as creating a queue, on hello new namespace, you need tooobtain hello management credentials for the namespace.</span></span>

1. <span data-ttu-id="b4a32-176">Hello vänstra navigeringsfönstret, klicka på hello **Service Bus** noder för att visa hello listan över tillgängliga namnområden.</span><span class="sxs-lookup"><span data-stu-id="b4a32-176">In hello left navigation pane, click hello **Service Bus** node to display hello list of available namespaces.</span></span>
   <span data-ttu-id="b4a32-177">![Tillgängliga namnområden skärmbild][avail_namespaces]</span><span class="sxs-lookup"><span data-stu-id="b4a32-177">![Available Namespaces screenshot][avail_namespaces]</span></span>
2. <span data-ttu-id="b4a32-178">Välj hello-namnområde som du nyss skapat från hello listan som visas.</span><span class="sxs-lookup"><span data-stu-id="b4a32-178">Select hello namespace you just created from hello list shown.</span></span>
   <span data-ttu-id="b4a32-179">![Namespace-listan skärmbild][namespace_list]</span><span class="sxs-lookup"><span data-stu-id="b4a32-179">![Namespace List screenshot][namespace_list]</span></span>
3. <span data-ttu-id="b4a32-180">hello högra **egenskaper** rutan visar hello egenskaper för det nya namnområdet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-180">hello right-hand **Properties** pane lists hello properties for the new namespace.</span></span>
   <span data-ttu-id="b4a32-181">![Egenskaper för fönstret skärmbild][properties_pane]</span><span class="sxs-lookup"><span data-stu-id="b4a32-181">![Properties Pane screenshot][properties_pane]</span></span>
4. <span data-ttu-id="b4a32-182">Hej **standard nyckeln** är dolt.</span><span class="sxs-lookup"><span data-stu-id="b4a32-182">hello **Default Key** is hidden.</span></span> <span data-ttu-id="b4a32-183">Klicka på hello **visa** knappen toodisplay hello-identifiering.</span><span class="sxs-lookup"><span data-stu-id="b4a32-183">Click hello **View** button toodisplay hello security credentials.</span></span>
   <span data-ttu-id="b4a32-184">![Skärmbild som standard nyckel][default_key]</span><span class="sxs-lookup"><span data-stu-id="b4a32-184">![Default Key screenshot][default_key]</span></span>
5. <span data-ttu-id="b4a32-185">Anteckna hello **standard utfärdaren** och hello **standard nyckeln** som du vill använda den här informationen nedan tooperform åtgärder med namnområdet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-185">Make a note of hello **Default Issuer** and hello **Default Key** as you will use this information below tooperform operations with the namespace.</span></span>

## <a name="how-toocreate-a-java-application-that-performs-a-compute-intensive-task"></a><span data-ttu-id="b4a32-186">Hur toocreate ett Java-program som utför en krävande uppgift</span><span class="sxs-lookup"><span data-stu-id="b4a32-186">How toocreate a Java application that performs a compute-intensive task</span></span>
1. <span data-ttu-id="b4a32-187">På utvecklingsdatorn (som inte har toobe hello virtuell dator som du skapade), hämta hello [Azure SDK för Java](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="b4a32-187">On your development machine (which does not have toobe hello virtual machine that you created), download hello [Azure SDK for Java](https://azure.microsoft.com/develop/java/).</span></span>
2. <span data-ttu-id="b4a32-188">Skapa ett Java-konsolprogram med hello exempelkod hello slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-188">Create a Java console application using hello example code at hello end of this section.</span></span> <span data-ttu-id="b4a32-189">I den här kursen använder vi **TSPSolver.java** som hello Java-filnamn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-189">In this tutorial, we'll use **TSPSolver.java** as hello Java file name.</span></span> <span data-ttu-id="b4a32-190">Ändra hello **din\_service\_bus\_namnområde**, **din\_service\_bus\_ägare**, och **din\_service\_bus\_nyckeln** platshållare toouse din service bus **namnområde**, **standard utfärdaren** och  **Standard nyckeln** värdena respektive.</span><span class="sxs-lookup"><span data-stu-id="b4a32-190">Modify hello **your\_service\_bus\_namespace**, **your\_service\_bus\_owner**, and **your\_service\_bus\_key** placeholders toouse your service bus **namespace**, **Default Issuer** and **Default Key** values, respectively.</span></span>
3. <span data-ttu-id="b4a32-191">Efter kodning krävs export hello programmet tooa körbara Java Arkiv (JAR) och paketet hello bibliotek i hello genereras JAR.</span><span class="sxs-lookup"><span data-stu-id="b4a32-191">After coding, export hello application tooa runnable Java archive (JAR), and package hello required libraries into hello generated JAR.</span></span> <span data-ttu-id="b4a32-192">I den här kursen använder vi **TSPSolver.jar** som hello genereras JAR namn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-192">In this tutorial, we'll use **TSPSolver.jar** as hello generated JAR name.</span></span>

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often tooprovide an update toohello console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as hello default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing toooccur other than creating hello queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing toooccur other than deleting hello queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume hello value passed in is hello number of cities toosolve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-toocreate-a-java-application-that-monitors-hello-progress-of-hello-compute-intensive-task"></a><span data-ttu-id="b4a32-193">Hur toocreate ett Java-program som övervakar hello status för hello beräkningsintensiva aktivitet</span><span class="sxs-lookup"><span data-stu-id="b4a32-193">How toocreate a Java application that monitors hello progress of hello compute-intensive task</span></span>
1. <span data-ttu-id="b4a32-194">Skapa en Java-konsoltillämpning med hello exempelkod hello slutet av det här avsnittet på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-194">On your development machine, create a Java console application using hello example code at hello end of this section.</span></span> <span data-ttu-id="b4a32-195">I den här kursen använder vi **TSPClient.java** som hello Java-filnamn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-195">In this tutorial, we'll use **TSPClient.java** as hello Java file name.</span></span> <span data-ttu-id="b4a32-196">Som tidigare kan du ändra hello **din\_service\_bus\_namnområde**, **din\_service\_bus\_ägare**, och **din\_service\_bus\_nyckeln** platshållare toouse din service bus **namnområde**, **standard utfärdaren**och **standard nyckeln** värdena respektive.</span><span class="sxs-lookup"><span data-stu-id="b4a32-196">As shown earlier, modify hello **your\_service\_bus\_namespace**, **your\_service\_bus\_owner**, and **your\_service\_bus\_key** placeholders toouse your service bus **namespace**, **Default Issuer** and **Default Key** values, respectively.</span></span>
2. <span data-ttu-id="b4a32-197">Exportera hello programmet tooa körbara JAR och paketet hello krävs bibliotek i hello genereras JAR.</span><span class="sxs-lookup"><span data-stu-id="b4a32-197">Export hello application tooa runnable JAR, and package hello required libraries into hello generated JAR.</span></span> <span data-ttu-id="b4a32-198">I den här kursen använder vi **TSPClient.jar** som hello genereras JAR namn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-198">In this tutorial, we'll use **TSPClient.jar** as hello generated JAR name.</span></span>

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as hello default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display hello queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing toooccur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // hello queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-toorun-hello-java-applications"></a><span data-ttu-id="b4a32-199">Hur toorun hello Java-program</span><span class="sxs-lookup"><span data-stu-id="b4a32-199">How toorun hello Java applications</span></span>
<span data-ttu-id="b4a32-200">Kör hello beräkningsintensiva programmet hello första toocreate hello kön och sedan toosolve reser Saleseman Problem som lägger till hello aktuella bästa vägen toohello service bus-kö.</span><span class="sxs-lookup"><span data-stu-id="b4a32-200">Run hello compute-intensive application, first toocreate hello queue, then toosolve hello Traveling Saleseman Problem, which will add hello current best route toohello service bus queue.</span></span> <span data-ttu-id="b4a32-201">När hello beräkningsintensiva program är igång (eller senare), kör hello klienten toodisplay resultat från hello service bus-kö.</span><span class="sxs-lookup"><span data-stu-id="b4a32-201">While hello compute-intensive application is running (or afterwards), run hello client toodisplay results from hello service bus queue.</span></span>

### <a name="toorun-hello-compute-intensive-application"></a><span data-ttu-id="b4a32-202">toorun hello beräkningsintensiva program</span><span class="sxs-lookup"><span data-stu-id="b4a32-202">toorun hello compute-intensive application</span></span>
1. <span data-ttu-id="b4a32-203">Logga in tooyour virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b4a32-203">Log on tooyour virtual machine.</span></span>
2. <span data-ttu-id="b4a32-204">Skapa en mapp där du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-204">Create a folder where you will run your application.</span></span> <span data-ttu-id="b4a32-205">Till exempel **c:\TSP**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-205">For example, **c:\TSP**.</span></span>
3. <span data-ttu-id="b4a32-206">Kopiera **TSPSolver.jar** för**c:\TSP**,</span><span class="sxs-lookup"><span data-stu-id="b4a32-206">Copy **TSPSolver.jar** too**c:\TSP**,</span></span>
4. <span data-ttu-id="b4a32-207">Skapa en fil med namnet **c:\TSP\cities.txt** med hello efter innehållet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-207">Create a file named **c:\TSP\cities.txt** with hello following contents.</span></span>
   
        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33
5. <span data-ttu-id="b4a32-208">Ändra kataloger tooc:\TSP vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="b4a32-208">At a command prompt, change directories tooc:\TSP.</span></span>
6. <span data-ttu-id="b4a32-209">Kontrollera hello JRE bin-mappen är i hello PATH-miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="b4a32-209">Ensure hello JRE's bin folder is in hello PATH environment variable.</span></span>
7. <span data-ttu-id="b4a32-210">Du behöver toocreate hello service bus-kö innan du kör hello Telefontjänstprovider solver permutationer.</span><span class="sxs-lookup"><span data-stu-id="b4a32-210">You'll need toocreate hello service bus queue before you run hello TSP solver permutations.</span></span> <span data-ttu-id="b4a32-211">Kör följande kommando toocreate hello service bus-kö hello.</span><span class="sxs-lookup"><span data-stu-id="b4a32-211">Run hello following command toocreate hello service bus queue.</span></span>
   
        java -jar TSPSolver.jar createqueue
8. <span data-ttu-id="b4a32-212">Nu när hello kön har skapats kan köra du hello Telefontjänstprovider solver permutationer.</span><span class="sxs-lookup"><span data-stu-id="b4a32-212">Now that hello queue is created, you can run hello TSP solver permutations.</span></span> <span data-ttu-id="b4a32-213">Till exempel köra följande kommando toorun hello solver för 8 städer hello.</span><span class="sxs-lookup"><span data-stu-id="b4a32-213">For example, run hello following command toorun hello solver for 8 cities.</span></span>
   
        java -jar TSPSolver.jar 8
   
   <span data-ttu-id="b4a32-214">Om du inte anger ett tal, körs efter 10 städer.</span><span class="sxs-lookup"><span data-stu-id="b4a32-214">If you don't specify a number, it will run for 10 cities.</span></span> <span data-ttu-id="b4a32-215">Eftersom hello solver hittar aktuella kortaste vägar, läggs dem toohello kön.</span><span class="sxs-lookup"><span data-stu-id="b4a32-215">As hello solver finds current shortest routes, it will add them toohello queue.</span></span>

> [!NOTE]
> <span data-ttu-id="b4a32-216">hello större hello tal som du anger, hello längre hello solver körs.</span><span class="sxs-lookup"><span data-stu-id="b4a32-216">hello larger hello number that you specify, hello longer hello solver will run.</span></span> <span data-ttu-id="b4a32-217">Till exempel köra för 14 orter kan ta flera minuter och köras för 15 orter kan ta flera timmar.</span><span class="sxs-lookup"><span data-stu-id="b4a32-217">For example, running for 14 cities could take several minutes, and running for 15 cities could take several hours.</span></span> <span data-ttu-id="b4a32-218">Öka too16 eller flera orter resultera i dagar för körning (slutligen veckor, månader och år).</span><span class="sxs-lookup"><span data-stu-id="b4a32-218">Increasing too16 or more cities could result in days of runtime (eventually weeks, months, and years).</span></span> <span data-ttu-id="b4a32-219">Detta är på grund av toohello snabb ökning hello antalet permutationer utvärderas skiljde hello som hello antalet orter ökar.</span><span class="sxs-lookup"><span data-stu-id="b4a32-219">This is due toohello rapid increase in hello number of permutations evaluated by hello solver as hello number of cities increases.</span></span>
> 
> 

### <a name="how-toorun-hello-monitoring-client-application"></a><span data-ttu-id="b4a32-220">Hur toorun hello övervakning klientprogrammet</span><span class="sxs-lookup"><span data-stu-id="b4a32-220">How toorun hello monitoring client application</span></span>
1. <span data-ttu-id="b4a32-221">Logga in tooyour datorn där du kör hello-klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-221">Log on tooyour machine where you will run hello client application.</span></span> <span data-ttu-id="b4a32-222">Detta behövs inte toobe hello samma dator som kör hello **TSPSolver** program, även om det kan vara.</span><span class="sxs-lookup"><span data-stu-id="b4a32-222">This does not need toobe hello same machine running hello **TSPSolver** application, although it can be.</span></span>
2. <span data-ttu-id="b4a32-223">Skapa en mapp där du kör programmet.</span><span class="sxs-lookup"><span data-stu-id="b4a32-223">Create a folder where you will run your application.</span></span> <span data-ttu-id="b4a32-224">Till exempel **c:\TSP**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-224">For example, **c:\TSP**.</span></span>
3. <span data-ttu-id="b4a32-225">Kopiera **TSPClient.jar** för**c:\TSP**,</span><span class="sxs-lookup"><span data-stu-id="b4a32-225">Copy **TSPClient.jar** too**c:\TSP**,</span></span>
4. <span data-ttu-id="b4a32-226">Kontrollera hello JRE bin-mappen är i hello PATH-miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="b4a32-226">Ensure hello JRE's bin folder is in hello PATH environment variable.</span></span>
5. <span data-ttu-id="b4a32-227">Ändra kataloger tooc:\TSP vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="b4a32-227">At a command prompt, change directories tooc:\TSP.</span></span>
6. <span data-ttu-id="b4a32-228">Kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="b4a32-228">Run hello following command.</span></span>
   
        java -jar TSPClient.jar
   
    <span data-ttu-id="b4a32-229">Du kan också ange hello antal minuter toosleep between kontrollerar hello kö genom att passera i ett kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="b4a32-229">Optionally, specify hello number of minutes toosleep in between checking hello queue, by passing in a command-line argument.</span></span> <span data-ttu-id="b4a32-230">hello strömsparläge standardperioden för att kontrollera hello kön är 3 minuter, som används om inget kommandoradsargument som skickas för**TSPClient**.</span><span class="sxs-lookup"><span data-stu-id="b4a32-230">hello default sleep period for checking hello queue is 3 minutes, which is used if no command-line argument is passed too**TSPClient**.</span></span> <span data-ttu-id="b4a32-231">Om du vill toouse ett annat värde för hello strömsparläge intervall, till exempel köra en minut hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="b4a32-231">If you want toouse a different value for hello sleep interval, for example, one minute, run hello following command.</span></span>
   
        java -jar TSPClient.jar 1
   
    <span data-ttu-id="b4a32-232">hello-klienten körs förrän den ser ett kömeddelande på ”Slutför”.</span><span class="sxs-lookup"><span data-stu-id="b4a32-232">hello client will run until it sees a queue message of "Complete".</span></span> <span data-ttu-id="b4a32-233">Observera att om du kör flera förekomster av hello solver utan att köra hello klienten kanske måste toorun hello klienten flera gånger toocompletely tom hello kön.</span><span class="sxs-lookup"><span data-stu-id="b4a32-233">Note that if you run multiple occurrences of hello solver without running hello client, you may need toorun hello client multiple times toocompletely empty hello queue.</span></span> <span data-ttu-id="b4a32-234">Alternativt kan du ta bort hello kö och sedan skapa den igen.</span><span class="sxs-lookup"><span data-stu-id="b4a32-234">Alternatively, you can delete hello queue and then create it again.</span></span> <span data-ttu-id="b4a32-235">toodelete hello kö, kör följande hello **TSPSolver** (inte **TSPClient**) kommando.</span><span class="sxs-lookup"><span data-stu-id="b4a32-235">toodelete hello queue, run hello following **TSPSolver** (not **TSPClient**)  command.</span></span>
   
        java -jar TSPSolver.jar deletequeue
   
    <span data-ttu-id="b4a32-236">hello solver kommer att köras tills det är färdigt undersöka alla vägar.</span><span class="sxs-lookup"><span data-stu-id="b4a32-236">hello solver will run until it finishes examining all routes.</span></span>

## <a name="how-toostop-hello-java-applications"></a><span data-ttu-id="b4a32-237">Hur toostop hello Java-program</span><span class="sxs-lookup"><span data-stu-id="b4a32-237">How toostop hello Java applications</span></span>
<span data-ttu-id="b4a32-238">För både hello solver och klientprogram kan du trycka på **Ctrl + C** tooexit om du vill tooend tidigare toonormal slutförande.</span><span class="sxs-lookup"><span data-stu-id="b4a32-238">For both hello solver and client applications, you can press **Ctrl+C** tooexit if you want tooend prior toonormal completion.</span></span>

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
