---
title: "Publicera en app för vår start som en dockerbehållare med hjälp av Azure-verktygen för Eclipse | Microsoft Docs"
description: "Lär dig hur du publicerar ett webbprogram till Microsoft Azure som en dockerbehållare med hjälp av Azure-verktygen för Eclipse."
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
ms.openlocfilehash: fcb60fcfbda26f5f37bfb0edcb01f8737188b6bc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="c3e1a-103">Publicera en app för vår start som en dockerbehållare med hjälp av Azure-verktygen för Eclipse</span><span class="sxs-lookup"><span data-stu-id="c3e1a-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="c3e1a-104">Den [Vårversionen Framework] är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="c3e1a-105">En av de mer populära projekt som bygger som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="c3e1a-106">[Docker] är en lösning för öppen källkod som gör att utvecklare kan automatisera distribution, skalning och hanteringen av sina program som körs i behållare.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="c3e1a-107">Den här självstudiekursen vägleder dig genom stegen för att distribuera en källan startprogrammet som en dockerbehållare till Microsoft Azure med hjälp av Azure-verktygen för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a><span data-ttu-id="c3e1a-108">Klona lagringsplatsen standard källan Start Docker</span><span class="sxs-lookup"><span data-stu-id="c3e1a-108">Clone the default Spring Boot Docker repository</span></span>

### <a name="import-the-public-repository"></a><span data-ttu-id="c3e1a-109">Importera den offentliga lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="c3e1a-109">Import the public repository</span></span>

<span data-ttu-id="c3e1a-110">Följande steg vägleder dig genom kloning källan Start Docker-databasen till den lokala datorn med hjälp av IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-110">The following steps walk you through cloning the Spring Boot Docker repository to your local computer by using IntelliJ.</span></span> <span data-ttu-id="c3e1a-111">Om du vill använda en kommandorad Se [distribuerar ett källan Start-program på Linux i Azure Container Service][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="c3e1a-111">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="c3e1a-112">Öppna Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-112">Open Eclipse.</span></span>

1. <span data-ttu-id="c3e1a-113">Klicka på **filen** > **importera**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-113">Click **File** > **Import**.</span></span>

   ![Importera Arkivmenyn][CL01]

1. <span data-ttu-id="c3e1a-115">När den **importera** öppnas:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-115">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="c3e1a-116">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-116">a.</span></span> <span data-ttu-id="c3e1a-117">Expandera **Git**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-117">Expand **Git**.</span></span>

   <span data-ttu-id="c3e1a-118">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-118">b.</span></span> <span data-ttu-id="c3e1a-119">Välj **projekt från Git**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="c3e1a-120">c.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-120">c.</span></span> <span data-ttu-id="c3e1a-121">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-121">Click **Next**.</span></span>

   ![Dialogrutan Importera][CL02]

1. <span data-ttu-id="c3e1a-123">På den **Välj Lagerkälla** sidan:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-123">On the **Select Repository Source** page:</span></span>

   <span data-ttu-id="c3e1a-124">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-124">a.</span></span> <span data-ttu-id="c3e1a-125">Välj **klona URI**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="c3e1a-126">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-126">b.</span></span> <span data-ttu-id="c3e1a-127">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-127">Click **Next**.</span></span>

   ![Välj Lagerkälla sida][CL03]

1. <span data-ttu-id="c3e1a-129">På den **Git-Lagerkälla** sidan:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-129">On the **Source Git Repository** page:</span></span>

   <span data-ttu-id="c3e1a-130">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-130">a.</span></span> <span data-ttu-id="c3e1a-131">För **URI**, ange `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="c3e1a-132">Det här steget ska automatiskt fylla i den **värden** och **databassökvägen** fält med rätt värden.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-132">This step should automatically populate the **Host** and **Repository path** fields with the correct values.</span></span>
   
   <span data-ttu-id="c3e1a-133">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-133">b.</span></span> <span data-ttu-id="c3e1a-134">Källan Start-databasen är offentliga så bör du inte ange Git-användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-134">The Spring Boot repository is public, so you should not have to enter your Git username and password.</span></span>
   
   <span data-ttu-id="c3e1a-135">c.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-135">c.</span></span> <span data-ttu-id="c3e1a-136">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-136">Click **Next**.</span></span>

   ![Datakälla på sidan för Git-lagringsplats][CL04]

1. <span data-ttu-id="c3e1a-138">På den **val av gren** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-138">On the **Branch Selection** page, click **Next**.</span></span>

   ![Sidan för val av gren][CL05]

