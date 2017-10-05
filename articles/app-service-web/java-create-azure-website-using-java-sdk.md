---
title: "Skapa en Webbapp i Azure App Service med hjälp av Azure SDK för Java"
description: "Lär dig hur du skapar en Webbapp i Azure App Service via programmering med Azure SDK för Java."
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
ms.openlocfilehash: 08bb53de8cf437a5a2b1c3b38bce9f81b8349493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a><span data-ttu-id="7cf40-103">Skapa en Webbapp i Azure App Service med hjälp av Azure SDK för Java</span><span class="sxs-lookup"><span data-stu-id="7cf40-103">Create a Web App in Azure App Service using the Azure SDK for Java</span></span>
<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a><span data-ttu-id="7cf40-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7cf40-104">Overview</span></span>
<span data-ttu-id="7cf40-105">Den här genomgången visar hur du skapar ett Azure SDK för Java-program som skapar en Webbapp i [Azure App Service][Azure App Service], distribuera program till den.</span><span class="sxs-lookup"><span data-stu-id="7cf40-105">This walkthrough shows you how to create an Azure SDK for Java application that creates a Web App in [Azure App Service][Azure App Service], then deploy an application to it.</span></span> <span data-ttu-id="7cf40-106">Det består av två delar:</span><span class="sxs-lookup"><span data-stu-id="7cf40-106">It consists of two parts:</span></span>

* <span data-ttu-id="7cf40-107">Del 1 visar hur du skapar en Java-program som skapar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="7cf40-107">Part 1 demonstrates how to build a Java application that creates a web app.</span></span>
* <span data-ttu-id="7cf40-108">Del 2 visar hur du skapar en enkel JSP ”Hello World” program och sedan använda en FTP-klient för att distribuera kod till App Service.</span><span class="sxs-lookup"><span data-stu-id="7cf40-108">Part 2 demonstrates how to create a simple JSP "Hello World" application, then use an FTP client to deploy code to App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7cf40-109">Krav</span><span class="sxs-lookup"><span data-stu-id="7cf40-109">Prerequisites</span></span>
### <a name="software-installations"></a><span data-ttu-id="7cf40-110">Programinstallationer</span><span class="sxs-lookup"><span data-stu-id="7cf40-110">Software Installations</span></span>
<span data-ttu-id="7cf40-111">Programkoden AzureWebDemo i den här artikeln skrevs med Azure Java SDK 0.7.0, som du kan installera med hjälp av den [installationsprogram för webbplattform] [ Web Platform Installer] (WebPI).</span><span class="sxs-lookup"><span data-stu-id="7cf40-111">The AzureWebDemo application code in this article was written using Azure Java SDK 0.7.0, which you can install using the [Web Platform Installer][Web Platform Installer] (WebPI).</span></span> <span data-ttu-id="7cf40-112">Dessutom se till att använda den senaste versionen av den [Azure Toolkit för Eclipse][Azure Toolkit for Eclipse].</span><span class="sxs-lookup"><span data-stu-id="7cf40-112">In addition, make sure to use the latest version of the [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse].</span></span> <span data-ttu-id="7cf40-113">När du har installerat SDK uppdatera beroenden i Eclipse-projektet genom att köra **uppdatera Index** i **Maven databaser**, sedan lägger till den senaste versionen av varje paket i den **beroenden** fönster.</span><span class="sxs-lookup"><span data-stu-id="7cf40-113">After you install the SDK, update the dependencies in your Eclipse project by running **Update Index** in **Maven Repositories**, then re-add the latest version of each package in the **Dependencies** window.</span></span> <span data-ttu-id="7cf40-114">Du kan kontrollera vilken version av installerad programvara i Eclipse genom att klicka på **Hjälp > installationsinformationen**; du bör ha minst följande versioner:</span><span class="sxs-lookup"><span data-stu-id="7cf40-114">You can verify the version of your installed software in Eclipse by clicking **Help > Installation Details**; you should have at least the following versions:</span></span>

* <span data-ttu-id="7cf40-115">Paket för Microsoft Azure-biblioteken för Java 0.7.0.20150309</span><span class="sxs-lookup"><span data-stu-id="7cf40-115">Package for Microsoft Azure Libraries for Java 0.7.0.20150309</span></span>
* <span data-ttu-id="7cf40-116">Eclipse IDE för Java EE-utvecklare 4.4.2.20150219</span><span class="sxs-lookup"><span data-stu-id="7cf40-116">Eclipse IDE for Java EE Developers 4.4.2.20150219</span></span>

### <a name="create-and-configure-cloud-resources-in-azure"></a><span data-ttu-id="7cf40-117">Skapa och konfigurera Molnresurser i Azure</span><span class="sxs-lookup"><span data-stu-id="7cf40-117">Create and Configure Cloud Resources in Azure</span></span>
<span data-ttu-id="7cf40-118">Innan du påbörjar den här proceduren måste du har en aktiv Azure-prenumeration och skapa en standard-Active Directory (AD) på Azure.</span><span class="sxs-lookup"><span data-stu-id="7cf40-118">Before you begin this procedure, you need to have an active Azure subscription and set up a default Active Directory (AD) on Azure.</span></span>

### <a name="create-an-active-directory-ad-in-azure"></a><span data-ttu-id="7cf40-119">Skapa en Active Directory (AD) i Azure</span><span class="sxs-lookup"><span data-stu-id="7cf40-119">Create an Active Directory (AD) in Azure</span></span>
<span data-ttu-id="7cf40-120">Om du inte redan har en Active Directory (AD) på din Azure-prenumeration logga in på den [klassiska Azure-portalen] [ Azure classic portal] med ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="7cf40-120">If you do not already have an Active Directory (AD) on your Azure subscription, log into the [Azure classic portal][Azure classic portal] with your Microsoft account.</span></span> <span data-ttu-id="7cf40-121">Om du har flera prenumerationer, klickar du på **prenumerationer** och välj standardkatalogen för den prenumeration som du vill använda för det här projektet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-121">If you have multiple subscriptions, click **Subscriptions** and select the default directory for the subscription you want to use for this project.</span></span> <span data-ttu-id="7cf40-122">Klicka på **tillämpa** växla till den prenumerationsvyn.</span><span class="sxs-lookup"><span data-stu-id="7cf40-122">Then click **Apply** to switch to that subscription view.</span></span>

