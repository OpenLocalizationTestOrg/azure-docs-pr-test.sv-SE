---
title: "Publicera en app för vår start som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ | Microsoft Docs"
description: "Lär dig hur du publicerar ett webbprogram till Microsoft Azure som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ."
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
ms.openlocfilehash: b771238934183c953615ac33c42a275d80657556
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="66c5d-103">Publicera en app för vår start som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="66c5d-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="66c5d-104">Den [Vårversionen Framework] är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="66c5d-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="66c5d-105">En av de mer populära projekt som bygger som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.</span><span class="sxs-lookup"><span data-stu-id="66c5d-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="66c5d-106">[Docker] är en lösning för öppen källkod som gör att utvecklare kan automatisera distribution, skalning och hanteringen av sina program som körs i behållare.</span><span class="sxs-lookup"><span data-stu-id="66c5d-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="66c5d-107">Den här självstudiekursen vägleder dig genom stegen för att distribuera en källan startprogrammet som en dockerbehållare till Microsoft Azure med hjälp av Azure-verktyget för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="66c5d-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repo"></a><span data-ttu-id="66c5d-108">Klona lagringsplatsen standard källan Start Docker</span><span class="sxs-lookup"><span data-stu-id="66c5d-108">Clone the default Spring Boot Docker repo</span></span>

