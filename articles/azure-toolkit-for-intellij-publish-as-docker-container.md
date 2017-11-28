---
title: "aaaPublish en dockerbehållare med hjälp av hello Azure Toolkit för IntelliJ | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: bee94cb269ea707ae7ad55232e23e915aec48c34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a><span data-ttu-id="66d33-103">Publicera ett webbprogram som en dockerbehållare med hello Azure Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="66d33-103">Publish a web app as a Docker container by using hello Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="66d33-104">Docker-behållare är en mycket vanlig metod för att distribuera webbprogram.</span><span class="sxs-lookup"><span data-stu-id="66d33-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="66d33-105">Genom att använda Docker-behållare kan konsolidera utvecklare sina projektfiler och beroenden i ett enda paket för distribution tooa server.</span><span class="sxs-lookup"><span data-stu-id="66d33-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment tooa server.</span></span> <span data-ttu-id="66d33-106">hello Azure Toolkit för IntelliJ förenklar processen för Java-utvecklare genom att lägga till *Publicera som Dockerbehållare* funktioner för distribution tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="66d33-106">hello Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment tooMicrosoft Azure.</span></span> <span data-ttu-id="66d33-107">Den här artikeln vägleder dig genom hello steg krävs toopublish program-tooAzure som Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="66d33-107">This article walks you through hello steps required toopublish your applications tooAzure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="66d33-108">Mer information om Docker är tillgänglig på hello [Docker webbplats].</span><span class="sxs-lookup"><span data-stu-id="66d33-108">More information about Docker is available on hello [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="66d33-109">Publicera din web app tooAzure med hjälp av en dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="66d33-109">Publish your web app tooAzure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="66d33-110">toopublish ditt webbprogram måste du skapa en distribution redo artefakt.</span><span class="sxs-lookup"><span data-stu-id="66d33-110">toopublish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="66d33-111">toolearn finns fler hello [ytterligare information om hur du skapar artefakter](#artifacts) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="66d33-111">toolearn more, see hello [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="66d33-112">När du har slutfört guiden för distribution av hello minst en gång, används de flesta av inställningarna som standard när du kör hello guiden igen.</span><span class="sxs-lookup"><span data-stu-id="66d33-112">After you have completed hello deployment wizard at least once, most of your settings are used as defaults when you run hello wizard again.</span></span>
>