1. <span data-ttu-id="7cf40-123">Välj **Active Directory** på menyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="7cf40-123">Select **Active Directory** from the menu at left.</span></span> <span data-ttu-id="7cf40-124">**Klicka på Ny > Directory > Skapa anpassade**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-124">**Click New > Directory > Custom Create**.</span></span>
2. <span data-ttu-id="7cf40-125">I **Lägg till katalog**väljer **Skapa ny katalog**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-125">In **Add Directory**, select **Create New Directory**.</span></span>
3. <span data-ttu-id="7cf40-126">I **namn**, ange namnet på en katalog.</span><span class="sxs-lookup"><span data-stu-id="7cf40-126">In **Name**, enter a directory name.</span></span>
4. <span data-ttu-id="7cf40-127">I **domän**, ange ett domännamn.</span><span class="sxs-lookup"><span data-stu-id="7cf40-127">In **Domain**, enter a domain name.</span></span> <span data-ttu-id="7cf40-128">Detta är ett grundläggande domännamn som ingår som standard med din katalog. den har formatet `<domain_name>.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-128">This is a basic domain name that is included by default with your directory; it has the form `<domain_name>.onmicrosoft.com`.</span></span> <span data-ttu-id="7cf40-129">Du kan kalla det baserat på katalognamnet eller ett annat domännamn som du äger.</span><span class="sxs-lookup"><span data-stu-id="7cf40-129">You can name it based on the directory name or another domain name that you own.</span></span> <span data-ttu-id="7cf40-130">Du kan senare lägga till ett annat domännamn som din organisation redan använder.</span><span class="sxs-lookup"><span data-stu-id="7cf40-130">Later, you can add another domain name that your organization already uses.</span></span>
5. <span data-ttu-id="7cf40-131">I **land eller region**, Välj ditt språk.</span><span class="sxs-lookup"><span data-stu-id="7cf40-131">In **Country or region**, select your locale.</span></span>

<span data-ttu-id="7cf40-132">Mer information om AD finns [vad är Azure AD-katalog][What is an Azure AD directory]?</span><span class="sxs-lookup"><span data-stu-id="7cf40-132">For more information on AD, see [What is an Azure AD directory][What is an Azure AD directory]?</span></span>

### <a name="create-a-management-certificate-for-azure"></a><span data-ttu-id="7cf40-133">Skapa ett Hanteringscertifikat för Azure</span><span class="sxs-lookup"><span data-stu-id="7cf40-133">Create a Management Certificate for Azure</span></span>
<span data-ttu-id="7cf40-134">Azure SDK för Java använder certifikat för att autentisera med Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="7cf40-134">The Azure SDK for Java uses management certificates to authenticate with Azure subscriptions.</span></span> <span data-ttu-id="7cf40-135">Dessa är X.509 v3-certifikat som du använder för att autentisera ett klientprogram som använder Service Management API för att agera för att hantera prenumerationsresurser prenumerationsägaren.</span><span class="sxs-lookup"><span data-stu-id="7cf40-135">These are X.509 v3 certificates you use to authenticate a client application that uses the Service Management API to act on behalf of the subscription owner to manage subscription resources.</span></span>

<span data-ttu-id="7cf40-136">Koden i den här proceduren använder ett självsignerat certifikat för att autentisera med Azure.</span><span class="sxs-lookup"><span data-stu-id="7cf40-136">The code in this procedure uses a self-signed certificate to authenticate with Azure.</span></span> <span data-ttu-id="7cf40-137">För den här proceduren måste du skapa ett certifikat och överföra den till den [klassiska Azure-portalen] [ Azure classic portal] i förväg.</span><span class="sxs-lookup"><span data-stu-id="7cf40-137">For this procedure, you need to create a certificate and upload it to the [Azure classic portal][Azure classic portal] beforehand.</span></span> <span data-ttu-id="7cf40-138">Detta omfattar följande steg:</span><span class="sxs-lookup"><span data-stu-id="7cf40-138">This involves the following steps:</span></span>

* <span data-ttu-id="7cf40-139">Generera en PFX-fil som representerar klientcertifikatet och spara den lokalt.</span><span class="sxs-lookup"><span data-stu-id="7cf40-139">Generate a PFX file representing your client certificate and save it locally.</span></span>
* <span data-ttu-id="7cf40-140">Skapa ett hanteringscertifikat (CER-fil) från PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-140">Generate a management certificate (CER file) from the PFX file.</span></span>
* <span data-ttu-id="7cf40-141">Överföra CER-filen till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7cf40-141">Upload the CER file to your Azure subscription.</span></span>
* <span data-ttu-id="7cf40-142">Konvertera PFX-filen till JKS, eftersom Java används formatet för att autentisera med certifikat.</span><span class="sxs-lookup"><span data-stu-id="7cf40-142">Convert the PFX file into JKS, because Java uses that format to authenticate using certificates.</span></span>
* <span data-ttu-id="7cf40-143">Skriva programmets Autentiseringskod som refererar till den lokala JKS-filen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-143">Write the application's authentication code, which refers to the local JKS file.</span></span>

<span data-ttu-id="7cf40-144">När du slutför den här proceduren CER certifikatet ska placeras i din Azure-prenumeration och JKS certifikat kommer att finnas på en lokal hårddisk.</span><span class="sxs-lookup"><span data-stu-id="7cf40-144">When you complete this procedure, the CER certificate will reside in your Azure subscription and the JKS certificate will reside on your local drive.</span></span> <span data-ttu-id="7cf40-145">Mer information om certifikat finns [skapa och ladda upp ett Hanteringscertifikat för Azure][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="7cf40-145">For more information on management certificates, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="create-a-certificate"></a><span data-ttu-id="7cf40-146">Skapa ett certifikat</span><span class="sxs-lookup"><span data-stu-id="7cf40-146">Create a certificate</span></span>
<span data-ttu-id="7cf40-147">Öppna en kommandokonsolen på operativsystemet och kör följande kommandon om du vill skapa ett eget självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="7cf40-147">To create your own self-signed certificate, open a command console on your operating system and run the following commands.</span></span>

> <span data-ttu-id="7cf40-148">**Obs:** datorn där du kör det här kommandot måste ha JDK installerad.</span><span class="sxs-lookup"><span data-stu-id="7cf40-148">**Note:**  The computer on which you run this command must have the JDK installed.</span></span> <span data-ttu-id="7cf40-149">Sökvägen till keytool beror också på den plats där du installerar JDK.</span><span class="sxs-lookup"><span data-stu-id="7cf40-149">Also, the path to the keytool depends on the location in which you install the JDK.</span></span> <span data-ttu-id="7cf40-150">Mer information finns i [nyckeln och certifikatet Management Tool (keytool)] [ Key and Certificate Management Tool (keytool)] i Java online dokumenten.</span><span class="sxs-lookup"><span data-stu-id="7cf40-150">For more information, see [Key and Certificate Management Tool (keytool)][Key and Certificate Management Tool (keytool)] in the Java online docs.</span></span>
> 
> 

<span data-ttu-id="7cf40-151">Skapa .pfx-filen:</span><span class="sxs-lookup"><span data-stu-id="7cf40-151">To create the .pfx file:</span></span>

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

<span data-ttu-id="7cf40-152">Så här skapar du den .cer-fil:</span><span class="sxs-lookup"><span data-stu-id="7cf40-152">To create the .cer file:</span></span>

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

<span data-ttu-id="7cf40-153">Där:</span><span class="sxs-lookup"><span data-stu-id="7cf40-153">where:</span></span>

* <span data-ttu-id="7cf40-154">`<java-install-dir>`är sökvägen till den katalog där du installerade Java.</span><span class="sxs-lookup"><span data-stu-id="7cf40-154">`<java-install-dir>` is the path to the directory in which you installed Java.</span></span>
* <span data-ttu-id="7cf40-155">`<keystore-id>`identifierare för keystore-post (till exempel `AzureRemoteAccess`).</span><span class="sxs-lookup"><span data-stu-id="7cf40-155">`<keystore-id>` is the keystore entry identifier (for example, `AzureRemoteAccess`).</span></span>
* <span data-ttu-id="7cf40-156">`<cert-store-dir>`sökvägen till katalogen där du vill lagra certifikat (till exempel `C:/Certificates`).</span><span class="sxs-lookup"><span data-stu-id="7cf40-156">`<cert-store-dir>` is the path to the directory in which you want to store certificates (for example `C:/Certificates`).</span></span>
* <span data-ttu-id="7cf40-157">`<cert-file-name>`är namnet på certifikatfilen (till exempel `AzureWebDemoCert`).</span><span class="sxs-lookup"><span data-stu-id="7cf40-157">`<cert-file-name>` is the name of the certificate file (for example `AzureWebDemoCert`).</span></span>
* <span data-ttu-id="7cf40-158">`<password>`är det lösenord som du väljer att skydda certifikatet. Det måste vara minst 6 tecken.</span><span class="sxs-lookup"><span data-stu-id="7cf40-158">`<password>` is the password you choose to protect the certificate; it must be at least 6 characters long.</span></span> <span data-ttu-id="7cf40-159">Du kan ange inga lösenord, men detta inte rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="7cf40-159">You can enter no password, although this is not recommended.</span></span>
* <span data-ttu-id="7cf40-160">`<dname>`är det unika namnet X.500 som ska associeras med alias och används som utfärdare och ämne fält i ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="7cf40-160">`<dname>` is the X.500 Distinguished Name to be associated with alias, and is used as the issuer and subject fields in the self-signed certificate.</span></span>

<span data-ttu-id="7cf40-161">Mer information finns i [skapa och ladda upp ett Hanteringscertifikat för Azure][Create and Upload a Management Certificate for Azure].</span><span class="sxs-lookup"><span data-stu-id="7cf40-161">For more information, see [Create and Upload a Management Certificate for Azure][Create and Upload a Management Certificate for Azure].</span></span>

#### <a name="upload-the-certificate"></a><span data-ttu-id="7cf40-162">Ladda upp certifikatet</span><span class="sxs-lookup"><span data-stu-id="7cf40-162">Upload the certificate</span></span>
<span data-ttu-id="7cf40-163">Om du vill överföra ett självsignerat certifikat till Azure, gå till den **inställningar** sidan i den klassiska portalen och klicka sedan på den **Hanteringscertifikat** fliken. Klicka på **överför** längst ned på sidan och navigera till platsen för den CER-fil som du skapade.</span><span class="sxs-lookup"><span data-stu-id="7cf40-163">To upload a self-signed certificate to Azure, go to the **Settings** page in the classic portal, then click the **Management Certificates** tab. Click **Upload** at the bottom of the page and navigate to the location of the CER file you created.</span></span>

#### <a name="convert-the-pfx-file-into-jks"></a><span data-ttu-id="7cf40-164">Konvertera PFX-filen till JKS</span><span class="sxs-lookup"><span data-stu-id="7cf40-164">Convert the PFX file into JKS</span></span>
<span data-ttu-id="7cf40-165">I Windows kommandotolk (Kör som administratör), CD: n till den katalog som innehåller certifikat och kör följande kommando, där `<java-install-dir>` är den katalog där du installerade Java på datorn:</span><span class="sxs-lookup"><span data-stu-id="7cf40-165">In the Windows Command Prompt (running as admin), cd to the directory containing the certificates and run the following command, where `<java-install-dir>` is the directory in which you installed Java on your computer:</span></span>

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. <span data-ttu-id="7cf40-166">När du uppmanas ange mål keystore lösenord. Det här är lösenordet för filen JKS.</span><span class="sxs-lookup"><span data-stu-id="7cf40-166">When prompted, enter the destination keystore password; this will be the password for the JKS file.</span></span>
2. <span data-ttu-id="7cf40-167">När du uppmanas ange lösenord för datakällan keystore; Detta är det lösenord som du angav för PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-167">When prompted, enter the source keystore password; this is the password you specified for the PFX file.</span></span>

<span data-ttu-id="7cf40-168">De båda lösenorden behöver inte vara samma.</span><span class="sxs-lookup"><span data-stu-id="7cf40-168">The two passwords do not have to be the same.</span></span> <span data-ttu-id="7cf40-169">Du kan ange inga lösenord, men detta inte rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="7cf40-169">You can enter no password, although this is not recommended.</span></span>

## <a name="build-a-web-app-creation-application"></a><span data-ttu-id="7cf40-170">Skapa ett program för att skapa webbprogram</span><span class="sxs-lookup"><span data-stu-id="7cf40-170">Build a Web App creation application</span></span>
### <a name="create-the-eclipse-workspace-and-maven-project"></a><span data-ttu-id="7cf40-171">Skapa Eclipse arbetsytan och Maven-projekt</span><span class="sxs-lookup"><span data-stu-id="7cf40-171">Create the Eclipse Workspace and Maven Project</span></span>
<span data-ttu-id="7cf40-172">I det här avsnittet skapar du en arbetsyta och ett Maven-projekt för appen att skapa webbprogrammet med namnet AzureWebDemo.</span><span class="sxs-lookup"><span data-stu-id="7cf40-172">In this section you create a workspace and a Maven project for the web app creation application, named AzureWebDemo.</span></span>

1. <span data-ttu-id="7cf40-173">Skapa ett nytt Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="7cf40-173">Create a new Maven project.</span></span> <span data-ttu-id="7cf40-174">Klicka på **Arkiv > Nytt > Maven-projekt**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-174">Click **File > New > Maven Project**.</span></span> <span data-ttu-id="7cf40-175">I **nya Maven-projekt**väljer **skapa ett enkelt projekt** och **använda standardplatsen för arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-175">In **New Maven Project**, select **Create a simple project** and **Use default workspace location**.</span></span>
2. <span data-ttu-id="7cf40-176">På den andra sidan av **nya Maven-projekt**, anger du följande:</span><span class="sxs-lookup"><span data-stu-id="7cf40-176">On the second page of **New Maven Project**, specify the following:</span></span>
   
   * <span data-ttu-id="7cf40-177">Grupp-ID:`com.<username>.azure.webdemo`</span><span class="sxs-lookup"><span data-stu-id="7cf40-177">Group ID: `com.<username>.azure.webdemo`</span></span>
   * <span data-ttu-id="7cf40-178">Artefakt-ID: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="7cf40-178">Artifact ID: AzureWebDemo</span></span>
   * <span data-ttu-id="7cf40-179">Version: 0.0.1-SNAPSHOT</span><span class="sxs-lookup"><span data-stu-id="7cf40-179">Version: 0.0.1-SNAPSHOT</span></span>
   * <span data-ttu-id="7cf40-180">Paketera: jar</span><span class="sxs-lookup"><span data-stu-id="7cf40-180">Packaging: jar</span></span>
   * <span data-ttu-id="7cf40-181">Namn: AzureWebDemo</span><span class="sxs-lookup"><span data-stu-id="7cf40-181">Name: AzureWebDemo</span></span>
     
     <span data-ttu-id="7cf40-182">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-182">Click **Finish**.</span></span>
3. <span data-ttu-id="7cf40-183">Öppna filen för det nya projektet pom.xml i Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="7cf40-183">Open the new project's pom.xml file in Project Explorer.</span></span> <span data-ttu-id="7cf40-184">Välj den **beroenden** fliken. Eftersom det är ett nytt projekt visas inga paket ännu.</span><span class="sxs-lookup"><span data-stu-id="7cf40-184">Select the **Dependencies** tab. As this is a new project, no packages are listed yet.</span></span>
4. <span data-ttu-id="7cf40-185">Öppna Maven-databaser.</span><span class="sxs-lookup"><span data-stu-id="7cf40-185">Open the Maven Repositories view.</span></span> <span data-ttu-id="7cf40-186">**Klicka på fönstret > Visa > andra > Maven > Maven databaser** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-186">**Click Window > Show View > Other > Maven > Maven Repositories** and click **OK**.</span></span> <span data-ttu-id="7cf40-187">Den **Maven databaser** vyn visas längst ned i IDE.</span><span class="sxs-lookup"><span data-stu-id="7cf40-187">The **Maven Repositories** view will appear at the bottom of the IDE.</span></span>
5. <span data-ttu-id="7cf40-188">Öppna **globala databaser**, högerklicka på den **centrala** databasen och välj **Rebuild Index**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-188">Open **Global Repositories**, right-click the **central** repository, and select **Rebuild Index**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="7cf40-189">Det här steget kan ta flera minuter beroende på anslutningen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-189">This step can take several minutes depending on the speed of your connection.</span></span> <span data-ttu-id="7cf40-190">När indexet återskapas, bör du se Microsoft Azure-paket i den **centrala** Maven-databasen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-190">When the index rebuilds, you should see the Microsoft Azure packages in the **central** Maven repository.</span></span>
6. <span data-ttu-id="7cf40-191">I **beroenden**, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-191">In **Dependencies**, click **Add**.</span></span> <span data-ttu-id="7cf40-192">I **Ange grupp-ID....**  ange `azure-management`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-192">In **Enter Group ID...** enter `azure-management`.</span></span> <span data-ttu-id="7cf40-193">Välj paket för grundläggande hantering och hantering av App Service Web Apps:</span><span class="sxs-lookup"><span data-stu-id="7cf40-193">Select the packages for base management and App Service Web Apps management:</span></span>
   
        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites
   
   > <span data-ttu-id="7cf40-194">**Obs:** om du uppdaterar beroenden när en ny version-version du måste lägga till var och en av beroenden i den här listan igen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-194">**Note:** If you are updating the dependencies after a new version release, you need to re-add each of the dependencies in this list.</span></span>
   > <span data-ttu-id="7cf40-195">När du klickar på **Lägg till** och markera varje beroende, visas det med ett nytt versionsnummer i den **beroenden** lista.</span><span class="sxs-lookup"><span data-stu-id="7cf40-195">After you click **Add** and select each dependency, it appears with the new version number in the **Dependencies** list.</span></span>
   > 
   > 

<span data-ttu-id="7cf40-196">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-196">Click **OK**.</span></span> <span data-ttu-id="7cf40-197">Azure-paket visas sedan i den **beroenden** lista.</span><span class="sxs-lookup"><span data-stu-id="7cf40-197">The Azure packages then appear in the **Dependencies** list.</span></span>

### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a><span data-ttu-id="7cf40-198">Skriver Java-kod för att skapa ett webbprogram genom att anropa Azure SDK</span><span class="sxs-lookup"><span data-stu-id="7cf40-198">Writing Java Code to Create a Web App by Calling the Azure SDK</span></span>
<span data-ttu-id="7cf40-199">Skriv sedan den kod som anropar API: er i Azure SDK för Java skapa App Service webbapp.</span><span class="sxs-lookup"><span data-stu-id="7cf40-199">Next, write the code that calls APIs in the Azure SDK for Java to create the App Service web app.</span></span>

1. <span data-ttu-id="7cf40-200">Skapa en Java-klass för att innehålla huvudsakliga post punkt kod.</span><span class="sxs-lookup"><span data-stu-id="7cf40-200">Create a Java class to contain the main entry point code.</span></span> <span data-ttu-id="7cf40-201">Högerklicka på projektnoden i Projektutforskaren, och välj **New > klassen**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-201">In Project Explorer, right-click on the project node and select **New > Class**.</span></span>
2. <span data-ttu-id="7cf40-202">I **ny Java-klass**, klassen namnet `WebCreator` och kontrollera den **offentliga statiska void main** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="7cf40-202">In **New Java Class**, name the class `WebCreator` and check the **public static void main** checkbox.</span></span> <span data-ttu-id="7cf40-203">Valen ska visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7cf40-203">The selections should appear as follows:</span></span>
   
    ![][2]
3. <span data-ttu-id="7cf40-204">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-204">Click **Finish**.</span></span> <span data-ttu-id="7cf40-205">Filen WebCreator.java visas i Projektutforskaren.</span><span class="sxs-lookup"><span data-stu-id="7cf40-205">The WebCreator.java file appears in Project Explorer.</span></span>

### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a><span data-ttu-id="7cf40-206">Anropar Azure API för att skapa en Apptjänst-Webbapp</span><span class="sxs-lookup"><span data-stu-id="7cf40-206">Calling the Azure API to Create an App Service Web App</span></span>
#### <a name="add-necessary-imports"></a><span data-ttu-id="7cf40-207">Lägga till nödvändiga importer</span><span class="sxs-lookup"><span data-stu-id="7cf40-207">Add necessary imports</span></span>
<span data-ttu-id="7cf40-208">I WebCreator.java, lägger du till följande importer; importen ger åtkomst till klasserna i management-bibliotek för förbrukning av Azure API: er:</span><span class="sxs-lookup"><span data-stu-id="7cf40-208">In WebCreator.java, add the following imports; these imports provide access to classes in the management libraries for consuming Azure APIs:</span></span>

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


#### <a name="define-the-main-entry-point-class"></a><span data-ttu-id="7cf40-209">Definiera klassen huvudsakliga post point</span><span class="sxs-lookup"><span data-stu-id="7cf40-209">Define the main entry point class</span></span>
<span data-ttu-id="7cf40-210">Eftersom programmet AzureWebDemo syftar till att skapa en App Service Web App, huvudsakliga klassen namnet för det här programmet `WebAppCreator`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-210">Because the purpose of the AzureWebDemo application is to create an App Service Web App, name the main class for this application `WebAppCreator`.</span></span> <span data-ttu-id="7cf40-211">Den här klassen innehåller den huvudsakliga post punkt kod som anropar Azure Service Management API för att skapa webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-211">This class provides the main entry point code that calls the Azure Service Management API to create the web app.</span></span>

<span data-ttu-id="7cf40-212">Lägg till följande parameterdefinitioner för webbapp och webbutrymmet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-212">Add the following parameter definitions for the web app and webspace.</span></span> <span data-ttu-id="7cf40-213">Du måste ange din egen Azure-prenumeration ID eller certifikat.</span><span class="sxs-lookup"><span data-stu-id="7cf40-213">You will need to provide your own Azure subscription ID and certificate information.</span></span>

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

<span data-ttu-id="7cf40-214">Där:</span><span class="sxs-lookup"><span data-stu-id="7cf40-214">where:</span></span>

* <span data-ttu-id="7cf40-215">`<subscription-id>`är Azure prenumerations-ID som du vill skapa resursen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-215">`<subscription-id>` is the Azure subscription ID in which you want to create the resource.</span></span>
* <span data-ttu-id="7cf40-216">`<certificate-store-path>`är sökvägen och filnamnet till JKS-filen i katalogen lagra lokala certifikatarkivet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-216">`<certificate-store-path>` is the path and filename to the JKS file in your local certificate store directory.</span></span> <span data-ttu-id="7cf40-217">Till exempel `C:/Certificates/CertificateName.jks` för Linux och `C:\Certificates\CertificateName.jks` för Windows.</span><span class="sxs-lookup"><span data-stu-id="7cf40-217">For example, `C:/Certificates/CertificateName.jks` for Linux and `C:\Certificates\CertificateName.jks` for Windows.</span></span>
* <span data-ttu-id="7cf40-218">`<certificate-password>`är det lösenord som du angav när du skapade ditt JKS certifikat.</span><span class="sxs-lookup"><span data-stu-id="7cf40-218">`<certificate-password>` is the password you specified when you created your JKS certificate.</span></span>
* <span data-ttu-id="7cf40-219">`webAppName`kan vara vilket namn som du väljer; den här proceduren används namnet `WebDemoWebApp`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-219">`webAppName` can be any name you choose; this procedure uses the name `WebDemoWebApp`.</span></span> <span data-ttu-id="7cf40-220">Det fullständiga domännamnet är den `webAppName` med den `domainName` läggs, så i detta fall hela domännamnet är `webdemowebapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-220">The full domain name is the `webAppName` with the `domainName` appended, so in this case the full domain is `webdemowebapp.azurewebsites.net`.</span></span>
* <span data-ttu-id="7cf40-221">`domainName`ska anges som ovan.</span><span class="sxs-lookup"><span data-stu-id="7cf40-221">`domainName` should be specified as shown above.</span></span>
* <span data-ttu-id="7cf40-222">`webSpaceName`måste vara ett av de värden som definieras i den [WebSpaceNames] [ WebSpaceNames] klass.</span><span class="sxs-lookup"><span data-stu-id="7cf40-222">`webSpaceName` should be one of the values defined in the [WebSpaceNames][WebSpaceNames] class.</span></span>
* <span data-ttu-id="7cf40-223">`appServicePlanName`ska anges som ovan.</span><span class="sxs-lookup"><span data-stu-id="7cf40-223">`appServicePlanName` should be specified as shown above.</span></span>

