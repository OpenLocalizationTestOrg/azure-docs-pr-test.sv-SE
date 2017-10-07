---
title: "aaaCreate en Webbapp i Azure App Service med hello Azure SDK för Java"
description: "Lär dig hur toocreate en Webbapp i Azure App Service via programmering med hello Azure SDK för Java."
tags: azure-classic-portal
services: app-service-web
documentationcenter: Java
author: donntrenton
manager: erikre
editor: jimbe
ms.assetid: 8954c456-1275-4d57-aff4-ca7d6374b71e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/25/2016
ms.author: v-donntr
ms.openlocfilehash: 42ba86b7fbb5668b3675198d0c5bb454525f706b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-hello-azure-sdk-for-java"></a><span data-ttu-id="5825f-103">Skapa en Webbapp i Azure App Service med hello Azure SDK för Java</span><span class="sxs-lookup"><span data-stu-id="5825f-103">Create a Web App in Azure App Service using hello Azure SDK for Java</span></span>
<!-- Azure Active Directory workflow is not yet available on hello Azure Portal -->

## <a name="overview"></a><span data-ttu-id="5825f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="5825f-104">Overview</span></span>
<span data-ttu-id="5825f-105">Den här genomgången visar hur toocreate ett Azure SDK för Java-program som skapar en Webbapp i [Azure App Service][Azure App Service], distribuera ett program tooit.</span><span class="sxs-lookup"><span data-stu-id="5825f-105">This walkthrough shows you how toocreate an Azure SDK for Java application that creates a Web App in [Azure App Service][Azure App Service], then deploy an application tooit.</span></span> <span data-ttu-id="5825f-106">Det består av två delar:</span><span class="sxs-lookup"><span data-stu-id="5825f-106">It consists of two parts:</span></span>