1. <span data-ttu-id="c3e1a-140">På den **lokal Destination** sidan:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-140">On the **Local Destination** page:</span></span>

   <span data-ttu-id="c3e1a-141">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-141">a.</span></span> <span data-ttu-id="c3e1a-142">Ange den lokala mappen där du vill att din lokala lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-142">Specify the local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="c3e1a-143">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-143">b.</span></span> <span data-ttu-id="c3e1a-144">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-144">Click **Next**.</span></span>

   ![Lokala målsida][CL06]

1. <span data-ttu-id="c3e1a-146">På den **Välj en guide för importprojekt** sidan:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-146">On the **Select a wizard to use for importing projects** page:</span></span>

   <span data-ttu-id="c3e1a-147">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-147">a.</span></span> <span data-ttu-id="c3e1a-148">Välj **Import som ett allmänt projekt**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="c3e1a-149">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-149">b.</span></span> <span data-ttu-id="c3e1a-150">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-150">Click **Next**.</span></span>

   ![På sidan ”Välj en guide för importprojekt”][CL07]

1. <span data-ttu-id="c3e1a-152">På den **Importera projekt** sidan:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-152">On the **Import Projects** page:</span></span>

   <span data-ttu-id="c3e1a-153">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-153">a.</span></span> <span data-ttu-id="c3e1a-154">Ange ditt projektnamn.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-154">Specify your project name.</span></span>
   
   <span data-ttu-id="c3e1a-155">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-155">b.</span></span> <span data-ttu-id="c3e1a-156">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-156">Click **Finish**.</span></span>

   ![Importera projekt sida][CL08]

