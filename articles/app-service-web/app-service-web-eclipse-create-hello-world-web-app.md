---
title: "en grundläggande Azure web app med hjälp av aaaCreate solförmörkelse | Microsoft Docs"
description: "Den här kursen visar hur toouse hello Azure Toolkit för Eclipse toocreate Hello World Web App för Azure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: b2f42e0e7a5b98760ec02fab2fc38f9f07b1156b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a><span data-ttu-id="0468a-103">Skapa en grundläggande Azure-webbapp med Eclipse</span><span class="sxs-lookup"><span data-stu-id="0468a-103">Create a basic Azure web app using Eclipse</span></span>
<span data-ttu-id="0468a-104">Den här kursen visar hur toocreate och distribuera en grundläggande Hello World programmet tooAzure som ett webbprogram med hjälp av hello [Azure Toolkit för Eclipse].</span><span class="sxs-lookup"><span data-stu-id="0468a-104">This tutorial shows how toocreate and deploy a basic Hello World application tooAzure as a Web App by using hello [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="0468a-105">En grundläggande JSP-exempel visas för enkelhetens skull, men liknande steg skulle vara lämplig för ett Java-servlet som Azure-distribution är.</span><span class="sxs-lookup"><span data-stu-id="0468a-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="0468a-106">När du har slutfört den här självstudiekursen ser liknande toohello följande bild visas i en webbläsare i ditt program:</span><span class="sxs-lookup"><span data-stu-id="0468a-106">When you have completed this tutorial, your application will look similar toohello following illustration when you view it in a web browser:</span></span>

![Förhandsgranskning av Hello World-app][01]

## <a name="prerequisites"></a><span data-ttu-id="0468a-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0468a-108">Prerequisites</span></span>
* <span data-ttu-id="0468a-109">En Java Developer Kit (JDK), v 1,8 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0468a-109">A Java Developer Kit (JDK), v 1.8 or later.</span></span>
* <span data-ttu-id="0468a-110">Solförmörkelse IDE för Java EE-utvecklare, Luna eller senare.</span><span class="sxs-lookup"><span data-stu-id="0468a-110">Eclipse IDE for Java EE Developers, Luna or later.</span></span> <span data-ttu-id="0468a-111">Detta kan hämtas från <http://www.eclipse.org/downloads/>.</span><span class="sxs-lookup"><span data-stu-id="0468a-111">This can be downloaded from <http://www.eclipse.org/downloads/>.</span></span>
* <span data-ttu-id="0468a-112">En distributionsplats i en Java-baserad webbserver eller programserver som [Apache Tomcat] eller [Jetty].</span><span class="sxs-lookup"><span data-stu-id="0468a-112">A distribution of a Java-based web server or application server, such as [Apache Tomcat] or [Jetty].</span></span>
* <span data-ttu-id="0468a-113">En Azure-prenumeration som kan erhållas från <https://azure.microsoft.com/free/> eller <http://azure.microsoft.com/pricing/purchase-options/>.</span><span class="sxs-lookup"><span data-stu-id="0468a-113">An Azure subscription, which can be acquired from <https://azure.microsoft.com/free/> or <http://azure.microsoft.com/pricing/purchase-options/>.</span></span>
* <span data-ttu-id="0468a-114">Hej [Azure Toolkit för Eclipse].</span><span class="sxs-lookup"><span data-stu-id="0468a-114">hello [Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="0468a-115">Information om hur du installerar hello Azure Toolkit finns [installerar hello Azure Toolkit för Eclipse].</span><span class="sxs-lookup"><span data-stu-id="0468a-115">For information about installing hello Azure Toolkit, see [Installing hello Azure Toolkit for Eclipse].</span></span>

## <a name="toocreate-a-hello-world-application"></a><span data-ttu-id="0468a-116">toocreate ett Hello World-program</span><span class="sxs-lookup"><span data-stu-id="0468a-116">toocreate a Hello World application</span></span>
<span data-ttu-id="0468a-117">Först ska vi börja med att skapa ett Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="0468a-117">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="0468a-118">Starta Eclipse och hello-menyn klickar du på **filen**, klickar du på **ny**, och klicka sedan på **dynamiskt webbprojekt**.</span><span class="sxs-lookup"><span data-stu-id="0468a-118">Start Eclipse, and at hello menu click **File**, click **New**, and then click **Dynamic Web Project**.</span></span> <span data-ttu-id="0468a-119">(Om du inte ser **dynamiskt webbprojekt** visas som tillgängliga projekt när du klickar på **filen** och **ny**, sedan hello följande: Klicka på **filen**, klickar du på **ny**, klickar du på **projekt...** , expandera **Web**, klickar du på **dynamiskt webbprojekt**, och klicka på **nästa**.)</span><span class="sxs-lookup"><span data-stu-id="0468a-119">(If you don't see **Dynamic Web Project** listed as an available project after clicking **File** and **New**, then do hello following: click **File**, click **New**, click **Project...**, expand **Web**, click **Dynamic Web Project**, and click **Next**.)</span></span>
2. <span data-ttu-id="0468a-120">Namn för den här kursen är hello projektet **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="0468a-120">For purposes of this tutorial, name hello project **MyWebApp**.</span></span> <span data-ttu-id="0468a-121">Skärmen visas liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="0468a-121">Your screen will appear similar toohello following:</span></span>
   
    ![Skapa en ny dynamiskt webbprojekt][02]
3. <span data-ttu-id="0468a-123">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="0468a-123">Click **Finish**.</span></span>
4. <span data-ttu-id="0468a-124">I Eclipses Projektutforskaren vy Expandera **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="0468a-124">Within Eclipse's Project Explorer view, expand **MyWebApp**.</span></span> <span data-ttu-id="0468a-125">Högerklicka på **Webbinnehåll**, klicka på **Ny** och sedan på **JSP-fil**.</span><span class="sxs-lookup"><span data-stu-id="0468a-125">Right-click **WebContent**, click **New**, and then click **JSP File**.</span></span>
5. <span data-ttu-id="0468a-126">I hello **ny JSP-fil** dialogrutan, namnet hello fil **index.jsp**, hålla hello överordnade mappen som **MyWebApp/webbinnehåll**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="0468a-126">In hello **New JSP File** dialog box, name hello file **index.jsp**, keep hello parent folder as **MyWebApp/WebContent**, and then click **Next**.</span></span>
6. <span data-ttu-id="0468a-127">I hello **Välj JSP-mall** dialogrutan för den här självstudiekursen väljer **ny JSP-fil (html)**, och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="0468a-127">In hello **Select JSP Template** dialog box, for purposes of this tutorial select **New JSP File (html)**, and then click **Finish**.</span></span>
7. <span data-ttu-id="0468a-128">När index.jsp-filen öppnas i Eclipse lägga till texten toodynamically visas på **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="0468a-128">When your index.jsp file opens in Eclipse, add in text toodynamically display **Hello World!**</span></span> <span data-ttu-id="0468a-129">inom hello befintliga `<body>` element.</span><span class="sxs-lookup"><span data-stu-id="0468a-129">within hello existing `<body>` element.</span></span> <span data-ttu-id="0468a-130">Uppdaterade `<body>` innehåll bör likna följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="0468a-130">Your updated `<body>` content should resemble hello following example:</span></span>
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. <span data-ttu-id="0468a-131">Spara index.jsp.</span><span class="sxs-lookup"><span data-stu-id="0468a-131">Save index.jsp.</span></span>

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a><span data-ttu-id="0468a-132">toodeploy dina program tooan Azure Web App-behållare</span><span class="sxs-lookup"><span data-stu-id="0468a-132">toodeploy your application tooan Azure Web App Container</span></span>
<span data-ttu-id="0468a-133">Det finns flera olika sätt som du kan distribuera en Java web application tooAzure.</span><span class="sxs-lookup"><span data-stu-id="0468a-133">There are several ways by which you can deploy a Java web application tooAzure.</span></span> <span data-ttu-id="0468a-134">Den här självstudiekursen beskriver en hello enklaste: ditt program kommer att distribuerade tooan Azure Web App-behållare – krävs inget speciellt projekttyp eller andra verktyg.</span><span class="sxs-lookup"><span data-stu-id="0468a-134">This tutorial describes one of hello simplest: your application will be deployed tooan Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="0468a-135">hello JDK hello web behållaren programvara och kommer att tillhandahållas för dig av Azure, så det finns inget behov av tooupload egna; allt du behöver är Java-Webbappen.</span><span class="sxs-lookup"><span data-stu-id="0468a-135">hello JDK and hello web container software will be provided for you by Azure, so there is no need tooupload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="0468a-136">Därför tar hello publiceringsprocessen för tillämpningsprogrammet sekunder, inte minuter.</span><span class="sxs-lookup"><span data-stu-id="0468a-136">As a result, hello publishing process for your application will take seconds, not minutes.</span></span>

1. <span data-ttu-id="0468a-137">Högerklicka i Eclipses Projektutforskaren **MyWebApp**.</span><span class="sxs-lookup"><span data-stu-id="0468a-137">In Eclipse's Project Explorer, right-click **MyWebApp**.</span></span>
2. <span data-ttu-id="0468a-138">Hello snabbmenyn Välj **Azure**, klicka på **Publicera som Azure Web App...**</span><span class="sxs-lookup"><span data-stu-id="0468a-138">In hello context menu, select **Azure**, then click **Publish as Azure Web App...**</span></span>
   
    ![Publicera som Azure Webbapp][03]
   
    <span data-ttu-id="0468a-140">Medan webbprojektet för programmet är markerat i hello Projektutforskaren, du kan också klicka på hello **publicera** nedrullningsbara knappen hello verktygsfältet och välj **Publicera som Azure Web App** där:</span><span class="sxs-lookup"><span data-stu-id="0468a-140">Alternatively, while your web application project is selected in hello Project Explorer, you can click hello **Publish** dropdown button on hello toolbar and select **Publish as Azure Web App** from there:</span></span>
   
    ![Publicera som Azure Webbapp][14]
3. <span data-ttu-id="0468a-142">Om du inte redan loggat in Azure från Eclipse kommer du att ange toosign i ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="0468a-142">If you have not already signed into Azure from Eclipse, you will be prompted toosign into your Azure account:</span></span>
   
    ![Azure logga In dialogrutan][04]
   
    <span data-ttu-id="0468a-144">Om du har flera Azure-konton, vissa hello frågor under hello logga in processen kanske visas mer än en gång, även om de visas toobe hello samma.</span><span class="sxs-lookup"><span data-stu-id="0468a-144">If you have multiple Azure accounts, some of hello prompts during hello sign in process may be shown more than once, even if they appear toobe hello same.</span></span> <span data-ttu-id="0468a-145">Då kan fortsätta efter hello inloggning instruktioner.</span><span class="sxs-lookup"><span data-stu-id="0468a-145">When this happens, continue following hello sign in instructions.</span></span>
4. <span data-ttu-id="0468a-146">När du har loggat in ditt Azure-konto, hello **hantera prenumerationer** dialogrutan visas en lista över prenumerationer som är associerade med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0468a-146">After you have successfully signed into your Azure account, hello **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="0468a-147">Om det finns flera prenumerationerna i listan och du vill toowork med bara en delmängd av dem, kan du avmarkera hello som du vill toouse (valfritt).</span><span class="sxs-lookup"><span data-stu-id="0468a-147">If there are multiple subscriptions listed and you want toowork with only a specific subset of them, you may optionally uncheck hello ones you do want toouse.</span></span> <span data-ttu-id="0468a-148">När du har valt dina prenumerationer, klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="0468a-148">When you have selected your subscriptions, click **Close**.</span></span>
   
    ![Hantera prenumerationer dialogrutan][05]
5. <span data-ttu-id="0468a-150">När hello **distribuera tooAzure Web App behållaren** i dialogrutan visas alla webbprogram-behållare som du tidigare har skapat; om du inte har skapat några behållare, hello listan är tom.</span><span class="sxs-lookup"><span data-stu-id="0468a-150">When hello **Deploy tooAzure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, hello list will be empty.</span></span>
   
    ![Distribuera tooAzure App Webbehållaren dialogrutan][06]
6. <span data-ttu-id="0468a-152">Om du inte har skapat ett Azure Web App-behållare innan, eller om du vill att toopublish dina program tooa ny behållare, använda hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="0468a-152">If you have not created an Azure Web App Container before, or if you would like toopublish your application tooa new container, use hello following steps.</span></span> <span data-ttu-id="0468a-153">Annars markerar du en befintlig Web Appbehållare och hoppa över toostep 7 nedan.</span><span class="sxs-lookup"><span data-stu-id="0468a-153">Otherwise, select an existing Web App Container and skip toostep 7 below.</span></span>
   
   1. <span data-ttu-id="0468a-154">Klicka på **nya...**</span><span class="sxs-lookup"><span data-stu-id="0468a-154">Click **New...**</span></span>
      
       ![Distribuera tooAzure App Webbehållaren dialogrutan][15]
   2. <span data-ttu-id="0468a-156">Hej **ny Web Appbehållare** dialogrutan visas:</span><span class="sxs-lookup"><span data-stu-id="0468a-156">hello **New Web App Container** dialog box will be displayed:</span></span>
      
       ![Dialogrutan Nytt Web Appbehållare][07a]
   3. <span data-ttu-id="0468a-158">Ange en **DNS-etikett** för App Webbehållaren; kommer att skapas hello löv DNS-etikett hello värd-URL för ditt webbprogram i Azure.</span><span class="sxs-lookup"><span data-stu-id="0468a-158">Enter a **DNS Label** for your Web App Container; this will form hello leaf DNS label of hello host URL for your web application in Azure.</span></span> <span data-ttu-id="0468a-159">(Observera att hello-namn måste vara tillgängliga och följa tooAzure krav för webbprogram.)</span><span class="sxs-lookup"><span data-stu-id="0468a-159">(Note that hello name must be available and conform tooAzure Web App naming requirements.)</span></span>
   4. <span data-ttu-id="0468a-160">I hello **Webbehållaren** nedrullningsbara menyn, Välj hello rätt programvara för ditt program.</span><span class="sxs-lookup"><span data-stu-id="0468a-160">In hello **Web Container** drop-down menu, select hello appropriate software for your application.</span></span>
      
       <span data-ttu-id="0468a-161">För närvarande kan du välja mellan Tomcat 8, 7 Tomcat eller Jetty 9.</span><span class="sxs-lookup"><span data-stu-id="0468a-161">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="0468a-162">En senaste fördelning av hello valt programvara kommer att tillhandahållas av Azure och körs på en senaste fördelning av JDK 8 skapats av Oracle och från Azure.</span><span class="sxs-lookup"><span data-stu-id="0468a-162">A recent distribution of hello selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>
   5. <span data-ttu-id="0468a-163">I hello **prenumeration** nedrullningsbara menyn, Välj hello-prenumeration som du vill toouse för den här distributionen.</span><span class="sxs-lookup"><span data-stu-id="0468a-163">In hello **Subscription** drop-down menu, select hello subscription you want toouse for this deployment.</span></span>
   6. <span data-ttu-id="0468a-164">I hello **resursgruppen** nedrullningsbara menyn, Välj hello resursgrupp som du vill använda tooassociate ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0468a-164">In hello **Resource Group** drop-down menu, select hello Resource Group with which you want tooassociate your Web App.</span></span> <span data-ttu-id="0468a-165">(Azure resursgrupper kan du toogroup relaterade resurser tillsammans så att till exempel kan du ta bort tillsammans.)</span><span class="sxs-lookup"><span data-stu-id="0468a-165">(Azure Resource Groups allow you toogroup related resources together so that, for example, they can be deleted together.)</span></span>
      
       <span data-ttu-id="0468a-166">Du kan välja en befintlig resursgrupp (om du har några) och hoppa över toostep g nedan eller Använd hello följa dessa steg toocreate en ny resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="0468a-166">You can select an existing Resource Group (if you have any) and skip toostep g below, or use hello following these steps toocreate a new Resource Group:</span></span>
      
      * <span data-ttu-id="0468a-167">Klicka på **nya...**</span><span class="sxs-lookup"><span data-stu-id="0468a-167">Click **New...**</span></span>
      * <span data-ttu-id="0468a-168">Hej **ny resursgrupp** dialogrutan visas:</span><span class="sxs-lookup"><span data-stu-id="0468a-168">hello **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Dialogrutan Ny resursgrupp][08]
      * <span data-ttu-id="0468a-170">I hello hello **namn** textruta, ange ett namn för din nya resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0468a-170">In hello hello **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="0468a-171">I hello hello **Region** nedrullningsbara menyn, Välj hello lämplig Azure Datacenter plats för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0468a-171">In hello hello **Region** drop-down menu, select hello appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="0468a-172">Valfritt: Som standard en senaste fördelning av Java 8 kommer att distribueras av Azure automatiskt tooyour web appbehållare som din JVM.</span><span class="sxs-lookup"><span data-stu-id="0468a-172">OPTIONAL: By default, a recent distribution of Java 8 will be deployed by Azure automatically tooyour web app container as your JVM.</span></span> <span data-ttu-id="0468a-173">Du kan dock ange en annan version och distribution av hello JVM om ditt program kräver.</span><span class="sxs-lookup"><span data-stu-id="0468a-173">However, you can specify a different version and distribution of hello JVM if your Web App requires it.</span></span> <span data-ttu-id="0468a-174">toospecify hello JDK för ditt webbprogram, klicka på hello **JDK** och välj något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="0468a-174">toospecify hello JDK for your Web App, click hello **JDK** tab, and select one of hello following options:</span></span>
        
        * <span data-ttu-id="0468a-175">**Distribuera hello standard JDK som erbjuds av Azure Web Apps service**: det här alternativet distribuerar en senaste fördelning av Java 8.</span><span class="sxs-lookup"><span data-stu-id="0468a-175">**Deploy hello default JDK offered by Azure Web Apps service**: This option will deploy a recent distribution of Java 8.</span></span>
        * <span data-ttu-id="0468a-176">**Distribuera en 3 part JDK som är tillgängliga på Azure**: det här alternativet kan du toochoose hello listan över JDKs som tillhandahålls av Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0468a-176">**Deploy a 3rd party JDK available on Azure**: This option allows you toochoose from hello list of JDKs which are provided by Microsoft Azure.</span></span>
        * <span data-ttu-id="0468a-177">**Distribuera min egen JDK från den här hämtningsplats**: det här alternativet kan du toospecify egna JDK-distribution som måste paketeras som en ZIP-filen och överföra tooeither offentligt tillgängliga hämtningsplats eller ett Azure storage-konto som du ha åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0468a-177">**Deploy my own JDK from this download location**: This option allows you toospecify your own JDK distribution, which must be packaged as a ZIP file and uploaded tooeither a publicly available download location or an Azure storage account for which you have access.</span></span>
          
          ![Dialogrutan Nytt Web Appbehållare][07b]
   7. <span data-ttu-id="0468a-179">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0468a-179">Click **OK**.</span></span>
   8. <span data-ttu-id="0468a-180">Hej **Apptjänstplan** listrutan visar hello apptjänstplaner som är associerade med hello resursgrupp som du har valt.</span><span class="sxs-lookup"><span data-stu-id="0468a-180">hello **App Service Plan** drop-down menu lists hello app service plans that are associated with hello Resource Group that you selected.</span></span> <span data-ttu-id="0468a-181">(Programtjänstplaner ange information, till exempel hello platsen för ditt webbprogram, hello prisnivå och hello instansstorleken för beräkning.</span><span class="sxs-lookup"><span data-stu-id="0468a-181">(App Service Plans specify information such as hello location of your Web App, hello pricing tier and hello compute instance size.</span></span> <span data-ttu-id="0468a-182">En enda App Service-Plan kan användas för flera webbprogram, vilket är anledningen till att den hanteras separat från en viss distribution för webbprogrammet.)</span><span class="sxs-lookup"><span data-stu-id="0468a-182">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       <span data-ttu-id="0468a-183">Du kan välja en befintlig App Service-Plan (om du har några) och hoppa över toostep h nedan eller använda hello följa dessa steg toocreate en ny App Service-Plan:</span><span class="sxs-lookup"><span data-stu-id="0468a-183">You can select an existing App Service Plan (if you have any) and skip toostep h below, or use hello following these steps toocreate a new App Service Plan:</span></span>
      
      * <span data-ttu-id="0468a-184">Klicka på **nya...**</span><span class="sxs-lookup"><span data-stu-id="0468a-184">Click **New...**</span></span>
      * <span data-ttu-id="0468a-185">Hej **nya App Service-Plan** dialogrutan visas:</span><span class="sxs-lookup"><span data-stu-id="0468a-185">hello **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Dialogrutan för ny App Service-Plan][09]
      * <span data-ttu-id="0468a-187">I hello hello **namn** textruta, ange ett namn för din nya App Service-Plan.</span><span class="sxs-lookup"><span data-stu-id="0468a-187">In hello hello **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="0468a-188">I hello hello **plats** nedrullningsbara menyn, Välj hello lämplig Azure Datacenter plats för hello-plan.</span><span class="sxs-lookup"><span data-stu-id="0468a-188">In hello hello **Location** drop-down menu, select hello appropriate Azure data center location for hello plan.</span></span>
      * <span data-ttu-id="0468a-189">I hello hello **prisnivån** nedrullningsbara menyn, Välj hello lämpliga priser för hello plan.</span><span class="sxs-lookup"><span data-stu-id="0468a-189">In hello hello **Pricing Tier** drop-down menu, select hello appropriate pricing for hello plan.</span></span> <span data-ttu-id="0468a-190">I testsyfte kan du välja **lediga**.</span><span class="sxs-lookup"><span data-stu-id="0468a-190">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="0468a-191">I hello hello **Instansstorleken** nedrullningsbara menyn, Välj hello lämplig instans storlek för hello-plan.</span><span class="sxs-lookup"><span data-stu-id="0468a-191">In hello hello **Instance Size** drop-down menu, select hello appropriate instance size for hello plan.</span></span> <span data-ttu-id="0468a-192">I testsyfte kan du välja **små**.</span><span class="sxs-lookup"><span data-stu-id="0468a-192">For testing purposes you can choose **Small**.</span></span>
   9. <span data-ttu-id="0468a-193">När du har slutfört alla hello senare steg, bör hello ny Web Appbehållare dialogrutan likna följande illustration hello:</span><span class="sxs-lookup"><span data-stu-id="0468a-193">Once you have completed all of hello above steps, hello New Web App Container dialog box should resemble hello following illustration:</span></span>
      
       ![Dialogrutan Nytt Web Appbehållare][10]
   10. <span data-ttu-id="0468a-195">Klicka på **OK** toocomplete hello skapandet av din nya Webbapp-behållaren.</span><span class="sxs-lookup"><span data-stu-id="0468a-195">Click **OK** toocomplete hello creation of your new Web App container.</span></span>
       
        <span data-ttu-id="0468a-196">Vänta några sekunder hello lista över hello webbprogrammet behållare toobe uppdateras och nyskapade app webbehållaren ska nu vara markerade i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="0468a-196">Wait a few seconds for hello list of hello Web App containers toobe refreshed, and your newly-created web app container should now be selected in hello list.</span></span>
7. <span data-ttu-id="0468a-197">Du är nu redo toocomplete hello initiala distributionen av ditt webbprogram tooAzure:</span><span class="sxs-lookup"><span data-stu-id="0468a-197">You are now ready toocomplete hello initial deployment of your Web App tooAzure:</span></span>
   
    ![Distribuera tooAzure App Webbehållaren dialogrutan][11]
   
    <span data-ttu-id="0468a-199">Klicka på **OK** toodeploy toohello din Java-programmet valt Web App behållare.</span><span class="sxs-lookup"><span data-stu-id="0468a-199">Click **OK** toodeploy your Java application toohello selected Web App container.</span></span>
   
    <span data-ttu-id="0468a-200">Programmet kommer att distribueras som en underkatalog till hello programservern som standard.</span><span class="sxs-lookup"><span data-stu-id="0468a-200">By default, your application will be deployed as a subdirectory of hello application server.</span></span> <span data-ttu-id="0468a-201">Om du vill att den distribueras som hello rotprogram toobe, kontrollera hello **distribuera tooroot** kryssrutan innan du klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0468a-201">If you want it toobe deployed as hello root application, check hello **Deploy tooroot** checkbox before clicking **OK**.</span></span>
8. <span data-ttu-id="0468a-202">Därefter bör du se hello **Azure-aktivitetsloggen** vy, som visar hello Distributionsstatus för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0468a-202">Next, you should see hello **Azure Activity Log** view, which will indicate hello deployment status of your Web App.</span></span>
   
    ![Azure-aktivitetsloggen][12]
   
    <span data-ttu-id="0468a-204">hello-processen för att distribuera webbprogram-tooAzure tar endast några sekunder toocomplete.</span><span class="sxs-lookup"><span data-stu-id="0468a-204">hello process of deploying your Web App tooAzure should take only a few seconds toocomplete.</span></span> <span data-ttu-id="0468a-205">När din programmet kan påbörjas kan du visa en länk med namnet **publicerade** i hello **Status** kolumn.</span><span class="sxs-lookup"><span data-stu-id="0468a-205">When your application ready, you will see a link named **Published** in hello **Status** column.</span></span> <span data-ttu-id="0468a-206">När du klickar på länken hello tar du tooyour distribueras Web App startsidan.</span><span class="sxs-lookup"><span data-stu-id="0468a-206">When you click hello link, it will take you tooyour deployed Web App's home page.</span></span>

## <a name="updating-your-web-app"></a><span data-ttu-id="0468a-207">Uppdatera din webbapp</span><span class="sxs-lookup"><span data-stu-id="0468a-207">Updating your web app</span></span>
<span data-ttu-id="0468a-208">Uppdatering av ett befintligt kör Azure Web App är en snabb och enkel process och har du två alternativ för att uppdatera:</span><span class="sxs-lookup"><span data-stu-id="0468a-208">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="0468a-209">Du kan uppdatera hello distribution av en befintlig Java-Webbapp.</span><span class="sxs-lookup"><span data-stu-id="0468a-209">You can update hello deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="0468a-210">Du kan publicera en ytterligare Java-program toohello Web Appbehållare.</span><span class="sxs-lookup"><span data-stu-id="0468a-210">You can publish an additional Java application toohello same Web App Container.</span></span>

<span data-ttu-id="0468a-211">I båda fallen hello processen är identiska och tar endast några sekunder:</span><span class="sxs-lookup"><span data-stu-id="0468a-211">In either case, hello process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="0468a-212">Högerklicka på hello Java-program du vill tooupdate eller lägga till tooan befintliga Web Appbehållare i hello Eclipse Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="0468a-212">In hello Eclipse project explorer, right-click hello Java application you want tooupdate or add tooan existing Web App Container.</span></span>
2. <span data-ttu-id="0468a-213">Välj när hello snabbmenyn visas **Azure** och sedan **Publicera som Azure Web App...**</span><span class="sxs-lookup"><span data-stu-id="0468a-213">When hello context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="0468a-214">Eftersom du har redan loggat in tidigare, visas en lista över dina befintliga Web App-behållare.</span><span class="sxs-lookup"><span data-stu-id="0468a-214">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="0468a-215">Välj hello en du vill toopublish eller publicera din Java-programmet tooand klickar **OK**.</span><span class="sxs-lookup"><span data-stu-id="0468a-215">Select hello one you want toopublish or re-publish your Java application tooand click **OK**.</span></span>

<span data-ttu-id="0468a-216">Några sekunder senare hello **Azure-aktivitetsloggen** vyer visar uppdaterade distributionen som **publicerade** och du kommer att kunna tooverify uppdaterade programmet i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="0468a-216">A few seconds later, hello **Azure Activity Log** view will show your updated deployment as **Published** and you will be able tooverify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="0468a-217">Starta, stoppa eller starta om en befintlig webbapp</span><span class="sxs-lookup"><span data-stu-id="0468a-217">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="0468a-218">toostart eller stoppa en befintlig Azure Web App-behållare (inklusive alla hello distribueras Java-program i den), kan du använda hello **Azure Explorer** vyn.</span><span class="sxs-lookup"><span data-stu-id="0468a-218">toostart or stop an existing Azure Web App container, (including all hello deployed Java applications in it), you can use hello **Azure Explorer** view.</span></span>

<span data-ttu-id="0468a-219">Om hello **Azure Explorer** vyn inte är öppen, kan du öppna det genom att klicka på sedan **fönstret** Klicka på menyn i Eclipse **visa**, sedan **andra...** , sedan **Azure**, och klicka sedan på **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="0468a-219">If hello **Azure Explorer** view is not already open, you can open it by clicking then **Window** menu in Eclipse, then click **Show View**, then **Other...**, then **Azure**, and then click **Azure Explorer**.</span></span> <span data-ttu-id="0468a-220">Om du inte tidigare har loggat in uppmanas du toodo så.</span><span class="sxs-lookup"><span data-stu-id="0468a-220">If you have not previously logged in, it will prompt you toodo so.</span></span>

<span data-ttu-id="0468a-221">När hello **Azure Explorer** visas, Använd följer du dessa steg toostart eller stoppa ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="0468a-221">When hello **Azure Explorer** view is displayed, use follow these steps toostart or stop your Web App:</span></span> 

1. <span data-ttu-id="0468a-222">Expandera hello **Azure** nod.</span><span class="sxs-lookup"><span data-stu-id="0468a-222">Expand hello **Azure** node.</span></span>
2. <span data-ttu-id="0468a-223">Expandera hello **Web Apps** nod.</span><span class="sxs-lookup"><span data-stu-id="0468a-223">Expand hello **Web Apps** node.</span></span> 
3. <span data-ttu-id="0468a-224">Högerklicka på hello önskat webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0468a-224">Right-click hello desired Web App.</span></span>
4. <span data-ttu-id="0468a-225">På snabbmenyn för hello visas **starta**, **stoppa**, eller **starta om**.</span><span class="sxs-lookup"><span data-stu-id="0468a-225">When hello context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="0468a-226">Observera att hello menyalternativen kontextmedvetna, så att du kan bara stoppa en webbapp som körs eller starta ett webbprogram som inte körs.</span><span class="sxs-lookup"><span data-stu-id="0468a-226">Note that hello menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
    ![Stoppa en befintlig Webbapp][13]

## <a name="next-steps"></a><span data-ttu-id="0468a-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0468a-228">Next Steps</span></span>
<span data-ttu-id="0468a-229">Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="0468a-229">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="0468a-230">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="0468a-230">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="0468a-231">[installerar hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="0468a-231">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="0468a-232">*Skapa en Hello World-Webbapp för Azure i Eclipse (den här artikeln)*</span><span class="sxs-lookup"><span data-stu-id="0468a-232">*Create a Hello World Web App for Azure in Eclipse (This Article)*</span></span>
  * <span data-ttu-id="0468a-233">[Vad är nytt i hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="0468a-233">[What's New in hello Azure Toolkit for Eclipse]</span></span>
* <span data-ttu-id="0468a-234">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="0468a-234">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="0468a-235">[Installera hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="0468a-235">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="0468a-236">[Skapa en Hello World-Webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="0468a-236">[Create a Hello World Web App for Azure in IntelliJ]</span></span>
  * <span data-ttu-id="0468a-237">[Vad är nytt i hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="0468a-237">[What's New in hello Azure Toolkit for IntelliJ]</span></span>

<span data-ttu-id="0468a-238">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="0468a-238">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

<span data-ttu-id="0468a-239">Mer information om hur du skapar Azure Web Apps finns hello [översikt över Web Apps].</span><span class="sxs-lookup"><span data-stu-id="0468a-239">For additional information about creating Azure Web Apps, see hello [Web Apps Overview].</span></span>

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[installerar hello Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[översikt över Web Apps]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
