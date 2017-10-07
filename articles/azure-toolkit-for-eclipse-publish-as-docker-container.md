---
title: "aaaPublish en dockerbehållare med hjälp av hello Azure Toolkit för Eclipse | Microsoft Docs"
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
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 53ec3a7f7a171691024e03622fd48d6f1e257b50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="a46e1-103">Publicera ett webbprogram som en dockerbehållare med hello Azure Toolkit för Eclipse</span><span class="sxs-lookup"><span data-stu-id="a46e1-103">Publish a web app as a Docker container by using hello Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="a46e1-104">Docker-behållare är en mycket vanlig metod för att distribuera webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a46e1-104">Docker containers are a widely used method for deploying web applications.</span></span> <span data-ttu-id="a46e1-105">Genom att använda Docker-behållare kan konsolidera utvecklare sina projektfiler och beroenden i ett enda paket för distribution tooa server.</span><span class="sxs-lookup"><span data-stu-id="a46e1-105">By using Docker containers, developers can consolidate all their project files and dependencies into a single package for deployment tooa server.</span></span> <span data-ttu-id="a46e1-106">hello Azure Toolkit för Eclipse förenklar processen för Java-utvecklare genom att lägga till *Publicera som Dockerbehållare* funktioner för distribution tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="a46e1-106">hello Azure Toolkit for Eclipse simplifies this process for Java developers by adding *Publish as Docker Container* features for deployment tooMicrosoft Azure.</span></span> <span data-ttu-id="a46e1-107">Den här artikeln vägleder dig genom hello steg krävs toopublish program-tooAzure som Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="a46e1-107">This article walks you through hello steps required toopublish your applications tooAzure as Docker containers.</span></span>

> [!NOTE]
> <span data-ttu-id="a46e1-108">Mer information om Docker är tillgänglig på hello [Docker webbplats].</span><span class="sxs-lookup"><span data-stu-id="a46e1-108">More information about Docker is available on hello [Docker website].</span></span>
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="a46e1-109">Publicera din web app tooAzure med hjälp av en dockerbehållare</span><span class="sxs-lookup"><span data-stu-id="a46e1-109">Publish your web app tooAzure by using a Docker container</span></span>

1. <span data-ttu-id="a46e1-110">Öppna projektet web app i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="a46e1-110">Open your web app project in Eclipse.</span></span>

2. <span data-ttu-id="a46e1-111">toostart hello **Publicera som Dockerbehållare** guiden gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="a46e1-111">toostart hello **Publish as Docker Container** wizard, do either of hello following:</span></span>

   * <span data-ttu-id="a46e1-112">I hello **Navigator** visa, högerklicka på ditt projekt, klickar du på **Azure**, och klicka sedan på **Publicera som Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="a46e1-112">In hello **Navigator** view, right-click your project, click **Azure**, and then click **Publish as Docker Container**.</span></span>

      ![Visa Navigator Publicera som Dockerbehållare kommando][PUB01]

   * <span data-ttu-id="a46e1-114">Verktygsfältet hello Eclipse och klicka på hello **publicera** knappen och klicka sedan på **Publicera som Dockerbehållare**.</span><span class="sxs-lookup"><span data-stu-id="a46e1-114">On hello Eclipse toolbar, click hello **Publish** button, and then click **Publish as Docker Container**.</span></span>

      ![Eclipse verktygsfältet Publicera som Dockerbehållare kommando][PUB02]
      
    <span data-ttu-id="a46e1-116">Hej **distribuera Dockerbehållare på Azure** öppnas guiden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-116">hello **Deploy Docker Container on Azure** wizard opens.</span></span>

    ![hello distribuera Dockerbehållare på Azure-guiden][PUB03]