> <span data-ttu-id="7cf40-224">**Obs:** varje gång du kör det här programmet måste du ändra värdet för `webAppName` och `appServicePlanName` (eller ta bort webbprogrammet på Azure Portal) innan du kör programmet igen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-224">**Note:** Each time you run this application, you need to change the value of `webAppName` and `appServicePlanName` (or delete the web app on the Azure Portal) before running the application again.</span></span> <span data-ttu-id="7cf40-225">Annars misslyckas körningen eftersom samma resursen finns redan på Azure.</span><span class="sxs-lookup"><span data-stu-id="7cf40-225">Otherwise, execution will fail because the same resource already exists on Azure.</span></span>
> 
> 

#### <a name="define-the-web-creation-method"></a><span data-ttu-id="7cf40-226">Definiera metod för skapande av webbprogram</span><span class="sxs-lookup"><span data-stu-id="7cf40-226">Define the web creation method</span></span>
<span data-ttu-id="7cf40-227">Sedan definiera en metod för att skapa webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-227">Next, define a method to create the web app.</span></span> <span data-ttu-id="7cf40-228">Den här metoden `createWebApp`, anger du parametrarna för webbappen och webbutrymmet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-228">This method, `createWebApp`, specifies the parameters of the web app and the webspace.</span></span> <span data-ttu-id="7cf40-229">Dessutom skapar och konfigurerar Hanteringsklient App Service Web Apps som definieras av den [WebSiteManagementClient] [ WebSiteManagementClient] objekt.</span><span class="sxs-lookup"><span data-stu-id="7cf40-229">It also creates and configures the App Service Web Apps management client, which is defined by the [WebSiteManagementClient][WebSiteManagementClient] object.</span></span> <span data-ttu-id="7cf40-230">Management-klienten är nyckeln till att skapa webbprogram.</span><span class="sxs-lookup"><span data-stu-id="7cf40-230">The management client is key to creating Web Apps.</span></span> <span data-ttu-id="7cf40-231">Det ger RESTful-webbtjänster som gör att program för att hantera webbappar (utför åtgärder som skapar, uppdatera och ta bort) genom att anropa service management API.</span><span class="sxs-lookup"><span data-stu-id="7cf40-231">It provides RESTful web services that allow applications to manage web apps (performing operations such as create, update, and delete) by calling the service management API.</span></span>

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
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
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

