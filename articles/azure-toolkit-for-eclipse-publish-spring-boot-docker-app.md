---
title: "aaaPublish en start av vår app som en dockerbehållare med hjälp av hello Azure Toolkit för Eclipse | Microsoft Docs"
description: "Lär dig hur toopublish en web app tooMicrosoft Azure som en dockerbehållare med hjälp av hello Azure Toolkit för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/21/2017
ms.author: robmcm
ms.openlocfilehash: 29390c49c339a1ebb87cb3951b21cea01c0da15f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="b9524-103">Publicera en start av vår app som en dockerbehållare med hello Azure Toolkit för Eclipse</span><span class="sxs-lookup"><span data-stu-id="b9524-103">Publish a Spring Boot app as a Docker container by using hello Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="b9524-104">Hej [Vårversionen Framework] är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="b9524-104">hello [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="b9524-105">En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.</span><span class="sxs-lookup"><span data-stu-id="b9524-105">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="b9524-106">[Docker] är en lösning för öppen källkod som gör att utvecklare kan automatisera hello distribution, skalning och hanteringen av sina program som körs i behållare.</span><span class="sxs-lookup"><span data-stu-id="b9524-106">[Docker] is an open-source solution that helps developers automate hello deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="b9524-107">Den här självstudiekursen vägleder dig genom hello steg toodeploy en källan startprogrammet som en Docker-behållare tooMicrosoft Azure med hjälp av hello Azure Toolkit för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="b9524-107">This tutorial walks you through hello steps toodeploy a Spring Boot application as a Docker container tooMicrosoft Azure by using hello Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repository"></a><span data-ttu-id="b9524-108">Klona hello standarddatabas källan Start Docker</span><span class="sxs-lookup"><span data-stu-id="b9524-108">Clone hello default Spring Boot Docker repository</span></span>

### <a name="import-hello-public-repository"></a><span data-ttu-id="b9524-109">Importera hello offentliga lagringsplats</span><span class="sxs-lookup"><span data-stu-id="b9524-109">Import hello public repository</span></span>

<span data-ttu-id="b9524-110">hello vägleder följande steg dig genom kloning hello källan Start Docker databasen tooyour lokala datorn med hjälp av IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="b9524-110">hello following steps walk you through cloning hello Spring Boot Docker repository tooyour local computer by using IntelliJ.</span></span> <span data-ttu-id="b9524-111">Om du vill toouse kommandoraden finns [distribuerar ett källan Start-program på Linux i Azure Container Service][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="b9524-111">If you want toouse a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="b9524-112">Öppna Eclipse.</span><span class="sxs-lookup"><span data-stu-id="b9524-112">Open Eclipse.</span></span>

1. <span data-ttu-id="b9524-113">Klicka på **filen** > **importera**.</span><span class="sxs-lookup"><span data-stu-id="b9524-113">Click **File** > **Import**.</span></span>

   ![Importera Arkivmenyn][CL01]

1. <span data-ttu-id="b9524-115">När hello **importera** öppnas:</span><span class="sxs-lookup"><span data-stu-id="b9524-115">When hello **Import** dialog box opens:</span></span>

   <span data-ttu-id="b9524-116">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-116">a.</span></span> <span data-ttu-id="b9524-117">Expandera **Git**.</span><span class="sxs-lookup"><span data-stu-id="b9524-117">Expand **Git**.</span></span>

   <span data-ttu-id="b9524-118">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-118">b.</span></span> <span data-ttu-id="b9524-119">Välj **projekt från Git**.</span><span class="sxs-lookup"><span data-stu-id="b9524-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="b9524-120">c.</span><span class="sxs-lookup"><span data-stu-id="b9524-120">c.</span></span> <span data-ttu-id="b9524-121">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b9524-121">Click **Next**.</span></span>

   ![Dialogrutan Importera][CL02]

1. <span data-ttu-id="b9524-123">På hello **Välj Lagerkälla** sidan:</span><span class="sxs-lookup"><span data-stu-id="b9524-123">On hello **Select Repository Source** page:</span></span>

   <span data-ttu-id="b9524-124">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-124">a.</span></span> <span data-ttu-id="b9524-125">Välj **klona URI**.</span><span class="sxs-lookup"><span data-stu-id="b9524-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="b9524-126">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-126">b.</span></span> <span data-ttu-id="b9524-127">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b9524-127">Click **Next**.</span></span>

   ![Välj Lagerkälla sida][CL03]

1. <span data-ttu-id="b9524-129">På hello **Git-Lagerkälla** sidan:</span><span class="sxs-lookup"><span data-stu-id="b9524-129">On hello **Source Git Repository** page:</span></span>

   <span data-ttu-id="b9524-130">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-130">a.</span></span> <span data-ttu-id="b9524-131">För **URI**, ange `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span><span class="sxs-lookup"><span data-stu-id="b9524-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="b9524-132">Det här steget ska automatiskt fylla i hello **värden** och **databassökvägen** fält med hello korrigera värden.</span><span class="sxs-lookup"><span data-stu-id="b9524-132">This step should automatically populate hello **Host** and **Repository path** fields with hello correct values.</span></span>
   
   <span data-ttu-id="b9524-133">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-133">b.</span></span> <span data-ttu-id="b9524-134">hello källan Start databas är offentliga så du inte bör ha tooenter Git användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="b9524-134">hello Spring Boot repository is public, so you should not have tooenter your Git username and password.</span></span>
   
   <span data-ttu-id="b9524-135">c.</span><span class="sxs-lookup"><span data-stu-id="b9524-135">c.</span></span> <span data-ttu-id="b9524-136">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b9524-136">Click **Next**.</span></span>

   ![Datakälla på sidan för Git-lagringsplats][CL04]

1. <span data-ttu-id="b9524-138">På hello **val av gren** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b9524-138">On hello **Branch Selection** page, click **Next**.</span></span>

   ![Sidan för val av gren][CL05]

1. <span data-ttu-id="b9524-140">På hello **lokal Destination** sidan:</span><span class="sxs-lookup"><span data-stu-id="b9524-140">On hello **Local Destination** page:</span></span>

   <span data-ttu-id="b9524-141">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-141">a.</span></span> <span data-ttu-id="b9524-142">Ange hello lokala mappen där du vill att din lokala lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="b9524-142">Specify hello local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="b9524-143">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-143">b.</span></span> <span data-ttu-id="b9524-144">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b9524-144">Click **Next**.</span></span>

   ![Lokala målsida][CL06]

1. <span data-ttu-id="b9524-146">På hello **väljer en toouse guide för importprojekt** sidan:</span><span class="sxs-lookup"><span data-stu-id="b9524-146">On hello **Select a wizard toouse for importing projects** page:</span></span>

   <span data-ttu-id="b9524-147">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-147">a.</span></span> <span data-ttu-id="b9524-148">Välj **Import som ett allmänt projekt**.</span><span class="sxs-lookup"><span data-stu-id="b9524-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="b9524-149">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-149">b.</span></span> <span data-ttu-id="b9524-150">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b9524-150">Click **Next**.</span></span>

   ![På sidan ”Välj ett toouse guide för importprojekt”][CL07]

1. <span data-ttu-id="b9524-152">På hello **Importera projekt** sidan:</span><span class="sxs-lookup"><span data-stu-id="b9524-152">On hello **Import Projects** page:</span></span>

   <span data-ttu-id="b9524-153">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-153">a.</span></span> <span data-ttu-id="b9524-154">Ange ditt projektnamn.</span><span class="sxs-lookup"><span data-stu-id="b9524-154">Specify your project name.</span></span>
   
   <span data-ttu-id="b9524-155">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-155">b.</span></span> <span data-ttu-id="b9524-156">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="b9524-156">Click **Finish**.</span></span>

   ![Importera projekt sida][CL08]

1. <span data-ttu-id="b9524-158">När hello databasen är klonad lyckas, ser du alla hello filer i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="b9524-158">When hello repository is cloned successfully, you see all hello files listed in Eclipse.</span></span>

   ![Lokal databas][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="b9524-160">Skapa ett Maven-projekt från din lokala databas</span><span class="sxs-lookup"><span data-stu-id="b9524-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="b9524-161">hello källan Start Docker-databasen innehåller en slutförd Maven-projekt som du ska använda för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="b9524-161">hello Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="b9524-162">Klicka på **filen** > **importera**.</span><span class="sxs-lookup"><span data-stu-id="b9524-162">Click **File** > **Import**.</span></span>

   ![Importkommando hello Arkiv-menyn][CL01]

1. <span data-ttu-id="b9524-164">När hello **importera** öppnas:</span><span class="sxs-lookup"><span data-stu-id="b9524-164">When hello **Import** dialog box opens:</span></span>

   <span data-ttu-id="b9524-165">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-165">a.</span></span> <span data-ttu-id="b9524-166">Expandera **Maven**.</span><span class="sxs-lookup"><span data-stu-id="b9524-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="b9524-167">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-167">b.</span></span> <span data-ttu-id="b9524-168">Välj **befintliga Maven-projekt**.</span><span class="sxs-lookup"><span data-stu-id="b9524-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="b9524-169">c.</span><span class="sxs-lookup"><span data-stu-id="b9524-169">c.</span></span> <span data-ttu-id="b9524-170">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b9524-170">Click **Next**.</span></span>

   ![Dialogrutan Importera][MV01]

1. <span data-ttu-id="b9524-172">På hello **Maven-projekt** sidan:</span><span class="sxs-lookup"><span data-stu-id="b9524-172">On hello **Maven Projects** page:</span></span>

   <span data-ttu-id="b9524-173">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-173">a.</span></span> <span data-ttu-id="b9524-174">För **rotkatalog**, ange hello **fullständig** mappen i lokala databasen.</span><span class="sxs-lookup"><span data-stu-id="b9524-174">For **Root Directory**, specify hello **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="b9524-175">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-175">b.</span></span> <span data-ttu-id="b9524-176">Expandera hello **Avancerat** , och ange ett eget namn för **mall**.</span><span class="sxs-lookup"><span data-stu-id="b9524-176">Expand hello **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="b9524-177">c.</span><span class="sxs-lookup"><span data-stu-id="b9524-177">c.</span></span> <span data-ttu-id="b9524-178">Välj hello rutan för hello **pom.xml** fil i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="b9524-178">Select hello box for hello **pom.xml** file in hello project.</span></span>
   
   <span data-ttu-id="b9524-179">d.</span><span class="sxs-lookup"><span data-stu-id="b9524-179">d.</span></span> <span data-ttu-id="b9524-180">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="b9524-180">Click **Finish**.</span></span>

   ![Sidan för maven-projekt][MV02]

1. <span data-ttu-id="b9524-182">När hello Maven-projekt öppnas har, kan du se ett andra projekt i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="b9524-182">When hello Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![Lokala Maven-projekt][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="b9524-184">Skapa din app för vår start med hjälp av Maven</span><span class="sxs-lookup"><span data-stu-id="b9524-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="b9524-185">Välj hello Maven-projekt i hello Eclipse Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="b9524-185">In hello Eclipse Project Explorer, select hello Maven project.</span></span>

1. <span data-ttu-id="b9524-186">Klicka på **kör** > **kör som** > **Maven build**.</span><span class="sxs-lookup"><span data-stu-id="b9524-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![Kommandon toorun som Maven build][BU01]

1. <span data-ttu-id="b9524-188">När programmet är skapat visar hello konsolfönstret hello status.</span><span class="sxs-lookup"><span data-stu-id="b9524-188">When your application is successfully built, hello console window shows hello status.</span></span>

   ![Lyckad Maven-version][BU02]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="b9524-190">Publicera din web app tooAzure med hjälp av en dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="b9524-190">Publish your web app tooAzure by using a Docker container</span></span>

1. <span data-ttu-id="b9524-191">Välj hello Maven-projekt i hello Eclipse Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="b9524-191">In hello Eclipse Project Explorer, select hello Maven project.</span></span>

1. <span data-ttu-id="b9524-192">Klicka på hello Azure **publicera** -menyn och klicka sedan på **Publicera som Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="b9524-192">Click hello Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![Publicera som Dockerbehållare kommando][PU01]

1. <span data-ttu-id="b9524-194">När hello **distribuera Dockerbehållare på Azure** dialogrutan visas:</span><span class="sxs-lookup"><span data-stu-id="b9524-194">When hello **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="b9524-195">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-195">a.</span></span> <span data-ttu-id="b9524-196">Ange ett eget namn för Docker-bild.</span><span class="sxs-lookup"><span data-stu-id="b9524-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="b9524-197">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-197">b.</span></span> <span data-ttu-id="b9524-198">För **artefakt toodeploy**, ange hello sökvägen toohello **gs-källan-Start-docker-0.1.0.jar** filen som du precis har skapat.</span><span class="sxs-lookup"><span data-stu-id="b9524-198">For **Artifact toodeploy**, specify hello path toohello **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Ange alternativ för Docker][PU02]

   <span data-ttu-id="b9524-200">Alla befintliga Docker-värdar visas.</span><span class="sxs-lookup"><span data-stu-id="b9524-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="b9524-201">Om du väljer toodeploy tooan befintliga värden kan du hoppa över toostep 5.</span><span class="sxs-lookup"><span data-stu-id="b9524-201">If you choose toodeploy tooan existing host, you can skip toostep 5.</span></span> <span data-ttu-id="b9524-202">Annars Använd hello följande steg toocreate en värd:</span><span class="sxs-lookup"><span data-stu-id="b9524-202">Otherwise, use hello following steps toocreate a host:</span></span>

   <span data-ttu-id="b9524-203">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-203">a.</span></span> <span data-ttu-id="b9524-204">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b9524-204">Click **Add**.</span></span>

      ![Lägga till en ny Docker-värd][PU03]

   <span data-ttu-id="b9524-206">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-206">b.</span></span> <span data-ttu-id="b9524-207">När hello **skapa Docker värden** dialogrutan visas, kan du välja tooaccept hello standardinställningar eller du kan ange anpassade inställningar för din nya Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="b9524-207">When hello **Create Docker Host** dialog box appears, you can choose tooaccept hello defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="b9524-208">(Detaljerade beskrivningar av hello olika inställningar, finns i [publicera ett webbprogram som en dockerbehållare med hello Azure Toolkit för IntelliJ][Publish Container with Azure Toolkit].) Klicka på **nästa** när du har angett vilka inställningar toouse.</span><span class="sxs-lookup"><span data-stu-id="b9524-208">(For detailed descriptions of hello various settings, see [Publish a web app as a Docker container by using hello Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings toouse.</span></span>

      ![Ange alternativ för Docker-värden][PU04]

   <span data-ttu-id="b9524-210">c.</span><span class="sxs-lookup"><span data-stu-id="b9524-210">c.</span></span> <span data-ttu-id="b9524-211">Du kan välja toouse befintliga autentiseringsuppgifter för inloggning från en Azure key vault eller du tooenter nya Docker-inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b9524-211">You can choose toouse existing login credentials from an Azure key vault, or you can choose tooenter new Docker login credentials.</span></span> <span data-ttu-id="b9524-212">Klicka på **Slutför** när du har angett dina alternativ.</span><span class="sxs-lookup"><span data-stu-id="b9524-212">Click **Finish** when you have specified your options.</span></span>

      ![Ange autentiseringsuppgifter för Docker-värden][PU05]

1. <span data-ttu-id="b9524-214">Välj Docker-värd och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b9524-214">Select your Docker host, and then click **Next**.</span></span>

   ![Välj Docker värden toouse][PU06]

1. <span data-ttu-id="b9524-216">På hello sista sidan i hello **distribuera Dockerbehållare på Azure** dialogrutan Ange hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="b9524-216">On hello last page of hello **Deploying Docker Container on Azure** dialog box, specify hello following options:</span></span>

   <span data-ttu-id="b9524-217">a.</span><span class="sxs-lookup"><span data-stu-id="b9524-217">a.</span></span> <span data-ttu-id="b9524-218">Du kan välja toospecify ett eget namn för hello-behållare som är värd för din dockerbehållare eller du kan acceptera hello standard.</span><span class="sxs-lookup"><span data-stu-id="b9524-218">You can choose toospecify a custom name for hello container that will host your Docker container, or you can accept hello default.</span></span>

   <span data-ttu-id="b9524-219">b.</span><span class="sxs-lookup"><span data-stu-id="b9524-219">b.</span></span> <span data-ttu-id="b9524-220">Ange hello TCP-portar för docker-värden med hjälp av hello följande syntax: *[extern port]*:*[Intern port]*.</span><span class="sxs-lookup"><span data-stu-id="b9524-220">Enter hello TCP ports for your docker host by using hello following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="b9524-221">Till exempel **80:8080** anger ett extern port 80 och hello interna källan Start standardporten 8080.</span><span class="sxs-lookup"><span data-stu-id="b9524-221">For example, **80:8080** specifies an external port of 80 and hello default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="b9524-222">Om du har anpassat en intern port (t.ex, genom att redigera hello application.yml filen), behöver du toospecify hello-portnumret för hello rätt routning toooccur i Azure.</span><span class="sxs-lookup"><span data-stu-id="b9524-222">If you have customized your internal port (for example, by editing hello application.yml file), you need toospecify hello port number for hello correct routing toooccur in Azure.</span></span>

   <span data-ttu-id="b9524-223">c.</span><span class="sxs-lookup"><span data-stu-id="b9524-223">c.</span></span> <span data-ttu-id="b9524-224">När du konfigurerar dessa alternativ klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="b9524-224">After you configure these options, click **Finish**.</span></span>

   ![Distribuera en dockerbehållare på Azure][PU07]

1. <span data-ttu-id="b9524-226">När hello Azure Toolkit har publicerats hello Azure-aktivitetsloggen visar **publicerade** hello status.</span><span class="sxs-lookup"><span data-stu-id="b9524-226">When hello Azure Toolkit has finished publishing, hello Azure Activity Log displays **Published** for hello status.</span></span>

   ![Distribuera Docker-värden][PU08]

## <a name="next-steps"></a><span data-ttu-id="b9524-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9524-228">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Vårversionen Start]: http://projects.spring.io/spring-boot/
[Vårversionen Framework]: https://spring.io/

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