3. <span data-ttu-id="a46e1-118">I hello **skriver du ett namn, Välj hello artefakt sökväg och kontrollera en Docker värden toobe används** fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="a46e1-118">In hello **Type an image name, select hello artifact's path and check a Docker host toobe used** window, do hello following:</span></span>

    <span data-ttu-id="a46e1-119">a.</span><span class="sxs-lookup"><span data-stu-id="a46e1-119">a.</span></span> <span data-ttu-id="a46e1-120">I hello **Docker avbildningsnamn** ange ett unikt namn för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-120">In hello **Docker image name** box, enter a unique name for your Docker host.</span></span> <span data-ttu-id="a46e1-121">(hello guiden skapar automatiskt ett namn, men du kan ändra den.)</span><span class="sxs-lookup"><span data-stu-id="a46e1-121">(hello wizard automatically creates a name, but you can modify it.)</span></span>

    <span data-ttu-id="a46e1-122">b.</span><span class="sxs-lookup"><span data-stu-id="a46e1-122">b.</span></span> <span data-ttu-id="a46e1-123">Hej **värdar** området visas alla Docker-värdar som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="a46e1-123">hello **Hosts** area displays any Docker hosts that you have already created.</span></span> <span data-ttu-id="a46e1-124">Gör något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="a46e1-124">Do either of hello following:</span></span>

    * <span data-ttu-id="a46e1-125">Om du har en befintlig Docker-värd kan distribuera du din web app tooit.</span><span class="sxs-lookup"><span data-stu-id="a46e1-125">If you have an existing Docker host, you can deploy your web app tooit.</span></span>
    * <span data-ttu-id="a46e1-126">toocreate nytt Docker-värd, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a46e1-126">toocreate a new Docker host, click **Add**.</span></span>  
      
    <span data-ttu-id="a46e1-127">Hej **skapa Docker värden** öppnas.</span><span class="sxs-lookup"><span data-stu-id="a46e1-127">hello **Create Docker Host** dialog box opens.</span></span>

    ![Distribuera Dockerbehållare på Azure-guiden][PUB04a]