<span data-ttu-id="7cf40-232">Koden kommer utgående HTTP-status i svaret som anger lyckad eller misslyckad, och om det lyckas, kommer utdata namnet på webbappen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-232">The code will output the HTTP status of the response indicating success or failure, and if successful, will output the name of the created web app.</span></span>

#### <a name="define-the-main-method"></a><span data-ttu-id="7cf40-233">Definiera main()-metoden</span><span class="sxs-lookup"><span data-stu-id="7cf40-233">Define the main() method</span></span>
<span data-ttu-id="7cf40-234">Ange den main() metod kod som anropar createWebApp() för att skapa webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-234">Provide the main() method code that calls createWebApp() to create the web app.</span></span>

<span data-ttu-id="7cf40-235">Slutligen anropa `createWebApp` från `main`:</span><span class="sxs-lookup"><span data-stu-id="7cf40-235">Finally, call `createWebApp` from `main`:</span></span>

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a><span data-ttu-id="7cf40-236">Kör programmet och kontrollera att skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="7cf40-236">Run the application and verify web app creation</span></span>
<span data-ttu-id="7cf40-237">Kontrollera att programmet körs, klicka på **kör > kör**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-237">To verify that your application runs, click **Run > Run**.</span></span> <span data-ttu-id="7cf40-238">När programmet har slutförts körs, bör du se följande utdata i Eclipse-konsolen:</span><span class="sxs-lookup"><span data-stu-id="7cf40-238">When the application completes running, you should see the following output in the Eclipse console:</span></span>

    ----------
    Web app created - HTTP response 200

    ----------

    Name of web app created: WebDemoWebApp

    ----------

<span data-ttu-id="7cf40-239">Logga in på den klassiska Azure-portalen och på **Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-239">Log into the Azure classic portal and click **Web Apps**.</span></span> <span data-ttu-id="7cf40-240">Den nya webbappen ska visas i listan över Web Apps inom några minuter.</span><span class="sxs-lookup"><span data-stu-id="7cf40-240">The new web app should appear in the Web Apps list within a few minutes.</span></span>

## <a name="deploying-an-application-to-the-web-app"></a><span data-ttu-id="7cf40-241">Distribuera ett program till Webbappen</span><span class="sxs-lookup"><span data-stu-id="7cf40-241">Deploying an Application to the Web App</span></span>
<span data-ttu-id="7cf40-242">När du har kört AzureWebDemo och skapa den nya webbappen, logga in på den klassiska portalen på **Web Apps**, och välj **WebDemoWebApp** i den **Web Apps** lista.</span><span class="sxs-lookup"><span data-stu-id="7cf40-242">After you have run AzureWebDemo and created the new web app, log into the classic portal, click **Web Apps**, and select **WebDemoWebApp** in the **Web Apps** list.</span></span> <span data-ttu-id="7cf40-243">På webbappens instrumentpanelen klickar du på **Bläddra** (eller klicka på Webbadressen `webdemowebapp.azurewebsites.net`) att navigera till den.</span><span class="sxs-lookup"><span data-stu-id="7cf40-243">In the web app's dashboard page, click **Browse** (or click the URL, `webdemowebapp.azurewebsites.net`) to navigate to it.</span></span> <span data-ttu-id="7cf40-244">En tom platshållare-sida visas eftersom inget innehåll inte har ännu publicerats till webbappen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-244">You will see a blank placeholder page, because no content has been published to the web app yet.</span></span>

<span data-ttu-id="7cf40-245">Nästa du skapar ett ”Hello World”-program och distribuera den till webbappen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-245">Next you will create a "Hello World" application and deploy it to the web app.</span></span>

