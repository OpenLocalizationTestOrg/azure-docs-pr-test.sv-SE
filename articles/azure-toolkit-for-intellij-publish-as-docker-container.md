---
title: "Publicera en dockerbehållare med hjälp av Azure-verktyget för IntelliJ | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 96680319a6c4c0f0a4673cd6303a5b172f428797
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a><span data-ttu-id="91464-103">Publicera ett webbprogram som en dockerbehållare med hjälp av Azure-verktyget för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="91464-103">Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="91464-104">Docker-behållare är en mycket vanlig metod för att distribuera webbprogram.</span><span class="sxs-lookup"><span data-stu-id="91464-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="91464-105">Genom att använda Docker-behållare kan konsolidera utvecklare sina projektfiler och beroenden i ett enda paket för distribution till en server.</span><span class="sxs-lookup"><span data-stu-id="91464-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment to a server.</span></span> <span data-ttu-id="91464-106">Azure-verktygen för IntelliJ förenklar processen för Java-utvecklare genom att lägga till *Publicera som Dockerbehållare* funktioner för distribution till Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="91464-106">The Azure Toolkit for IntelliJ simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment to Microsoft Azure.</span></span> <span data-ttu-id="91464-107">Den här artikeln vägleder dig igenom de steg som krävs för att publicera dina program till Azure som Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="91464-107">This article walks you through the steps required to publish your applications to Azure as Docker containers.</span></span>