4. <span data-ttu-id="a46e1-129">I hello **konfigurera hello ny virtuell dator** fönstret Ange hello följande alternativ för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-129">In hello **Configure hello new virtual machine** window, specify hello following options for your Docker host.</span></span> <span data-ttu-id="a46e1-130">(hello guiden genererar automatiskt de flesta av hello alternativ för dig, men du kan ändra någon av dem.)</span><span class="sxs-lookup"><span data-stu-id="a46e1-130">(hello wizard automatically generates most of hello options for you, but you can modify any of them.)</span></span>

   <span data-ttu-id="a46e1-131">a.</span><span class="sxs-lookup"><span data-stu-id="a46e1-131">a.</span></span> <span data-ttu-id="a46e1-132">**Namnet**: Ange ett unikt namn för hello Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-132">**Name**: Enter a unique name for hello Docker host.</span></span> <span data-ttu-id="a46e1-133">(Det är inte hello samma som hello Docker avbildningens namn som du angav tidigare.)</span><span class="sxs-lookup"><span data-stu-id="a46e1-133">(It is not hello same as hello Docker image name that you specified earlier.)</span></span>

   <span data-ttu-id="a46e1-134">b.</span><span class="sxs-lookup"><span data-stu-id="a46e1-134">b.</span></span> <span data-ttu-id="a46e1-135">**Prenumerationen**: Ange hello Azure-prenumeration som du använder för värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-135">**Subscription**: Enter hello Azure subscription that you use for your host.</span></span>

   <span data-ttu-id="a46e1-136">c.</span><span class="sxs-lookup"><span data-stu-id="a46e1-136">c.</span></span> <span data-ttu-id="a46e1-137">**Region**: Ange hello geografiska region där värden finns.</span><span class="sxs-lookup"><span data-stu-id="a46e1-137">**Region**: Enter hello geographical region where your host is located.</span></span>

   <span data-ttu-id="a46e1-138">d.</span><span class="sxs-lookup"><span data-stu-id="a46e1-138">d.</span></span> <span data-ttu-id="a46e1-139">På hello **Värdoperativsystem och storlek** fliken:</span><span class="sxs-lookup"><span data-stu-id="a46e1-139">On hello **Host OS and Size** tab:</span></span>
     * <span data-ttu-id="a46e1-140">**Värd för OS**: Ange hello operativsystemet för hello virtuell dator som innehåller värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-140">**Host OS**: Enter hello operating system for hello virtual machine that contains your host.</span></span>
     * <span data-ttu-id="a46e1-141">**Storlek**: Ange hello storlek för virtuell dator för värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-141">**Size**: Enter hello virtual-machine size for your host.</span></span>

   <span data-ttu-id="a46e1-142">e.</span><span class="sxs-lookup"><span data-stu-id="a46e1-142">e.</span></span> <span data-ttu-id="a46e1-143">På hello **resursgruppen** fliken:</span><span class="sxs-lookup"><span data-stu-id="a46e1-143">On hello **Resource Group** tab:</span></span>
     * <span data-ttu-id="a46e1-144">**Ny resursgrupp**: skapa en ny resursgrupp för värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-144">**New resource group**: Create a new resource group for your host.</span></span>
     * <span data-ttu-id="a46e1-145">**Befintlig resursgrupp**: Ange en befintlig resursgrupp från ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a46e1-145">**Existing resource group**: Enter an existing resource group from your Azure account.</span></span>

   <span data-ttu-id="a46e1-146">f.</span><span class="sxs-lookup"><span data-stu-id="a46e1-146">f.</span></span> <span data-ttu-id="a46e1-147">På hello **nätverk** fliken:</span><span class="sxs-lookup"><span data-stu-id="a46e1-147">On hello **Network** tab:</span></span>
     * <span data-ttu-id="a46e1-148">**Nytt virtuellt nätverk**: skapa ett nytt virtuellt nätverk för värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-148">**New virtual network**: Create a new virtual network for your host.</span></span>
     * <span data-ttu-id="a46e1-149">**Befintligt virtuellt nätverk**: Ange ett befintligt virtuellt nätverk från ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a46e1-149">**Existing virtual network**: Enter an existing virtual network from your Azure account.</span></span>

   <span data-ttu-id="a46e1-150">g.</span><span class="sxs-lookup"><span data-stu-id="a46e1-150">g.</span></span> <span data-ttu-id="a46e1-151">På hello **lagring** fliken:</span><span class="sxs-lookup"><span data-stu-id="a46e1-151">On hello **Storage** tab:</span></span>
     * <span data-ttu-id="a46e1-152">**Nytt lagringskonto**: skapa ett nytt lagringskonto för värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-152">**New storage account**: Create a new storage account for your host.</span></span>
     * <span data-ttu-id="a46e1-153">**Befintligt lagringskonto**: Ange ett befintligt lagringskonto från Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a46e1-153">**Existing storage account**: Enter an existing storage account from your Azure account.</span></span>

5. <span data-ttu-id="a46e1-154">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a46e1-154">Click **Next**.</span></span>