### <a name="create-a-jsp-hello-world-application"></a><span data-ttu-id="7cf40-246">Skapa ett JSP Hello World-program</span><span class="sxs-lookup"><span data-stu-id="7cf40-246">Create a JSP Hello World application</span></span>
#### <a name="create-the-application"></a><span data-ttu-id="7cf40-247">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="7cf40-247">Create the application</span></span>
<span data-ttu-id="7cf40-248">För att demonstrera hur du distribuerar ett program på webben visar i följande procedur hur du skapar ett enkelt ”Hello World” Java-program och överföra den till App Service Web App som skapats av ditt program.</span><span class="sxs-lookup"><span data-stu-id="7cf40-248">In order to demonstrate how to deploy an application to the web, the following procedure shows you how to create a simple "Hello World" Java application and upload it to the App Service Web App that your application created.</span></span>

1. <span data-ttu-id="7cf40-249">Klicka på **Arkiv > Nytt > dynamiskt webbprojekt**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-249">Click **File > New > Dynamic Web Project**.</span></span> <span data-ttu-id="7cf40-250">Ge den namnet `JSPHello`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-250">Name it `JSPHello`.</span></span> <span data-ttu-id="7cf40-251">Du behöver inte göra några ändringar i den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7cf40-251">You do not need to change any other settings in this dialog.</span></span> <span data-ttu-id="7cf40-252">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-252">Click **Finish**.</span></span>
   
    ![][3]
2. <span data-ttu-id="7cf40-253">I Projektutforskaren, expanderar den **JSPHello** projekt, högerklicka på **webbinnehåll**, klicka på **New > JSP-fil**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-253">In Project Explorer, expand the **JSPHello** project, right-click **WebContent**, then click **New > JSP File**.</span></span> <span data-ttu-id="7cf40-254">I dialogrutan Ny JSP-fil namn för den nya filen `index.jsp`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-254">In the New JSP File dialog, name the new file `index.jsp`.</span></span> <span data-ttu-id="7cf40-255">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-255">Click **Next**.</span></span>
3. <span data-ttu-id="7cf40-256">I den **Välj JSP-mall** väljer **ny JSP-fil (html)** och på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-256">In the **Select JSP Template** dialog, select **New JSP File (html)** and click **Finish**.</span></span>
4. <span data-ttu-id="7cf40-257">I index.jsp, lägger du till följande kod i den `<head>` och `<body>` tagga avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7cf40-257">In index.jsp, add the following code in the `<head>` and `<body>` tag sections:</span></span>
   
        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
   
        <body>
          Hello, the time is <%= date %> 
        </body>

#### <a name="run-the-hello-world-application-in-localhost"></a><span data-ttu-id="7cf40-258">Kör programmet Hello World i localhost</span><span class="sxs-lookup"><span data-stu-id="7cf40-258">Run the Hello World application in localhost</span></span>
<span data-ttu-id="7cf40-259">Innan du kör det här programmet måste du konfigurera några egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7cf40-259">Before you run this application, you need to configure a few properties.</span></span>

1. <span data-ttu-id="7cf40-260">Högerklicka på den **JSPHello** projektet och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-260">Right-click the **JSPHello** project and select **Properties**.</span></span>
2. <span data-ttu-id="7cf40-261">I den **egenskaper** dialogrutan: Välj **Java byggsökväg**, Välj den **ordning och exportera** markerar **JRE systembibliotek**, klicka sedan på **in** att flytta den till överst i listan.</span><span class="sxs-lookup"><span data-stu-id="7cf40-261">In the **Properties** dialog: select **Java Build Path**, select the **Order and Export** tab, check **JRE System Library**, then click **Up** to move it to the top of the list.</span></span>
   
    ![][4]
3. <span data-ttu-id="7cf40-262">Även i den **egenskaper** dialogrutan: Välj **Körningsmål** och på **ny**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-262">Also in the **Properties** dialog: select **Targeted Runtimes** and click **New**.</span></span>
4. <span data-ttu-id="7cf40-263">I den **nya Server-körningsmiljön** dialogrutan, Välj en server som **Apache Tomcat v7.0** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-263">In the **New Server Runtime Environment** dialog, select a server such as **Apache Tomcat v7.0** and click **Next**.</span></span> <span data-ttu-id="7cf40-264">I den **Tomcat Server** dialogrutan, ange **namn** till `Apache Tomcat v7.0`, och ange **Tomcat installationskatalog** till den katalog där du installerade versionen av Tomcat-server som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="7cf40-264">In the **Tomcat Server** dialog, set **Name** to `Apache Tomcat v7.0`, and set **Tomcat Installation Directory** to the directory in which you installed the version of Tomcat server you want to use.</span></span>
   
    ![][5]
   
    <span data-ttu-id="7cf40-265">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-265">Click **Finish**.</span></span>
5. <span data-ttu-id="7cf40-266">Gå tillbaka till den **Körningsmål** sida av den **egenskaper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7cf40-266">You then return to the **Targeted Runtimes** page of the **Properties** dialog.</span></span> <span data-ttu-id="7cf40-267">Välj **Apache Tomcat v7.0**, klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-267">Select **Apache Tomcat v7.0**, then click **OK**.</span></span>
   
    ![][6]
6. <span data-ttu-id="7cf40-268">I Eclipse **kör** -menyn klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-268">In the Eclipse **Run** menu, click **Run**.</span></span> <span data-ttu-id="7cf40-269">I den **kör som** markerar **körs på servern**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-269">In the **Run As** dialog, select **Run on Server**.</span></span> <span data-ttu-id="7cf40-270">I den **körs på servern** markerar **Tomcat v7.0-Server**:</span><span class="sxs-lookup"><span data-stu-id="7cf40-270">In the **Run on Server** dialog, select **Tomcat v7.0 Server**:</span></span>
   
    ![][7]
   
    <span data-ttu-id="7cf40-271">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-271">Click **Finish**.</span></span>
7. <span data-ttu-id="7cf40-272">När programmet körs, bör du se den **JSPHello** visas sidan i ett fönster för localhost i Eclipse (`http://localhost:8080/JSPHello/`), visas följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="7cf40-272">When the application runs, you should see the **JSPHello** page appear in a localhost window in Eclipse (`http://localhost:8080/JSPHello/`), displaying the following message:</span></span>
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="export-the-application-as-a-war"></a><span data-ttu-id="7cf40-273">Exportera det som en WAR-fil</span><span class="sxs-lookup"><span data-stu-id="7cf40-273">Export the application as a WAR</span></span>
<span data-ttu-id="7cf40-274">Exportera project web-filer som en webb-arkivfil (WAR) så att du kan distribuera den till webbappen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-274">Export the web project files as a web archive (WAR) file so that you can deploy it to the web app.</span></span> <span data-ttu-id="7cf40-275">Följande web projektfiler finns i mappen webbinnehåll:</span><span class="sxs-lookup"><span data-stu-id="7cf40-275">The following web project files reside in the WebContent folder:</span></span>

    META-INF
    WEB-INF
    index.jsp

1. <span data-ttu-id="7cf40-276">Högerklicka på mappen webbinnehåll och välj **exportera**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-276">Right-click the WebContent folder and select **Export**.</span></span>
2. <span data-ttu-id="7cf40-277">I den **exportera Välj** dialogrutan klickar du på **Web > WAR** filen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-277">In the **Export Select** dialog, click **Web > WAR** file, then click **Next**.</span></span>
3. <span data-ttu-id="7cf40-278">I den **WAR-Export** dialogrutan Välj src-katalog i det aktuella projektet och inkludera namnet på WAR-filen i slutet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-278">In the **WAR Export** dialog, select the src directory in the current project, and include the name of the WAR file at the end.</span></span> <span data-ttu-id="7cf40-279">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7cf40-279">For example:</span></span>
   
    `<project-path>/JSPHello/src/JSPHello.war`

<span data-ttu-id="7cf40-280">Mer information om hur du distribuerar WAR-filerna finns [lägga till en Java-program i Azure App Service Web Apps](web-sites-java-add-app.md).</span><span class="sxs-lookup"><span data-stu-id="7cf40-280">For more information on deploying WAR files, see [Add a Java application to Azure App Service Web Apps](web-sites-java-add-app.md).</span></span>

### <a name="deploying-the-hello-world-application-using-ftp"></a><span data-ttu-id="7cf40-281">Distribuera programmet Hello World med FTP</span><span class="sxs-lookup"><span data-stu-id="7cf40-281">Deploying the Hello World Application Using FTP</span></span>
<span data-ttu-id="7cf40-282">Välj en tredje parts FTP-klient för att publicera programmet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-282">Select a third-party FTP client to publish the application.</span></span> <span data-ttu-id="7cf40-283">Den här proceduren beskrivs två alternativ: Kudu-konsol som är inbyggda i Azure; och FileZilla, ett populära verktyg med en lämplig, grafiskt användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="7cf40-283">This procedure describes two options: the Kudu console built into Azure; and FileZilla, a popular tool with a convenient, graphical UI.</span></span>