1. <span data-ttu-id="66d33-113">Öppna din webbappsprojektet i IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="66d33-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="66d33-114">toostart hello **Publicera som Dockerbehållare** guiden gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="66d33-114">toostart hello **Publish as Docker Container** wizard, do either of hello following:</span></span>

   * <span data-ttu-id="66d33-115">I hello **projekt** verktyg fönstret, högerklicka på ditt projekt, klickar du på **Azure**, och klicka sedan på **Publicera som Dockerbehållare**:</span><span class="sxs-lookup"><span data-stu-id="66d33-115">In hello **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![hello Publicera som Dockerbehållare kommando][PUB01]

   * <span data-ttu-id="66d33-117">Hello på hello IntelliJ verktygsfältet **publicera grupp** knappen och klicka sedan på **Publicera som Dockerbehållare**:</span><span class="sxs-lookup"><span data-stu-id="66d33-117">On hello IntelliJ toolbar, click hello **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="66d33-118">![hello Publicera som Dockerbehållare kommando][PUB02]</span><span class="sxs-lookup"><span data-stu-id="66d33-118">![hello Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="66d33-119">Hej **distribuera Dockerbehållare på Azure** öppnas guiden.</span><span class="sxs-lookup"><span data-stu-id="66d33-119">hello **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![hello distribuera Dockerbehållare på Azure-guiden][PUB03]

3. <span data-ttu-id="66d33-121">I hello **skriver du ett namn, Välj hello artefakt sökväg och kontrollera en Docker värden toobe används** fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="66d33-121">In hello **Type an image name, select hello artifact's path and check a Docker host toobe used** window, do hello following:</span></span> 

   <span data-ttu-id="66d33-122">a.</span><span class="sxs-lookup"><span data-stu-id="66d33-122">a.</span></span> <span data-ttu-id="66d33-123">I hello **Docker avbildningsnamn** ange ett unikt namn för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-123">In hello **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="66d33-124">(hello guiden skapar automatiskt ett namn, men du kan ändra den.)</span><span class="sxs-lookup"><span data-stu-id="66d33-124">(hello wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="66d33-125">b.</span><span class="sxs-lookup"><span data-stu-id="66d33-125">b.</span></span> <span data-ttu-id="66d33-126">Hej **värdar** området visas alla Docker-värdar som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="66d33-126">hello **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="66d33-127">Gör något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="66d33-127">Do either of hello following:</span></span> 
      * <span data-ttu-id="66d33-128">Om du har en befintlig Docker-värd kan distribuera du din web app tooit.</span><span class="sxs-lookup"><span data-stu-id="66d33-128">If you have an existing Docker host, you can deploy your web app tooit.</span></span>
      * <span data-ttu-id="66d33-129">toocreate en Docker-värd, klicka hello grönt plustecken (**+**).</span><span class="sxs-lookup"><span data-stu-id="66d33-129">toocreate a Docker host, click hello green plus sign (**+**).</span></span>  
       <span data-ttu-id="66d33-130">Hej **skapa Docker värden** öppnas.</span><span class="sxs-lookup"><span data-stu-id="66d33-130">hello **Create Docker Host** dialog box opens.</span></span> 

      ![Distribuera Dockerbehållare på Azure-guiden][PUB04a]

4. <span data-ttu-id="66d33-132">I hello **konfigurera hello ny virtuell dator** fönstret Ange hello följande information om Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-132">In hello **Configure hello new virtual machine** window, provide hello following information about your Docker host.</span></span> <span data-ttu-id="66d33-133">(hello guiden genererar automatiskt de flesta av hello-information, men du kan ändra någon av dem.)</span><span class="sxs-lookup"><span data-stu-id="66d33-133">(hello wizard automatically generates most of hello information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="66d33-134">a.</span><span class="sxs-lookup"><span data-stu-id="66d33-134">a.</span></span> <span data-ttu-id="66d33-135">I hello **namn** ange ett unikt namn för hello Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-135">In hello **Name** box, enter a unique name for hello Docker host.</span></span> <span data-ttu-id="66d33-136">(Det är inte hello samma som hello Docker avbildningens namn som du angav tidigare.)</span><span class="sxs-lookup"><span data-stu-id="66d33-136">(It is not hello same as hello Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="66d33-137">b.</span><span class="sxs-lookup"><span data-stu-id="66d33-137">b.</span></span> <span data-ttu-id="66d33-138">I hello **prenumeration** ange hello Azure-prenumeration som du använder för värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-138">In hello **Subscription** box, enter hello Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="66d33-139">c.</span><span class="sxs-lookup"><span data-stu-id="66d33-139">c.</span></span> <span data-ttu-id="66d33-140">I hello **Region** ange hello geografisk region där värden finns.</span><span class="sxs-lookup"><span data-stu-id="66d33-140">In hello **Region** box, enter hello geographical region where your host is located.</span></span>
      
   <span data-ttu-id="66d33-141">d.</span><span class="sxs-lookup"><span data-stu-id="66d33-141">d.</span></span> <span data-ttu-id="66d33-142">På hello **OS- och** fliken, hello följande:</span><span class="sxs-lookup"><span data-stu-id="66d33-142">On hello **OS and Size** tab, do hello following:</span></span>      
      * <span data-ttu-id="66d33-143">**Värd för OS**: Ange hello operativsystemet för hello virtuell dator som innehåller värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-143">**Host OS**: Enter hello operating system for hello virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="66d33-144">**Storlek**: Ange hello storlek för virtuell dator för värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-144">**Size**: Enter hello virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="66d33-145">e.</span><span class="sxs-lookup"><span data-stu-id="66d33-145">e.</span></span> <span data-ttu-id="66d33-146">På hello **resursgruppen** väljer du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="66d33-146">On hello **Resource Group** tab, select either of hello following:</span></span>      
      * <span data-ttu-id="66d33-147">**Ny resursgrupp**: skapa en resursgrupp för värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="66d33-148">**Befintlig resursgrupp**: Ange en befintlig resursgrupp från ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="66d33-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="66d33-149">f.</span><span class="sxs-lookup"><span data-stu-id="66d33-149">f.</span></span> <span data-ttu-id="66d33-150">På hello **nätverk** väljer du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="66d33-150">On hello **Network** tab, select either of hello following:</span></span>      
      * <span data-ttu-id="66d33-151">**Nytt virtuellt nätverk**: skapa ett virtuellt nätverk för värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="66d33-152">**Befintligt virtuellt nätverk**: Ange ett befintligt virtuellt nätverk från ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="66d33-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="66d33-153">g.</span><span class="sxs-lookup"><span data-stu-id="66d33-153">g.</span></span> <span data-ttu-id="66d33-154">På hello **lagring** väljer du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="66d33-154">On hello **Storage** tab, select either of hello following:</span></span>      
      * <span data-ttu-id="66d33-155">**Nytt lagringskonto**: skapa ett lagringskonto för värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="66d33-156">**Befintligt lagringskonto**: Ange ett befintligt lagringskonto från Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="66d33-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="66d33-157">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="66d33-157">Click **Next**.</span></span>  
     <span data-ttu-id="66d33-158">Hej **Konfigurera logg i autentiseringsuppgifter och portinställningarna** öppnas.</span><span class="sxs-lookup"><span data-stu-id="66d33-158">hello **Configure log in credentials and port settings** window opens.</span></span>

      ![hello Konfigurera logg i autentiseringsuppgifter och port inställningar][PUB05]

6. <span data-ttu-id="66d33-160">Välj något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="66d33-160">Select one of hello following options:</span></span>

      * <span data-ttu-id="66d33-161">**Importera autentiseringsuppgifter från Azure Key Vault**: Ange en tidigare sparad uppsättning autentiseringsuppgifter som lagras i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="66d33-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="66d33-162">Ett Azure key vault som har skapats med ett visst konto eller tjänstens huvudnamn är inte tillgänglig automatiskt av ett annat konto eller tjänstens huvudnamn som delar hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="66d33-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares hello subscription.</span></span> <span data-ttu-id="66d33-163">tooallow ett annat konto eller service principal toouse hello nyckeln valvet, måste du använda hello Azure portal tooadd hello konto eller tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="66d33-163">tooallow another account or service principal toouse hello key vault, you must use hello Azure portal tooadd hello account or service principal.</span></span>

      * <span data-ttu-id="66d33-164">**Ny logg i autentiseringsuppgifter**: skapa en ny uppsättning autentiseringsuppgifter för inloggning.</span><span class="sxs-lookup"><span data-stu-id="66d33-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="66d33-165">Om du väljer det här alternativet hello följande:</span><span class="sxs-lookup"><span data-stu-id="66d33-165">If you select this option, do hello following:</span></span>

        <span data-ttu-id="66d33-166">a.</span><span class="sxs-lookup"><span data-stu-id="66d33-166">a.</span></span> <span data-ttu-id="66d33-167">På hello **VM autentiseringsuppgifter** och ange följande information för hello inloggningsuppgifterna för virtuella datorer i din Docker-värd hello: * **användarnamn**: Ange hello användarnamn för virtuell dator-inloggning autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="66d33-167">On hello **VM Credentials** tab, provide hello following information for hello virtual-machine login credentials of your Docker host: * **Username**: Enter hello username for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="66d33-168">* **Lösenordet** och **Bekräfta**: Ange hello lösenord för dina inloggningsuppgifter för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="66d33-168">* **Password** and **Confirm**: Enter hello password for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="66d33-169">* **SSH**: Ange inställningar för hello SSH (Secure Shell) för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="66d33-169">* **SSH**: Enter hello Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="66d33-170">Du kan välja något av följande alternativ för hello: * **ingen**: Anger att den virtuella datorn inte tillåter SSH-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="66d33-170">You can select one of hello following options: * **None**: Specifies that your virtual machine does not allow SSH connections.</span></span>
                <span data-ttu-id="66d33-171">* **Autogenerera**: skapar automatiskt hello nödvändiga inställningar för att ansluta via SSH.</span><span class="sxs-lookup"><span data-stu-id="66d33-171">* **Auto-generate**: Automatically creates hello requisite settings for connecting via SSH.</span></span>
                <span data-ttu-id="66d33-172">* **Importera från directory**: tillåter toospecify en katalog som innehåller en uppsättning tidigare sparade SSH-inställningar.</span><span class="sxs-lookup"><span data-stu-id="66d33-172">* **Import from directory**: Allows you toospecify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="66d33-173">hello directory måste innehålla hello följande två filer:</span><span class="sxs-lookup"><span data-stu-id="66d33-173">hello directory must contain hello following two files:</span></span>
                
                  * *id_rsa*: Contains hello RSA identification for a user.
                  * *id_rsa.pub*: Contains hello RSA public key that is used for authentication.
            
        <span data-ttu-id="66d33-174">b.</span><span class="sxs-lookup"><span data-stu-id="66d33-174">b.</span></span> <span data-ttu-id="66d33-175">På hello **Docker Daemon åtkomst** fliken, ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="66d33-175">On hello **Docker Daemon Access** tab, provide hello following information:</span></span>

          ![Skapa Docker-värd][PUB06]
    
             * **Docker Daemon port**: Enter hello unique TCP port for your Docker host.
             * **TLS Security**: Enter hello Transport Layer Security settings for your Docker host. You can choose from hello following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates hello requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. hello directory must contain hello following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain hello certificate and public key for hello TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain hello client certificate and public key that is used for TLS authentication.

7. <span data-ttu-id="66d33-177">När du har angett hello krävs information klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="66d33-177">After you have entered hello required information, click **Finish**.</span></span>  
    <span data-ttu-id="66d33-178">Hej **distribuera Dockerbehållare på Azure** guiden visas igen.</span><span class="sxs-lookup"><span data-stu-id="66d33-178">hello **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![Distribuera Dockerbehållare på Azure-guiden][PUB07]

8. <span data-ttu-id="66d33-180">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="66d33-180">Click **Next**.</span></span>  
    <span data-ttu-id="66d33-181">Hej **konfigurera hello Docker behållare toobe skapade** öppnas.</span><span class="sxs-lookup"><span data-stu-id="66d33-181">hello **Configure hello Docker container toobe created** window opens.</span></span>

   ![hello konfigurera hello Docker behållare toobe skapade fönster][PUB08]

9. <span data-ttu-id="66d33-183">I hello **konfigurera hello Docker behållare toobe skapade** fönstret Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="66d33-183">In hello **Configure hello Docker container toobe created** window, provide hello following information:</span></span> 

   <span data-ttu-id="66d33-184">a.</span><span class="sxs-lookup"><span data-stu-id="66d33-184">a.</span></span> <span data-ttu-id="66d33-185">I hello **Docker behållarnamn** ange ett unikt namn för din dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="66d33-185">In hello **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="66d33-186">b.</span><span class="sxs-lookup"><span data-stu-id="66d33-186">b.</span></span> <span data-ttu-id="66d33-187">Välj något av hello följande Docker bilder:</span><span class="sxs-lookup"><span data-stu-id="66d33-187">Choose one of hello following Docker images:</span></span> 

      * <span data-ttu-id="66d33-188">**Fördefinierade Docker bild**: Ange en befintlig Docker-avbildning från Azure.</span><span class="sxs-lookup"><span data-stu-id="66d33-188">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="66d33-189">hello listan med Docker bilder i den här rutan består av flera avbildningar som hello Azure Toolkit har konfigurerats toopatch så att din artefakt distribueras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="66d33-189">hello list of Docker images in this box consists of several images that hello Azure Toolkit has been configured toopatch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="66d33-190">**Anpassade Dockerfile**: Ange en tidigare sparad Dockerfile från den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="66d33-190">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="66d33-191">Detta är en avancerad funktion för utvecklare som vill toodeploy sina egna Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="66d33-191">This is a more advanced feature for developers who want toodeploy their own Dockerfile.</span></span> <span data-ttu-id="66d33-192">Dock är det upp toodevelopers som använder det här alternativet tooensure som deras Dockerfile är uppbyggd.</span><span class="sxs-lookup"><span data-stu-id="66d33-192">However, it is up toodevelopers who use this option tooensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="66d33-193">Eftersom hello Azure Toolkit inte kan valideras hello innehållet i en anpassad Dockerfile, misslyckas hello distributionen om hello Dockerfile har problem.</span><span class="sxs-lookup"><span data-stu-id="66d33-193">Because hello Azure Toolkit does not validate hello content in a custom Dockerfile, hello deployment might fail if hello Dockerfile has issues.</span></span> <span data-ttu-id="66d33-194">Dessutom eftersom hello Azure Toolkit förväntar hello anpassade Dockerfile toocontain en web app artefakt, försöker den tooopen en HTTP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="66d33-194">In addition, because hello Azure Toolkit expects hello custom Dockerfile toocontain a web app artifact, it attempts tooopen an HTTP connection.</span></span> <span data-ttu-id="66d33-195">Om utvecklare publicerar en annan typ av artefakt, kan de få harmlöst fel efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="66d33-195">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="66d33-196">c.</span><span class="sxs-lookup"><span data-stu-id="66d33-196">c.</span></span> <span data-ttu-id="66d33-197">I hello **portinställningarna** ange hello unik TCP-port bindning för Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="66d33-197">In hello **Port settings** box, enter hello unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="66d33-198">När du har slutfört föregående steg hello, klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="66d33-198">After you have completed hello preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="66d33-199">hello Azure Toolkit börjar distribuera din web app tooAzure i en dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="66d33-199">hello Azure Toolkit begins deploying your web app tooAzure in a Docker container.</span></span> <span data-ttu-id="66d33-200">Om du har konfigurerat IntelliJ toobe distribuerats i hello bakgrund, en **distribuera tooAzure** förloppsindikatorn visas.</span><span class="sxs-lookup"><span data-stu-id="66d33-200">Unless you have configured IntelliJ toobe deployed in hello background, a **Deploying tooAzure** progress bar appears.</span></span> 

![hello förloppsindikatorn för distribution][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="66d33-202">Mer information om hur du skapar artefakter</span><span class="sxs-lookup"><span data-stu-id="66d33-202">Additional information about creating artifacts</span></span>

<span data-ttu-id="66d33-203">toocreate distribution redo-artefakt hello följande:</span><span class="sxs-lookup"><span data-stu-id="66d33-203">toocreate a deployment-ready artifact, do hello following:</span></span>

1. <span data-ttu-id="66d33-204">Öppna din webbappsprojektet i IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="66d33-204">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="66d33-205">Klicka på **filen**, och klicka sedan på **projektstruktur**.</span><span class="sxs-lookup"><span data-stu-id="66d33-205">Click **File**, and then click **Project Structure**.</span></span>

   ![hello projektstruktur kommando][ART01]

3. <span data-ttu-id="66d33-207">tooadd en artefakt, klicka hello grönt plustecken (**+**), och klicka sedan på **webbprogrammet: Arkiv**.</span><span class="sxs-lookup"><span data-stu-id="66d33-207">tooadd an artifact, click hello green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![Hej ”program: webbarkiv” kommando][ART02]

4. <span data-ttu-id="66d33-209">I hello **namn** anger du ett namn för din artefakt (inkluderar inte hello *.war* tillägget), och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66d33-209">In hello **Name** box, enter a name for your artifact (do not include hello *.war* extension), and then click **OK**.</span></span>

   ![hello artefakt namn.][ART03]

<span data-ttu-id="66d33-211">Läs mer om hur du skapar artefakter i IntelliJ [konfigurera artefakter] på hello JetBrains webbplats.</span><span class="sxs-lookup"><span data-stu-id="66d33-211">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on hello JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66d33-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66d33-212">Next steps</span></span>
<span data-ttu-id="66d33-213">Mer information om hello Azure-verktyg för Java IDEs finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="66d33-213">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="66d33-214">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="66d33-214">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="66d33-215">[Vad är nytt i hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="66d33-215">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="66d33-216">[Installera hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="66d33-216">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="66d33-217">[Logga in anvisningar hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="66d33-217">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="66d33-218">[Skapa en Hello World-webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="66d33-218">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="66d33-219">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="66d33-219">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="66d33-220">[Vad är nytt i hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="66d33-220">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="66d33-221">[Installera hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="66d33-221">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="66d33-222">[Logga in instruktioner för hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="66d33-222">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="66d33-223">[Skapa en Hello World-webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="66d33-223">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="66d33-224">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="66d33-224">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="66d33-225">För ytterligare resurser för Docker Se hello officiella [Docker webbplats].</span><span class="sxs-lookup"><span data-stu-id="66d33-225">For additional resources for Docker, see hello official [Docker website].</span></span>

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga in anvisningar hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga in instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

[Docker webbplats]: https://www.docker.com/
[konfigurera artefakter]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
