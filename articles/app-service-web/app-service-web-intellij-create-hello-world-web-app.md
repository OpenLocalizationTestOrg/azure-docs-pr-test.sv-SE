---
title: "aaaCreate en grundläggande Azure-webbapp i IntelliJ | Microsoft Docs"
description: "Den här kursen visar hur toouse hello Azure Toolkit för IntelliJ toocreate Hello World Web App för Azure."
services: app-service\web
documentationcenter: java
author: selvasingh
manager: erikre
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4667497213cac3ddf754d164e614c809f338cce5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a><span data-ttu-id="89aa1-103">Skapa en grundläggande Azure webbapp i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="89aa1-103">Create a basic Azure web app in IntelliJ</span></span>
<span data-ttu-id="89aa1-104">Den här kursen visar hur toocreate och distribuera en grundläggande Hello World programmet tooAzure som ett webbprogram med hjälp av hello [Azure Toolkit för IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="89aa1-104">This tutorial shows how toocreate and deploy a basic Hello World application tooAzure as a Web App by using hello [Azure Toolkit for IntelliJ].</span></span> <span data-ttu-id="89aa1-105">En grundläggande JSP-exempel visas för enkelhetens skull, men liknande steg skulle vara lämplig för ett Java-servlet som Azure-distribution är.</span><span class="sxs-lookup"><span data-stu-id="89aa1-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="89aa1-106">När du har slutfört den här självstudiekursen ser liknande toohello följande bild visas i en webbläsare i ditt program:</span><span class="sxs-lookup"><span data-stu-id="89aa1-106">When you have completed this tutorial, your application will look similar toohello following illustration when you view it in a web browser:</span></span>

![Exempelsida][01]

## <a name="prerequisites"></a><span data-ttu-id="89aa1-108">Krav</span><span class="sxs-lookup"><span data-stu-id="89aa1-108">Prerequisites</span></span>
* <span data-ttu-id="89aa1-109">En Java Developer Kit (JDK), v 1,8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="89aa1-109">A Java Developer Kit (JDK), v 1.8 or later.</span></span>
* <span data-ttu-id="89aa1-110">IntelliJ IDEA Ultimate Edition.</span><span class="sxs-lookup"><span data-stu-id="89aa1-110">IntelliJ IDEA Ultimate Edition.</span></span> <span data-ttu-id="89aa1-111">Detta kan hämtas från <https://www.jetbrains.com/idea/download/index.html>.</span><span class="sxs-lookup"><span data-stu-id="89aa1-111">This can be downloaded from <https://www.jetbrains.com/idea/download/index.html>.</span></span>
* <span data-ttu-id="89aa1-112">En distributionsplats i en Java-baserad webbserver eller programserver som [Apache Tomcat] eller [Jetty].</span><span class="sxs-lookup"><span data-stu-id="89aa1-112">A distribution of a Java-based web server or application server, such as [Apache Tomcat] or [Jetty].</span></span>
* <span data-ttu-id="89aa1-113">En Azure-prenumeration som kan erhållas från <https://azure.microsoft.com/free/> eller <http://azure.microsoft.com/pricing/purchase-options/>.</span><span class="sxs-lookup"><span data-stu-id="89aa1-113">An Azure subscription, which can be acquired from <https://azure.microsoft.com/free/> or <http://azure.microsoft.com/pricing/purchase-options/>.</span></span>
* <span data-ttu-id="89aa1-114">Hej [Azure Toolkit för IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="89aa1-114">hello [Azure Toolkit for IntelliJ].</span></span> <span data-ttu-id="89aa1-115">Information om hur du installerar hello Azure Toolkit finns [installerar hello Azure Toolkit för IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="89aa1-115">For information about installing hello Azure Toolkit, see [Installing hello Azure Toolkit for IntelliJ].</span></span>

## <a name="toocreate-a-hello-world-application"></a><span data-ttu-id="89aa1-116">toocreate ett Hello World-program</span><span class="sxs-lookup"><span data-stu-id="89aa1-116">toocreate a Hello World application</span></span>
<span data-ttu-id="89aa1-117">Först ska vi börja med att skapa ett Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="89aa1-117">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="89aa1-118">Starta IntelliJ och klicka på hello **filen** -menyn och klicka sedan på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-118">Start IntelliJ and click hello **File** menu, then click **New**, and then click **Project**.</span></span>
   
    ![Filen nytt projekt][02]
2. <span data-ttu-id="89aa1-120">Hello nytt projekt i dialogrutan Välj **Java**, sedan **webbprogram**, och klicka sedan på **ny** tooadd en projekt-SDK.</span><span class="sxs-lookup"><span data-stu-id="89aa1-120">In hello New Project dialog box, select **Java**, then **Web Application**, and then click **New** tooadd a Project SDK.</span></span>
   
    ![Dialogrutan Nytt projekt][03a]
   
3. <span data-ttu-id="89aa1-122">I hello Välj arbetskatalog för JDK dialogrutan Välj hello mappen där din JDK är installerat och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-122">In hello Select Home Directory for JDK dialog box, select hello folder where your JDK is installed, and then click **OK**.</span></span> <span data-ttu-id="89aa1-123">Klicka på **nästa** i hello nytt projekt dialogrutan rutan toocontinue.</span><span class="sxs-lookup"><span data-stu-id="89aa1-123">Click **Next** in hello New Project dialog box toocontinue.</span></span>
   
    ![Ange arbetskatalog för JDK][03b]
4. <span data-ttu-id="89aa1-125">Namn för den här kursen är hello projektet **Java-webb-App-på-Azure**, och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-125">For purposes of this tutorial, name hello project **Java-Web-App-On-Azure**, and then click **Finish**.</span></span>
   
    ![Dialogrutan Nytt projekt][04]
5. <span data-ttu-id="89aa1-127">I vyn Projektutforskaren Intellijs Expandera **Java-webb-App-på-Azure**, expandera **web**, och dubbelklicka sedan på **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-127">Within IntelliJ's Project Explorer view, expand **Java-Web-App-On-Azure**, then expand **web**, and then double-click **index.jsp**.</span></span>
   
    ![Öppna indexsidan][05c]
6. <span data-ttu-id="89aa1-129">När index.jsp-filen öppnas i IntelliJ, lägga till texten toodynamically visas på **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="89aa1-129">When your index.jsp file opens in IntelliJ, add in text toodynamically display **Hello World!**</span></span> <span data-ttu-id="89aa1-130">inom hello befintliga `<body>` element.</span><span class="sxs-lookup"><span data-stu-id="89aa1-130">within hello existing `<body>` element.</span></span> <span data-ttu-id="89aa1-131">Uppdaterade `<body>` innehåll bör likna följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="89aa1-131">Your updated `<body>` content should resemble hello following example:</span></span>
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
7. <span data-ttu-id="89aa1-132">Spara index.jsp.</span><span class="sxs-lookup"><span data-stu-id="89aa1-132">Save index.jsp.</span></span>

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a><span data-ttu-id="89aa1-133">toodeploy dina program tooan Azure Web App-behållare</span><span class="sxs-lookup"><span data-stu-id="89aa1-133">toodeploy your application tooan Azure Web App Container</span></span>
<span data-ttu-id="89aa1-134">Det finns flera olika sätt som du kan distribuera en Java web application tooAzure.</span><span class="sxs-lookup"><span data-stu-id="89aa1-134">There are several ways by which you can deploy a Java web application tooAzure.</span></span> <span data-ttu-id="89aa1-135">Den här självstudiekursen beskriver en hello enklaste: ditt program kommer att distribuerade tooan Azure Web App-behållare – krävs inget speciellt projekttyp eller andra verktyg.</span><span class="sxs-lookup"><span data-stu-id="89aa1-135">This tutorial describes one of hello simplest: your application will be deployed tooan Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="89aa1-136">hello JDK hello web behållaren programvara och kommer att tillhandahållas för dig av Azure, så det finns inget behov av tooupload egna; allt du behöver är Java-Webbappen.</span><span class="sxs-lookup"><span data-stu-id="89aa1-136">hello JDK and hello web container software will be provided for you by Azure, so there is no need tooupload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="89aa1-137">Därför tar hello publiceringsprocessen för tillämpningsprogrammet sekunder, inte minuter.</span><span class="sxs-lookup"><span data-stu-id="89aa1-137">As a result, hello publishing process for your application will take seconds, not minutes.</span></span>

<span data-ttu-id="89aa1-138">Innan du publicerar ditt program måste du först tooconfigure modulinställningarna i.</span><span class="sxs-lookup"><span data-stu-id="89aa1-138">Before you publish your application, you first need tooconfigure your module settings.</span></span> <span data-ttu-id="89aa1-139">toodo så Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="89aa1-139">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="89aa1-140">Högerklicka i Intellijs Projektutforskaren hello **Java-webb-App-på-Azure** projekt.</span><span class="sxs-lookup"><span data-stu-id="89aa1-140">In IntelliJ's Project Explorer, right-click hello **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="89aa1-141">På snabbmenyn för hello visas **öppna Modulinställningar**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-141">When hello context menu appears, click **Open Module Settings**.</span></span>

    ![Öppna modulinställningarna för][05a]
2. <span data-ttu-id="89aa1-143">När dialogrutan för hello projektstruktur visas:</span><span class="sxs-lookup"><span data-stu-id="89aa1-143">When hello Project Structure dialog box appears:</span></span>

   <span data-ttu-id="89aa1-144">a.</span><span class="sxs-lookup"><span data-stu-id="89aa1-144">a.</span></span> <span data-ttu-id="89aa1-145">Klicka på **artefakter** i hello lista över **Projektinställningar**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-145">Click **Artifacts** in hello list of **Project Settings**.</span></span>
   <span data-ttu-id="89aa1-146">b.</span><span class="sxs-lookup"><span data-stu-id="89aa1-146">b.</span></span> <span data-ttu-id="89aa1-147">Ändra hello artefakt namn i hello **namn** så att den inte innehåller blanksteg eller specialtecken; detta är nödvändigt eftersom hello namn kommer att användas i hello identifierare URI (Uniform Resource).</span><span class="sxs-lookup"><span data-stu-id="89aa1-147">Change hello artifact name in hello **Name** box so that it doesn't contain whitespace or special characters; this is necessary since hello name will be used in hello Uniform Resource Identifier (URI).</span></span>
   <span data-ttu-id="89aa1-148">c.</span><span class="sxs-lookup"><span data-stu-id="89aa1-148">c.</span></span> <span data-ttu-id="89aa1-149">Ändra hello **typen** för**webbprogrammet: Arkiv**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-149">Change hello **Type** too**Web Application: Archive**.</span></span>
   <span data-ttu-id="89aa1-150">d.</span><span class="sxs-lookup"><span data-stu-id="89aa1-150">d.</span></span> <span data-ttu-id="89aa1-151">Klicka på **OK** tooclose hello projektstrukturen i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="89aa1-151">Click **OK** tooclose hello Project Structure dialog box.</span></span>

    ![Öppna modulinställningarna för][05b]

<span data-ttu-id="89aa1-153">När du har konfigurerat modulinställningarna för, kan du publicera program-tooAzure med hjälp av hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="89aa1-153">When you have configured your module settings, you can publish your application tooAzure by using hello following steps:</span></span>

1. <span data-ttu-id="89aa1-154">Högerklicka i Intellijs Projektutforskaren hello **Java-webb-App-på-Azure** projekt.</span><span class="sxs-lookup"><span data-stu-id="89aa1-154">In IntelliJ's Project Explorer, right-click hello **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="89aa1-155">Välj när hello snabbmenyn visas **Azure**, och klicka sedan på **Publicera som Azure Web App...**</span><span class="sxs-lookup"><span data-stu-id="89aa1-155">When hello context menu appears, select **Azure**, and then click **Publish as Azure Web App...**</span></span>
   
    ![Azure publicera snabbmenyn][06]
2. <span data-ttu-id="89aa1-157">Om du inte redan loggat in Azure från IntelliJ kommer du att tillfrågas toosign i ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="89aa1-157">If you have not already signed into Azure from IntelliJ, you will be prompted toosign into your Azure account.</span></span> <span data-ttu-id="89aa1-158">(Om du har flera Azure-konton kan vissa hello frågor under hello inloggning i processen för kanske visas mer än en gång, även om de visas toobe hello samma.</span><span class="sxs-lookup"><span data-stu-id="89aa1-158">(If you have multiple Azure accounts, some of hello prompts during hello sign in process may be shown more than once, even if they appear toobe hello same.</span></span> <span data-ttu-id="89aa1-159">När detta inträffar kan fortsätta toofollow hello logga i anvisningarna.)</span><span class="sxs-lookup"><span data-stu-id="89aa1-159">When this happens, continue toofollow hello sign in instructions.)</span></span>
   
    ![Azure logganalys i dialogrutan][07]
3. <span data-ttu-id="89aa1-161">När du har loggat in ditt Azure-konto, hello **hantera prenumerationer** dialogrutan visas en lista över prenumerationer som är associerade med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="89aa1-161">After you have successfully signed into your Azure account, hello **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="89aa1-162">(Om det finns flera prenumerationerna i listan och du vill toowork med bara en delmängd av dem, du kan du avmarkera hello prenumerationer som du inte vill toouse.) När du har valt dina prenumerationer, klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-162">(If there are multiple subscriptions listed and you want toowork with only a specific subset of them, you may optionally uncheck hello subscriptions you don't want toouse.) When you have selected your subscriptions, click **Close**.</span></span>
   
    ![Hantera prenumerationer][08]
4. <span data-ttu-id="89aa1-164">När hello **distribuera tooAzure Web App behållaren** i dialogrutan visas alla webbprogram-behållare som du tidigare har skapat; om du inte har skapat några behållare, hello listan är tom.</span><span class="sxs-lookup"><span data-stu-id="89aa1-164">When hello **Deploy tooAzure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, hello list will be empty.</span></span>
   
    ![App-behållare][09]
5. <span data-ttu-id="89aa1-166">Om du inte har skapat ett Azure Web App-behållare innan, eller om du vill att toopublish dina program tooa ny behållare, använda hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="89aa1-166">If you have not created an Azure Web App Container before, or if you would like toopublish your application tooa new container, use hello following steps.</span></span> <span data-ttu-id="89aa1-167">Annars markerar du en befintlig Web Appbehållare och hoppa över toostep 6 nedan.</span><span class="sxs-lookup"><span data-stu-id="89aa1-167">Otherwise, select an existing Web App Container and skip toostep 6 below.</span></span>
   
   1. <span data-ttu-id="89aa1-168">Klicka på**+**</span><span class="sxs-lookup"><span data-stu-id="89aa1-168">Click **+**</span></span>
      
       ![Lägg till Appbehållare][10]
   2. <span data-ttu-id="89aa1-170">Hej **ny Web Appbehållare** dialogrutan kommer att visas som kommer att vara används för hello bredvid flera steg.</span><span class="sxs-lookup"><span data-stu-id="89aa1-170">hello **New Web App Container** dialog box will be displayed, which will be used for hello next several steps.</span></span>
      
       ![Ny Appbehållare][11a]
   3. <span data-ttu-id="89aa1-172">Ange en **DNS-etikett** för App Webbehållaren; kommer att skapas hello löv DNS-etikett hello värd-URL för ditt webbprogram i Azure.</span><span class="sxs-lookup"><span data-stu-id="89aa1-172">Enter a **DNS Label** for your Web App Container; this will form hello leaf DNS label of hello host URL for your web application in Azure.</span></span> <span data-ttu-id="89aa1-173">Observera att hello-namnet måste vara tillgängliga och uppfylla krav för tooAzure Web App.</span><span class="sxs-lookup"><span data-stu-id="89aa1-173">Note that hello name must be available and conform tooAzure Web App naming requirements.</span></span>
   4. <span data-ttu-id="89aa1-174">I hello **Webbehållaren** nedrullningsbara menyn, Välj hello rätt programvara för ditt program.</span><span class="sxs-lookup"><span data-stu-id="89aa1-174">In hello **Web Container** drop-down menu, select hello appropriate software for your application.</span></span>
      
       <span data-ttu-id="89aa1-175">För närvarande kan du välja mellan Tomcat 8, 7 Tomcat eller Jetty 9.</span><span class="sxs-lookup"><span data-stu-id="89aa1-175">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="89aa1-176">En senaste fördelning av hello valt programvara kommer att tillhandahållas av Azure och körs på en senaste fördelning av JDK 8 skapats av Oracle och från Azure.</span><span class="sxs-lookup"><span data-stu-id="89aa1-176">A recent distribution of hello selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>
   5. <span data-ttu-id="89aa1-177">I hello **prenumeration** nedrullningsbara menyn, Välj hello-prenumeration som du vill toouse för den här distributionen.</span><span class="sxs-lookup"><span data-stu-id="89aa1-177">In hello **Subscription** drop-down menu, select hello subscription you want toouse for this deployment.</span></span>
   6. <span data-ttu-id="89aa1-178">I hello **resursgruppen** nedrullningsbara menyn, Välj hello resursgrupp som du vill använda tooassociate ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="89aa1-178">In hello **Resource Group** drop-down menu, select hello Resource Group with which you want tooassociate your Web App.</span></span> <span data-ttu-id="89aa1-179">(Azure resursgrupper kan du toogroup relaterade resurser tillsammans så att till exempel kan du ta bort tillsammans.)</span><span class="sxs-lookup"><span data-stu-id="89aa1-179">(Azure Resource Groups allow you toogroup related resources together so that, for example, they can be deleted together.)</span></span>
      
       <span data-ttu-id="89aa1-180">Du kan välja en befintlig resursgrupp (om du har några) och hoppa över toostep g nedan eller Använd hello följande steg toocreate en ny resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="89aa1-180">You can select an existing Resource Group (if you have any) and skip toostep g below, or use hello following steps toocreate a new Resource Group:</span></span>
      
      * <span data-ttu-id="89aa1-181">Välj  **&lt; &lt; Skapa ny resursgrupp &gt; &gt;**  i hello **resursgruppen** nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="89aa1-181">Select **&lt;&lt; Create new Resource Group &gt;&gt;** in hello **Resource Group** drop-down menu.</span></span>
      * <span data-ttu-id="89aa1-182">Hej **ny resursgrupp** dialogrutan visas:</span><span class="sxs-lookup"><span data-stu-id="89aa1-182">hello **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Ny resursgrupp][12]
      * <span data-ttu-id="89aa1-184">I hello hello **namn** textruta, ange ett namn för din nya resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="89aa1-184">In hello hello **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="89aa1-185">I hello hello **Region** nedrullningsbara menyn, Välj hello lämplig Azure Datacenter plats för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="89aa1-185">In hello hello **Region** drop-down menu, select hello appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="89aa1-186">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-186">Click **OK**.</span></span>
   7. <span data-ttu-id="89aa1-187">Hej **Apptjänstplan** listrutan visar hello apptjänstplaner som är associerade med hello resursgrupp som du har valt.</span><span class="sxs-lookup"><span data-stu-id="89aa1-187">hello **App Service Plan** drop-down menu lists hello app service plans that are associated with hello Resource Group that you selected.</span></span> <span data-ttu-id="89aa1-188">(En Apptjänstplan anger information, till exempel hello platsen för ditt webbprogram, hello prisnivå och hello instansstorleken för beräkning.</span><span class="sxs-lookup"><span data-stu-id="89aa1-188">(An App Service Plan specifies information such as hello location of your Web App, hello pricing tier and hello compute instance size.</span></span> <span data-ttu-id="89aa1-189">En enda App Service-Plan kan användas för flera webbprogram, vilket är anledningen till att den hanteras separat från en viss distribution för webbprogrammet.)</span><span class="sxs-lookup"><span data-stu-id="89aa1-189">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       <span data-ttu-id="89aa1-190">Du kan välja en befintlig App Service-Plan (om du har några) och hoppa över toostep h nedan eller använda följande steg toocreate en ny App Service-Plan hello:</span><span class="sxs-lookup"><span data-stu-id="89aa1-190">You can select an existing App Service Plan (if you have any) and skip toostep h below, or use hello following steps toocreate a new App Service Plan:</span></span>
      
      * <span data-ttu-id="89aa1-191">Välj  **&lt; &lt; skapa nya App Service-Plan &gt; &gt;**  i hello **Apptjänstplan** nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="89aa1-191">Select **&lt;&lt; Create new App Service Plan &gt;&gt;** in hello **App Service Plan** drop-down menu.</span></span>
      * <span data-ttu-id="89aa1-192">Hej **nya App Service-Plan** dialogrutan visas:</span><span class="sxs-lookup"><span data-stu-id="89aa1-192">hello **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Ny Programtjänstplan][13]
      * <span data-ttu-id="89aa1-194">I hello hello **namn** textruta, ange ett namn för din nya App Service-Plan.</span><span class="sxs-lookup"><span data-stu-id="89aa1-194">In hello hello **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="89aa1-195">I hello hello **plats** nedrullningsbara menyn, Välj hello lämplig Azure Datacenter plats för hello-plan.</span><span class="sxs-lookup"><span data-stu-id="89aa1-195">In hello hello **Location** drop-down menu, select hello appropriate Azure data center location for hello plan.</span></span>
      * <span data-ttu-id="89aa1-196">I hello hello **prisnivån** nedrullningsbara menyn, Välj hello lämpliga priser för hello plan.</span><span class="sxs-lookup"><span data-stu-id="89aa1-196">In hello hello **Pricing Tier** drop-down menu, select hello appropriate pricing for hello plan.</span></span> <span data-ttu-id="89aa1-197">I testsyfte kan du välja **lediga**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-197">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="89aa1-198">I hello hello **Instansstorleken** nedrullningsbara menyn, Välj hello lämplig instans storlek för hello-plan.</span><span class="sxs-lookup"><span data-stu-id="89aa1-198">In hello hello **Instance Size** drop-down menu, select hello appropriate instance size for hello plan.</span></span> <span data-ttu-id="89aa1-199">I testsyfte kan du välja **små**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-199">For testing purposes you can choose **Small**.</span></span>
      * <span data-ttu-id="89aa1-200">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-200">Click **OK**.</span></span>
   8. <span data-ttu-id="89aa1-201">(Valfritt) Som standard ska en senaste fördelning av Java 8 automatiskt distribueras som din JVM enligt tooyour Azure web appbehållare.</span><span class="sxs-lookup"><span data-stu-id="89aa1-201">(Optional) By default, a recent distribution of Java 8 will be automatically deployed as your JVM by Azure tooyour web app container.</span></span> <span data-ttu-id="89aa1-202">Du kan dock välja en annan version och distribution av hello JVM.</span><span class="sxs-lookup"><span data-stu-id="89aa1-202">However, you can select a different version and distribution of hello JVM.</span></span> <span data-ttu-id="89aa1-203">toodo så Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="89aa1-203">toodo so, use hello following steps:</span></span>
      
      * <span data-ttu-id="89aa1-204">Klicka på hello **JDK** fliken i hello **ny Web Appbehållare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="89aa1-204">Click hello **JDK** tab in hello **New Web App Container** dialog box.</span></span>
      * <span data-ttu-id="89aa1-205">Du kan välja något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="89aa1-205">You can choose from one of hello following options:</span></span>
        
        * <span data-ttu-id="89aa1-206">Distribuera hello standard JDK som erbjuds av Azure</span><span class="sxs-lookup"><span data-stu-id="89aa1-206">Deploy hello default JDK which is offered by Azure</span></span>
        * <span data-ttu-id="89aa1-207">Distribuera en 3 part JDK från listrutan av ytterligare JDKs som är tillgängliga på Azure</span><span class="sxs-lookup"><span data-stu-id="89aa1-207">Deploy a 3rd party JDK from a drop-down list of additional JDKs which are available on Azure</span></span>
        * <span data-ttu-id="89aa1-208">Distribuera en anpassad JDK som måste paketeras som en ZIP-fil och antingen offentligt tillgängliga eller i Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="89aa1-208">Deploy a custom JDK, which must be packaged as a ZIP file and either publicly available or in your Azure storage account</span></span>
        
        ![Ny App behållaren JDK flik][11b]
   9. <span data-ttu-id="89aa1-210">När du har slutfört alla hello senare steg, bör hello ny Web Appbehållare dialogrutan likna följande illustration hello:</span><span class="sxs-lookup"><span data-stu-id="89aa1-210">Once you have completed all of hello above steps, hello New Web App Container dialog box should resemble hello following illustration:</span></span>
      
       ![Ny Appbehållare][14]
   10. <span data-ttu-id="89aa1-212">Klicka på **OK** toocomplete hello skapandet av din nya Webbapp-behållaren.</span><span class="sxs-lookup"><span data-stu-id="89aa1-212">Click **OK** toocomplete hello creation of your new Web App container.</span></span>
       
        <span data-ttu-id="89aa1-213">Vänta några sekunder hello lista över hello webbprogrammet behållare toobe uppdateras och nyskapade app webbehållaren ska nu vara markerade i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="89aa1-213">Wait a few seconds for hello list of hello Web App containers toobe refreshed, and your newly-created web app container should now be selected in hello list.</span></span>
6. <span data-ttu-id="89aa1-214">Du är nu redo toocomplete hello initiala distributionen av ditt webbprogram tooAzure; Klicka på **OK** toodeploy toohello din Java-programmet valt Web App behållare.</span><span class="sxs-lookup"><span data-stu-id="89aa1-214">You are now ready toocomplete hello initial deployment of your Web App tooAzure; click **OK** toodeploy your Java application toohello selected Web App container.</span></span> <span data-ttu-id="89aa1-215">Programmet kommer att distribueras som en underkatalog till hello programservern som standard.</span><span class="sxs-lookup"><span data-stu-id="89aa1-215">By default, your application will be deployed as a subdirectory of hello application server.</span></span> <span data-ttu-id="89aa1-216">Om du vill att den distribueras som hello rotprogram toobe, kontrollera hello **distribuera tooroot** kryssrutan innan du klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-216">If you want it toobe deployed as hello root application, check hello **Deploy tooroot** checkbox before clicking **OK**.</span></span>
   
    ![Distribuera tooAzure][15]
7. <span data-ttu-id="89aa1-218">Därefter bör du se hello **Azure-aktivitetsloggen** vy, som visar hello Distributionsstatus för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="89aa1-218">Next, you should see hello **Azure Activity Log** view, which will indicate hello deployment status of your Web App.</span></span>
   
    ![Förloppsindikator][16]
   
    <span data-ttu-id="89aa1-220">hello-processen för att distribuera webbprogram-tooAzure tar endast några sekunder toocomplete.</span><span class="sxs-lookup"><span data-stu-id="89aa1-220">hello process of deploying your Web App tooAzure should take only a few seconds toocomplete.</span></span> <span data-ttu-id="89aa1-221">När din programmet kan påbörjas kan du visa en länk med namnet **publicerade** i hello **Status** kolumn.</span><span class="sxs-lookup"><span data-stu-id="89aa1-221">When your application ready, you will see a link named **Published** in hello **Status** column.</span></span> <span data-ttu-id="89aa1-222">När du klickar på länken hello tar du tooyour distribueras Web App startsidan eller du kan använda hello steg i hello följande avsnitt toobrowse tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="89aa1-222">When you click hello link, it will take you tooyour deployed Web App's home page, or you can use hello steps in hello following section toobrowse tooyour web app.</span></span>

## <a name="browsing-tooyour-web-app-on-azure"></a><span data-ttu-id="89aa1-223">Bläddra tooyour Web App på Azure</span><span class="sxs-lookup"><span data-stu-id="89aa1-223">Browsing tooyour Web App on Azure</span></span>
<span data-ttu-id="89aa1-224">toobrowse tooyour Web App på Azure, kan du använda hello **Azure Explorer** vyn.</span><span class="sxs-lookup"><span data-stu-id="89aa1-224">toobrowse tooyour Web App on Azure, you can use hello **Azure Explorer** view.</span></span>

<span data-ttu-id="89aa1-225">Om hello **Azure Explorer** vyn inte är öppen, kan du öppna det genom att klicka på sedan **visa** -menyn i IntelliJ, klicka på **verktyget Windows**, och klicka sedan på  **Tjänsten Explorer**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-225">If hello **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="89aa1-226">Om du inte tidigare har loggat in uppmanas du toodo så.</span><span class="sxs-lookup"><span data-stu-id="89aa1-226">If you have not previously logged in, it will prompt you toodo so.</span></span>

<span data-ttu-id="89aa1-227">När hello **Azure Explorer** visas, Använd följer du dessa steg toobrowse tooyour Web App:</span><span class="sxs-lookup"><span data-stu-id="89aa1-227">When hello **Azure Explorer** view is displayed, use follow these steps toobrowse tooyour Web App:</span></span> 

1. <span data-ttu-id="89aa1-228">Expandera hello **Azure** nod.</span><span class="sxs-lookup"><span data-stu-id="89aa1-228">Expand hello **Azure** node.</span></span>
2. <span data-ttu-id="89aa1-229">Expandera hello **Web Apps** nod.</span><span class="sxs-lookup"><span data-stu-id="89aa1-229">Expand hello **Web Apps** node.</span></span> 
3. <span data-ttu-id="89aa1-230">Högerklicka på hello önskat webbprogram.</span><span class="sxs-lookup"><span data-stu-id="89aa1-230">Right-click hello desired Web App.</span></span>
4. <span data-ttu-id="89aa1-231">På snabbmenyn för hello visas **öppna i webbläsare**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-231">When hello context menu appears, click **Open in Browser**.</span></span>
   
    ![Bläddra Webbapp][17]

## <a name="updating-your-web-app"></a><span data-ttu-id="89aa1-233">Uppdatera din webbapp</span><span class="sxs-lookup"><span data-stu-id="89aa1-233">Updating your web app</span></span>
<span data-ttu-id="89aa1-234">Uppdatering av ett befintligt kör Azure Web App är en snabb och enkel process och har du två alternativ för att uppdatera:</span><span class="sxs-lookup"><span data-stu-id="89aa1-234">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="89aa1-235">Du kan uppdatera hello distribution av en befintlig Java-Webbapp.</span><span class="sxs-lookup"><span data-stu-id="89aa1-235">You can update hello deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="89aa1-236">Du kan publicera en ytterligare Java-program toohello Web Appbehållare.</span><span class="sxs-lookup"><span data-stu-id="89aa1-236">You can publish an additional Java application toohello same Web App Container.</span></span>

<span data-ttu-id="89aa1-237">I båda fallen hello processen är identiska och tar endast några sekunder:</span><span class="sxs-lookup"><span data-stu-id="89aa1-237">In either case, hello process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="89aa1-238">Högerklicka på hello Java-program du vill tooupdate eller lägga till tooan befintliga Web Appbehållare i hello IntelliJ Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="89aa1-238">In hello IntelliJ project explorer, right-click hello Java application you want tooupdate or add tooan existing Web App Container.</span></span>
2. <span data-ttu-id="89aa1-239">Välj när hello snabbmenyn visas **Azure** och sedan **Publicera som Azure Web App...**</span><span class="sxs-lookup"><span data-stu-id="89aa1-239">When hello context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="89aa1-240">Eftersom du har redan loggat in tidigare, visas en lista över dina befintliga Web App-behållare.</span><span class="sxs-lookup"><span data-stu-id="89aa1-240">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="89aa1-241">Välj hello en du vill toopublish eller publicera din Java-programmet tooand klickar **OK**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-241">Select hello one you want toopublish or re-publish your Java application tooand click **OK**.</span></span>

<span data-ttu-id="89aa1-242">Några sekunder senare hello **Azure-aktivitetsloggen** vyer visar uppdaterade distributionen som **publicerade** och du kommer att kunna tooverify uppdaterade programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="89aa1-242">A few seconds later, hello **Azure Activity Log** view will show your updated deployment as **Published** and you will be able tooverify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="89aa1-243">Starta, stoppa eller starta om en befintlig webbapp</span><span class="sxs-lookup"><span data-stu-id="89aa1-243">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="89aa1-244">toostart eller stoppa en befintlig Azure Web App-behållare (inklusive alla hello distribueras Java-program i den), kan du använda hello **Azure Explorer** vyn.</span><span class="sxs-lookup"><span data-stu-id="89aa1-244">toostart or stop an existing Azure Web App container, (including all hello deployed Java applications in it), you can use hello **Azure Explorer** view.</span></span>

<span data-ttu-id="89aa1-245">Om hello **Azure Explorer** vyn inte är öppen, kan du öppna det genom att klicka på sedan **visa** -menyn i IntelliJ, klicka på **verktyget Windows**, och klicka sedan på  **Tjänsten Explorer**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-245">If hello **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="89aa1-246">Om du inte tidigare har loggat in uppmanas du toodo så.</span><span class="sxs-lookup"><span data-stu-id="89aa1-246">If you have not previously logged in, it will prompt you toodo so.</span></span>

<span data-ttu-id="89aa1-247">När hello **Azure Explorer** visas, Använd följer du dessa steg toostart eller stoppa ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="89aa1-247">When hello **Azure Explorer** view is displayed, use follow these steps toostart or stop your Web App:</span></span> 

1. <span data-ttu-id="89aa1-248">Expandera hello **Azure** nod.</span><span class="sxs-lookup"><span data-stu-id="89aa1-248">Expand hello **Azure** node.</span></span>
2. <span data-ttu-id="89aa1-249">Expandera hello **Web Apps** nod.</span><span class="sxs-lookup"><span data-stu-id="89aa1-249">Expand hello **Web Apps** node.</span></span> 
3. <span data-ttu-id="89aa1-250">Högerklicka på hello önskat webbprogram.</span><span class="sxs-lookup"><span data-stu-id="89aa1-250">Right-click hello desired Web App.</span></span>
4. <span data-ttu-id="89aa1-251">På snabbmenyn för hello visas **starta**, **stoppa**, eller **starta om**.</span><span class="sxs-lookup"><span data-stu-id="89aa1-251">When hello context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="89aa1-252">Observera att hello menyalternativen kontextmedvetna, så att du kan bara stoppa en webbapp som körs eller starta ett webbprogram som inte körs.</span><span class="sxs-lookup"><span data-stu-id="89aa1-252">Note that hello menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
    ![Stoppa webbprogram][18]

## <a name="next-steps"></a><span data-ttu-id="89aa1-254">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89aa1-254">Next Steps</span></span>
<span data-ttu-id="89aa1-255">Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="89aa1-255">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="89aa1-256">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="89aa1-256">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="89aa1-257">[Installera hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="89aa1-257">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="89aa1-258">[Skapa en Hello World-Webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="89aa1-258">[Create a Hello World Web App for Azure in Eclipse]</span></span>
  * <span data-ttu-id="89aa1-259">[Vad är nytt i hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="89aa1-259">[What's New in hello Azure Toolkit for Eclipse]</span></span>
* <span data-ttu-id="89aa1-260">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="89aa1-260">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="89aa1-261">[installerar hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="89aa1-261">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="89aa1-262">*Skapa en Hello World-Webbapp för Azure i IntelliJ (den här artikeln)*</span><span class="sxs-lookup"><span data-stu-id="89aa1-262">*Create a Hello World Web App for Azure in IntelliJ (This Article)*</span></span>
  * <span data-ttu-id="89aa1-263">[Vad är nytt i hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="89aa1-263">[What's New in hello Azure Toolkit for IntelliJ]</span></span>

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="89aa1-264">Se även</span><span class="sxs-lookup"><span data-stu-id="89aa1-264">See Also</span></span>
<span data-ttu-id="89aa1-265">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="89aa1-265">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

<span data-ttu-id="89aa1-266">Mer information om hur du skapar Azure Web Apps finns hello [översikt över Web Apps].</span><span class="sxs-lookup"><span data-stu-id="89aa1-266">For additional information about creating Azure Web Apps, see hello [Web Apps Overview].</span></span>

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[installerar hello Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[översikt över Web Apps]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