> <span data-ttu-id="7cf40-284">**Obs:** Azure Toolkit för Eclipse har stöd för distribution till storage-konton och cloud services, men inte stöder distribution av webbappar.</span><span class="sxs-lookup"><span data-stu-id="7cf40-284">**Note:** The Azure Toolkit for Eclipse supports deployment to storage accounts and cloud services, but does not currently support deployment to web apps.</span></span> <span data-ttu-id="7cf40-285">Du kan distribuera till storage-konton och molntjänster med ett projekt för distribution av Azure enligt beskrivningen i [skapar en Hello World program för Azure i Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), men inte för web apps.</span><span class="sxs-lookup"><span data-stu-id="7cf40-285">You can deploy to storage accounts and cloud services using an Azure Deployment Project as described in [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/azure/hh690944.aspx), but not to web apps.</span></span> <span data-ttu-id="7cf40-286">Använda andra metoder som FTP- eller GitHub för att överföra filer till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="7cf40-286">Use other methods such as FTP or GitHub to transfer files to your web app.</span></span>
> 
> <span data-ttu-id="7cf40-287">**Obs:** rekommenderar vi inte med FTP från Windows kommandotolk (FTP.EXE kommandoradsverktyget som levereras med Windows).</span><span class="sxs-lookup"><span data-stu-id="7cf40-287">**Note:** We do not recommend using FTP from the Windows command prompt (the command-line FTP.EXE utility that ships with Windows).</span></span> <span data-ttu-id="7cf40-288">FTP-klienter som använder active FTP, till exempel FTP.EXE, misslyckas ofta ska fungera över brandväggar.</span><span class="sxs-lookup"><span data-stu-id="7cf40-288">FTP clients that use active FTP, such as FTP.EXE, often fail to work over firewalls.</span></span> <span data-ttu-id="7cf40-289">Aktiva FTP anger en intern nätverksbaserade adress som FTP-servern misslyckas som sannolikt att ansluta.</span><span class="sxs-lookup"><span data-stu-id="7cf40-289">Active FTP specifies an internal LAN-based address, to which an FTP server will likely fail to connect.</span></span>
> 
> 

<span data-ttu-id="7cf40-290">Mer information om distribution till en Apptjänst-webbapp med hjälp av FTP finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7cf40-290">For more information on deployment to an App Service web app using FTP, see the following topics:</span></span>

* [<span data-ttu-id="7cf40-291">Distribuera med hjälp av ett FTP-verktyg</span><span class="sxs-lookup"><span data-stu-id="7cf40-291">Deploy using an FTP utility</span></span>](web-sites-deploy.md)

#### <a name="set-up-deployment-credentials"></a><span data-ttu-id="7cf40-292">Ställ in autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="7cf40-292">Set up deployment credentials</span></span>
<span data-ttu-id="7cf40-293">Kontrollera att du har kört den **AzureWebDemo** programmet för att skapa ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="7cf40-293">Make sure you have run the **AzureWebDemo** application to create a web app.</span></span> <span data-ttu-id="7cf40-294">Du kommer att överföra filer till den här platsen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-294">You will transfer files to this location.</span></span>

1. <span data-ttu-id="7cf40-295">Logga in på den klassiska portalen och på **Web Apps**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-295">Log into the classic portal and click **Web Apps**.</span></span> <span data-ttu-id="7cf40-296">Kontrollera att **WebDemoWebApp** visas i listan över webbappar och se till att den körs.</span><span class="sxs-lookup"><span data-stu-id="7cf40-296">Make sure **WebDemoWebApp** appears in the list of web apps, and make sure that it is running.</span></span> <span data-ttu-id="7cf40-297">Klicka på **WebDemoWebApp** att öppna dess **instrumentpanelen** sidan.</span><span class="sxs-lookup"><span data-stu-id="7cf40-297">Click **WebDemoWebApp** to open its **Dashboard** page.</span></span>
2. <span data-ttu-id="7cf40-298">På den **instrumentpanelen** sidan under **snabb i korthet**, klickar du på **ställa in dina autentiseringsuppgifter för distribution** (om du redan har autentiseringsuppgifter för distribution läser detta **återställa dina autentiseringsuppgifter för distribution**).</span><span class="sxs-lookup"><span data-stu-id="7cf40-298">On the **Dashboard** page, under **Quick Glance**, click **Set up your deployment credentials** (if you already have deployment credentials, this reads **Reset your deployment credentials**).</span></span>
   
    <span data-ttu-id="7cf40-299">Autentiseringsuppgifter för distribution är associerade med ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="7cf40-299">Deployment credentials are associated with a Microsoft account.</span></span> <span data-ttu-id="7cf40-300">Du måste ange ett användarnamn och lösenord som du kan använda för att distribuera med Git och FTP.</span><span class="sxs-lookup"><span data-stu-id="7cf40-300">You need to specify a username and password that you can use to deploy using Git and FTP.</span></span> <span data-ttu-id="7cf40-301">Du kan använda dessa autentiseringsuppgifter för att distribuera till ett webbprogram i alla Azure-prenumerationer som är kopplade till ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="7cf40-301">You can use these credentials to deploy to any web app in all Azure subscriptions associated with your Microsoft account.</span></span> <span data-ttu-id="7cf40-302">Ange autentiseringsuppgifter för Git och FTP-distribution i dialogrutan och anteckna användarnamnet och lösenordet för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="7cf40-302">Provide Git and FTP deployment credentials in the dialog, and record the username and password for future use.</span></span>

#### <a name="get-ftp-connection-information"></a><span data-ttu-id="7cf40-303">Hämta information om FTP-anslutning</span><span class="sxs-lookup"><span data-stu-id="7cf40-303">Get FTP connection information</span></span>
<span data-ttu-id="7cf40-304">Om du vill använda FTP för att distribuera programfiler till den nya webbappen, behöver du anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-304">To use FTP to deploy application files to the newly created web app, you need to obtain connection information.</span></span> <span data-ttu-id="7cf40-305">Det finns två sätt att hämta anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-305">There are two ways to obtain connection information.</span></span> <span data-ttu-id="7cf40-306">Ett sätt är att besöka webbappens **instrumentpanelen** sidan; det andra sättet är för att hämta webben appens publiceringsprofil.</span><span class="sxs-lookup"><span data-stu-id="7cf40-306">One way is to visit the web app's **Dashboard** page; the other way is to download the web app's publish profile.</span></span> <span data-ttu-id="7cf40-307">Profilen som är en XML-fil som innehåller information som FTP-värden namn och inloggning autentiseringsuppgifter för dina webbappar i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7cf40-307">The publish profile is an XML file that provides information such as FTP host name and logon credentials for your web apps in Azure App Service.</span></span> <span data-ttu-id="7cf40-308">Du kan använda det här användarnamnet och lösenordet för att distribuera till ett webbprogram i alla prenumerationer som är kopplade till det Azure-kontot, inte bara i en.</span><span class="sxs-lookup"><span data-stu-id="7cf40-308">You can use this username and password to deploy to any web app in all subscriptions associated with the Azure account, not only this one.</span></span>

<span data-ttu-id="7cf40-309">Att hämta information om FTP-anslutning från den webbapps blad i den [Azure Portal][Azure Portal]:</span><span class="sxs-lookup"><span data-stu-id="7cf40-309">To obtain FTP connection information from the web app's blade in the [Azure Portal][Azure Portal]:</span></span>

1. <span data-ttu-id="7cf40-310">Under **Essentials**, hitta och kopiera den **värdnamn för FTP-**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-310">Under **Essentials**, find and copy the **FTP hostname**.</span></span> <span data-ttu-id="7cf40-311">Detta är en URI som liknar `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-311">This is a URI similar to `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.</span></span>
2. <span data-ttu-id="7cf40-312">Under **Essentials**, hitta och kopiera **användarnamn för FTP-/ Distributionsanvändare**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-312">Under **Essentials**, find and copy **FTP/Deployment username**.</span></span> <span data-ttu-id="7cf40-313">Detta innebär att formuläret *webappname\deployment-username*, till exempel `WebDemoWebApp\deployer77`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-313">This will have the form *webappname\deployment-username*; for example `WebDemoWebApp\deployer77`.</span></span>

<span data-ttu-id="7cf40-314">Du kan hämta information om FTP-anslutning från profilen:</span><span class="sxs-lookup"><span data-stu-id="7cf40-314">To obtain FTP connection information from the publish profile:</span></span>

1. <span data-ttu-id="7cf40-315">I den webbapps blad klickar du på **Get publiceringsprofil**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-315">In the web app's blade, click **Get publish profile**.</span></span> <span data-ttu-id="7cf40-316">En .publishsettings-fil hämtas till din lokala enhet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-316">This will download a .publishsettings file to your local drive.</span></span>
2. <span data-ttu-id="7cf40-317">Öppna .publishsettings-fil i en XML-redigerare eller textredigerare och hitta det `<publishProfile>` element som innehåller `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-317">Open the .publishsettings file in an XML editor or text editor and find the `<publishProfile>` element containing `publishMethod="FTP"`.</span></span> <span data-ttu-id="7cf40-318">Det bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="7cf40-318">It should look like the following:</span></span>
   
        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>
3. <span data-ttu-id="7cf40-319">Observera att webbappens `publishProfile` inställningar karta till FileZilla Platshanteraren inställningarna på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7cf40-319">Note that the web app's `publishProfile` settings map to the FileZilla Site Manager settings as follows:</span></span>