<span data-ttu-id="66c5d-109">Följande steg vägleder dig genom kloning källan Start Docker-lagringsplatsen med hjälp av IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="66c5d-109">The following steps walk you through cloning the Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="66c5d-110">Om du vill använda en kommandorad Se [distribuerar ett källan Start-program på Linux i Azure Container Service][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="66c5d-110">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="66c5d-111">Öppna IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="66c5d-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="66c5d-112">På välkomstskärmen väljer du den **GitHub** alternativet i den **checka ut från versionskontroll** lista.</span><span class="sxs-lookup"><span data-stu-id="66c5d-112">On the welcome screen, select the **GitHub** option in the **Check out from Version Control** list.</span></span>

   ![GitHub-alternativet för versionskontroll][CL01]

1. <span data-ttu-id="66c5d-114">Ange dina autentiseringsuppgifter om du uppmanas att logga in.</span><span class="sxs-lookup"><span data-stu-id="66c5d-114">Enter your credentials if you are prompted to log in.</span></span>

   * <span data-ttu-id="66c5d-115">Om du använder ett användarnamn/lösenord för att logga in på GitHub:</span><span class="sxs-lookup"><span data-stu-id="66c5d-115">If you are using a username/password to log in to GitHub:</span></span>

      ![Dialogrutan för att ange GitHub-användarnamn och lösenord][CL02a]

   * <span data-ttu-id="66c5d-117">Om du använder en token för att logga in på GitHub:</span><span class="sxs-lookup"><span data-stu-id="66c5d-117">If you are using a token to log in to GitHub:</span></span>

      ![Dialogrutan för att ange en GitHub-token][CL02b]

1. <span data-ttu-id="66c5d-119">Ange **https://github.com/spring-guides/gs-spring-boot-docker.git** Ange lokala sökvägen och mappinformation för lagringsplatsen-URL och klicka sedan på **klona**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for the repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![Klona lagringsplatsen dialogrutan][CL03]

1. <span data-ttu-id="66c5d-121">När du uppmanas att skapa ett projekt för IntelliJ Välj **nr**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-121">When you're prompted to create an IntelliJ project, select **No**.</span></span>

   ![Acceptera att skapa ett projekt för IntelliJ][CL04]

1. <span data-ttu-id="66c5d-123">På sidan Välkommen **Importera projekt**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-123">On the welcome page, click **Import Project**.</span></span>

   ![Val av Importera projekt][CL05]

1. <span data-ttu-id="66c5d-125">Leta upp den sökväg där du klona lagringsplatsen källan Start, Välj den **fullständig** mapp under roten och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-125">Locate the path where you cloned the Spring Boot repo, select the **complete** folder under the root, and then click **OK**.</span></span>

   ![Välj en mapp för import][CL06]

1. <span data-ttu-id="66c5d-127">När du uppmanas, välja **skapa projekt från befintliga datakällor**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![Alternativet för att skapa ett projekt från befintliga datakällor][CL07]

1. <span data-ttu-id="66c5d-129">Ange ditt projektnamn eller acceptera standardinställningarna, kontrollera att rätt sökväg till den **fullständig** mappen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-129">Specify your project name or accept the default, verify the correct path to the **complete** folder, and then click **Next**.</span></span>

   ![Ange projektets namn][CL08]

1. <span data-ttu-id="66c5d-131">Anpassa alla kataloger för att importera och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![Välj kataloger][CL09]

1. <span data-ttu-id="66c5d-133">Granska bibliotek för att importera och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-133">Review the libraries to import, and then click **Next**.</span></span>

   ![Granska projektet bibliotek][CL10]

1. <span data-ttu-id="66c5d-135">Granska modulstrukturen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-135">Review the module structure, and then click **Next**.</span></span>

   ![Granska modulstrukturen][CL11]

1. <span data-ttu-id="66c5d-137">Ange din JDK och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-137">Specify your JDK, and then click **Next**.</span></span>

   ![Ange en JDK][CL12]

1. <span data-ttu-id="66c5d-139">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-139">Click **Finish**.</span></span>

   ![Slutför][CL13]

<span data-ttu-id="66c5d-141">IntelliJ importerar källan Start-appen som ett projekt och visar strukturen när importen är klar.</span><span class="sxs-lookup"><span data-stu-id="66c5d-141">IntelliJ imports the Spring Boot app as a project and displays the structure when the import has finished.</span></span>

![Källan Start app i IntelliJ][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="66c5d-143">Skapa din källan Start app</span><span class="sxs-lookup"><span data-stu-id="66c5d-143">Build your Spring Boot app</span></span>

### <a name="build-the-app-by-using-the-maven-pom"></a><span data-ttu-id="66c5d-144">Bygga appen med hjälp av Maven POM</span><span class="sxs-lookup"><span data-stu-id="66c5d-144">Build the app by using the Maven POM</span></span>

1. <span data-ttu-id="66c5d-145">Öppna fönstret Maven-verktyget om den inte redan är öppen.</span><span class="sxs-lookup"><span data-stu-id="66c5d-145">Open the Maven tool window if it is not already opened.</span></span> <span data-ttu-id="66c5d-146">Klicka på **visa** > **verktyget Windows** > **Maven-projekt**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![Kommandona verktyget Windows och Maven-projekt][BU01]

1. <span data-ttu-id="66c5d-148">Högerklicka i fönstret Maven verktyget **paketet** och välj **kör Maven skapa**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-148">In the Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="66c5d-149">(Om Maven-projekt inte visas automatiskt, klickar du på den **importera** ikonen i verktygsfältet Maven.)</span><span class="sxs-lookup"><span data-stu-id="66c5d-149">(If your Maven project does not show up automatically, click the **Reimport** icon on the Maven toolbar.)</span></span>

   ![Kör skapa Maven-kommando][BU02]

1. <span data-ttu-id="66c5d-151">IntelliJ ska visa en **skapa lyckade** meddelande när appen källan Start har skapats.</span><span class="sxs-lookup"><span data-stu-id="66c5d-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![Skapa meddelande][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="66c5d-153">Skapa en distribution redo artefakt</span><span class="sxs-lookup"><span data-stu-id="66c5d-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="66c5d-154">Om du vill publicera appen källan Start måste du skapa en distribution redo artefakt.</span><span class="sxs-lookup"><span data-stu-id="66c5d-154">To publish your Spring Boot app, you need to create a deployment-ready artifact.</span></span> <span data-ttu-id="66c5d-155">Använd följande steg:</span><span class="sxs-lookup"><span data-stu-id="66c5d-155">Use the following steps:</span></span>

1. <span data-ttu-id="66c5d-156">Öppna din webbappsprojektet i IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="66c5d-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="66c5d-157">Klicka på **filen**, och klicka sedan på **projektstruktur**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-157">Click **File**, and then click **Project Structure**.</span></span>

   ![Projektet struktur kommando][ART01]

1. <span data-ttu-id="66c5d-159">Klicka på den gröna plus (**+**) symbolen om du vill lägga till en artefakt klickar du på **JAR**, och klicka sedan på **tom**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-159">Click the green plus (**+**) symbol to add an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![Lägg till en artefakt][ART02]

1. <span data-ttu-id="66c5d-161">Namnge din artefakt samtidigt till att du inte lägga till filnamnstillägget ”.jar” och ange målmappen för Maven-utdata.</span><span class="sxs-lookup"><span data-stu-id="66c5d-161">Name your artifact while making sure not to add the ".jar" extension, and then specify the target folder for the Maven output.</span></span>

   ![Ange egenskaper för artefakt][ART03]

1. <span data-ttu-id="66c5d-163">Skapa ett manifest för din artefakt (valfritt):</span><span class="sxs-lookup"><span data-stu-id="66c5d-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="66c5d-164">a.</span><span class="sxs-lookup"><span data-stu-id="66c5d-164">a.</span></span> <span data-ttu-id="66c5d-165">Klicka på **skapa manifestet**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-165">Click **Create Manifest**.</span></span>

      ![Klicka på knappen Skapa Manifest][ART04a]

   <span data-ttu-id="66c5d-167">b.</span><span class="sxs-lookup"><span data-stu-id="66c5d-167">b.</span></span> <span data-ttu-id="66c5d-168">Välj standardsökvägen för artefakten och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-168">Choose the default path for the artifact, and then click **OK**.</span></span>

      ![Ange artefakt sökväg][ART04b]

   <span data-ttu-id="66c5d-170">c.</span><span class="sxs-lookup"><span data-stu-id="66c5d-170">c.</span></span> <span data-ttu-id="66c5d-171">Klicka på ellipsknappen (**...** ) att hitta klassen huvudsakliga.</span><span class="sxs-lookup"><span data-stu-id="66c5d-171">Click the ellipsis (**...**) to locate the main class.</span></span>

      ![Leta upp huvudsakliga klass][ART04c]

   <span data-ttu-id="66c5d-173">d.</span><span class="sxs-lookup"><span data-stu-id="66c5d-173">d.</span></span> <span data-ttu-id="66c5d-174">Välj huvudklassen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-174">Choose your main class, and then click **OK**.</span></span>

      ![Ange huvudsakliga klass][ART04d]

1. <span data-ttu-id="66c5d-176">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-176">Click **OK**.</span></span>

   ![Stäng dialogrutan projektstruktur][ART05]

> [!NOTE]
> <span data-ttu-id="66c5d-178">Läs mer om hur du skapar artefakter i IntelliJ [konfigurera artefakter] på webbplatsen JetBrains.</span><span class="sxs-lookup"><span data-stu-id="66c5d-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on the JetBrains website.</span></span>
>

### <a name="build-the-artifact-for-deployment"></a><span data-ttu-id="66c5d-179">Skapa artefakt för distribution</span><span class="sxs-lookup"><span data-stu-id="66c5d-179">Build the artifact for deployment</span></span>

1. <span data-ttu-id="66c5d-180">Klicka på **skapa**, och klicka sedan på **artefakter**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![Skapa artefakter kommando][BU04]

1. <span data-ttu-id="66c5d-182">När den **skapa artefakt** snabbmenyn visas klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-182">When the **Build Artifact** context menu appears, click **Build**.</span></span>

   ![Skapa artefakt snabbmenyn][BU05]

<span data-ttu-id="66c5d-184">IntelliJ ska visa slutförda artefakten för vår Start-app i projektfönstret-verktyget.</span><span class="sxs-lookup"><span data-stu-id="66c5d-184">IntelliJ should display the completed artifact for your Spring Boot app in the project tool window.</span></span>

   ![Skapade artefakt][BU06]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="66c5d-186">Publicera webbappen till Azure med hjälp av en dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="66c5d-186">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="66c5d-187">Om du inte har loggat in på Azure-konto, följer du stegen i [inloggning instruktioner för Azure-Toolkit för IntelliJ][Azure Sign In for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="66c5d-187">If you have not signed in to your Azure account, follow the steps in [Sign-in instructions for the Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="66c5d-188">I verktyget Projektutforskaren, högerklicka på projektet och välj sedan **Azure** > **Publicera som Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-188">In the Project Explorer tool window, right-click the project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![Publicera som Dockerbehållare kommando][PU01]

1. <span data-ttu-id="66c5d-190">När den **distribuera Dockerbehållare på Azure** dialogruta visas alla befintliga Docker-värdar.</span><span class="sxs-lookup"><span data-stu-id="66c5d-190">When the **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="66c5d-191">Om du vill distribuera till en befintlig värd kan du gå vidare till steg 4.</span><span class="sxs-lookup"><span data-stu-id="66c5d-191">If you choose to deploy to an existing host, you can skip to step 4.</span></span> <span data-ttu-id="66c5d-192">Annars använder du följande steg för att skapa en värd:</span><span class="sxs-lookup"><span data-stu-id="66c5d-192">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="66c5d-193">a.</span><span class="sxs-lookup"><span data-stu-id="66c5d-193">a.</span></span> <span data-ttu-id="66c5d-194">Klicka på den gröna plus (**+**) symbolen.</span><span class="sxs-lookup"><span data-stu-id="66c5d-194">Click the green plus (**+**) symbol.</span></span>

      ![Lägga till en ny Docker-värd][PU02]

   <span data-ttu-id="66c5d-196">b.</span><span class="sxs-lookup"><span data-stu-id="66c5d-196">b.</span></span> <span data-ttu-id="66c5d-197">När den **skapa Docker värden** dialogrutan visas, kan du acceptera standardinställningarna och du kan ange anpassade inställningar för din nya Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="66c5d-197">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="66c5d-198">(Detaljerade beskrivningar av olika inställningar finns i [publicera ett webbprogram som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ][Publish Container with Azure Toolkit].) Klicka på **nästa** när du har angett vilka inställningar som ska användas.</span><span class="sxs-lookup"><span data-stu-id="66c5d-198">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Ange alternativ för Docker-värden][PU03a]

   <span data-ttu-id="66c5d-200">c.</span><span class="sxs-lookup"><span data-stu-id="66c5d-200">c.</span></span> <span data-ttu-id="66c5d-201">Du kan välja att använda befintliga autentiseringsuppgifter för inloggning från en Azure key vault eller du kan välja att ange nya autentiseringsuppgifter för Docker.</span><span class="sxs-lookup"><span data-stu-id="66c5d-201">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="66c5d-202">Klicka på **Slutför** när du har angett dina alternativ.</span><span class="sxs-lookup"><span data-stu-id="66c5d-202">Click **Finish** when you have specified your options.</span></span>

      ![Ange autentiseringsuppgifter för Docker-värden][PU03b]

1. <span data-ttu-id="66c5d-204">Välj Docker-värd och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-204">Select your Docker host, and then click **Next**.</span></span>

   ![Välj Docker-värden ska använda][PU04]

1. <span data-ttu-id="66c5d-206">På den sista sidan i den **distribuera Dockerbehållare på Azure** dialogrutan anger du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="66c5d-206">On the last page of the **Deploy Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="66c5d-207">a.</span><span class="sxs-lookup"><span data-stu-id="66c5d-207">a.</span></span> <span data-ttu-id="66c5d-208">Du kan välja att ange ett eget namn för den behållare som är värd för din dockerbehållare eller du kan acceptera standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="66c5d-208">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="66c5d-209">b.</span><span class="sxs-lookup"><span data-stu-id="66c5d-209">b.</span></span> <span data-ttu-id="66c5d-210">Ange TCP-portar för docker-värden med följande syntax: *[extern port]*:*[Intern port]*.</span><span class="sxs-lookup"><span data-stu-id="66c5d-210">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="66c5d-211">Till exempel **80:8080** anger en extern port 80 och interna källan Start standardporten 8080.</span><span class="sxs-lookup"><span data-stu-id="66c5d-211">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="66c5d-212">Om du har anpassat en intern port (t.ex, genom att redigera filen application.yml), måste du ange portnumret för rätt routning i Azure.</span><span class="sxs-lookup"><span data-stu-id="66c5d-212">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="66c5d-213">c.</span><span class="sxs-lookup"><span data-stu-id="66c5d-213">c.</span></span> <span data-ttu-id="66c5d-214">När du har konfigurerat dessa alternativ klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="66c5d-214">After you have configured these options, click **Finish**.</span></span>

   ![Distribuera en dockerbehållare på Azure][PU05]

1. <span data-ttu-id="66c5d-216">När Azure Toolkit har publicerats visas Azure-aktivitetsloggen **publicerade** status.</span><span class="sxs-lookup"><span data-stu-id="66c5d-216">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![Distribuera Docker-värden][PU06]

## <a name="next-steps"></a><span data-ttu-id="66c5d-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66c5d-218">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="66c5d-219">Läs om ytterligare metoder för att skapa källan Start appar med hjälp av IntelliJ i [skapa källan Start projekt](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) på webbplatsen JetBrains.</span><span class="sxs-lookup"><span data-stu-id="66c5d-219">To learn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on the JetBrains website.</span></span>

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
<span data-ttu-id="66c5d-220">[konfigurera artefakter]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span><span class="sxs-lookup"><span data-stu-id="66c5d-220">[Configuring Artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span></span>
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
<span data-ttu-id="66c5d-221">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="66c5d-221">[Docker]: https://www.docker.com/</span></span>
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
<span data-ttu-id="66c5d-222">[Vårversionen Start]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="66c5d-222">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="66c5d-223">[Vårversionen Framework]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="66c5d-223">[Spring Framework]: https://spring.io/</span></span>

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL01.png
[CL02a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02a.png
[CL02b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL02b.png
[CL03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL09.png
[CL10]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL10.png
[CL11]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL11.png
[CL12]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL12.png
[CL13]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL13.png
[CL14]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/CL14.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART03.png
[ART04a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04a.png
[ART04b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04b.png
[ART04c]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04c.png
[ART04d]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART04d.png
[ART05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/ART05.png

[BU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU02.png
[BU03]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU03.png
[BU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU04.png
[BU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU05.png
[BU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/BU06.png

[PU01]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU02.png
[PU03a]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03a.png
[PU03b]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU03b.png
[PU04]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-intellij-publish-spring-boot-docker-app/PU06.png
