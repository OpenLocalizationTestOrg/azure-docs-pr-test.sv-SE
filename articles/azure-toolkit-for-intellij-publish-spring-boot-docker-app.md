---
title: "aaaPublish en start av vår app som en dockerbehållare med hjälp av hello Azure Toolkit för IntelliJ | Microsoft Docs"
description: "Lär dig hur toopublish en web app tooMicrosoft Azure som en dockerbehållare med hjälp av hello Azure Toolkit för IntelliJ."
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
ms.openlocfilehash: 8964cb33fd8f61a39f091633ae9074d9658232fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a><span data-ttu-id="bfd33-103">Publicera en start av vår app som en dockerbehållare med hello Azure Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="bfd33-103">Publish a Spring Boot app as a Docker container by using hello Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="bfd33-104">Hej [Vårversionen Framework] är en öppen källkod som hjälper till att Java-utvecklare som skapar program på företagsnivå.</span><span class="sxs-lookup"><span data-stu-id="bfd33-104">hello [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="bfd33-105">En mer populära hello-projekt som är inbyggd som plattform är [Vårversionen Start], vilket ger en förenklad metod för att skapa fristående Java-program.</span><span class="sxs-lookup"><span data-stu-id="bfd33-105">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="bfd33-106">[Docker] är en lösning för öppen källkod som gör att utvecklare kan automatisera hello distribution, skalning och hanteringen av sina program som körs i behållare.</span><span class="sxs-lookup"><span data-stu-id="bfd33-106">[Docker] is an open-source solution that helps developers automate hello deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="bfd33-107">Den här självstudiekursen vägleder dig genom hello steg toodeploy en källan startprogrammet som en Docker-behållare tooMicrosoft Azure med hjälp av hello Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="bfd33-107">This tutorial walks you through hello steps toodeploy a Spring Boot application as a Docker container tooMicrosoft Azure by using hello Azure Toolkit for IntelliJ.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repo"></a><span data-ttu-id="bfd33-108">Klona hello standard källan Start Docker lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="bfd33-108">Clone hello default Spring Boot Docker repo</span></span>

<span data-ttu-id="bfd33-109">hello vägleder följande steg dig genom kloning hello källan Start Docker lagringsplatsen med hjälp av IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="bfd33-109">hello following steps walk you through cloning hello Spring Boot Docker repo by using IntelliJ.</span></span> <span data-ttu-id="bfd33-110">Om du vill toouse kommandoraden finns [distribuerar ett källan Start-program på Linux i Azure Container Service][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="bfd33-110">If you want toouse a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="bfd33-111">Öppna IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="bfd33-111">Open IntelliJ.</span></span>

1. <span data-ttu-id="bfd33-112">På välkomstskärmen hello väljer hello **GitHub** alternativ i hello **checka ut från versionskontroll** lista.</span><span class="sxs-lookup"><span data-stu-id="bfd33-112">On hello welcome screen, select hello **GitHub** option in hello **Check out from Version Control** list.</span></span>

   ![GitHub-alternativet för versionskontroll][CL01]

1. <span data-ttu-id="bfd33-114">Ange dina autentiseringsuppgifter om du tillfrågas toolog i.</span><span class="sxs-lookup"><span data-stu-id="bfd33-114">Enter your credentials if you are prompted toolog in.</span></span>

   * <span data-ttu-id="bfd33-115">Om du använder en användarnamn/lösenord toolog i tooGitHub:</span><span class="sxs-lookup"><span data-stu-id="bfd33-115">If you are using a username/password toolog in tooGitHub:</span></span>

      ![Dialogrutan för att ange GitHub-användarnamn och lösenord][CL02a]

   * <span data-ttu-id="bfd33-117">Om du använder en token toolog i tooGitHub:</span><span class="sxs-lookup"><span data-stu-id="bfd33-117">If you are using a token toolog in tooGitHub:</span></span>

      ![Dialogrutan för att ange en GitHub-token][CL02b]

1. <span data-ttu-id="bfd33-119">Ange **https://github.com/spring-guides/gs-spring-boot-docker.git** Ange lokala sökvägen och mappinformation för hello lagringsplatsen URL och klicka sedan på **klona**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-119">Enter **https://github.com/spring-guides/gs-spring-boot-docker.git** for hello repo URL, specify your local path and folder information, and then click **Clone**.</span></span>

   ![Klona lagringsplatsen dialogrutan][CL03]

1. <span data-ttu-id="bfd33-121">När du uppmanas toocreate en IntelliJ projektet, Välj **nr**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-121">When you're prompted toocreate an IntelliJ project, select **No**.</span></span>

   ![Avböja toocreate IntelliJ-projekt][CL04]

1. <span data-ttu-id="bfd33-123">Klicka på välkomstsidan hello **Importera projekt**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-123">On hello welcome page, click **Import Project**.</span></span>

   ![Val av Importera projekt][CL05]

1. <span data-ttu-id="bfd33-125">Hitta hello sökväg där du har klonat hello källan Start lagringsplatsen, Välj hello **fullständig** mapp under hello rot och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-125">Locate hello path where you cloned hello Spring Boot repo, select hello **complete** folder under hello root, and then click **OK**.</span></span>

   ![Välj en mapp för import][CL06]

1. <span data-ttu-id="bfd33-127">När du uppmanas, välja **skapa projekt från befintliga datakällor**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-127">When you're prompted, select **Create project from existing sources**.</span></span>

   ![Alternativet toocreate ett projekt från befintliga datakällor][CL07]

1. <span data-ttu-id="bfd33-129">Ange ditt projektnamn eller accepterar standardinställningen hello, kontrollera hello rätt sökväg toohello **fullständig** mappen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-129">Specify your project name or accept hello default, verify hello correct path toohello **complete** folder, and then click **Next**.</span></span>

   ![Ange hello projektnamn][CL08]

1. <span data-ttu-id="bfd33-131">Anpassa alla kataloger för att importera och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-131">Customize any directories for importing, and then click **Next**.</span></span>

   ![Välj kataloger][CL09]

1. <span data-ttu-id="bfd33-133">Granska hello bibliotek tooimport och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-133">Review hello libraries tooimport, and then click **Next**.</span></span>

   ![Granska projektet bibliotek][CL10]

1. <span data-ttu-id="bfd33-135">Granska hello modulstrukturen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-135">Review hello module structure, and then click **Next**.</span></span>

   ![Granska modulstrukturen][CL11]

1. <span data-ttu-id="bfd33-137">Ange din JDK och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-137">Specify your JDK, and then click **Next**.</span></span>

   ![Ange en JDK][CL12]

1. <span data-ttu-id="bfd33-139">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-139">Click **Finish**.</span></span>

   ![Slutför][CL13]

<span data-ttu-id="bfd33-141">IntelliJ importerar hello källan Start appen som ett projekt och visar hello struktur när hello importen är klar.</span><span class="sxs-lookup"><span data-stu-id="bfd33-141">IntelliJ imports hello Spring Boot app as a project and displays hello structure when hello import has finished.</span></span>

![Källan Start app i IntelliJ][CL14]

## <a name="build-your-spring-boot-app"></a><span data-ttu-id="bfd33-143">Skapa din källan Start app</span><span class="sxs-lookup"><span data-stu-id="bfd33-143">Build your Spring Boot app</span></span>

### <a name="build-hello-app-by-using-hello-maven-pom"></a><span data-ttu-id="bfd33-144">Skapa hello program med hjälp av hello Maven POM</span><span class="sxs-lookup"><span data-stu-id="bfd33-144">Build hello app by using hello Maven POM</span></span>

1. <span data-ttu-id="bfd33-145">Öppna hello Maven verktygsfönster om den inte redan är öppen.</span><span class="sxs-lookup"><span data-stu-id="bfd33-145">Open hello Maven tool window if it is not already opened.</span></span> <span data-ttu-id="bfd33-146">Klicka på **visa** > **verktyget Windows** > **Maven-projekt**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-146">Click **View** > **Tool Windows** > **Maven Projects**.</span></span>

   ![Kommandona verktyget Windows och Maven-projekt][BU01]

1. <span data-ttu-id="bfd33-148">I hello Maven verktyget fönstret högerklickar du på **paketet** och välj **kör Maven skapa**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-148">In hello Maven tool window, right-click **package** and select **Run Maven Build**.</span></span> <span data-ttu-id="bfd33-149">(Om Maven-projekt inte visas automatiskt, klickar du på hello **importera** ikonen i verktygsfältet för hello Maven.)</span><span class="sxs-lookup"><span data-stu-id="bfd33-149">(If your Maven project does not show up automatically, click hello **Reimport** icon on hello Maven toolbar.)</span></span>

   ![Kör skapa Maven-kommando][BU02]

1. <span data-ttu-id="bfd33-151">IntelliJ ska visa en **skapa lyckade** meddelande när appen källan Start har skapats.</span><span class="sxs-lookup"><span data-stu-id="bfd33-151">IntelliJ should display a **BUILD SUCCESS** message when your Spring Boot app is successfully created.</span></span>

   ![Skapa meddelande][BU03]

### <a name="create-a-deployment-ready-artifact"></a><span data-ttu-id="bfd33-153">Skapa en distribution redo artefakt</span><span class="sxs-lookup"><span data-stu-id="bfd33-153">Create a deployment-ready artifact</span></span>

<span data-ttu-id="bfd33-154">toopublish appen källan Start måste toocreate en artefakt distribution är redo.</span><span class="sxs-lookup"><span data-stu-id="bfd33-154">toopublish your Spring Boot app, you need toocreate a deployment-ready artifact.</span></span> <span data-ttu-id="bfd33-155">Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bfd33-155">Use hello following steps:</span></span>

1. <span data-ttu-id="bfd33-156">Öppna din webbappsprojektet i IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="bfd33-156">Open your web app project in IntelliJ.</span></span>

1. <span data-ttu-id="bfd33-157">Klicka på **filen**, och klicka sedan på **projektstruktur**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-157">Click **File**, and then click **Project Structure**.</span></span>

   ![Projektet struktur kommando][ART01]

1. <span data-ttu-id="bfd33-159">Klicka på hello grön plus (**+**) symbol tooadd en artefakt klickar du på **JAR**, och klicka sedan på **tom**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-159">Click hello green plus (**+**) symbol tooadd an artifact, click **JAR**, and then click **Empty**.</span></span>

   ![Lägg till en artefakt][ART02]

1. <span data-ttu-id="bfd33-161">Namnge din artefakt samtidigt inte tooadd hello ”.jar” tillägg och sedan ange hello målmappen för hello Maven utdata.</span><span class="sxs-lookup"><span data-stu-id="bfd33-161">Name your artifact while making sure not tooadd hello ".jar" extension, and then specify hello target folder for hello Maven output.</span></span>

   ![Ange egenskaper för artefakt][ART03]

1. <span data-ttu-id="bfd33-163">Skapa ett manifest för din artefakt (valfritt):</span><span class="sxs-lookup"><span data-stu-id="bfd33-163">Create a manifest for your artifact (optional):</span></span>

   <span data-ttu-id="bfd33-164">a.</span><span class="sxs-lookup"><span data-stu-id="bfd33-164">a.</span></span> <span data-ttu-id="bfd33-165">Klicka på **skapa manifestet**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-165">Click **Create Manifest**.</span></span>

      ![Klicka på hello skapa Manifest][ART04a]

   <span data-ttu-id="bfd33-167">b.</span><span class="sxs-lookup"><span data-stu-id="bfd33-167">b.</span></span> <span data-ttu-id="bfd33-168">Välj hello standardsökvägen för hello artefakt och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-168">Choose hello default path for hello artifact, and then click **OK**.</span></span>

      ![Ange artefakt sökväg][ART04b]

   <span data-ttu-id="bfd33-170">c.</span><span class="sxs-lookup"><span data-stu-id="bfd33-170">c.</span></span> <span data-ttu-id="bfd33-171">Klicka på ellipsknappen hello (**... **) toolocate hello huvudsakliga klass.</span><span class="sxs-lookup"><span data-stu-id="bfd33-171">Click hello ellipsis (**...**) toolocate hello main class.</span></span>

      ![Leta upp huvudsakliga klass][ART04c]

   <span data-ttu-id="bfd33-173">d.</span><span class="sxs-lookup"><span data-stu-id="bfd33-173">d.</span></span> <span data-ttu-id="bfd33-174">Välj huvudklassen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-174">Choose your main class, and then click **OK**.</span></span>

      ![Ange huvudsakliga klass][ART04d]

1. <span data-ttu-id="bfd33-176">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-176">Click **OK**.</span></span>

   ![Stäng dialogrutan för hello projektstruktur][ART05]

> [!NOTE]
> <span data-ttu-id="bfd33-178">Läs mer om hur du skapar artefakter i IntelliJ [konfigurera artefakter] på hello JetBrains webbplats.</span><span class="sxs-lookup"><span data-stu-id="bfd33-178">For more information about creating artifacts in IntelliJ, see [Configuring Artifacts] on hello JetBrains website.</span></span>
>

### <a name="build-hello-artifact-for-deployment"></a><span data-ttu-id="bfd33-179">Skapa hello artefakt för distribution</span><span class="sxs-lookup"><span data-stu-id="bfd33-179">Build hello artifact for deployment</span></span>

1. <span data-ttu-id="bfd33-180">Klicka på **skapa**, och klicka sedan på **artefakter**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-180">Click **Build**, and then click **Artifacts**.</span></span>

   ![Skapa artefakter kommando][BU04]

1. <span data-ttu-id="bfd33-182">När hello **skapa artefakt** snabbmenyn visas klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-182">When hello **Build Artifact** context menu appears, click **Build**.</span></span>

   ![Skapa artefakt snabbmenyn][BU05]

<span data-ttu-id="bfd33-184">IntelliJ ska visa hello slutförts artefakt för vår Start-app i hello projekt verktyget-fönstret.</span><span class="sxs-lookup"><span data-stu-id="bfd33-184">IntelliJ should display hello completed artifact for your Spring Boot app in hello project tool window.</span></span>

   ![Skapade artefakt][BU06]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="bfd33-186">Publicera din web app tooAzure med hjälp av en dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="bfd33-186">Publish your web app tooAzure by using a Docker container</span></span>

1. <span data-ttu-id="bfd33-187">Om du inte har loggat in tooyour Azure-konto, gör hello i [inloggning instruktioner för hello Azure Toolkit för IntelliJ][Azure Sign In for IntelliJ].</span><span class="sxs-lookup"><span data-stu-id="bfd33-187">If you have not signed in tooyour Azure account, follow hello steps in [Sign-in instructions for hello Azure Toolkit for IntelliJ][Azure Sign In for IntelliJ].</span></span>

1. <span data-ttu-id="bfd33-188">Högerklicka på hello-projekt i hello verktyget Projektutforskaren, och välj sedan **Azure** > **Publicera som Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-188">In hello Project Explorer tool window, right-click hello project, and then select **Azure** > **Publish as Docker Container**.</span></span>

   ![Publicera som Dockerbehållare kommando][PU01]

1. <span data-ttu-id="bfd33-190">När hello **distribuera Dockerbehållare på Azure** dialogruta visas alla befintliga Docker-värdar.</span><span class="sxs-lookup"><span data-stu-id="bfd33-190">When hello **Deploy Docker Container on Azure** dialog box appears, any existing Docker hosts are displayed.</span></span> <span data-ttu-id="bfd33-191">Om du väljer toodeploy tooan befintliga värden kan du hoppa över toostep 4.</span><span class="sxs-lookup"><span data-stu-id="bfd33-191">If you choose toodeploy tooan existing host, you can skip toostep 4.</span></span> <span data-ttu-id="bfd33-192">Annars Använd hello följande steg toocreate en värd:</span><span class="sxs-lookup"><span data-stu-id="bfd33-192">Otherwise, use hello following steps toocreate a host:</span></span>

   <span data-ttu-id="bfd33-193">a.</span><span class="sxs-lookup"><span data-stu-id="bfd33-193">a.</span></span> <span data-ttu-id="bfd33-194">Klicka på hello grön plus (**+**) symbolen.</span><span class="sxs-lookup"><span data-stu-id="bfd33-194">Click hello green plus (**+**) symbol.</span></span>

      ![Lägga till en ny Docker-värd][PU02]

   <span data-ttu-id="bfd33-196">b.</span><span class="sxs-lookup"><span data-stu-id="bfd33-196">b.</span></span> <span data-ttu-id="bfd33-197">När hello **skapa Docker värden** dialogrutan visas, kan du välja tooaccept hello standardinställningar eller du kan ange anpassade inställningar för din nya Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="bfd33-197">When hello **Create Docker Host** dialog box appears, you can choose tooaccept hello defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="bfd33-198">(Detaljerade beskrivningar av hello olika inställningar, finns i [publicera ett webbprogram som en dockerbehållare med hello Azure Toolkit för IntelliJ][Publish Container with Azure Toolkit].) Klicka på **nästa** när du har angett vilka inställningar toouse.</span><span class="sxs-lookup"><span data-stu-id="bfd33-198">(For detailed descriptions of hello various settings, see [Publish a web app as a Docker container by using hello Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings toouse.</span></span>

      ![Ange alternativ för Docker-värden][PU03a]

   <span data-ttu-id="bfd33-200">c.</span><span class="sxs-lookup"><span data-stu-id="bfd33-200">c.</span></span> <span data-ttu-id="bfd33-201">Du kan välja toouse befintliga autentiseringsuppgifter för inloggning från en Azure key vault eller du tooenter nya Docker-inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bfd33-201">You can choose toouse existing login credentials from an Azure key vault, or you can choose tooenter new Docker login credentials.</span></span> <span data-ttu-id="bfd33-202">Klicka på **Slutför** när du har angett dina alternativ.</span><span class="sxs-lookup"><span data-stu-id="bfd33-202">Click **Finish** when you have specified your options.</span></span>

      ![Ange autentiseringsuppgifter för Docker-värden][PU03b]

1. <span data-ttu-id="bfd33-204">Välj Docker-värd och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-204">Select your Docker host, and then click **Next**.</span></span>

   ![Välj hello Docker värden toouse][PU04]

1. <span data-ttu-id="bfd33-206">På hello sista sidan i hello **distribuera Dockerbehållare på Azure** dialogrutan Ange hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="bfd33-206">On hello last page of hello **Deploy Docker Container on Azure** dialog box, specify hello following options:</span></span>

   <span data-ttu-id="bfd33-207">a.</span><span class="sxs-lookup"><span data-stu-id="bfd33-207">a.</span></span> <span data-ttu-id="bfd33-208">Du kan välja toospecify ett eget namn för hello-behållare som är värd för din dockerbehållare eller du kan acceptera hello standard.</span><span class="sxs-lookup"><span data-stu-id="bfd33-208">You can choose toospecify a custom name for hello container that will host your Docker container, or you can accept hello default.</span></span>

   <span data-ttu-id="bfd33-209">b.</span><span class="sxs-lookup"><span data-stu-id="bfd33-209">b.</span></span> <span data-ttu-id="bfd33-210">Ange hello TCP-portar för docker-värden med hjälp av hello följande syntax: *[extern port]*:*[Intern port]*.</span><span class="sxs-lookup"><span data-stu-id="bfd33-210">Enter hello TCP ports for your docker host by using hello following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="bfd33-211">Till exempel **80:8080** anger ett extern port 80 och hello interna källan Start standardporten 8080.</span><span class="sxs-lookup"><span data-stu-id="bfd33-211">For example, **80:8080** specifies an external port of 80 and hello default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="bfd33-212">Om du har anpassat en intern port (t.ex, genom att redigera hello application.yml filen), behöver du toospecify hello-portnumret för hello rätt routning toooccur i Azure.</span><span class="sxs-lookup"><span data-stu-id="bfd33-212">If you have customized your internal port (for example, by editing hello application.yml file), you need toospecify hello port number for hello correct routing toooccur in Azure.</span></span>

   <span data-ttu-id="bfd33-213">c.</span><span class="sxs-lookup"><span data-stu-id="bfd33-213">c.</span></span> <span data-ttu-id="bfd33-214">När du har konfigurerat dessa alternativ klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="bfd33-214">After you have configured these options, click **Finish**.</span></span>

   ![Distribuera en dockerbehållare på Azure][PU05]

1. <span data-ttu-id="bfd33-216">När hello Azure Toolkit har publicerats hello Azure-aktivitetsloggen visar **publicerade** hello status.</span><span class="sxs-lookup"><span data-stu-id="bfd33-216">When hello Azure Toolkit has finished publishing, hello Azure Activity Log displays **Published** for hello status.</span></span>

   ![Distribuera Docker-värden][PU06]

## <a name="next-steps"></a><span data-ttu-id="bfd33-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bfd33-218">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="bfd33-219">toolearn om ytterligare metoder för att skapa källan Start appar med hjälp av IntelliJ, se [skapa källan Start projekt](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) på hello JetBrains webbplats.</span><span class="sxs-lookup"><span data-stu-id="bfd33-219">toolearn about additional methods for creating Spring Boot apps by using IntelliJ, see [Creating Spring Boot Projects](https://www.jetbrains.com/help/idea/creating-spring-boot-projects.html) on hello JetBrains website.</span></span>

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Sign In for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Konfigurera artefakter]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Källan Start]: http://projects.spring.io/spring-boot/
[Källan Framework]: https://spring.io/

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