* <span data-ttu-id="7cf40-320">`publishUrl`samma som **värdnamn för FTP-**, värdet i **värden**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-320">`publishUrl` is the same as **FTP host name**, the value you set in **Host**.</span></span>
* <span data-ttu-id="7cf40-321">`publishMethod="FTP"`innebär att du ställer in **protokollet** till **FTP - File Transfer Protocol**, och **kryptering** till **använder vanlig FTP**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-321">`publishMethod="FTP"` means that you set **Protocol** to **FTP - File Transfer Protocol**, and **Encryption** to **Use plain FTP**.</span></span>
* <span data-ttu-id="7cf40-322">`userName`och `userPWD` är nycklarna för den faktiska användarnamn och lösenord som du angav när du återställer autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="7cf40-322">`userName` and `userPWD` are keys for the actual username and password values you specified when you reset the deployment credentials.</span></span> <span data-ttu-id="7cf40-323">`userName`samma som **distribution / FTP-användare**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-323">`userName` is the same as **Deployment / FTP user**.</span></span> <span data-ttu-id="7cf40-324">Mappa till **användare** och **lösenord** i FileZilla.</span><span class="sxs-lookup"><span data-stu-id="7cf40-324">They map to **User** and **Password** in FileZilla.</span></span>
* <span data-ttu-id="7cf40-325">`ftpPassiveMode="True"`innebär att FTP-platsen använder passiv FTP-överföringen. Välj **passiva** på den **Överföringsinställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="7cf40-325">`ftpPassiveMode="True"` means that the FTP site uses passive FTP transfer; select **Passive** on the **Transfer Settings** tab.</span></span>

#### <a name="configure-the-web-app-to-host-a-java-application"></a><span data-ttu-id="7cf40-326">Konfigurera Webbappen som värd för ett Java-program</span><span class="sxs-lookup"><span data-stu-id="7cf40-326">Configure the Web App to host a Java application</span></span>
<span data-ttu-id="7cf40-327">Innan du publicerar program måste du ändra några konfigurationsinställningar för så att webbprogrammet kan vara värd för ett Java-program.</span><span class="sxs-lookup"><span data-stu-id="7cf40-327">Before you publish the application, you need to change a few configuration settings so that the web app can host a Java application.</span></span>

1. <span data-ttu-id="7cf40-328">I den klassiska portalen går du till webbappens **instrumentpanelen** och klickar på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-328">In the classic portal, go to the web app's **Dashboard** page and click **Configure**.</span></span> <span data-ttu-id="7cf40-329">På den **konfigurera** anger du följande inställningar.</span><span class="sxs-lookup"><span data-stu-id="7cf40-329">On the **Configure** page, specify the following settings.</span></span>
2. <span data-ttu-id="7cf40-330">I **Java version** standardvärdet är **av**; Välj Java version program-mål, till exempel 1.7.0_51.</span><span class="sxs-lookup"><span data-stu-id="7cf40-330">In **Java version** the default is **Off**; select the Java version your application targets; for example 1.7.0_51.</span></span> <span data-ttu-id="7cf40-331">När du gör det också se till att **webbehållaren** anges till en version av Tomcat-Server.</span><span class="sxs-lookup"><span data-stu-id="7cf40-331">After you do this, also make sure that **Web container** is set to a version of Tomcat Server.</span></span>
3. <span data-ttu-id="7cf40-332">I **standarddokument**, lägga till index.jsp och flytta upp i listan.</span><span class="sxs-lookup"><span data-stu-id="7cf40-332">In **Default Documents**, add index.jsp and move it up to the top of the list.</span></span> <span data-ttu-id="7cf40-333">(Standardfilen för webbprogram är hostingstart.html.)</span><span class="sxs-lookup"><span data-stu-id="7cf40-333">(The default file for web apps is hostingstart.html.)</span></span>
4. <span data-ttu-id="7cf40-334">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-334">Click **Save**.</span></span>

#### <a name="publish-your-application-using-kudu"></a><span data-ttu-id="7cf40-335">Publicera programmet med hjälp av Kudu</span><span class="sxs-lookup"><span data-stu-id="7cf40-335">Publish your application using Kudu</span></span>
<span data-ttu-id="7cf40-336">Ett sätt att publicera programmet är att använda Kudu-Felsökningskonsolen som är inbyggda i Azure.</span><span class="sxs-lookup"><span data-stu-id="7cf40-336">One way to publish the application is to use the Kudu debug console built into Azure.</span></span> <span data-ttu-id="7cf40-337">Kudu har visat sig vara stabila och konsekvent med App Service Web Apps och Tomcat-Server.</span><span class="sxs-lookup"><span data-stu-id="7cf40-337">Kudu is known to be stable and consistent with App Service Web Apps and Tomcat Server.</span></span> <span data-ttu-id="7cf40-338">Du har åtkomst till konsolen för webbappen genom att bläddra till URL: en följande format:</span><span class="sxs-lookup"><span data-stu-id="7cf40-338">You access the console for the web app by browsing to a URL of the following form:</span></span>

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. <span data-ttu-id="7cf40-339">Den här proceduren finns Kudu-konsolen på följande URL; Bläddra till den här platsen:</span><span class="sxs-lookup"><span data-stu-id="7cf40-339">For this procedure, the Kudu console is located at the following URL; browse to this location:</span></span>
   
    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`
2. <span data-ttu-id="7cf40-340">På den översta menyn, Välj **Felsökningskonsolen > CMD**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-340">From the top menu, select **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="7cf40-341">I kommandoraden konsolen navigerar du till `/site/wwwroot` (eller klicka på `site`, sedan `wwwroot` i vyn directory överst på sidan):</span><span class="sxs-lookup"><span data-stu-id="7cf40-341">In the console command line, navigate to `/site/wwwroot` (or click `site`, then `wwwroot` in the directory view at the top of the page):</span></span>
   
    `cd /site/wwwroot`
4. <span data-ttu-id="7cf40-342">När du har angett **Java version**, Tomcat-server ska skapa en katalog för webbappar.</span><span class="sxs-lookup"><span data-stu-id="7cf40-342">After you specify **Java version**, Tomcat server should create a webapps directory.</span></span> <span data-ttu-id="7cf40-343">Gå till katalogen webbappar på kommandoraden konsolen:</span><span class="sxs-lookup"><span data-stu-id="7cf40-343">In the console command line, navigate to the webapps directory:</span></span>
   
    `mkdir webapps`
   
    `cd webapps`
5. <span data-ttu-id="7cf40-344">Dra JSPHello.war från `<project-path>/JSPHello/src/` och släpp det i vyn Kudu directory under `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-344">Drag JSPHello.war from `<project-path>/JSPHello/src/` and drop it into the Kudu directory view under `/site/wwwroot/webapps`.</span></span> <span data-ttu-id="7cf40-345">Dra inte den till området ”dra hit om du vill ladda upp och zip” eftersom Tomcat kommer packa upp den.</span><span class="sxs-lookup"><span data-stu-id="7cf40-345">Do not drag it to the "Drag here to upload and zip" area, because Tomcat will unzip it.</span></span>
   
   ![][8]

<span data-ttu-id="7cf40-346">Första JSPHello.war visas i området directory ensamt:</span><span class="sxs-lookup"><span data-stu-id="7cf40-346">At first JSPHello.war appears in the directory area by itself:</span></span>

  ![][9]

<span data-ttu-id="7cf40-347">Under en kort tid (förmodligen mindre än 5 minuter) kommer Tomcat Server packa WAR-filen till en uppackade JSPHello katalog.</span><span class="sxs-lookup"><span data-stu-id="7cf40-347">In a short time (probably less than 5 minutes) Tomcat Server will unzip the WAR file into an unpacked JSPHello directory.</span></span> <span data-ttu-id="7cf40-348">Klicka på rotkatalogen för att se om index.jsp har uppackade och kopieras dit.</span><span class="sxs-lookup"><span data-stu-id="7cf40-348">Click the ROOT directory to see whether index.jsp has been unzipped and copied there.</span></span> <span data-ttu-id="7cf40-349">I så fall, gå tillbaka till katalogen webbappar att se om den uppackade JSPHello katalogen har skapats.</span><span class="sxs-lookup"><span data-stu-id="7cf40-349">If so, navigate back to the webapps directory to see whether the unpacked JSPHello directory has been created.</span></span> <span data-ttu-id="7cf40-350">Om du inte kan se dessa objekt vänta och upprepa.</span><span class="sxs-lookup"><span data-stu-id="7cf40-350">If you do not see these items, wait and repeat.</span></span>

  ![][10]