6. <span data-ttu-id="a46e1-155">I hello **Konfigurera logg i autentiseringsuppgifter och portinställningarna** fönster, Välj något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="a46e1-155">In hello **Configure log in credentials and port settings** window, select one of hello following options:</span></span>

    * <span data-ttu-id="a46e1-156">**Importera autentiseringsuppgifter från Azure Key Vault**: Anger en tidigare sparad uppsättning autentiseringsuppgifter som lagras i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a46e1-156">**Import credentials from Azure Key Vault**: Specifies a previously saved set of credentials that are stored in your Azure subscription.</span></span>

      >[!NOTE]
      ><span data-ttu-id="a46e1-157">Ett Azure Key Vault som har skapats med ett visst konto eller huvudnamn för tjänsten är inte tillgänglig automatiskt av ett annat konto eller tjänstens huvudnamn som delar hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a46e1-157">An Azure Key Vault that's created with a specific account or service principal is not automatically accessible by another account or service principal that shares hello subscription.</span></span> <span data-ttu-id="a46e1-158">tooallow ett annat konto eller service principal toouse Hej Key Vault, måste du använda hello Azure portal tooadd hello konto eller tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="a46e1-158">tooallow another account or service principal toouse hello Key Vault, you must use hello Azure portal tooadd hello account or service principal.</span></span>

    * <span data-ttu-id="a46e1-159">**Ny logg i autentiseringsuppgifter**: skapar en ny uppsättning autentiseringsuppgifter för inloggning.</span><span class="sxs-lookup"><span data-stu-id="a46e1-159">**New log in credentials**: Creates a new set of login credentials.</span></span> <span data-ttu-id="a46e1-160">Om du väljer det här alternativet hello följande:</span><span class="sxs-lookup"><span data-stu-id="a46e1-160">If you select this option, do hello following:</span></span>
    
      * <span data-ttu-id="a46e1-161">På hello **VM autentiseringsuppgifter** väljer du något av följande hello alternativ för hello virtuella datorer inloggningsuppgifterna för Docker-värd:</span><span class="sxs-lookup"><span data-stu-id="a46e1-161">On hello **VM Credentials** tab, choose one of hello following options for hello virtual-machine login credentials of your Docker host:</span></span>

          * <span data-ttu-id="a46e1-162">**Användarnamnet**: Ange hello användarnamn för dina inloggningsuppgifter för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a46e1-162">**Username**: Enter hello username for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="a46e1-163">**Lösenordet** och **Bekräfta**: Ange hello lösenord för dina inloggningsuppgifter för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a46e1-163">**Password** and **Confirm**: Enter hello password for your virtual machine login credentials.</span></span>
          * <span data-ttu-id="a46e1-164">**SSH**: Ange inställningar för hello SSH (Secure Shell) för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-164">**SSH**: Enter hello Secure Shell (SSH) settings for your Docker host.</span></span> <span data-ttu-id="a46e1-165">Du kan välja mellan följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="a46e1-165">You can choose from hello following options:</span></span>
            * <span data-ttu-id="a46e1-166">**Ingen**: Anger att den virtuella datorn inte tillåter att SSH-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="a46e1-166">**None**: Specifies that your virtual machine will not allow SSH connections.</span></span>
            * <span data-ttu-id="a46e1-167">**Autogenerera**: skapar automatiskt hello nödvändiga inställningar för att ansluta via SSH.</span><span class="sxs-lookup"><span data-stu-id="a46e1-167">**Auto-generate**: Automatically creates hello requisite settings for connecting via SSH.</span></span>
            * <span data-ttu-id="a46e1-168">**Importera från directory**: Anger en katalog som innehåller en uppsättning tidigare sparade SSH-inställningar.</span><span class="sxs-lookup"><span data-stu-id="a46e1-168">**Import from directory**: Specifies a directory that contains a set of previously saved SSH settings.</span></span> <span data-ttu-id="a46e1-169">hello directory måste innehålla hello följande två filer:</span><span class="sxs-lookup"><span data-stu-id="a46e1-169">hello directory must contain hello following two files:</span></span>
                * <span data-ttu-id="a46e1-170">*id_rsa*: innehåller hello RSA-identifiering för en användare.</span><span class="sxs-lookup"><span data-stu-id="a46e1-170">*id_rsa*: Contains hello RSA identification for a user.</span></span>
                * <span data-ttu-id="a46e1-171">*id_rsa.pub*: innehåller hello offentliga RSA-nyckel som används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a46e1-171">*id_rsa.pub*: Contains hello RSA public key that is used for authentication.</span></span>
        
        ![Skapa Docker-värd][PUB05]

      * <span data-ttu-id="a46e1-173">På hello **Docker Daemon autentiseringsuppgifter** anger hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="a46e1-173">On hello **Docker Daemon Credentials** tab, specify hello following options:</span></span>

          * <span data-ttu-id="a46e1-174">**Docker Daemon port**: Ange hello unika TCP-port för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-174">**Docker Daemon port**: Enter hello unique TCP port for your Docker host.</span></span>
          * <span data-ttu-id="a46e1-175">**TLS-säkerhet**: Ange hello Transport Layer Security-inställningar för Docker-värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-175">**TLS Security**: Enter hello Transport Layer Security settings for your Docker host.</span></span> <span data-ttu-id="a46e1-176">Du kan välja mellan följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="a46e1-176">You can choose from hello following options:</span></span>
            * <span data-ttu-id="a46e1-177">**Ingen**: Anger att den virtuella datorn inte tillåter att TLS-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="a46e1-177">**None**: Specifies that your virtual machine will not allow TLS connections.</span></span>
            * <span data-ttu-id="a46e1-178">**Autogenerera**: skapar automatiskt hello nödvändiga inställningar för att ansluta via TLS.</span><span class="sxs-lookup"><span data-stu-id="a46e1-178">**Auto-generate**: Automatically creates hello requisite settings for connecting via TLS.</span></span>
            * <span data-ttu-id="a46e1-179">**Importera från directory**: Anger en katalog som innehåller en uppsättning tidigare sparade TLS-inställningar.</span><span class="sxs-lookup"><span data-stu-id="a46e1-179">**Import from directory**: Specifies a directory that contains a set of previously saved TLS settings.</span></span> <span data-ttu-id="a46e1-180">Mer specifikt måste hello directory innehålla hello följande sex filer:</span><span class="sxs-lookup"><span data-stu-id="a46e1-180">More specifically, hello directory must contain hello following six files:</span></span>
                * <span data-ttu-id="a46e1-181">*CA.PEM* och *ca-key.pem*: innehålla hello certifikat och offentlig nyckel för hello TLS-certifikatets utfärdare.</span><span class="sxs-lookup"><span data-stu-id="a46e1-181">*ca.pem* and *ca-key.pem*: Contain hello certificate and public key for hello TLS Certificate Authority.</span></span>
                * <span data-ttu-id="a46e1-182">*CERT.PEM* och *key.pem*: innehålla hello klientcertifikatet och en offentlig nyckel som används för TLS-autentisering.</span><span class="sxs-lookup"><span data-stu-id="a46e1-182">*cert.pem* and *key.pem*: Contain hello client certificate and public key that is used for TLS authentication.</span></span>
                * <span data-ttu-id="a46e1-183">*Server.PEM* och *server key.pem*: innehålla hello servercertifikat och en offentlig nyckel för hello värden.</span><span class="sxs-lookup"><span data-stu-id="a46e1-183">*server.pem* and *server-key.pem*: Contain hello server certificate and public key for hello host.</span></span>

        ![Skapa Docker-värd][PUB06]