1. <span data-ttu-id="c3e1a-158">När databasen är klonad lyckas, ser du alla filer i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-158">When the repository is cloned successfully, you see all the files listed in Eclipse.</span></span>

   ![Lokal databas][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="c3e1a-160">Skapa ett Maven-projekt från din lokala databas</span><span class="sxs-lookup"><span data-stu-id="c3e1a-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="c3e1a-161">Källan Start Docker-databasen innehåller en slutförd Maven-projekt som du ska använda för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-161">The Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="c3e1a-162">Klicka på **filen** > **importera**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-162">Click **File** > **Import**.</span></span>

   ![Importkommandot på Arkiv-menyn][CL01]

1. <span data-ttu-id="c3e1a-164">När den **importera** öppnas:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-164">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="c3e1a-165">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-165">a.</span></span> <span data-ttu-id="c3e1a-166">Expandera **Maven**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="c3e1a-167">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-167">b.</span></span> <span data-ttu-id="c3e1a-168">Välj **befintliga Maven-projekt**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="c3e1a-169">c.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-169">c.</span></span> <span data-ttu-id="c3e1a-170">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-170">Click **Next**.</span></span>

   ![Dialogrutan Importera][MV01]

1. <span data-ttu-id="c3e1a-172">På den **Maven-projekt** sidan:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-172">On the **Maven Projects** page:</span></span>

   <span data-ttu-id="c3e1a-173">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-173">a.</span></span> <span data-ttu-id="c3e1a-174">För **rotkatalog**, ange den **fullständig** mappen i lokala databasen.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-174">For **Root Directory**, specify the **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="c3e1a-175">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-175">b.</span></span> <span data-ttu-id="c3e1a-176">Expandera den **Avancerat** , och ange ett eget namn för **mall**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-176">Expand the **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="c3e1a-177">c.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-177">c.</span></span> <span data-ttu-id="c3e1a-178">Markera kryssrutan för den **pom.xml** filen i projektet.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-178">Select the box for the **pom.xml** file in the project.</span></span>
   
   <span data-ttu-id="c3e1a-179">d.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-179">d.</span></span> <span data-ttu-id="c3e1a-180">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-180">Click **Finish**.</span></span>

   ![Sidan för maven-projekt][MV02]

1. <span data-ttu-id="c3e1a-182">När Maven-projekt öppnas har, kan du se ett andra projekt i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-182">When the Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![Lokala Maven-projekt][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="c3e1a-184">Skapa din app för vår start med hjälp av Maven</span><span class="sxs-lookup"><span data-stu-id="c3e1a-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="c3e1a-185">Välj Maven-projekt i Projektutforskaren Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-185">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="c3e1a-186">Klicka på **kör** > **kör som** > **Maven build**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![Skapa kommandon som ska köras som Maven][BU01]

1. <span data-ttu-id="c3e1a-188">När programmet har har skapat, visas statusen i konsolfönstret.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-188">When your application is successfully built, the console window shows the status.</span></span>

   ![Lyckad Maven-version][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="c3e1a-190">Publicera webbappen till Azure med hjälp av en dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="c3e1a-190">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="c3e1a-191">Välj Maven-projekt i Projektutforskaren Eclipse.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-191">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="c3e1a-192">Klicka på Azure **publicera** -menyn och klicka sedan på **Publicera som Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-192">Click the Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![Publicera som Dockerbehållare kommando][PU01]

1. <span data-ttu-id="c3e1a-194">När den **distribuera Dockerbehållare på Azure** dialogrutan visas:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-194">When the **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="c3e1a-195">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-195">a.</span></span> <span data-ttu-id="c3e1a-196">Ange ett eget namn för Docker-bild.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="c3e1a-197">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-197">b.</span></span> <span data-ttu-id="c3e1a-198">För **artefakt att distribuera**, ange sökvägen till den **gs-källan-Start-docker-0.1.0.jar** filen som du precis har skapat.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-198">For **Artifact to deploy**, specify the path to the **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Ange alternativ för Docker][PU02]

   <span data-ttu-id="c3e1a-200">Alla befintliga Docker-värdar visas.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="c3e1a-201">Om du vill distribuera till en befintlig värd kan du gå vidare till steg 5.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-201">If you choose to deploy to an existing host, you can skip to step 5.</span></span> <span data-ttu-id="c3e1a-202">Annars använder du följande steg för att skapa en värd:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-202">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="c3e1a-203">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-203">a.</span></span> <span data-ttu-id="c3e1a-204">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-204">Click **Add**.</span></span>

      ![Lägga till en ny Docker-värd][PU03]

   <span data-ttu-id="c3e1a-206">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-206">b.</span></span> <span data-ttu-id="c3e1a-207">När den **skapa Docker värden** dialogrutan visas, kan du acceptera standardinställningarna och du kan ange anpassade inställningar för din nya Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-207">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="c3e1a-208">(Detaljerade beskrivningar av olika inställningar finns i [publicera ett webbprogram som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ][Publish Container with Azure Toolkit].) Klicka på **nästa** när du har angett vilka inställningar som ska användas.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-208">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Ange alternativ för Docker-värden][PU04]

   <span data-ttu-id="c3e1a-210">c.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-210">c.</span></span> <span data-ttu-id="c3e1a-211">Du kan välja att använda befintliga autentiseringsuppgifter för inloggning från en Azure key vault eller du kan välja att ange nya autentiseringsuppgifter för Docker.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-211">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="c3e1a-212">Klicka på **Slutför** när du har angett dina alternativ.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-212">Click **Finish** when you have specified your options.</span></span>

      ![Ange autentiseringsuppgifter för Docker-värden][PU05]

1. <span data-ttu-id="c3e1a-214">Välj Docker-värd och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-214">Select your Docker host, and then click **Next**.</span></span>

   ![Välj Docker-värden ska använda][PU06]

1. <span data-ttu-id="c3e1a-216">På den sista sidan i den **distribuera Dockerbehållare på Azure** dialogrutan anger du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="c3e1a-216">On the last page of the **Deploying Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="c3e1a-217">a.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-217">a.</span></span> <span data-ttu-id="c3e1a-218">Du kan välja att ange ett eget namn för den behållare som är värd för din dockerbehållare eller du kan acceptera standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-218">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="c3e1a-219">b.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-219">b.</span></span> <span data-ttu-id="c3e1a-220">Ange TCP-portar för docker-värden med följande syntax: *[extern port]*:*[Intern port]*.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-220">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="c3e1a-221">Till exempel **80:8080** anger en extern port 80 och interna källan Start standardporten 8080.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-221">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="c3e1a-222">Om du har anpassat en intern port (t.ex, genom att redigera filen application.yml), måste du ange portnumret för rätt routning i Azure.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-222">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="c3e1a-223">c.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-223">c.</span></span> <span data-ttu-id="c3e1a-224">När du konfigurerar dessa alternativ klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-224">After you configure these options, click **Finish**.</span></span>

   ![Distribuera en dockerbehållare på Azure][PU07]

1. <span data-ttu-id="c3e1a-226">När Azure Toolkit har publicerats visas Azure-aktivitetsloggen **publicerade** status.</span><span class="sxs-lookup"><span data-stu-id="c3e1a-226">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![Distribuera Docker-värden][PU08]

## <a name="next-steps"></a><span data-ttu-id="c3e1a-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3e1a-228">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
<span data-ttu-id="c3e1a-229">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="c3e1a-229">[Docker]: https://www.docker.com/</span></span>
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
<span data-ttu-id="c3e1a-230">[Vårversionen Start]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="c3e1a-230">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="c3e1a-231">[Vårversionen Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="c3e1a-231">[Spring Framework]: https://spring.io/</span></span>

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