#### <a name="publish-your-application-using-filezilla-optional"></a><span data-ttu-id="7cf40-351">Publicera programmet med hjälp av FileZilla (valfritt)</span><span class="sxs-lookup"><span data-stu-id="7cf40-351">Publish your application using FileZilla (optional)</span></span>
<span data-ttu-id="7cf40-352">Ett annat verktyg som du kan använda för att publicera programmet är FileZilla, en populär från tredje part FTP-klient med en lämplig, grafiskt användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="7cf40-352">Another tool you can use to publish the application is FileZilla, a popular third-party FTP client with a convenient, graphical UI.</span></span> <span data-ttu-id="7cf40-353">Du kan hämta och installera FileZilla från [http://filezilla-project.org/](http://filezilla-project.org/) om du inte redan har det.</span><span class="sxs-lookup"><span data-stu-id="7cf40-353">You can download and install FileZilla from [http://filezilla-project.org/](http://filezilla-project.org/) if you do not already have it.</span></span> <span data-ttu-id="7cf40-354">Mer information om hur du använder klienten finns i [FileZilla dokumentationen](https://wiki.filezilla-project.org/Documentation) och det här blogginlägget på [FTP-klienter - del 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span><span class="sxs-lookup"><span data-stu-id="7cf40-354">For more information on using the client, see the [FileZilla documentation](https://wiki.filezilla-project.org/Documentation) and this blog entry on [FTP Clients - Part 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).</span></span>

1. <span data-ttu-id="7cf40-355">Klicka på FileZilla, **fil > Platshanteraren**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-355">In FileZilla, click **File > Site Manager**.</span></span>
2. <span data-ttu-id="7cf40-356">I den **Platshanteraren** dialogrutan klickar du på **ny plats**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-356">In the **Site Manager** dialog, click **New Site**.</span></span> <span data-ttu-id="7cf40-357">En ny tom FTP-plats visas i **Välj posten** där du uppmanas att ange ett namn.</span><span class="sxs-lookup"><span data-stu-id="7cf40-357">A new blank FTP site will appear in **Select Entry** prompting you to provide a name.</span></span> <span data-ttu-id="7cf40-358">Den här proceduren namnet `AzureWebDemo-FTP`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-358">For this procedure, name it `AzureWebDemo-FTP`.</span></span>
   
    <span data-ttu-id="7cf40-359">På den **allmänna** anger du följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="7cf40-359">On the **General** tab, specify the following settings:</span></span>
   
   * <span data-ttu-id="7cf40-360">**Värden:** ange den **värdnamn för FTP-** som du kopierade från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-360">**Host:** Enter the **FTP Host Name** that you copied from the dashboard.</span></span>
   * <span data-ttu-id="7cf40-361">**Port:** (lämna det tomt eftersom det här är en passiv överföring och servern avgör vilken port som används.)</span><span class="sxs-lookup"><span data-stu-id="7cf40-361">**Port:** (Leave this blank, as this is a passive transfer and the server will determine the port to use.)</span></span>
   * <span data-ttu-id="7cf40-362">**Protokoll:** FTP File Transfer Protocol</span><span class="sxs-lookup"><span data-stu-id="7cf40-362">**Protocol:** FTP File Transfer Protocol</span></span>
   * <span data-ttu-id="7cf40-363">**Kryptering:** använder vanlig FTP</span><span class="sxs-lookup"><span data-stu-id="7cf40-363">**Encryption:** Use plain FTP</span></span>
   * <span data-ttu-id="7cf40-364">**Inloggningstyp:** Normal</span><span class="sxs-lookup"><span data-stu-id="7cf40-364">**Logon Type:** Normal</span></span>
   * <span data-ttu-id="7cf40-365">**Användare:** ange distributionen / FTP-användare som du kopierade från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-365">**User:** Enter the Deployment / FTP user that you copied from the dashboard.</span></span> <span data-ttu-id="7cf40-366">Detta är fullständig FTP-användarnamnet som har formatet *webappname\username*.</span><span class="sxs-lookup"><span data-stu-id="7cf40-366">This is the full FTP username, which has the form *webappname\username*.</span></span>
   * <span data-ttu-id="7cf40-367">**Lösenord:** ange lösenordet som du angav när du anger autentiseringsuppgifter för distribution.</span><span class="sxs-lookup"><span data-stu-id="7cf40-367">**Password:** Enter the password that you specified when you set the deployment credentials.</span></span>
     
     <span data-ttu-id="7cf40-368">På den **Överföringsinställningar** väljer **passiva**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-368">On the **Transfer Settings** tab, select **Passive**.</span></span>
3. <span data-ttu-id="7cf40-369">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-369">Click **Connect**.</span></span> <span data-ttu-id="7cf40-370">Om lyckas Filezillas konsolen visas en `Status: Connected` meddelande och utfärda en `LIST` kommando för att visa kataloginnehållet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-370">If successful, FileZilla's console will display a `Status: Connected` message and issue a `LIST` command to list the directory contents.</span></span>
4. <span data-ttu-id="7cf40-371">I den **lokala** plats Kontrollpanelen, Välj den katalog där filen JSPHello.war finns; sökvägen ska vara liknar följande:</span><span class="sxs-lookup"><span data-stu-id="7cf40-371">In the **Local** site panel, select the source directory in which the JSPHello.war file resides; the path will be similar to the following:</span></span>
   
    `<project-path>/JSPHello/src/`
5. <span data-ttu-id="7cf40-372">I den **Remote** plats Kontrollpanelen, Välj målmappen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-372">In the **Remote** site panel, select the destination folder.</span></span> <span data-ttu-id="7cf40-373">Du ska distribuera WAR-filen till den `webapps` katalog under roten för den webbapp.</span><span class="sxs-lookup"><span data-stu-id="7cf40-373">You will deploy the WAR file to the `webapps` directory under the web app's root.</span></span> <span data-ttu-id="7cf40-374">Gå till `/site/wwwroot`, högerklicka på `wwwroot`, och välj **skapa directory**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-374">Navigate to `/site/wwwroot`, right-click on `wwwroot`, and select **Create directory**.</span></span> <span data-ttu-id="7cf40-375">Namnge katalogen `webapps` och ange den katalogen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-375">Name the directory `webapps` and enter that directory.</span></span>
6. <span data-ttu-id="7cf40-376">Överför JSPHello.war till `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-376">Transfer JSPHello.war to `/site/wwwroot/webapps`.</span></span> <span data-ttu-id="7cf40-377">Välj JSPHello.war i den **lokala** filen listan högerklickar du på den och väljer **överför**.</span><span class="sxs-lookup"><span data-stu-id="7cf40-377">Select JSPHello.war in the **Local** file list, right-click on it and select **Upload**.</span></span> <span data-ttu-id="7cf40-378">Du bör se den visas i `/site/wwwroot/webapps`.</span><span class="sxs-lookup"><span data-stu-id="7cf40-378">You should see it appear in `/site/wwwroot/webapps`.</span></span>
7. <span data-ttu-id="7cf40-379">När du har kopierat JSPHello.war till katalogen webbappar Tomcat Server kommer automatiskt att packa upp (packa) filer i WAR-filen.</span><span class="sxs-lookup"><span data-stu-id="7cf40-379">After you copy JSPHello.war to the webapps directory, Tomcat Server will automatically unpack (unzip) the files in the WAR file.</span></span> <span data-ttu-id="7cf40-380">Även om Tomcat servern börjar uppackning nästan omedelbart, kan det ta lång tid (möjligen timmar) för filerna som ska visas i FTP-klienten.</span><span class="sxs-lookup"><span data-stu-id="7cf40-380">Although Tomcat Server begins unpacking almost immediately, it might take a long time (possibly hours) for the files to appear in the FTP client.</span></span>

#### <a name="run-the-hello-world-application-on-the-web-app"></a><span data-ttu-id="7cf40-381">Kör programmet Hello World i Webbappen</span><span class="sxs-lookup"><span data-stu-id="7cf40-381">Run the Hello World application on the Web App</span></span>
1. <span data-ttu-id="7cf40-382">När du har överfört WAR-filen och verifiera att Tomcat-server har skapat en uppackade `JSPHello` directory, bläddra till `http://webdemowebapp.azurewebsites.net/JSPHello` att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="7cf40-382">After you have uploaded the WAR file and verified that Tomcat server has created an unpacked `JSPHello` directory, browse to `http://webdemowebapp.azurewebsites.net/JSPHello` to run the application.</span></span>
   
   > <span data-ttu-id="7cf40-383">**Obs:** om du klickar på **Bläddra** från den klassiska portalen kan du få standard-webbsidan säger ”Java-baserad webbprogrammet har skapats”.</span><span class="sxs-lookup"><span data-stu-id="7cf40-383">**Note:** If you click **Browse** from the classic portal, you might get the default webpage, saying "This Java based web application has been successfully created."</span></span> <span data-ttu-id="7cf40-384">Du kan behöva uppdatera webbsida för att kunna visa programinnehåll i stället för standard-webbsidan.</span><span class="sxs-lookup"><span data-stu-id="7cf40-384">You might have to refresh the webpage in order to view the application output instead of the default webpage.</span></span>
   > 
   > 
2. <span data-ttu-id="7cf40-385">När programmet körs, bör du se en webbsida med följande utdata:</span><span class="sxs-lookup"><span data-stu-id="7cf40-385">When the application runs, you should see a web page with the following output:</span></span>
   
    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`

#### <a name="clean-up-azure-resources"></a><span data-ttu-id="7cf40-386">Rensa Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="7cf40-386">Clean up Azure resources</span></span>
<span data-ttu-id="7cf40-387">Den här proceduren skapar en webbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="7cf40-387">This procedure creates an App Service web app.</span></span> <span data-ttu-id="7cf40-388">Du kommer att debiteras för resursen som den finns.</span><span class="sxs-lookup"><span data-stu-id="7cf40-388">You will be billed for the resource as long as it exists.</span></span> <span data-ttu-id="7cf40-389">Om du planerar att fortsätta att använda webbprogrammet för testning och utveckling, bör du stoppa eller ta bort den.</span><span class="sxs-lookup"><span data-stu-id="7cf40-389">Unless you plan to continue using the web app for testing or development, you should consider stopping or deleting it.</span></span> <span data-ttu-id="7cf40-390">Ett webbprogram som har stoppats medför fortfarande en liten kostnad, men du kan starta om den när som helst.</span><span class="sxs-lookup"><span data-stu-id="7cf40-390">A web app that has been stopped will still incur a small charge, but you can restart it at any time.</span></span> <span data-ttu-id="7cf40-391">Om du tar bort ett webbprogram raderas alla data som du har laddat upp till den.</span><span class="sxs-lookup"><span data-stu-id="7cf40-391">Deleting a web app erases all data you have uploaded to it.</span></span>

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