7. <span data-ttu-id="a46e1-185">När du har angett alla hello föregående information klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a46e1-185">After you have entered all of hello preceding information, click **Finish**.</span></span>

8. <span data-ttu-id="a46e1-186">I hello **distribuera Dockerbehållare på Azure** guiden, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a46e1-186">In hello **Deploy Docker Container on Azure** wizard, click **Next**.</span></span>

   ![hello distribuera Dockerbehållare på Azure-guiden][PUB07]

9. <span data-ttu-id="a46e1-188">I hello **konfigurera hello Docker behållare toobe skapade** fönstret hello följande:</span><span class="sxs-lookup"><span data-stu-id="a46e1-188">In hello **Configure hello Docker container toobe created** window, do hello following:</span></span>

   <span data-ttu-id="a46e1-189">a.</span><span class="sxs-lookup"><span data-stu-id="a46e1-189">a.</span></span> <span data-ttu-id="a46e1-190">I hello **Docker behållarnamn** ange ett unikt namn för din dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="a46e1-190">In hello **Docker container name** box, enter a unique name for your Docker container.</span></span>

   <span data-ttu-id="a46e1-191">b.</span><span class="sxs-lookup"><span data-stu-id="a46e1-191">b.</span></span> <span data-ttu-id="a46e1-192">Välj något av hello följande Docker bilder:</span><span class="sxs-lookup"><span data-stu-id="a46e1-192">Choose one of hello following Docker images:</span></span>
     * <span data-ttu-id="a46e1-193">**Fördefinierade Docker bild**: Anger en befintlig Docker-avbildning från Azure.</span><span class="sxs-lookup"><span data-stu-id="a46e1-193">**Predefined Docker image**: Specifies a pre-existing Docker image from Azure.</span></span>

       >[!NOTE]
       ><span data-ttu-id="a46e1-194">hello listan med Docker bilder i den här rutan består av flera avbildningar som hello Azure Toolkit har konfigurerats toopatch så att din artefakt distribueras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a46e1-194">hello list of Docker images in this box consists of several images that hello Azure Toolkit has been configured toopatch so that your artifact is deployed automatically.</span></span>

     * <span data-ttu-id="a46e1-195">**Anpassade Dockerfile**: Anger en tidigare sparad Dockerfile från den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="a46e1-195">**Custom Dockerfile**: Specifies a previously saved Dockerfile from your local computer.</span></span>

       >[!NOTE]
       ><span data-ttu-id="a46e1-196">Detta är en avancerad funktion för utvecklare som vill toodeploy sina egna Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="a46e1-196">This is a more advanced feature for developers who want toodeploy their own Dockerfile.</span></span> <span data-ttu-id="a46e1-197">Dock är det upp toodevelopers som använder det här alternativet tooensure som deras Dockerfile är uppbyggd.</span><span class="sxs-lookup"><span data-stu-id="a46e1-197">However, it is up toodevelopers who use this option tooensure that their Dockerfile is built correctly.</span></span> <span data-ttu-id="a46e1-198">hello Azure Toolkit kan inte valideras hello innehållet i en anpassad Dockerfile så hello distributionen misslyckas om hello Dockerfile har problem.</span><span class="sxs-lookup"><span data-stu-id="a46e1-198">hello Azure Toolkit does not validate hello content in a custom Dockerfile, so hello deployment might fail if hello Dockerfile has issues.</span></span> <span data-ttu-id="a46e1-199">Dessutom hello Azure Toolkit förväntar hello anpassade Dockerfile toocontain en web app artefakt och försök tooopen en HTTP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="a46e1-199">In addition, hello Azure Toolkit expects hello custom Dockerfile toocontain a web app artifact, and it will attempt tooopen an HTTP connection.</span></span> <span data-ttu-id="a46e1-200">Om utvecklare publicerar en annan typ av artefakt, kan de få harmlöst fel efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="a46e1-200">If developers publish a different type of artifact, they may receive innocuous errors after deployment.</span></span>

   <span data-ttu-id="a46e1-201">c.</span><span class="sxs-lookup"><span data-stu-id="a46e1-201">c.</span></span> <span data-ttu-id="a46e1-202">**Portinställningarna**: Ange hello unik TCP-port bindning för Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="a46e1-202">**Port settings**: Enter hello unique TCP port binding for your Docker container.</span></span>

     ![hello konfigurera hello Docker behållare toobe skapade fönster][PUB08]