> [!NOTE]
>
> <span data-ttu-id="91464-108">Mer information om Docker är tillgängligt på den [Docker webbplats].</span><span class="sxs-lookup"><span data-stu-id="91464-108">More information about Docker is available on the [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="91464-109">Publicera webbappen till Azure med hjälp av en dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="91464-109">Publish your web app to Azure by using a Docker container</span></span>

> [!NOTE]
> * <span data-ttu-id="91464-110">För att publicera ditt webbprogram, måste du skapa en distribution redo artefakt.</span><span class="sxs-lookup"><span data-stu-id="91464-110">To publish your web app, you must create a deployment-ready artifact.</span></span> <span data-ttu-id="91464-111">Mer information finns i [ytterligare information om hur du skapar artefakter](#artifacts) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="91464-111">To learn more, see the [Additional information about creating artifacts](#artifacts) section.</span></span>
>
> * <span data-ttu-id="91464-112">När du har slutfört guiden minst en gång, är de flesta inställningarna används som standard när du kör guiden igen.</span><span class="sxs-lookup"><span data-stu-id="91464-112">After you have completed the deployment wizard at least once, most of your settings are used as defaults when you run the wizard again.</span></span>
>

1. <span data-ttu-id="91464-113">Öppna din webbappsprojektet i IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="91464-113">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="91464-114">Starta den **Publicera som Dockerbehållare** guiden gör du något av följande:</span><span class="sxs-lookup"><span data-stu-id="91464-114">To start the **Publish as Docker Container** wizard, do either of the following:</span></span>

   * <span data-ttu-id="91464-115">I den **projekt** verktyg fönstret, högerklicka på ditt projekt, klickar du på **Azure**, och klicka sedan på **Publicera som Dockerbehållare**:</span><span class="sxs-lookup"><span data-stu-id="91464-115">In the **Project** tool window, right-click your project, click **Azure**, and then click **Publish as Docker Container**:</span></span>

      ![Publicera som Dockerbehållare kommando][PUB01]

   * <span data-ttu-id="91464-117">Klicka på verktygsfältet IntelliJ den **publicera grupp** knappen och klicka sedan på **Publicera som Dockerbehållare**:</span><span class="sxs-lookup"><span data-stu-id="91464-117">On the IntelliJ toolbar, click the **Publish Group** button, and then click **Publish as Docker Container**:</span></span>

      <span data-ttu-id="91464-118">![Publicera som Dockerbehållare kommando][PUB02]</span><span class="sxs-lookup"><span data-stu-id="91464-118">![The Publish as Docker Container command][PUB02]</span></span>  
    <span data-ttu-id="91464-119">Den **distribuera Dockerbehållare på Azure** öppnas guiden.</span><span class="sxs-lookup"><span data-stu-id="91464-119">The **Deploy Docker Container on Azure** wizard opens.</span></span>

   ![Dockerbehållare distribuera på Azure-guiden][PUB03]

3. <span data-ttu-id="91464-121">I den **skriver du ett namn, Välj den artefakt sökväg och kontrollera en Docker-värden som ska användas** fönster, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="91464-121">In the **Type an image name, select the artifact's path and check a Docker host to be used** window, do the following:</span></span> 

   <span data-ttu-id="91464-122">a.</span><span class="sxs-lookup"><span data-stu-id="91464-122">a.</span></span> <span data-ttu-id="91464-123">I den **Docker avbildningsnamn** ange ett unikt namn för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="91464-123">In the **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="91464-124">(Skapar guiden automatiskt ett namn, men du kan ändra den.)</span><span class="sxs-lookup"><span data-stu-id="91464-124">(The wizard automatically creates a name, but you can modify it.)</span></span> 

   <span data-ttu-id="91464-125">b.</span><span class="sxs-lookup"><span data-stu-id="91464-125">b.</span></span> <span data-ttu-id="91464-126">Den **värdar** området visas alla Docker-värdar som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="91464-126">The **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="91464-127">Gör något av följande:</span><span class="sxs-lookup"><span data-stu-id="91464-127">Do either of the following:</span></span> 
      * <span data-ttu-id="91464-128">Om du har en befintlig Docker-värd kan distribuera du ditt webbprogram till den.</span><span class="sxs-lookup"><span data-stu-id="91464-128">If you have an existing Docker host, you can deploy your web app to it.</span></span>
      * <span data-ttu-id="91464-129">Klicka för att skapa en Docker-värd grönt plustecken (**+**).</span><span class="sxs-lookup"><span data-stu-id="91464-129">To create a Docker host, click the green plus sign (**+**).</span></span>  
       <span data-ttu-id="91464-130">Den **skapa Docker värden** öppnas.</span><span class="sxs-lookup"><span data-stu-id="91464-130">The **Create Docker Host** dialog box opens.</span></span> 

      ![Distribuera Dockerbehållare på Azure-guiden][PUB04a]

4. <span data-ttu-id="91464-132">I den **konfigurera den nya virtuella datorn** och ange följande information om Docker-värd.</span><span class="sxs-lookup"><span data-stu-id="91464-132">In the **Configure the new virtual machine** window, provide the following information about your Docker host.</span></span> <span data-ttu-id="91464-133">(Guiden genererar automatiskt de flesta av information för att du, men du kan ändra någon av dem.)</span><span class="sxs-lookup"><span data-stu-id="91464-133">(The wizard automatically generates most of the information for you, but you can modify any of them.)</span></span> 

   <span data-ttu-id="91464-134">a.</span><span class="sxs-lookup"><span data-stu-id="91464-134">a.</span></span> <span data-ttu-id="91464-135">I den **namn** ange ett unikt namn för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="91464-135">In the **Name** box, enter a unique name for the Docker host.</span></span> <span data-ttu-id="91464-136">(Det är inte samma som Docker avbildningens namn som du angav tidigare.)</span><span class="sxs-lookup"><span data-stu-id="91464-136">(It is not the same as the Docker image name that you specified earlier.)</span></span> 
    
   <span data-ttu-id="91464-137">b.</span><span class="sxs-lookup"><span data-stu-id="91464-137">b.</span></span> <span data-ttu-id="91464-138">I den **prenumeration** ange Azure-prenumeration som du använder för värden.</span><span class="sxs-lookup"><span data-stu-id="91464-138">In the **Subscription** box, enter the Azure subscription that you use for your host.</span></span> 
      
   <span data-ttu-id="91464-139">c.</span><span class="sxs-lookup"><span data-stu-id="91464-139">c.</span></span> <span data-ttu-id="91464-140">I den **Region** ange den geografiska region där värden finns.</span><span class="sxs-lookup"><span data-stu-id="91464-140">In the **Region** box, enter the geographical region where your host is located.</span></span>
      
   <span data-ttu-id="91464-141">d.</span><span class="sxs-lookup"><span data-stu-id="91464-141">d.</span></span> <span data-ttu-id="91464-142">På den **OS- och** gör följande:</span><span class="sxs-lookup"><span data-stu-id="91464-142">On the **OS and Size** tab, do the following:</span></span>      
      * <span data-ttu-id="91464-143">**Värd för OS**: Ange operativsystemet för den virtuella datorn som innehåller värden.</span><span class="sxs-lookup"><span data-stu-id="91464-143">**Host OS**: Enter the operating system for the virtual machine that contains your host.</span></span> 
      * <span data-ttu-id="91464-144">**Storlek**: Ange storleken på virtuella datorn för värden.</span><span class="sxs-lookup"><span data-stu-id="91464-144">**Size**: Enter the virtual-machine size for your host.</span></span>   
       
   <span data-ttu-id="91464-145">e.</span><span class="sxs-lookup"><span data-stu-id="91464-145">e.</span></span> <span data-ttu-id="91464-146">På den **resursgruppen** väljer du något av följande:</span><span class="sxs-lookup"><span data-stu-id="91464-146">On the **Resource Group** tab, select either of the following:</span></span>      
      * <span data-ttu-id="91464-147">**Ny resursgrupp**: skapa en resursgrupp för värden.</span><span class="sxs-lookup"><span data-stu-id="91464-147">**New resource group**: Create a resource group for your host.</span></span>
      * <span data-ttu-id="91464-148">**Befintlig resursgrupp**: Ange en befintlig resursgrupp från ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="91464-148">**Existing resource group**: Specify an existing resource group from your Azure account.</span></span> 
       
   <span data-ttu-id="91464-149">f.</span><span class="sxs-lookup"><span data-stu-id="91464-149">f.</span></span> <span data-ttu-id="91464-150">På den **nätverk** väljer du något av följande:</span><span class="sxs-lookup"><span data-stu-id="91464-150">On the **Network** tab, select either of the following:</span></span>      
      * <span data-ttu-id="91464-151">**Nytt virtuellt nätverk**: skapa ett virtuellt nätverk för värden.</span><span class="sxs-lookup"><span data-stu-id="91464-151">**New virtual network**: Create a virtual network for your host.</span></span>
      * <span data-ttu-id="91464-152">**Befintligt virtuellt nätverk**: Ange ett befintligt virtuellt nätverk från ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="91464-152">**Existing virtual network**: Specify an existing virtual network from your Azure account.</span></span> 
       
   <span data-ttu-id="91464-153">g.</span><span class="sxs-lookup"><span data-stu-id="91464-153">g.</span></span> <span data-ttu-id="91464-154">På den **lagring** väljer du något av följande:</span><span class="sxs-lookup"><span data-stu-id="91464-154">On the **Storage** tab, select either of the following:</span></span>      
      * <span data-ttu-id="91464-155">**Nytt lagringskonto**: skapa ett lagringskonto för värden.</span><span class="sxs-lookup"><span data-stu-id="91464-155">**New storage account**: Create a storage account for your host.</span></span>
      * <span data-ttu-id="91464-156">**Befintligt lagringskonto**: Ange ett befintligt lagringskonto från Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="91464-156">**Existing storage account**: Specify an existing storage account from your Azure account.</span></span>
       
5. <span data-ttu-id="91464-157">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="91464-157">Click **Next**.</span></span>  
     <span data-ttu-id="91464-158">Den **Konfigurera logg i autentiseringsuppgifter och portinställningarna** öppnas.</span><span class="sxs-lookup"><span data-stu-id="91464-158">The **Configure log in credentials and port settings** window opens.</span></span>

      ![Konfigurera loggen i autentiseringsuppgifter och port inställningar][PUB05]

6. <span data-ttu-id="91464-160">Välj något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="91464-160">Select one of the following options:</span></span>

      * <span data-ttu-id="91464-161">**Importera autentiseringsuppgifter från Azure Key Vault**: Ange en tidigare sparad uppsättning autentiseringsuppgifter som lagras i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="91464-161">**Import credentials from Azure Key Vault**: Specify a previously saved set of credentials that are stored in your Azure subscription.</span></span>

          > [!NOTE]
          > <span data-ttu-id="91464-162">Ett Azure key vault som har skapats med ett visst konto eller tjänstens huvudnamn är inte tillgänglig automatiskt av ett annat konto eller tjänstens huvudnamn som delar prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="91464-162">An Azure key vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares the subscription.</span></span> <span data-ttu-id="91464-163">För att ett annat konto eller service principal att använda nyckelvalvet, måste du använda Azure-portalen för att lägga till kontot eller tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="91464-163">To allow another account or service principal to use the key vault, you must use the Azure portal to add the account or service principal.</span></span>

      * <span data-ttu-id="91464-164">**Ny logg i autentiseringsuppgifter**: skapa en ny uppsättning autentiseringsuppgifter för inloggning.</span><span class="sxs-lookup"><span data-stu-id="91464-164">**New log in credentials**: Create a new set of login credentials.</span></span> <span data-ttu-id="91464-165">Om du väljer det här alternativet måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="91464-165">If you select this option, do the following:</span></span>

        <span data-ttu-id="91464-166">a.</span><span class="sxs-lookup"><span data-stu-id="91464-166">a.</span></span> <span data-ttu-id="91464-167">På den **VM autentiseringsuppgifter** och ange följande information för virtuell dator inloggningsuppgifterna för Docker-värd: * **användarnamn**: Ange användarnamnet för dina inloggningsuppgifter för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="91464-167">On the **VM Credentials** tab, provide the following information for the virtual-machine login credentials of your Docker host: * **Username**: Enter the username for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="91464-168">* **Lösenordet** och **Bekräfta**: Ange lösenordet för dina inloggningsuppgifter för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="91464-168">* **Password** and **Confirm**: Enter the password for your virtual-machine login credentials.</span></span>
             <span data-ttu-id="91464-169">* **SSH**: Ange inställningar för SSH (Secure Shell) för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="91464-169">* **SSH**: Enter the Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="91464-170">Du kan välja något av följande alternativ: * **ingen**: Anger att den virtuella datorn inte tillåter SSH-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="91464-170">You can select one of the following options: * **None**: Specifies that your virtual machine does not allow SSH connections.</span></span>
                <span data-ttu-id="91464-171">* **Autogenerera**: skapar automatiskt de nödvändiga inställningarna för att ansluta via SSH.</span><span class="sxs-lookup"><span data-stu-id="91464-171">* **Auto-generate**: Automatically creates the requisite settings for connecting via SSH.</span></span>
                <span data-ttu-id="91464-172">* **Importera från directory**: kan du ange en katalog som innehåller en uppsättning tidigare sparade SSH-inställningar.</span><span class="sxs-lookup"><span data-stu-id="91464-172">* **Import from directory**: Allows you to specify a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="91464-173">Katalogen måste innehålla följande två filer:</span><span class="sxs-lookup"><span data-stu-id="91464-173">The directory must contain the following two files:</span></span>
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        <span data-ttu-id="91464-174">b.</span><span class="sxs-lookup"><span data-stu-id="91464-174">b.</span></span> <span data-ttu-id="91464-175">På den **Docker Daemon åtkomst** och ange följande information:</span><span class="sxs-lookup"><span data-stu-id="91464-175">On the **Docker Daemon Access** tab, provide the following information:</span></span>

          ![Skapa Docker-värd][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. <span data-ttu-id="91464-177">När du har angett informationen som krävs, klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="91464-177">After you have entered the required information, click **Finish**.</span></span>  
    <span data-ttu-id="91464-178">Den **distribuera Dockerbehållare på Azure** guiden visas igen.</span><span class="sxs-lookup"><span data-stu-id="91464-178">The **Deploy Docker Container on Azure** wizard reappears.</span></span>

   ![Distribuera Dockerbehållare på Azure-guiden][PUB07]

8. <span data-ttu-id="91464-180">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="91464-180">Click **Next**.</span></span>  
    <span data-ttu-id="91464-181">Den **konfigurera dockerbehållare skapas** öppnas.</span><span class="sxs-lookup"><span data-stu-id="91464-181">The **Configure the Docker container to be created** window opens.</span></span>

   ![Konfigurera Docker-behållare skapas fönster][PUB08]

9. <span data-ttu-id="91464-183">I den **konfigurera dockerbehållare skapas** och ange följande information:</span><span class="sxs-lookup"><span data-stu-id="91464-183">In the **Configure the Docker container to be created** window, provide the following information:</span></span> 

   <span data-ttu-id="91464-184">a.</span><span class="sxs-lookup"><span data-stu-id="91464-184">a.</span></span> <span data-ttu-id="91464-185">I den **Docker behållarnamn** ange ett unikt namn för din dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="91464-185">In the **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="91464-186">b.</span><span class="sxs-lookup"><span data-stu-id="91464-186">b.</span></span> <span data-ttu-id="91464-187">Välj något av följande Docker-bilder:</span><span class="sxs-lookup"><span data-stu-id="91464-187">Choose one of the following Docker images:</span></span> 

      * <span data-ttu-id="91464-188">**Fördefinierade Docker bild**: Ange en befintlig Docker-avbildning från Azure.</span><span class="sxs-lookup"><span data-stu-id="91464-188">**Predefined Docker image**: Specify a pre-existing Docker image from Azure.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="91464-189">Listan med Docker bilder i den här rutan består av flera avbildningar som Azure-verktygen har konfigurerats för att korrigera så att din artefakt distribueras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="91464-189">The list of Docker images in this box consists of several images that the Azure Toolkit has been configured to patch so that your artifact is deployed automatically.</span></span> 

      * <span data-ttu-id="91464-190">**Anpassade Dockerfile**: Ange en tidigare sparad Dockerfile från den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="91464-190">**Custom Dockerfile**: Specify a previously saved Dockerfile from your local computer.</span></span>

        > [!NOTE]
        > <span data-ttu-id="91464-191">Detta är en avancerad funktion för utvecklare som vill distribuera sina egna Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="91464-191">This is a more advanced feature for developers who want to deploy their own Dockerfile.</span></span> <span data-ttu-id="91464-192">Men är det utvecklare som använder det här alternativet för att säkerställa att deras Dockerfile är uppbyggd.</span><span class="sxs-lookup"><span data-stu-id="91464-192">However, it is up to developers who use this option to ensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="91464-193">Eftersom Azure-verktyget inte validerar innehållet i en anpassad Dockerfile, kan distributionen misslyckas om Dockerfile har problem.</span><span class="sxs-lookup"><span data-stu-id="91464-193">Because the Azure Toolkit does not validate the content in a custom Dockerfile, the deployment might fail if the Dockerfile has issues.</span></span> <span data-ttu-id="91464-194">Dessutom eftersom Azure Toolkit förväntar anpassade Dockerfile ska innehålla en web app artefakt, försöker öppna en HTTP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="91464-194">In addition, because the Azure Toolkit expects the custom Dockerfile to contain a web app artifact, it attempts to open an HTTP connection.</span></span> <span data-ttu-id="91464-195">Om utvecklare publicerar en annan typ av artefakt, kan de få harmlöst fel efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="91464-195">If developers publish a different type of artifact, they might receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="91464-196">c.</span><span class="sxs-lookup"><span data-stu-id="91464-196">c.</span></span> <span data-ttu-id="91464-197">I den **portinställningarna** ange unika TCP-portbindningen för Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="91464-197">In the **Port settings** box, enter the unique TCP port binding for your Docker container.</span></span> 

10. <span data-ttu-id="91464-198">När du har slutfört föregående steg klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="91464-198">After you have completed the preceding steps, click **Finish**.</span></span> 

<span data-ttu-id="91464-199">Azure-Toolkit börjar distribuera webbappen till Azure i en dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="91464-199">The Azure Toolkit begins deploying your web app to Azure in a Docker container.</span></span> <span data-ttu-id="91464-200">Om du har konfigurerat IntelliJ som ska distribueras i bakgrunden, en **distribution till Azure** förloppsindikatorn visas.</span><span class="sxs-lookup"><span data-stu-id="91464-200">Unless you have configured IntelliJ to be deployed in the background, a **Deploying to Azure** progress bar appears.</span></span> 

![Förloppsindikatorn för distribution][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a><span data-ttu-id="91464-202">Mer information om hur du skapar artefakter</span><span class="sxs-lookup"><span data-stu-id="91464-202">Additional information about creating artifacts</span></span>

<span data-ttu-id="91464-203">Om du vill skapa en artefakt distribution är redo att göra följande:</span><span class="sxs-lookup"><span data-stu-id="91464-203">To create a deployment-ready artifact, do the following:</span></span>

1. <span data-ttu-id="91464-204">Öppna din webbappsprojektet i IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="91464-204">Open your web app project in IntelliJ.</span></span>

2. <span data-ttu-id="91464-205">Klicka på **filen**, och klicka sedan på **projektstruktur**.</span><span class="sxs-lookup"><span data-stu-id="91464-205">Click **File**, and then click **Project Structure**.</span></span>

   ![Kommandot projektstruktur][ART01]

3. <span data-ttu-id="91464-207">Lägg till en artefakt, klicka på grönt plustecken (**+**), och klicka sedan på **webbprogrammet: Arkiv**.</span><span class="sxs-lookup"><span data-stu-id="91464-207">To add an artifact, click the green plus sign (**+**), and then click **Web Application: Archive**.</span></span>

   ![Kommandot ”program: Webbarkiv”][ART02]

4. <span data-ttu-id="91464-209">I den **namn** anger du ett namn för din artefakt (inkluderar inte den *.war* tillägget), och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="91464-209">In the **Name** box, enter a name for your artifact (do not include the *.war* extension), and then click **OK**.</span></span>

   ![Rutan artefakt][ART03]

<span data-ttu-id="91464-211">Läs mer om hur du skapar artefakter i IntelliJ [konfigurera artefakter] på webbplatsen JetBrains.</span><span class="sxs-lookup"><span data-stu-id="91464-211">For more information about creating artifacts in IntelliJ, see [Configuring artifacts] on the JetBrains website.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91464-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91464-212">Next steps</span></span>
<span data-ttu-id="91464-213">Mer information om Azure-verktyg för Java IDEs finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="91464-213">For more information about the Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="91464-214">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="91464-214">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="91464-215">[Vad är nytt i Azure-verktygen för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="91464-215">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="91464-216">[Installera Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="91464-216">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="91464-217">[Logga in-instruktioner för Azure-verktygen för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="91464-217">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="91464-218">[Skapa en Hello World-webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="91464-218">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="91464-219">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="91464-219">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="91464-220">[Vad är nytt i Azure-verktygen för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="91464-220">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="91464-221">[Installera Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="91464-221">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="91464-222">[Logga in instruktioner för Azure-Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="91464-222">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="91464-223">[Skapa en Hello World-webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="91464-223">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="91464-224">Mer information om hur du använder Azure med Java finns på [Azure Java Developer Center] och i [Java Tools för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="91464-224">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="91464-225">Ytterligare resurser för Docker finns i officiellt [Docker webbplats].</span><span class="sxs-lookup"><span data-stu-id="91464-225">For additional resources for Docker, see the official [Docker website].</span></span>

<!-- URL List -->

<span data-ttu-id="91464-226">[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="91464-226">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="91464-227">[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="91464-227">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="91464-228">[Skapa en Hello World-webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="91464-228">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="91464-229">[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="91464-229">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="91464-230">[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="91464-230">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="91464-231">[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="91464-231">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="91464-232">[Logga in-instruktioner för Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="91464-232">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="91464-233">[Logga in instruktioner för Azure-Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="91464-233">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="91464-234">[Vad är nytt i Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="91464-234">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="91464-235">[Vad är nytt i Azure-verktygen för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="91464-235">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="91464-236">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="91464-236">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="91464-237">[Java Tools för Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="91464-237">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="91464-238">[Docker webbplats]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="91464-238">[Docker website]: https://www.docker.com/</span></span>
<span data-ttu-id="91464-239">[konfigurera artefakter]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span><span class="sxs-lookup"><span data-stu-id="91464-239">[Configuring artifacts]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html</span></span>

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