* <span data-ttu-id="5825f-107">Del 1 visar hur toobuild ett Java-program som skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5825f-107">Part 1 demonstrates how toobuild a Java application that creates a web app.</span></span>
* <span data-ttu-id="5825f-108">Del 2 visar hur toocreate en enkel JSP ”Hello World” program och sedan använda en FTP-klient toodeploy kod tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="5825f-108">Part 2 demonstrates how toocreate a simple JSP "Hello World" application, then use an FTP client toodeploy code tooApp Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5825f-109">Krav</span><span class="sxs-lookup"><span data-stu-id="5825f-109">Prerequisites</span></span>
### <a name="software-installations"></a><span data-ttu-id="5825f-110">Programinstallationer</span><span class="sxs-lookup"><span data-stu-id="5825f-110">Software Installations</span></span>
<span data-ttu-id="5825f-111">Hej AzureWebDemo programkod i den här artikeln skrevs med Azure Java SDK 0.7.0, som du kan installera med hjälp av hello [installationsprogram för webbplattform] [ Web Platform Installer] (WebPI).</span><span class="sxs-lookup"><span data-stu-id="5825f-111">hello AzureWebDemo application code in this article was written using Azure Java SDK 0.7.0, which you can install using hello [Web Platform Installer][Web Platform Installer] (WebPI).</span></span> <span data-ttu-id="5825f-112">Se också till att toouse hello senaste versionen av hello [Azure Toolkit för Eclipse][Azure Toolkit for Eclipse].</span><span class="sxs-lookup"><span data-stu-id="5825f-112">In addition, make sure toouse hello latest version of hello [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="5825f-113">När du har installerat hello SDK uppdatera hello beroenden i Eclipse-projektet genom att köra **uppdatera Index** i **Maven databaser**, sedan lägger till hello senaste versionen av varje paket i hello  **Beroenden** fönster.</span><span class="sxs-lookup"><span data-stu-id="5825f-113">After you install hello SDK, update hello dependencies in your Eclipse project by running **Update Index** in **Maven Repositories**, then re-add hello latest version of each package in hello **Dependencies** window.</span></span> <span data-ttu-id="5825f-114">Du kan verifiera hello version av installerad programvara i Eclipse genom att klicka på **Hjälp > installationsinformationen**; du bör ha minst hello följande versioner:</span><span class="sxs-lookup"><span data-stu-id="5825f-114">You can verify hello version of your installed software in Eclipse by clicking **Help > Installation Details**; you should have at least hello following versions:</span></span>

* <span data-ttu-id="5825f-115">Paket för Microsoft Azure-biblioteken för Java 0.7.0.20150309</span><span class="sxs-lookup"><span data-stu-id="5825f-115">Package for Microsoft Azure Libraries for Java 0.7.0.20150309</span></span>
* <span data-ttu-id="5825f-116">Eclipse IDE för Java EE-utvecklare 4.4.2.20150219</span><span class="sxs-lookup"><span data-stu-id="5825f-116">Eclipse IDE for Java EE Developers 4.4.2.20150219</span></span>

### <a name="create-and-configure-cloud-resources-in-azure"></a><span data-ttu-id="5825f-117">Skapa och konfigurera Molnresurser i Azure</span><span class="sxs-lookup"><span data-stu-id="5825f-117">Create and Configure Cloud Resources in Azure</span></span>
<span data-ttu-id="5825f-118">Innan du börjar den här proceduren måste du behöver toohave en aktiv Azure-prenumeration och skapa en standard-Active Directory (AD) på Azure.</span><span class="sxs-lookup"><span data-stu-id="5825f-118">Before you begin this procedure, you need toohave an active Azure subscription and set up a default Active Directory (AD) on Azure.</span></span>

### <a name="create-an-active-directory-ad-in-azure"></a><span data-ttu-id="5825f-119">Skapa en Active Directory (AD) i Azure</span><span class="sxs-lookup"><span data-stu-id="5825f-119">Create an Active Directory (AD) in Azure</span></span>
<span data-ttu-id="5825f-120">Om du inte redan har en Active Directory (AD) på din Azure-prenumeration logga in på hello [klassiska Azure-portalen] [ Azure classic portal] med ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="5825f-120">If you do not already have an Active Directory (AD) on your Azure subscription, log into hello [Azure classic portal][Azure classic portal] with your Microsoft account.</span></span> <span data-ttu-id="5825f-121">Om du har flera prenumerationer, klickar du på **prenumerationer** och välj hello standardkatalogen hello prenumeration du vill toouse för det här projektet.</span><span class="sxs-lookup"><span data-stu-id="5825f-121">If you have multiple subscriptions, click **Subscriptions** and select hello default directory for hello subscription you want toouse for this project.</span></span> <span data-ttu-id="5825f-122">Klicka på **tillämpa** tooswitch toothat prenumerationsvyn.</span><span class="sxs-lookup"><span data-stu-id="5825f-122">Then click **Apply** tooswitch toothat subscription view.</span></span>

1. <span data-ttu-id="5825f-123">Välj **Active Directory** hello menyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="5825f-123">Select **Active Directory** from hello menu at left.</span></span> <span data-ttu-id="5825f-124">**Klicka på Ny > Directory > Skapa anpassade**.</span><span class="sxs-lookup"><span data-stu-id="5825f-124">**Click New > Directory > Custom Create**.</span></span>
2. <span data-ttu-id="5825f-125">I **Lägg till katalog**väljer **Skapa ny katalog**.</span><span class="sxs-lookup"><span data-stu-id="5825f-125">In **Add Directory**, select **Create New Directory**.</span></span>
3. <span data-ttu-id="5825f-126">I **namn**, ange namnet på en katalog.</span><span class="sxs-lookup"><span data-stu-id="5825f-126">In **Name**, enter a directory name.</span></span>
4. <span data-ttu-id="5825f-127">I **domän**, ange ett domännamn.</span><span class="sxs-lookup"><span data-stu-id="5825f-127">In **Domain**, enter a domain name.</span></span> <span data-ttu-id="5825f-128">Detta är ett grundläggande domännamn som ingår som standard med din katalog. den har hello formuläret `<domain_name>.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="5825f-128">This is a basic domain name that is included by default with your directory; it has hello form `<domain_name>.onmicrosoft.com`.</span></span> <span data-ttu-id="5825f-129">Du kan kalla det baserat på hello directory eller ett annat domännamn som du äger.</span><span class="sxs-lookup"><span data-stu-id="5825f-129">You can name it based on hello directory name or another domain name that you own.</span></span> <span data-ttu-id="5825f-130">Du kan senare lägga till ett annat domännamn som din organisation redan använder.</span><span class="sxs-lookup"><span data-stu-id="5825f-130">Later, you can add another domain name that your organization already uses.</span></span>
5. <span data-ttu-id="5825f-131">I **land eller region**, Välj ditt språk.</span><span class="sxs-lookup"><span data-stu-id="5825f-131">In **Country or region**, select your locale.</span></span>

<span data-ttu-id="5825f-132">Mer information om AD finns [vad är Azure AD-katalog][What is an Azure AD directory]?</span><span class="sxs-lookup"><span data-stu-id="5825f-132">For more information on AD, see [What is an Azure AD directory][What is an Azure AD directory]?</span></span>

### <a name="create-a-management-certificate-for-azure"></a><span data-ttu-id="5825f-133">Skapa ett Hanteringscertifikat för Azure</span><span class="sxs-lookup"><span data-stu-id="5825f-133">Create a Management Certificate for Azure</span></span>
<span data-ttu-id="5825f-134">hello Azure SDK för Java använder management certifikat tooauthenticate med Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="5825f-134">hello Azure SDK for Java uses management certificates tooauthenticate with Azure subscriptions.</span></span> <span data-ttu-id="5825f-135">Dessa är X.509 v3-certifikat som du använder tooauthenticate ett klientprogram som använder hello Service Management API tooact uppdrag hello ägare toomanage prenumeration prenumerationsresurser.</span><span class="sxs-lookup"><span data-stu-id="5825f-135">These are X.509 v3 certificates you use tooauthenticate a client application that uses hello Service Management API tooact on behalf of hello subscription owner toomanage subscription resources.</span></span>

<span data-ttu-id="5825f-136">hello koden i den här proceduren använder ett självsignerat certifikat tooauthenticate med Azure.</span><span class="sxs-lookup"><span data-stu-id="5825f-136">hello code in this procedure uses a self-signed certificate tooauthenticate with Azure.</span></span> <span data-ttu-id="5825f-137">För den här proceduren ska du behöver toocreate ett certifikat och överföra den toohello [klassiska Azure-portalen] [ Azure classic portal] i förväg.</span><span class="sxs-lookup"><span data-stu-id="5825f-137">For this procedure, you need toocreate a certificate and upload it toohello [Azure classic portal][Azure classic portal] beforehand.</span></span> <span data-ttu-id="5825f-138">Detta omfattar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5825f-138">This involves hello following steps:</span></span>

* <span data-ttu-id="5825f-139">Generera en PFX-fil som representerar klientcertifikatet och spara den lokalt.</span><span class="sxs-lookup"><span data-stu-id="5825f-139">Generate a PFX file representing your client certificate and save it locally.</span></span>
* <span data-ttu-id="5825f-140">Skapa ett hanteringscertifikat (CER-fil) från hello PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="5825f-140">Generate a management certificate (CER file) from hello PFX file.</span></span>
* <span data-ttu-id="5825f-141">Överför hello CER-fil tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5825f-141">Upload hello CER file tooyour Azure subscription.</span></span>
* <span data-ttu-id="5825f-142">Konvertera hello PFX-filen till JKS, eftersom Java använder den format tooauthenticate med certifikat.</span><span class="sxs-lookup"><span data-stu-id="5825f-142">Convert hello PFX file into JKS, because Java uses that format tooauthenticate using certificates.</span></span>
* <span data-ttu-id="5825f-143">Skriva hello autentisering programkoden, vilket refererar toohello lokal JKS fil.</span><span class="sxs-lookup"><span data-stu-id="5825f-143">Write hello application's authentication code, which refers toohello local JKS file.</span></span>

<span data-ttu-id="5825f-144">När du slutför den här proceduren hello CER certifikatet ska placeras i din Azure-prenumeration och hello JKS certifikat kommer att finnas på en lokal hårddisk.</span><span class="sxs-lookup"><span data-stu-id="5825f-144">When you complete this procedure, hello CER certificate will reside in your Azure subscription and hello JKS certificate will reside on your local drive.</span></span> <span data-ttu-id="5825f-145">Mer information om certifikat finns [skapa och ladda upp ett Hanteringscertifikat för Azure][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="5825f-145">For more information on management certificates, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="create-a-certificate"></a><span data-ttu-id="5825f-146">Skapa ett certifikat</span><span class="sxs-lookup"><span data-stu-id="5825f-146">Create a certificate</span></span>
<span data-ttu-id="5825f-147">toocreate egna självsignerade certifikat, öppna kommandokonsolen i operativsystemet och kör följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="5825f-147">toocreate your own self-signed certificate, open a command console on your operating system and run hello following commands.</span></span>

> <span data-ttu-id="5825f-148">**Obs:** hello dator där du kör det här kommandot måste ha hello JDK installerad.</span><span class="sxs-lookup"><span data-stu-id="5825f-148">**Note:**  hello computer on which you run this command must have hello JDK installed.</span></span> <span data-ttu-id="5825f-149">Hello sökvägen toohello keytool beror också på hello plats där du installerar hello JDK.</span><span class="sxs-lookup"><span data-stu-id="5825f-149">Also, hello path toohello keytool depends on hello location in which you install hello JDK.</span></span> <span data-ttu-id="5825f-150">Mer information finns i [nyckeln och certifikatet Management Tool (keytool)] [ Key and Certificate Management Tool (keytool)] i hello Java online dokumenten.</span><span class="sxs-lookup"><span data-stu-id="5825f-150">For more information, see [Key and Certificate Management Tool (keytool)][Key and Certificate Management Tool (keytool)] in hello Java online docs.</span></span>
> 
> 

<span data-ttu-id="5825f-151">toocreate hello .pfx-fil:</span><span class="sxs-lookup"><span data-stu-id="5825f-151">toocreate hello .pfx file:</span></span>

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

<span data-ttu-id="5825f-152">toocreate hello .cer-fil:</span><span class="sxs-lookup"><span data-stu-id="5825f-152">toocreate hello .cer file:</span></span>

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

<span data-ttu-id="5825f-153">Där:</span><span class="sxs-lookup"><span data-stu-id="5825f-153">where:</span></span>

* <span data-ttu-id="5825f-154">`<java-install-dir>`är hello sökvägen toohello katalog där du installerade Java.</span><span class="sxs-lookup"><span data-stu-id="5825f-154">`<java-install-dir>` is hello path toohello directory in which you installed Java.</span></span>
* <span data-ttu-id="5825f-155">`<keystore-id>`Hej keystore post identifierare (till exempel `AzureRemoteAccess`).</span><span class="sxs-lookup"><span data-stu-id="5825f-155">`<keystore-id>` is hello keystore entry identifier (for example, `AzureRemoteAccess`).</span></span>
* <span data-ttu-id="5825f-156">`<cert-store-dir>`är hello sökvägen toohello katalog där du vill att toostore certifikat (till exempel `C:/Certificates`).</span><span class="sxs-lookup"><span data-stu-id="5825f-156">`<cert-store-dir>` is hello path toohello directory in which you want toostore certificates (for example `C:/Certificates`).</span></span>
* <span data-ttu-id="5825f-157">`<cert-file-name>`är hello namnet på hello certifikatfil (till exempel `AzureWebDemoCert`).</span><span class="sxs-lookup"><span data-stu-id="5825f-157">`<cert-file-name>` is hello name of hello certificate file (for example `AzureWebDemoCert`).</span></span>
* <span data-ttu-id="5825f-158">`<password>`hello-lösenord som du väljer tooprotect hello; Det måste vara minst 6 tecken.</span><span class="sxs-lookup"><span data-stu-id="5825f-158">`<password>` is hello password you choose tooprotect hello certificate; it must be at least 6 characters long.</span></span> <span data-ttu-id="5825f-159">Du kan ange inga lösenord, men detta inte rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="5825f-159">You can enter no password, although this is not recommended.</span></span>
* <span data-ttu-id="5825f-160">`<dname>`är hello X.500 huvudnamnet toobe som är associerade med alias och används som hello utfärdare och ämne fält i hello självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="5825f-160">`<dname>` is hello X.500 Distinguished Name toobe associated with alias, and is used as hello issuer and subject fields in hello self-signed certificate.</span></span>

<span data-ttu-id="5825f-161">Mer information finns i [skapa och ladda upp ett Hanteringscertifikat för Azure][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="5825f-161">For more information, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="upload-hello-certificate"></a><span data-ttu-id="5825f-162">Överför hello-certifikat</span><span class="sxs-lookup"><span data-stu-id="5825f-162">Upload hello certificate</span></span>
<span data-ttu-id="5825f-163">tooupload ett självsignerat certifikat tooAzure gå toohello **inställningar** på hello klassiska portalen och klicka sedan på hello **Hanteringscertifikat** fliken. Klicka på **överför** längst hello hello sidan och navigera toohello platsen för hello CER-fil som du skapade.</span><span class="sxs-lookup"><span data-stu-id="5825f-163">tooupload a self-signed certificate tooAzure, go toohello **Settings** page in hello classic portal, then click hello **Management Certificates** tab. Click **Upload** at hello bottom of hello page and navigate toohello location of hello CER file you created.</span></span>

#### <a name="convert-hello-pfx-file-into-jks"></a><span data-ttu-id="5825f-164">Konvertera hello PFX-filen till JKS</span><span class="sxs-lookup"><span data-stu-id="5825f-164">Convert hello PFX file into JKS</span></span>
<span data-ttu-id="5825f-165">I hello Windows kommandotolk (Kör som administratör), cd toohello katalog som innehåller hello certifikat och köra hello följande kommando, där `<java-install-dir>` är hello katalog där du installerade Java på datorn:</span><span class="sxs-lookup"><span data-stu-id="5825f-165">In hello Windows Command Prompt (running as admin), cd toohello directory containing hello certificates and run hello following command, where `<java-install-dir>` is hello directory in which you installed Java on your computer:</span></span>

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. <span data-ttu-id="5825f-166">När du uppmanas, ange hello mål keystore lösenord. Detta kan vara hello hello JKS filens lösenord.</span><span class="sxs-lookup"><span data-stu-id="5825f-166">When prompted, enter hello destination keystore password; this will be hello password for hello JKS file.</span></span>
2. <span data-ttu-id="5825f-167">När du uppmanas, ange hello källa keystore lösenord. Detta är hello lösenordet du angav för hello PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="5825f-167">When prompted, enter hello source keystore password; this is hello password you specified for hello PFX file.</span></span>

<span data-ttu-id="5825f-168">hello två lösenord har inte toobe hello samma.</span><span class="sxs-lookup"><span data-stu-id="5825f-168">hello two passwords do not have toobe hello same.</span></span> <span data-ttu-id="5825f-169">Du kan ange inga lösenord, men detta inte rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="5825f-169">You can enter no password, although this is not recommended.</span></span>

## <a name="build-a-web-app-creation-application"></a><span data-ttu-id="5825f-170">Skapa ett program för att skapa webbprogram</span><span class="sxs-lookup"><span data-stu-id="5825f-170">Build a Web App creation application</span></span>
### <a name="create-hello-eclipse-workspace-and-maven-project"></a><span data-ttu-id="5825f-171">Skapa hello Eclipse arbetsytan och Maven-projekt</span><span class="sxs-lookup"><span data-stu-id="5825f-171">Create hello Eclipse Workspace and Maven Project</span></span>
<span data-ttu-id="5825f-172">I det här avsnittet skapar du en arbetsyta och ett Maven-projekt för hello appen att skapa webbprogrammet, med namnet AzureWebDemo.</span><span class="sxs-lookup"><span data-stu-id="5825f-172">In this section you create a workspace and a Maven project for hello web app creation application, named AzureWebDemo.</span></span>

1. <span data-ttu-id="5825f-173">Skapa ett nytt Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="5825f-173">Create a new Maven project.</span></span> <span data-ttu-id="5825f-174">Klicka på **Arkiv > Nytt > Maven-projekt**.</span><span class="sxs-lookup"><span data-stu-id="5825f-174">Click **File > New > Maven Project**.</span></span> <span data-ttu-id="5825f-175">I **nya Maven-projekt**väljer **skapa ett enkelt projekt** och **använda standardplatsen för arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="5825f-175">In **New Maven Project**, select **Create a simple project** and **Use default workspace location**.</span></span>
2. <span data-ttu-id="5825f-176">På hello andra sidan i **nya Maven-projekt**, anger hello följande:</span><span class="sxs-lookup"><span data-stu-id="5825f-176">On hello second page of **New Maven Project**, specify hello following:</span></span>
   
   * <span data-ttu-id="5825f-177">Grupp-ID:`com.<username>.azure.webdemo`</span><span class="sxs-lookup"><span data-stu-id="5825f-177">Group ID: `com.<username>.azure.webdemo`</span></span>
   * <span data-ttu-id="5825f-178">Artefakt-ID: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="5825f-178">Artifact ID: AzureWebDemo</span></span>
   * <span data-ttu-id="5825f-179">Version: 0.0.1-SNAPSHOT</span><span class="sxs-lookup"><span data-stu-id="5825f-179">Version: 0.0.1-SNAPSHOT</span></span>
   * <span data-ttu-id="5825f-180">Paketera: jar</span><span class="sxs-lookup"><span data-stu-id="5825f-180">Packaging: jar</span></span>
   * <span data-ttu-id="5825f-181">Namn: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="5825f-181">Name: AzureWebDemo</span></span>
     
     <span data-ttu-id="5825f-182">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="5825f-182">Click **Finish**.</span></span>
3. <span data-ttu-id="5825f-183">Öppna filen för pom.xml av hello nytt projekt i Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="5825f-183">Open hello new project's pom.xml file in Project Explorer.</span></span> <span data-ttu-id="5825f-184">Välj hello **beroenden** fliken. Eftersom det är ett nytt projekt visas inga paket ännu.</span><span class="sxs-lookup"><span data-stu-id="5825f-184">Select hello **Dependencies** tab. As this is a new project, no packages are listed yet.</span></span>
4. <span data-ttu-id="5825f-185">Öppna hello Maven databaser visa.</span><span class="sxs-lookup"><span data-stu-id="5825f-185">Open hello Maven Repositories view.</span></span> <span data-ttu-id="5825f-186">**Klicka på fönstret > Visa > andra > Maven > Maven databaser** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5825f-186">**Click Window > Show View > Other > Maven > Maven Repositories** and click **OK**.</span></span> <span data-ttu-id="5825f-187">Hej **Maven databaser** vyn visas längst ned hello hello IDE.</span><span class="sxs-lookup"><span data-stu-id="5825f-187">hello **Maven Repositories** view will appear at hello bottom of hello IDE.</span></span>
5. <span data-ttu-id="5825f-188">Öppna **globala databaser**, högerklicka på hello **centrala** databasen och välj **Rebuild Index**.</span><span class="sxs-lookup"><span data-stu-id="5825f-188">Open **Global Repositories**, right-click hello **central** repository, and select **Rebuild Index**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="5825f-189">Det här steget kan ta flera minuter beroende på anslutningen hello hastighet.</span><span class="sxs-lookup"><span data-stu-id="5825f-189">This step can take several minutes depending on hello speed of your connection.</span></span> <span data-ttu-id="5825f-190">När hello index återskapar du bör se hello Microsoft Azure-paket i hello **centrala** Maven-databasen.</span><span class="sxs-lookup"><span data-stu-id="5825f-190">When hello index rebuilds, you should see hello Microsoft Azure packages in hello **central** Maven repository.</span></span>
6. <span data-ttu-id="5825f-191">I **beroenden**, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5825f-191">In **Dependencies**, click **Add**.</span></span> <span data-ttu-id="5825f-192">I **Ange grupp-ID....**  ange `azure-management`.</span><span class="sxs-lookup"><span data-stu-id="5825f-192">In **Enter Group ID...** enter `azure-management`.</span></span> <span data-ttu-id="5825f-193">Välj hello-paket för hanteringspaketdistributionen och hantering av App Service Web Apps:</span><span class="sxs-lookup"><span data-stu-id="5825f-193">Select hello packages for base management and App Service Web Apps management:</span></span>
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > <span data-ttu-id="5825f-194">**Obs:** om du uppdaterar hello beroenden när en ny version-version, måste toore-Lägg till var och en av hello beroenden i den här listan.</span><span class="sxs-lookup"><span data-stu-id="5825f-194">**Note:** If you are updating hello dependencies after a new version release, you need toore-add each of hello dependencies in this list.</span></span>
   > <span data-ttu-id="5825f-195">När du klickar på **Lägg till** och markera varje beroende, visas det med hello nytt versionsnummer i hello **beroenden** lista.</span><span class="sxs-lookup"><span data-stu-id="5825f-195">After you click **Add** and select each dependency, it appears with hello new version number in hello **Dependencies** list.</span></span>
   > 
   > 

<span data-ttu-id="5825f-196">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5825f-196">Click **OK**.</span></span> <span data-ttu-id="5825f-197">hello Azure-paket och sedan visas i hello **beroenden** lista.</span><span class="sxs-lookup"><span data-stu-id="5825f-197">hello Azure packages then appear in hello **Dependencies** list.</span></span>

### <a name="writing-java-code-toocreate-a-web-app-by-calling-hello-azure-sdk"></a><span data-ttu-id="5825f-198">Skriva Java-kod tooCreate en Webbapp som anropar hello Azure SDK</span><span class="sxs-lookup"><span data-stu-id="5825f-198">Writing Java Code tooCreate a Web App by Calling hello Azure SDK</span></span>
<span data-ttu-id="5825f-199">Skriv sedan hello-kod som anropar API: er i hello Azure SDK för Java toocreate hello App Service webbapp.</span><span class="sxs-lookup"><span data-stu-id="5825f-199">Next, write hello code that calls APIs in hello Azure SDK for Java toocreate hello App Service web app.</span></span>

1. <span data-ttu-id="5825f-200">Skapa en Java-klass toocontain hello huvudsakliga post punkt kod.</span><span class="sxs-lookup"><span data-stu-id="5825f-200">Create a Java class toocontain hello main entry point code.</span></span> <span data-ttu-id="5825f-201">Högerklicka på projektnoden hello i Projektutforskaren, och välj **New > klassen**.</span><span class="sxs-lookup"><span data-stu-id="5825f-201">In Project Explorer, right-click on hello project node and select **New > Class**.</span></span>
2. <span data-ttu-id="5825f-202">I **ny Java-klass**, namnge hello klassen `WebCreator` och kontrollera hello **offentliga statiska void main** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="5825f-202">In **New Java Class**, name hello class `WebCreator` and check hello **public static void main** checkbox.</span></span> <span data-ttu-id="5825f-203">hello val ska visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5825f-203">hello selections should appear as follows:</span></span>
   
    ![][2]
3. <span data-ttu-id="5825f-204">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="5825f-204">Click **Finish**.</span></span> <span data-ttu-id="5825f-205">Hej WebCreator.java filen visas i Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="5825f-205">hello WebCreator.java file appears in Project Explorer.</span></span>

### <a name="calling-hello-azure-api-toocreate-an-app-service-web-app"></a><span data-ttu-id="5825f-206">Anropar hello Azure API tooCreate en App Service Web App</span><span class="sxs-lookup"><span data-stu-id="5825f-206">Calling hello Azure API tooCreate an App Service Web App</span></span>
#### <a name="add-necessary-imports"></a><span data-ttu-id="5825f-207">Lägga till nödvändiga importer</span><span class="sxs-lookup"><span data-stu-id="5825f-207">Add necessary imports</span></span>
<span data-ttu-id="5825f-208">WebCreator.java, lägga till hello efter importen. importen ge åtkomst tooclasses i hello bibliotek för förbrukning av Azure API: er:</span><span class="sxs-lookup"><span data-stu-id="5825f-208">In WebCreator.java, add hello following imports; these imports provide access tooclasses in hello management libraries for consuming Azure APIs:</span></span>

    // General imports
    import java.net.URI;
    import java.util.ArrayList;

    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;

    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;

    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;

    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-hello-main-entry-point-class"></a><span data-ttu-id="5825f-209">Definiera hello huvudsakliga post punkt klass</span><span class="sxs-lookup"><span data-stu-id="5825f-209">Define hello main entry point class</span></span>
<span data-ttu-id="5825f-210">Eftersom hello syftet med hello AzureWebDemo program är toocreate en App Service Web App name hello huvudsakliga klass för det här programmet `WebAppCreator`.</span><span class="sxs-lookup"><span data-stu-id="5825f-210">Because hello purpose of hello AzureWebDemo application is toocreate an App Service Web App, name hello main class for this application `WebAppCreator`.</span></span> <span data-ttu-id="5825f-211">Den här klassen innehåller hello huvudsakliga post punkt kod som anropar hello Azure Service Management API toocreate hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="5825f-211">This class provides hello main entry point code that calls hello Azure Service Management API toocreate hello web app.</span></span>

<span data-ttu-id="5825f-212">Lägg till följande parameterdefinitioner för hello webbapp och webbutrymmet hello.</span><span class="sxs-lookup"><span data-stu-id="5825f-212">Add hello following parameter definitions for hello web app and webspace.</span></span> <span data-ttu-id="5825f-213">Du behöver tooprovide din egen Azure-prenumeration-ID och certifikat information.</span><span class="sxs-lookup"><span data-stu-id="5825f-213">You will need tooprovide your own Azure subscription ID and certificate information.</span></span>

    public class WebAppCreator {

        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";

        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

<span data-ttu-id="5825f-214">Där:</span><span class="sxs-lookup"><span data-stu-id="5825f-214">where:</span></span>

* <span data-ttu-id="5825f-215">`<subscription-id>`är hello Azure prenumerations-ID som du vill toocreate hello resurs.</span><span class="sxs-lookup"><span data-stu-id="5825f-215">`<subscription-id>` is hello Azure subscription ID in which you want toocreate hello resource.</span></span>
* <span data-ttu-id="5825f-216">`<certificate-store-path>`är hello sökvägen och filnamnet toohello JKS fil i katalogen lagra lokala certifikatarkivet.</span><span class="sxs-lookup"><span data-stu-id="5825f-216">`<certificate-store-path>` is hello path and filename toohello JKS file in your local certificate store directory.</span></span> <span data-ttu-id="5825f-217">Till exempel `C:/Certificates/CertificateName.jks` för Linux och `C:\Certificates\CertificateName.jks` för Windows.</span><span class="sxs-lookup"><span data-stu-id="5825f-217">For example, `C:/Certificates/CertificateName.jks` for Linux and `C:\Certificates\CertificateName.jks` for Windows.</span></span>
* <span data-ttu-id="5825f-218">`<certificate-password>`är hello-lösenord som du angav när du skapade ditt JKS certifikat.</span><span class="sxs-lookup"><span data-stu-id="5825f-218">`<certificate-password>` is hello password you specified when you created your JKS certificate.</span></span>
* <span data-ttu-id="5825f-219">`webAppName`kan vara vilket namn som du väljer; den här proceduren använder hello namn `WebDemoWebApp`.</span><span class="sxs-lookup"><span data-stu-id="5825f-219">`webAppName` can be any name you choose; this procedure uses hello name `WebDemoWebApp`.</span></span> <span data-ttu-id="5825f-220">hello fullständigt domännamn är hello `webAppName` med hello `domainName` läggs, så i detta fall hello fullständigt domännamn är `webdemowebapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="5825f-220">hello full domain name is hello `webAppName` with hello `domainName` appended, so in this case hello full domain is `webdemowebapp.azurewebsites.net`.</span></span>
* <span data-ttu-id="5825f-221">`domainName`ska anges som ovan.</span><span class="sxs-lookup"><span data-stu-id="5825f-221">`domainName` should be specified as shown above.</span></span>
* <span data-ttu-id="5825f-222">`webSpaceName`ska vara någon av hello värden som definieras i hello [WebSpaceNames] [ WebSpaceNames] klass.</span><span class="sxs-lookup"><span data-stu-id="5825f-222">`webSpaceName` should be one of hello values defined in hello [WebSpaceNames][WebSpaceNames] class.</span></span>
* <span data-ttu-id="5825f-223">`appServicePlanName`ska anges som ovan.</span><span class="sxs-lookup"><span data-stu-id="5825f-223">`appServicePlanName` should be specified as shown above.</span></span>

> <span data-ttu-id="5825f-224">**Obs:** varje gång du kör det här programmet måste du toochange hello värdet för `webAppName` och `appServicePlanName` (eller ta bort hello webbprogram på hello Azure Portal) innan du kör hello programmet igen.</span><span class="sxs-lookup"><span data-stu-id="5825f-224">**Note:** Each time you run this application, you need toochange hello value of `webAppName` and `appServicePlanName` (or delete hello web app on hello Azure Portal) before running hello application again.</span></span> <span data-ttu-id="5825f-225">Annars misslyckas körningen eftersom hello samma resursen finns redan på Azure.</span><span class="sxs-lookup"><span data-stu-id="5825f-225">Otherwise, execution will fail because hello same resource already exists on Azure.</span></span>
> 
> 

#### <a name="define-hello-web-creation-method"></a><span data-ttu-id="5825f-226">Definiera hello metod för skapande av webbprogram</span><span class="sxs-lookup"><span data-stu-id="5825f-226">Define hello web creation method</span></span>
<span data-ttu-id="5825f-227">Sedan definiera en metod toocreate hello-webbapp.</span><span class="sxs-lookup"><span data-stu-id="5825f-227">Next, define a method toocreate hello web app.</span></span> <span data-ttu-id="5825f-228">Den här metoden `createWebApp`, anger hello parametrar av hello webbapp och hello webbutrymmet.</span><span class="sxs-lookup"><span data-stu-id="5825f-228">This method, `createWebApp`, specifies hello parameters of hello web app and hello webspace.</span></span> <span data-ttu-id="5825f-229">Dessutom skapar och konfigurerar hello App Service Web Apps management-klienten, som definieras av hello [WebSiteManagementClient] [ WebSiteManagementClient] objekt.</span><span class="sxs-lookup"><span data-stu-id="5825f-229">It also creates and configures hello App Service Web Apps management client, which is defined by hello [WebSiteManagementClient][WebSiteManagementClient] object.</span></span> <span data-ttu-id="5825f-230">hello Hanteringsklient är viktiga toocreating Web Apps.</span><span class="sxs-lookup"><span data-stu-id="5825f-230">hello management client is key toocreating Web Apps.</span></span> <span data-ttu-id="5825f-231">Det ger RESTful webbtjänster som möjliggör program toomanage webbprogram (utför åtgärder som skapar, uppdatera och ta bort) genom att anropa hello service management API.</span><span class="sxs-lookup"><span data-stu-id="5825f-231">It provides RESTful web services that allow applications toomanage web apps (performing operations such as create, update, and delete) by calling hello service management API.</span></span>

    private static void createWebApp() throws Exception {

        // Specify configuration settings for hello App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path toohello JKS file
            keyStorePassword,  // Password for hello JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create hello App Service Web Apps management client toocall Azure APIs
        // and pass it hello App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for hello web app with hello specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that hello server farm name takes hello Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define hello web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create hello web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output hello HTTP status code of hello response; 200 indicates hello request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output hello name of hello web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

<span data-ttu-id="5825f-232">hello koden kommer utdata hello HTTP-status för hello-svar som visar lyckad eller misslyckad och om det lyckas, kommer att bli hello namnet på hello skapa webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5825f-232">hello code will output hello HTTP status of hello response indicating success or failure, and if successful, will output hello name of hello created web app.</span></span>

#### <a name="define-hello-main-method"></a><span data-ttu-id="5825f-233">Definiera hello main() metod</span><span class="sxs-lookup"><span data-stu-id="5825f-233">Define hello main() method</span></span>
<span data-ttu-id="5825f-234">Ange hello main() metoden kod som anropar createWebApp() toocreate hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="5825f-234">Provide hello main() method code that calls createWebApp() toocreate hello web app.</span></span>

<span data-ttu-id="5825f-235">Slutligen anropa `createWebApp` från `main`:</span><span class="sxs-lookup"><span data-stu-id="5825f-235">Finally, call `createWebApp` from `main`:</span></span>

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-hello-application-and-verify-web-app-creation"></a><span data-ttu-id="5825f-236">Kör programmet hello och kontrollera att skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="5825f-236">Run hello application and verify web app creation</span></span>
<span data-ttu-id="5825f-237">tooverify som programmet körs, klickar du på **kör > kör**.</span><span class="sxs-lookup"><span data-stu-id="5825f-237">tooverify that your application runs, click **Run > Run**.</span></span> <span data-ttu-id="5825f-238">När programmet hello har slutförts körs, bör du se hello följande utdata i hello Eclipse-konsolen:</span><span class="sxs-lookup"><span data-stu-id="5825f-238">When hello application completes running, you should see hello following output in hello Eclipse console:</span></span>

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

<span data-ttu-id="5825f-239">Logga in på hello klassiska Azure-portalen och på **Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="5825f-239">Log into hello Azure classic portal and click **Web Apps**.</span></span> <span data-ttu-id="5825f-240">hello nytt webbprogram ska visas i listan för hello Web Apps inom några minuter.</span><span class="sxs-lookup"><span data-stu-id="5825f-240">hello new web app should appear in hello Web Apps list within a few minutes.</span></span>

## <a name="deploying-an-application-toohello-web-app"></a><span data-ttu-id="5825f-241">Distribuera ett program toohello Web App</span><span class="sxs-lookup"><span data-stu-id="5825f-241">Deploying an Application toohello Web App</span></span>
<span data-ttu-id="5825f-242">När du har kört AzureWebDemo och skapat hello nytt webbprogram, logga in på hello klassiska portalen klickar du på **Web Apps**, och välj **WebDemoWebApp** i hello **Web Apps** lista.</span><span class="sxs-lookup"><span data-stu-id="5825f-242">After you have run AzureWebDemo and created hello new web app, log into hello classic portal, click **Web Apps**, and select **WebDemoWebApp** in hello **Web Apps** list.</span></span> <span data-ttu-id="5825f-243">På hello webbapp instrumentpanelen klickar du på **Bläddra** (eller klicka på hello URL `webdemowebapp.azurewebsites.net`) toonavigate tooit.</span><span class="sxs-lookup"><span data-stu-id="5825f-243">In hello web app's dashboard page, click **Browse** (or click hello URL, `webdemowebapp.azurewebsites.net`) toonavigate tooit.</span></span> <span data-ttu-id="5825f-244">En tom platshållare-sida visas eftersom inget innehåll har publicerade toohello webbprogrammet ännu.</span><span class="sxs-lookup"><span data-stu-id="5825f-244">You will see a blank placeholder page, because no content has been published toohello web app yet.</span></span>

<span data-ttu-id="5825f-245">Nästa du skapar ett ”Hello World”-program och distribuera den toohello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5825f-245">Next you will create a "Hello World" application and deploy it toohello web app.</span></span>

### <a name="create-a-jsp-hello-world-application"></a><span data-ttu-id="5825f-246">Skapa ett JSP Hello World-program</span><span class="sxs-lookup"><span data-stu-id="5825f-246">Create a JSP Hello World application</span></span>
#### <a name="create-hello-application"></a><span data-ttu-id="5825f-247">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="5825f-247">Create hello application</span></span>
<span data-ttu-id="5825f-248">I ordning toodemonstrate hur toodeploy en toohello web programmet hello följande procedur beskrivs hur du toocreate ett enkelt ”Hello World” Java-program och ladda upp den toohello App Service Web App som skapats av ditt program.</span><span class="sxs-lookup"><span data-stu-id="5825f-248">In order toodemonstrate how toodeploy an application toohello web, hello following procedure shows you how toocreate a simple "Hello World" Java application and upload it toohello App Service Web App that your application created.</span></span>

1. <span data-ttu-id="5825f-249">Klicka på **Arkiv > Nytt > dynamiskt webbprojekt**.</span><span class="sxs-lookup"><span data-stu-id="5825f-249">Click **File > New > Dynamic Web Project**.</span></span> <span data-ttu-id="5825f-250">Ge den namnet `JSPHello`.</span><span class="sxs-lookup"><span data-stu-id="5825f-250">Name it `JSPHello`.</span></span> <span data-ttu-id="5825f-251">Du behöver inte toochange andra inställningar i den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5825f-251">You do not need toochange any other settings in this dialog.</span></span> <span data-ttu-id="5825f-252">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="5825f-252">Click **Finish**.</span></span>
   
    ![][3]
2. <span data-ttu-id="5825f-253">Expandera hello i Projektutforskaren **JSPHello** projekt, högerklicka på **webbinnehåll**, klicka på **New > JSP-fil**.</span><span class="sxs-lookup"><span data-stu-id="5825f-253">In Project Explorer, expand hello **JSPHello** project, right-click **WebContent**, then click **New > JSP File**.</span></span> <span data-ttu-id="5825f-254">I dialogrutan för ny JSP-fil hello namn hello nya filen `index.jsp`.</span><span class="sxs-lookup"><span data-stu-id="5825f-254">In hello New JSP File dialog, name hello new file `index.jsp`.</span></span> <span data-ttu-id="5825f-255">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="5825f-255">Click **Next**.</span></span>
3. <span data-ttu-id="5825f-256">I hello **Välj JSP-mall** väljer **ny JSP-fil (html)** och på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="5825f-256">In hello **Select JSP Template** dialog, select **New JSP File (html)** and click **Finish**.</span></span>
4. <span data-ttu-id="5825f-257">Lägg till följande kod i hello hello i index.jsp, `<head>` och `<body>` tagga avsnitt:</span><span class="sxs-lookup"><span data-stu-id="5825f-257">In index.jsp, add hello following code in hello `<head>` and `<body>` tag sections:</span></span>
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, hello time is <%= date %> 
        </body>

#### <a name="run-hello-hello-world-application-in-localhost"></a><span data-ttu-id="5825f-258">Kör hello World Hello program i localhost</span><span class="sxs-lookup"><span data-stu-id="5825f-258">Run hello Hello World application in localhost</span></span>
<span data-ttu-id="5825f-259">Innan du kör det här programmet måste tooconfigure några egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5825f-259">Before you run this application, you need tooconfigure a few properties.</span></span>

1. <span data-ttu-id="5825f-260">Högerklicka på hello **JSPHello** projektet och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="5825f-260">Right-click hello **JSPHello** project and select **Properties**.</span></span>
2. <span data-ttu-id="5825f-261">I hello **egenskaper** dialogrutan: Välj **Java byggsökväg**väljer hello **ordning och exportera** markerar **JRE systembibliotek**, klicka på **In** toomove den toohello överkant hello-listan.</span><span class="sxs-lookup"><span data-stu-id="5825f-261">In hello **Properties** dialog: select **Java Build Path**, select hello **Order and Export** tab, check **JRE System Library**, then click **Up** toomove it toohello top of hello list.</span></span>
   
    ![][4]
3. <span data-ttu-id="5825f-262">Även i hello **egenskaper** dialogrutan: Välj **Körningsmål** och på **ny**.</span><span class="sxs-lookup"><span data-stu-id="5825f-262">Also in hello **Properties** dialog: select **Targeted Runtimes** and click **New**.</span></span>
4. <span data-ttu-id="5825f-263">I hello **nya Server-körningsmiljön** dialogrutan, Välj en server som **Apache Tomcat v7.0** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="5825f-263">In hello **New Server Runtime Environment** dialog, select a server such as **Apache Tomcat v7.0** and click **Next**.</span></span> <span data-ttu-id="5825f-264">I hello **Tomcat Server** dialogrutan, ange **namn** för`Apache Tomcat v7.0`, och ange **Tomcat installationskatalog** toohello katalog där du installerade hello-version Tomcat-server som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="5825f-264">In hello **Tomcat Server** dialog, set **Name** too`Apache Tomcat v7.0`, and set **Tomcat Installation Directory** toohello directory in which you installed hello version of Tomcat server you want toouse.</span></span>
   
    ![][5]
   
    <span data-ttu-id="5825f-265">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="5825f-265">Click **Finish**.</span></span>
5. <span data-ttu-id="5825f-266">Toohello returnerar **Körningsmål** sidan hello **egenskaper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5825f-266">You then return toohello **Targeted Runtimes** page of hello **Properties** dialog.</span></span> <span data-ttu-id="5825f-267">Välj **Apache Tomcat v7.0**, klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5825f-267">Select **Apache Tomcat v7.0**, then click **OK**.</span></span>
   
    ![][6]
6. <span data-ttu-id="5825f-268">I hello Eclipse **kör** -menyn klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="5825f-268">In hello Eclipse **Run** menu, click **Run**.</span></span> <span data-ttu-id="5825f-269">I hello **kör som** markerar **körs på servern**.</span><span class="sxs-lookup"><span data-stu-id="5825f-269">In hello **Run As** dialog, select **Run on Server**.</span></span> <span data-ttu-id="5825f-270">I hello **körs på servern** markerar **Tomcat v7.0-Server**:</span><span class="sxs-lookup"><span data-stu-id="5825f-270">In hello **Run on Server** dialog, select **Tomcat v7.0 Server**:</span></span>
   
    ![][7]
   
    <span data-ttu-id="5825f-271">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="5825f-271">Click **Finish**.</span></span>
7. <span data-ttu-id="5825f-272">Hej när programmet körs, bör du se hello **JSPHello** visas sidan i ett fönster för localhost i Eclipse (`http://localhost:8080/JSPHello/`), visa hello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="5825f-272">When hello application runs, you should see hello **JSPHello** page appear in a localhost window in Eclipse (`http://localhost:8080/JSPHello/`), displaying hello following message:</span></span>
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-hello-application-as-a-war"></a><span data-ttu-id="5825f-273">Exportera hello program som en WAR-fil</span><span class="sxs-lookup"><span data-stu-id="5825f-273">Export hello application as a WAR</span></span>
<span data-ttu-id="5825f-274">Exportera hello web project-filer som en webb-arkivfil (WAR) så att du kan distribuera den toohello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5825f-274">Export hello web project files as a web archive (WAR) file so that you can deploy it toohello web app.</span></span> <span data-ttu-id="5825f-275">hello finns följande web project-filer i hello webbinnehåll mapp:</span><span class="sxs-lookup"><span data-stu-id="5825f-275">hello following web project files reside in hello WebContent folder:</span></span>

    META-INF
    WEB-INF
    index.jsp

1. <span data-ttu-id="5825f-276">Högerklicka på mappen för hello webbinnehåll och välj **exportera**.</span><span class="sxs-lookup"><span data-stu-id="5825f-276">Right-click hello WebContent folder and select **Export**.</span></span>
2. <span data-ttu-id="5825f-277">I hello **exportera Välj** dialogrutan klickar du på **Web > WAR** filen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="5825f-277">In hello **Export Select** dialog, click **Web > WAR** file, then click **Next**.</span></span>
3. <span data-ttu-id="5825f-278">I hello **WAR-Export** dialogrutan Välj hello src katalog i hello aktuella projektet och inkludera hello namnet på hello WAR-filen hello slutet.</span><span class="sxs-lookup"><span data-stu-id="5825f-278">In hello **WAR Export** dialog, select hello src directory in hello current project, and include hello name of hello WAR file at hello end.</span></span> <span data-ttu-id="5825f-279">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5825f-279">For example:</span></span>
   
    `<project-path>/JSPHello/src/JSPHello.war`

<span data-ttu-id="5825f-280">Mer information om hur du distribuerar WAR-filerna finns [lägga till en Java-program tooAzure App Service Web Apps](web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="5825f-280">For more information on deploying WAR files, see [Add a Java application tooAzure App Service Web Apps](web-sites-java-add-app.md).</span></span>

### <a name="deploying-hello-hello-world-application-using-ftp"></a><span data-ttu-id="5825f-281">Distribuera hello Hello World program med hjälp av FTP</span><span class="sxs-lookup"><span data-stu-id="5825f-281">Deploying hello Hello World Application Using FTP</span></span>
<span data-ttu-id="5825f-282">Markera en FTP-klient toopublish hello tredjepartsprogram.</span><span class="sxs-lookup"><span data-stu-id="5825f-282">Select a third-party FTP client toopublish hello application.</span></span> <span data-ttu-id="5825f-283">Den här proceduren beskrivs två alternativ: hello Kudu-konsolen som är inbyggda i Azure och FileZilla, ett populära verktyg med en lämplig, grafiskt användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="5825f-283">This procedure describes two options: hello Kudu console built into Azure; and FileZilla, a popular tool with a convenient, graphical UI.</span></span>

> <span data-ttu-id="5825f-284">**Obs:** hello Azure Toolkit för Eclipse stöder distribution toostorage konton och cloud services, men stöder inte distribution tooweb appar.</span><span class="sxs-lookup"><span data-stu-id="5825f-284">**Note:** hello Azure Toolkit for Eclipse supports deployment toostorage accounts and cloud services, but does not currently support deployment tooweb apps.</span></span> <span data-ttu-id="5825f-285">Du kan distribuera toostorage konton och molntjänster med ett projekt för distribution av Azure enligt beskrivningen i [skapar en Hello World program för Azure i Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), men inte tooweb appar.</span><span class="sxs-lookup"><span data-stu-id="5825f-285">You can deploy toostorage accounts and cloud services using an Azure Deployment Project as described in [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), but not tooweb apps.</span></span> <span data-ttu-id="5825f-286">Använda andra metoder, till exempel FTP eller GitHub tootransfer filer tooyour webb-app.</span><span class="sxs-lookup"><span data-stu-id="5825f-286">Use other methods such as FTP or GitHub tootransfer files tooyour web app.</span></span>
> 
> <span data-ttu-id="5825f-287">**Obs:** rekommenderar vi inte med FTP från Kommandotolken för Windows hello (hello FTP.EXE kommandoradsverktyg som medföljer Windows).</span><span class="sxs-lookup"><span data-stu-id="5825f-287">**Note:** We do not recommend using FTP from hello Windows command prompt (hello command-line FTP.EXE utility that ships with Windows).</span></span> <span data-ttu-id="5825f-288">FTP-klienter som använder active FTP, till exempel FTP.EXE, växlar ofta toowork över brandväggar.</span><span class="sxs-lookup"><span data-stu-id="5825f-288">FTP clients that use active FTP, such as FTP.EXE, often fail toowork over firewalls.</span></span> <span data-ttu-id="5825f-289">Aktiva FTP anger en intern nätverksbaserade adress, toowhich en FTP-servern misslyckas sannolikt tooconnect.</span><span class="sxs-lookup"><span data-stu-id="5825f-289">Active FTP specifies an internal LAN-based address, toowhich an FTP server will likely fail tooconnect.</span></span>
> 
> 

<span data-ttu-id="5825f-290">Mer information om distribution tooan App Service webbapp med FTP finns hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="5825f-290">For more information on deployment tooan App Service web app using FTP, see hello following topics:</span></span>

* [<span data-ttu-id="5825f-291">Distribuera med hjälp av ett FTP-verktyg</span><span class="sxs-lookup"><span data-stu-id="5825f-291">Deploy using an FTP utility</span></span>](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a><span data-ttu-id="5825f-292">Ställ in autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="5825f-292">Set up deployment credentials</span></span>
<span data-ttu-id="5825f-293">Kontrollera att du har kört hello **AzureWebDemo** programmet toocreate ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5825f-293">Make sure you have run hello **AzureWebDemo** application toocreate a web app.</span></span> <span data-ttu-id="5825f-294">Du överför filer toothis plats.</span><span class="sxs-lookup"><span data-stu-id="5825f-294">You will transfer files toothis location.</span></span>

1. <span data-ttu-id="5825f-295">Logga in hello klassiska portal och klicka på **Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="5825f-295">Log into hello classic portal and click **Web Apps**.</span></span> <span data-ttu-id="5825f-296">Kontrollera att **WebDemoWebApp** visas i hello lista över webbappar och se till att den körs.</span><span class="sxs-lookup"><span data-stu-id="5825f-296">Make sure **WebDemoWebApp** appears in hello list of web apps, and make sure that it is running.</span></span> <span data-ttu-id="5825f-297">Klicka på **WebDemoWebApp** tooopen dess **instrumentpanelen** sidan.</span><span class="sxs-lookup"><span data-stu-id="5825f-297">Click **WebDemoWebApp** tooopen its **Dashboard** page.</span></span>
2. <span data-ttu-id="5825f-298">På hello **instrumentpanelen** sidan under **snabb i korthet**, klickar du på **ställa in dina autentiseringsuppgifter för distribution** (om du redan har autentiseringsuppgifter för distribution läser detta  **Återställa dina autentiseringsuppgifter för distribution**).</span><span class="sxs-lookup"><span data-stu-id="5825f-298">On hello **Dashboard** page, under **Quick Glance**, click **Set up your deployment credentials** (if you already have deployment credentials, this reads **Reset your deployment credentials**).</span></span>
   
    <span data-ttu-id="5825f-299">Autentiseringsuppgifter för distribution är associerade med ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="5825f-299">Deployment credentials are associated with a Microsoft account.</span></span> <span data-ttu-id="5825f-300">Du behöver toospecify ett användarnamn och lösenord som du kan använda toodeploy med Git och FTP.</span><span class="sxs-lookup"><span data-stu-id="5825f-300">You need toospecify a username and password that you can use toodeploy using Git and FTP.</span></span> <span data-ttu-id="5825f-301">Du kan använda dessa autentiseringsuppgifter toodeploy tooany webbprogram i alla Azure-prenumerationer som är kopplade till ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="5825f-301">You can use these credentials toodeploy tooany web app in all Azure subscriptions associated with your Microsoft account.</span></span> <span data-ttu-id="5825f-302">Ange autentiseringsuppgifter för Git och FTP-distribution i hello dialogrutan och registrera hello användarnamn och lösenord för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="5825f-302">Provide Git and FTP deployment credentials in hello dialog, and record hello username and password for future use.</span></span>

#### <a name="get-ftp-connection-information"></a><span data-ttu-id="5825f-303">Hämta information om FTP-anslutning</span><span class="sxs-lookup"><span data-stu-id="5825f-303">Get FTP connection information</span></span>
<span data-ttu-id="5825f-304">toouse FTP toodeploy programmet filer toohello nyskapad webbapp måste tooobtain anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="5825f-304">toouse FTP toodeploy application files toohello newly created web app, you need tooobtain connection information.</span></span> <span data-ttu-id="5825f-305">Det finns två sätt tooobtain anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="5825f-305">There are two ways tooobtain connection information.</span></span> <span data-ttu-id="5825f-306">Ett sätt är toovisit hello webbappens **instrumentpanelen** sidan; hello annat sätt är toodownload hello webbprogram Publicera profil.</span><span class="sxs-lookup"><span data-stu-id="5825f-306">One way is toovisit hello web app's **Dashboard** page; hello other way is toodownload hello web app's publish profile.</span></span> <span data-ttu-id="5825f-307">hello publiceringsprofil är en XML-fil som innehåller information som FTP-värden namn och inloggning autentiseringsuppgifter för dina webbappar i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5825f-307">hello publish profile is an XML file that provides information such as FTP host name and logon credentials for your web apps in Azure App Service.</span></span> <span data-ttu-id="5825f-308">Du kan använda det här användarnamnet och lösenordet toodeploy tooany webbprogrammet i alla prenumerationer som är kopplade till hello Azure-konto, inte bara det här en.</span><span class="sxs-lookup"><span data-stu-id="5825f-308">You can use this username and password toodeploy tooany web app in all subscriptions associated with hello Azure account, not only this one.</span></span>

<span data-ttu-id="5825f-309">tooobtain FTP anslutningsinformation från hello webbapps blad i hello [Azure Portal][Azure Portal]:</span><span class="sxs-lookup"><span data-stu-id="5825f-309">tooobtain FTP connection information from hello web app's blade in hello [Azure Portal][Azure Portal]:</span></span>

1. <span data-ttu-id="5825f-310">Under **Essentials**, hitta och kopiera hello **värdnamn för FTP-**.</span><span class="sxs-lookup"><span data-stu-id="5825f-310">Under **Essentials**, find and copy hello **FTP hostname**.</span></span> <span data-ttu-id="5825f-311">Detta är en liknande URI för`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="5825f-311">This is a URI similar too`ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span></span>
2. <span data-ttu-id="5825f-312">Under **Essentials**, hitta och kopiera **användarnamn för FTP-/ Distributionsanvändare**.</span><span class="sxs-lookup"><span data-stu-id="5825f-312">Under **Essentials**, find and copy **FTP/Deployment username**.</span></span> <span data-ttu-id="5825f-313">Detta innebär att hello formuläret *webappname\deployment-username*, till exempel `WebDemoWebApp\deployer77`.</span><span class="sxs-lookup"><span data-stu-id="5825f-313">This will have hello form *webappname\deployment-username*; for example `WebDemoWebApp\deployer77`.</span></span>

<span data-ttu-id="5825f-314">tooobtain FTP anslutningsinformation från hello Publicera profil:</span><span class="sxs-lookup"><span data-stu-id="5825f-314">tooobtain FTP connection information from hello publish profile:</span></span>

1. <span data-ttu-id="5825f-315">I hello webbapps blad klickar du på **Get publiceringsprofil**.</span><span class="sxs-lookup"><span data-stu-id="5825f-315">In hello web app's blade, click **Get publish profile**.</span></span> <span data-ttu-id="5825f-316">Detta kommer att ladda ned en .publishsettings-fil tooyour lokal enhet.</span><span class="sxs-lookup"><span data-stu-id="5825f-316">This will download a .publishsettings file tooyour local drive.</span></span>
2. <span data-ttu-id="5825f-317">Öppna hello .publishsettings-fil i en XML-redigerare eller textredigerare och hitta hello `<publishProfile>` element som innehåller `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="5825f-317">Open hello .publishsettings file in an XML editor or text editor and find hello `<publishProfile>` element containing `publishMethod="FTP"`.</span></span> <span data-ttu-id="5825f-318">Det bör se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="5825f-318">It should look like hello following:</span></span>
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. <span data-ttu-id="5825f-319">Observera att hello webbprogrammet `publishProfile` inställningar mappa toohello FileZilla Platshanteraren inställningarna enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5825f-319">Note that hello web app's `publishProfile` settings map toohello FileZilla Site Manager settings as follows:</span></span>

* <span data-ttu-id="5825f-320">`publishUrl`är hello samma som **värdnamn för FTP-**, hello värdet i **värden**.</span><span class="sxs-lookup"><span data-stu-id="5825f-320">`publishUrl` is hello same as **FTP host name**, hello value you set in **Host**.</span></span>
* <span data-ttu-id="5825f-321">`publishMethod="FTP"`innebär att du ställer in **protokollet** för**FTP - File Transfer Protocol**, och **kryptering** för**använder vanlig FTP**.</span><span class="sxs-lookup"><span data-stu-id="5825f-321">`publishMethod="FTP"` means that you set **Protocol** too**FTP - File Transfer Protocol**, and **Encryption** too**Use plain FTP**.</span></span>
* <span data-ttu-id="5825f-322">`userName`och `userPWD` är nycklarna för hello faktiska användarnamn och lösenord för värden som du angav när du återställer hello autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="5825f-322">`userName` and `userPWD` are keys for hello actual username and password values you specified when you reset hello deployment credentials.</span></span> <span data-ttu-id="5825f-323">`userName`är hello samma som **distribution / FTP-användare**.</span><span class="sxs-lookup"><span data-stu-id="5825f-323">`userName` is hello same as **Deployment / FTP user**.</span></span> <span data-ttu-id="5825f-324">Mappa för**användare** och **lösenord** i FileZilla.</span><span class="sxs-lookup"><span data-stu-id="5825f-324">They map too**User** and **Password** in FileZilla.</span></span>
* <span data-ttu-id="5825f-325">`ftpPassiveMode="True"`innebär att hello FTP-platsen använder passiv FTP-överföringen. Välj **passiva** på hello **Överföringsinställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="5825f-325">`ftpPassiveMode="True"` means that hello FTP site uses passive FTP transfer; select **Passive** on hello **Transfer Settings** tab.</span></span>

#### <a name="configure-hello-web-app-toohost-a-java-application"></a><span data-ttu-id="5825f-326">Konfigurera hello webbprogrammet toohost ett Java-program</span><span class="sxs-lookup"><span data-stu-id="5825f-326">Configure hello Web App toohost a Java application</span></span>
<span data-ttu-id="5825f-327">Innan du publicerar hello program behöver du toochange några konfigurationsinställningarna så att hello webbprogrammet kan vara värd för ett Java-program.</span><span class="sxs-lookup"><span data-stu-id="5825f-327">Before you publish hello application, you need toochange a few configuration settings so that hello web app can host a Java application.</span></span>

1. <span data-ttu-id="5825f-328">Hello klassiska portal, gå toohello webbappens **instrumentpanelen** och klickar på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="5825f-328">In hello classic portal, go toohello web app's **Dashboard** page and click **Configure**.</span></span> <span data-ttu-id="5825f-329">På hello **konfigurera** anger hello följande inställningar.</span><span class="sxs-lookup"><span data-stu-id="5825f-329">On hello **Configure** page, specify hello following settings.</span></span>
2. <span data-ttu-id="5825f-330">I **Java version** hello standardvärdet är **ut**; Välj hello Java version program-mål, till exempel 1.7.0_51.</span><span class="sxs-lookup"><span data-stu-id="5825f-330">In **Java version** hello default is **Off**; select hello Java version your application targets; for example 1.7.0_51.</span></span> <span data-ttu-id="5825f-331">När du gör det också se till att **webbehållaren** anges tooa version av Tomcat-Server.</span><span class="sxs-lookup"><span data-stu-id="5825f-331">After you do this, also make sure that **Web container** is set tooa version of Tomcat Server.</span></span>
3. <span data-ttu-id="5825f-332">I **standarddokument**, lägga till index.jsp och flytta upp toohello överkant hello-listan.</span><span class="sxs-lookup"><span data-stu-id="5825f-332">In **Default Documents**, add index.jsp and move it up toohello top of hello list.</span></span> <span data-ttu-id="5825f-333">(hello standardfilen för webbprogram är hostingstart.html.)</span><span class="sxs-lookup"><span data-stu-id="5825f-333">(hello default file for web apps is hostingstart.html.)</span></span>
4. <span data-ttu-id="5825f-334">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5825f-334">Click **Save**.</span></span>

#### <a name="publish-your-application-using-kudu"></a><span data-ttu-id="5825f-335">Publicera programmet med hjälp av Kudu</span><span class="sxs-lookup"><span data-stu-id="5825f-335">Publish your application using Kudu</span></span>
<span data-ttu-id="5825f-336">Enkelriktade toopublish hello program är toouse hello Kudu debug konsol som är inbyggda i Azure.</span><span class="sxs-lookup"><span data-stu-id="5825f-336">One way toopublish hello application is toouse hello Kudu debug console built into Azure.</span></span> <span data-ttu-id="5825f-337">Kudu kallas toobe stabil och konsekvent med App Service Web Apps och Tomcat-Server.</span><span class="sxs-lookup"><span data-stu-id="5825f-337">Kudu is known toobe stable and consistent with App Service Web Apps and Tomcat Server.</span></span> <span data-ttu-id="5825f-338">Du har åtkomst till hello konsolen för hello webbprogrammet genom att bläddra tooa URL för hello följande format:</span><span class="sxs-lookup"><span data-stu-id="5825f-338">You access hello console for hello web app by browsing tooa URL of hello following form:</span></span>

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. <span data-ttu-id="5825f-339">För den här proceduren finns hello Kudu-konsolen på hello följande URL-Adressen. Bläddra toothis plats:</span><span class="sxs-lookup"><span data-stu-id="5825f-339">For this procedure, hello Kudu console is located at hello following URL; browse toothis location:</span></span>
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. <span data-ttu-id="5825f-340">Välj hello översta menyn **Felsökningskonsolen > CMD**.</span><span class="sxs-lookup"><span data-stu-id="5825f-340">From hello top menu, select **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="5825f-341">På kommandoraden för hello-konsolen, gå för`/site/wwwroot` (eller klicka på `site`, sedan `wwwroot` hello directory vyn i hello överst på sidan för hello):</span><span class="sxs-lookup"><span data-stu-id="5825f-341">In hello console command line, navigate too`/site/wwwroot` (or click `site`, then `wwwroot` in hello directory view at hello top of hello page):</span></span>
   
    `cd /site/wwwroot`
4. <span data-ttu-id="5825f-342">När du har angett **Java version**, Tomcat-server ska skapa en katalog för webbappar.</span><span class="sxs-lookup"><span data-stu-id="5825f-342">After you specify **Java version**, Tomcat server should create a webapps directory.</span></span> <span data-ttu-id="5825f-343">Navigera toohello webbappar directory på kommandoraden för hello-konsolen:</span><span class="sxs-lookup"><span data-stu-id="5825f-343">In hello console command line, navigate toohello webapps directory:</span></span>
   
    `mkdir webapps`
   
    `cd webapps`
5. <span data-ttu-id="5825f-344">Dra JSPHello.war från `<project-path>/JSPHello/src/` och släpp hello Kudu directory vyn under `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="5825f-344">Drag JSPHello.war from `<project-path>/JSPHello/src/` and drop it into hello Kudu directory view under `/site/wwwroot/webapps`.</span></span> <span data-ttu-id="5825f-345">Dra inte den toohello ”dra hit tooupload och zip” område eftersom Tomcat kommer packa upp den.</span><span class="sxs-lookup"><span data-stu-id="5825f-345">Do not drag it toohello "Drag here tooupload and zip" area, because Tomcat will unzip it.</span></span>
   
   ![][8]

<span data-ttu-id="5825f-346">Första JSPHello.war visas i hello directory området ensamt:</span><span class="sxs-lookup"><span data-stu-id="5825f-346">At first JSPHello.war appears in hello directory area by itself:</span></span>

  ![][9]

<span data-ttu-id="5825f-347">Under en kort tid (förmodligen mindre än 5 minuter) kommer Tomcat Server packa hello WAR-filen till en uppackade JSPHello katalog.</span><span class="sxs-lookup"><span data-stu-id="5825f-347">In a short time (probably less than 5 minutes) Tomcat Server will unzip hello WAR file into an unpacked JSPHello directory.</span></span> <span data-ttu-id="5825f-348">Klicka på hello rot directory toosee om index.jsp har uppackade och kopieras dit.</span><span class="sxs-lookup"><span data-stu-id="5825f-348">Click hello ROOT directory toosee whether index.jsp has been unzipped and copied there.</span></span> <span data-ttu-id="5825f-349">I så fall, gå tillbaka toohello webbappar directory toosee om hello packat JSPHello katalogen har skapats.</span><span class="sxs-lookup"><span data-stu-id="5825f-349">If so, navigate back toohello webapps directory toosee whether hello unpacked JSPHello directory has been created.</span></span> <span data-ttu-id="5825f-350">Om du inte kan se dessa objekt vänta och upprepa.</span><span class="sxs-lookup"><span data-stu-id="5825f-350">If you do not see these items, wait and repeat.</span></span>

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a><span data-ttu-id="5825f-351">Publicera programmet med hjälp av FileZilla (valfritt)</span><span class="sxs-lookup"><span data-stu-id="5825f-351">Publish your application using FileZilla (optional)</span></span>
<span data-ttu-id="5825f-352">Ett annat verktyg som du kan använda toopublish hello program är FileZilla, en populär från tredje part FTP-klient med en lämplig, grafiskt användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="5825f-352">Another tool you can use toopublish hello application is FileZilla, a popular third-party FTP client with a convenient, graphical UI.</span></span> <span data-ttu-id="5825f-353">Du kan hämta och installera FileZilla från [http://filezilla-project.org/](http://filezilla-project.org/) om du inte redan har det.</span><span class="sxs-lookup"><span data-stu-id="5825f-353">You can download and install FileZilla from [http://filezilla-project.org/](http://filezilla-project.org/) if you do not already have it.</span></span> <span data-ttu-id="5825f-354">Mer information om hur du använder hello klienten finns hello [FileZilla dokumentationen](https://wiki.filezilla-project.org/Documentation) och det här blogginlägget på [FTP-klienter - del 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span><span class="sxs-lookup"><span data-stu-id="5825f-354">For more information on using hello client, see hello [FileZilla documentation](https://wiki.filezilla-project.org/Documentation) and this blog entry on [FTP Clients - Part 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span></span>

1. <span data-ttu-id="5825f-355">Klicka på FileZilla, **fil > Platshanteraren**.</span><span class="sxs-lookup"><span data-stu-id="5825f-355">In FileZilla, click **File > Site Manager**.</span></span>
2. <span data-ttu-id="5825f-356">I hello **Platshanteraren** dialogrutan klickar du på **ny plats**.</span><span class="sxs-lookup"><span data-stu-id="5825f-356">In hello **Site Manager** dialog, click **New Site**.</span></span> <span data-ttu-id="5825f-357">En ny tom FTP-plats visas i **Välj posten** uppmaning tooprovide ett namn.</span><span class="sxs-lookup"><span data-stu-id="5825f-357">A new blank FTP site will appear in **Select Entry** prompting you tooprovide a name.</span></span> <span data-ttu-id="5825f-358">Den här proceduren namnet `AzureWebDemo-FTP`.</span><span class="sxs-lookup"><span data-stu-id="5825f-358">For this procedure, name it `AzureWebDemo-FTP`.</span></span>
   
    <span data-ttu-id="5825f-359">På hello **allmänna** anger hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="5825f-359">On hello **General** tab, specify hello following settings:</span></span>
   
   * <span data-ttu-id="5825f-360">**Värden:** RETUR hello **värdnamn för FTP-** som du kopierade från hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="5825f-360">**Host:** Enter hello **FTP Host Name** that you copied from hello dashboard.</span></span>
   * <span data-ttu-id="5825f-361">**Port:** (lämna det tomt eftersom det här är en passiv överföring och hello servern avgör hello port toouse.)</span><span class="sxs-lookup"><span data-stu-id="5825f-361">**Port:** (Leave this blank, as this is a passive transfer and hello server will determine hello port toouse.)</span></span>
   * <span data-ttu-id="5825f-362">**Protokoll:** FTP File Transfer Protocol</span><span class="sxs-lookup"><span data-stu-id="5825f-362">**Protocol:** FTP File Transfer Protocol</span></span>
   * <span data-ttu-id="5825f-363">**Kryptering:** använder vanlig FTP</span><span class="sxs-lookup"><span data-stu-id="5825f-363">**Encryption:** Use plain FTP</span></span>
   * <span data-ttu-id="5825f-364">**Inloggningstyp:** Normal</span><span class="sxs-lookup"><span data-stu-id="5825f-364">**Logon Type:** Normal</span></span>
   * <span data-ttu-id="5825f-365">**Användare:** RETUR hello distribution / FTP-användare som du kopierade från hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="5825f-365">**User:** Enter hello Deployment / FTP user that you copied from hello dashboard.</span></span> <span data-ttu-id="5825f-366">Detta är hello fullständig FTP-användarnamn, som har hello formuläret *webappname\username*.</span><span class="sxs-lookup"><span data-stu-id="5825f-366">This is hello full FTP username, which has hello form *webappname\username*.</span></span>
   * <span data-ttu-id="5825f-367">**Lösenord:** ange hello lösenord som du angav när du anger autentiseringsuppgifter för distribution av hello.</span><span class="sxs-lookup"><span data-stu-id="5825f-367">**Password:** Enter hello password that you specified when you set hello deployment credentials.</span></span>
     
     <span data-ttu-id="5825f-368">På hello **Överföringsinställningar** väljer **passiva**.</span><span class="sxs-lookup"><span data-stu-id="5825f-368">On hello **Transfer Settings** tab, select **Passive**.</span></span>
3. <span data-ttu-id="5825f-369">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="5825f-369">Click **Connect**.</span></span> <span data-ttu-id="5825f-370">Om lyckas Filezillas konsolen visas en `Status: Connected` meddelande och utfärda en `LIST` kommandot toolist hello kataloginnehållet.</span><span class="sxs-lookup"><span data-stu-id="5825f-370">If successful, FileZilla's console will display a `Status: Connected` message and issue a `LIST` command toolist hello directory contents.</span></span>
4. <span data-ttu-id="5825f-371">I hello **lokala** plats Kontrollpanelen, Välj hello källkatalogen i vilken hello JSPHello.war fil finns; hello åtkomstsökvägen liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="5825f-371">In hello **Local** site panel, select hello source directory in which hello JSPHello.war file resides; hello path will be similar toohello following:</span></span>
   
    `<project-path>/JSPHello/src/`
5. <span data-ttu-id="5825f-372">I hello **Remote** plats Kontrollpanelen, Välj hello målmappen.</span><span class="sxs-lookup"><span data-stu-id="5825f-372">In hello **Remote** site panel, select hello destination folder.</span></span> <span data-ttu-id="5825f-373">Du ska distribuera hello WAR-filen toohello `webapps` katalog under roten för hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="5825f-373">You will deploy hello WAR file toohello `webapps` directory under hello web app's root.</span></span> <span data-ttu-id="5825f-374">Navigera för`/site/wwwroot`, högerklicka på `wwwroot`, och välj **skapa directory**.</span><span class="sxs-lookup"><span data-stu-id="5825f-374">Navigate too`/site/wwwroot`, right-click on `wwwroot`, and select **Create directory**.</span></span> <span data-ttu-id="5825f-375">Katalog över hello `webapps` och ange den katalogen.</span><span class="sxs-lookup"><span data-stu-id="5825f-375">Name hello directory `webapps` and enter that directory.</span></span>
6. <span data-ttu-id="5825f-376">Överför JSPHello.war för`/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="5825f-376">Transfer JSPHello.war too`/site/wwwroot/webapps`.</span></span> <span data-ttu-id="5825f-377">Välj JSPHello.war i hello **lokala** filen listan högerklickar du på den och väljer **överför**.</span><span class="sxs-lookup"><span data-stu-id="5825f-377">Select JSPHello.war in hello **Local** file list, right-click on it and select **Upload**.</span></span> <span data-ttu-id="5825f-378">Du bör se den visas i `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="5825f-378">You should see it appear in `/site/wwwroot/webapps`.</span></span>
7. <span data-ttu-id="5825f-379">När du har kopierat JSPHello.war toohello webbappar directory Tomcat Server kommer automatiskt att packa upp (packa) hello filer i hello WAR-filen.</span><span class="sxs-lookup"><span data-stu-id="5825f-379">After you copy JSPHello.war toohello webapps directory, Tomcat Server will automatically unpack (unzip) hello files in hello WAR file.</span></span> <span data-ttu-id="5825f-380">Även om Tomcat servern börjar uppackning nästan omedelbart, kan det ta lång tid (möjligen timmar) för hello filer tooappear hello FTP-klienten.</span><span class="sxs-lookup"><span data-stu-id="5825f-380">Although Tomcat Server begins unpacking almost immediately, it might take a long time (possibly hours) for hello files tooappear in hello FTP client.</span></span>

#### <a name="run-hello-hello-world-application-on-hello-web-app"></a><span data-ttu-id="5825f-381">Kör hello World Hello på hello Web App</span><span class="sxs-lookup"><span data-stu-id="5825f-381">Run hello Hello World application on hello Web App</span></span>
1. <span data-ttu-id="5825f-382">När du har överfört hello WAR-filen och verifiera att Tomcat-server har skapat en uppackade `JSPHello` directory, bläddra för`http://webdemowebapp.azurewebsites.net/JSPHello` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="5825f-382">After you have uploaded hello WAR file and verified that Tomcat server has created an unpacked `JSPHello` directory, browse too`http://webdemowebapp.azurewebsites.net/JSPHello` toorun hello application.</span></span>
   
   > <span data-ttu-id="5825f-383">**Obs:** om du klickar på **Bläddra** från hello klassiska portalen kan du få hello standard webbsidan säger ”Java-baserad webbprogrammet har skapats”.</span><span class="sxs-lookup"><span data-stu-id="5825f-383">**Note:** If you click **Browse** from hello classic portal, you might get hello default webpage, saying "This Java based web application has been successfully created."</span></span> <span data-ttu-id="5825f-384">Du kanske toorefresh hello webbsidan i ordning tooview hello programmet utdata i stället för hello standard webbsidan.</span><span class="sxs-lookup"><span data-stu-id="5825f-384">You might have toorefresh hello webpage in order tooview hello application output instead of hello default webpage.</span></span>
   > 
   > 
2. <span data-ttu-id="5825f-385">När programmet hello körs, bör du se en webbsida med hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="5825f-385">When hello application runs, you should see a web page with hello following output:</span></span>
   
    `Hello World, hello time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a><span data-ttu-id="5825f-386">Rensa Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="5825f-386">Clean up Azure resources</span></span>
<span data-ttu-id="5825f-387">Den här proceduren skapar en webbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="5825f-387">This procedure creates an App Service web app.</span></span> <span data-ttu-id="5825f-388">Du kommer att debiteras för hello resurs så länge den finns.</span><span class="sxs-lookup"><span data-stu-id="5825f-388">You will be billed for hello resource as long as it exists.</span></span> <span data-ttu-id="5825f-389">Om du planerar toocontinue med hello webbapp för testning och utveckling, bör du stoppa eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="5825f-389">Unless you plan toocontinue using hello web app for testing or development, you should consider stopping or deleting it.</span></span> <span data-ttu-id="5825f-390">Ett webbprogram som har stoppats medför fortfarande en liten kostnad, men du kan starta om den när som helst.</span><span class="sxs-lookup"><span data-stu-id="5825f-390">A web app that has been stopped will still incur a small charge, but you can restart it at any time.</span></span> <span data-ttu-id="5825f-391">Om du tar bort ett webbprogram raderas alla data som du har laddat upp tooit.</span><span class="sxs-lookup"><span data-stu-id="5825f-391">Deleting a web app erases all data you have uploaded tooit.</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

[1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
[2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
[3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
[4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
[5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
[6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
[7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
[8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
[9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
[10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png


[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure classic portal]: https://manage.windowsazure.com
[What is an Azure AD directory]: http://technet.microsoft.com/library/jj573650.aspx
[Create and Upload a Management Certificate for Azure]: ../cloud-services/cloud-services-certs-create.md
[Key and Certificate Management Tool (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com