10. <span data-ttu-id="a46e1-204">När du har slutfört alla hello föregående steg klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a46e1-204">After you have completed all of hello preceding steps, click **Finish**.</span></span>

<span data-ttu-id="a46e1-205">hello Azure Toolkit börjar distribuera din web app tooAzure i en dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="a46e1-205">hello Azure Toolkit begins deploying your web app tooAzure in a Docker container.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a46e1-206">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a46e1-206">Next steps</span></span>
<span data-ttu-id="a46e1-207">Mer information om hello Azure-verktyg för Java IDEs finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="a46e1-207">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="a46e1-208">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a46e1-208">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a46e1-209">[Vad är nytt i hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a46e1-209">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a46e1-210">[Installera hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a46e1-210">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a46e1-211">[Logga in anvisningar hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a46e1-211">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a46e1-212">[Skapa en Hello World-webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a46e1-212">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="a46e1-213">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a46e1-213">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a46e1-214">[Vad är nytt i hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a46e1-214">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a46e1-215">[Installera hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a46e1-215">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a46e1-216">[Logga in instruktioner för hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a46e1-216">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a46e1-217">[Skapa en Hello World-webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a46e1-217">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="a46e1-218">Mer information om hur du använder Azure med Java finns [Azure Java Developer Center] och [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="a46e1-218">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<span data-ttu-id="a46e1-219">För ytterligare resurser för Docker Se hello officiella [Docker webbplats].</span><span class="sxs-lookup"><span data-stu-id="a46e1-219">For additional resources for Docker, see hello official [Docker website].</span></span>

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

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png