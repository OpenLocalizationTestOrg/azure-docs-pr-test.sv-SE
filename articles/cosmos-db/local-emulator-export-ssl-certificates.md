---
title: Exportera Azure Cosmos DB emulatorn certifikat | Microsoft Docs
description: "När du utvecklar i språk och körningar som inte använder Windows certifikatarkiv måste du exportera och hantera SSL-certifikat. Den här posten ger steg för steg-instruktioner."
services: cosmos-db
documentationcenter: 
keywords: Azure Cosmos DB-emulatorn
author: voellm
manager: jhubbard
editor: 
ms.assetid: ef43deda-c2e9-4193-99e2-7f6a88a0319f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: tvoellm
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4add5028d50972316902cecd8c399781c012cb77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="export-the-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a><span data-ttu-id="02664-105">Exportera Azure Cosmos DB emulatorn certifikat för användning med Java, Python och Node.js</span><span class="sxs-lookup"><span data-stu-id="02664-105">Export the Azure Cosmos DB Emulator certificates for use with Java, Python, and Node.js</span></span>

[<span data-ttu-id="02664-106">**Hämta emulatorn**</span><span class="sxs-lookup"><span data-stu-id="02664-106">**Download the Emulator**</span></span>](https://aka.ms/cosmosdb-emulator)

<span data-ttu-id="02664-107">Azure-emulatorn Cosmos DB tillhandahåller en lokal miljö som emulerar Azure DB som Cosmos-tjänsten för utveckling, inklusive dess användning av SSL-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="02664-107">The Azure Cosmos DB Emulator provides a local environment that emulates the Azure Cosmos DB service for development purposes including its use of SSL connections.</span></span> <span data-ttu-id="02664-108">Det här exemplet visar hur du exportera SSL-certifikat för användning i språk och körningar som inte integreras med Windows certifikatarkiv, till exempel Java som använder sin egen [certifikatarkiv](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) och Python som använder [socket omslutningar](https://docs.python.org/2/library/ssl.html) och Node.js som använder [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="02664-108">This post demonstrates how to export the SSL certificates for use in languages and runtimes that do not integrate with the Windows Certificate Store such as Java which uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) and Python which uses [socket wrappers](https://docs.python.org/2/library/ssl.html) and Node.js which uses [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span> <span data-ttu-id="02664-109">Du kan läsa mer om emulatorn i [Använd Azure Cosmos DB-emulatorn för utveckling och testning](./local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="02664-109">You can read more about the emulator in [Use the Azure Cosmos DB Emulator for development and testing](./local-emulator.md).</span></span>

<span data-ttu-id="02664-110">Den här kursen ingår följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="02664-110">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="02664-111">Rotera certifikat</span><span class="sxs-lookup"><span data-stu-id="02664-111">Rotating certificates</span></span>
> * <span data-ttu-id="02664-112">Exportera SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="02664-112">Exporting SSL certificate</span></span>
> * <span data-ttu-id="02664-113">Lär dig hur du använder certifikat i Java, Python och Node.js</span><span class="sxs-lookup"><span data-stu-id="02664-113">Learning how to use the certificate in Java, Python, and Node.js</span></span>

## <a name="certification-rotation"></a><span data-ttu-id="02664-114">Certifikatutfärdare rotation</span><span class="sxs-lookup"><span data-stu-id="02664-114">Certification rotation</span></span>

<span data-ttu-id="02664-115">Certifikat i Azure Cosmos DB lokala emulatorn skapas första gången emulatorn körs.</span><span class="sxs-lookup"><span data-stu-id="02664-115">Certificates in the Azure Cosmos DB Local Emulator are generated the first time the emulator is run.</span></span> <span data-ttu-id="02664-116">Det finns två certifikat.</span><span class="sxs-lookup"><span data-stu-id="02664-116">There are two certificates.</span></span> <span data-ttu-id="02664-117">En som används för att ansluta till den lokala emulatorn och ett för att hantera hemligheter i emulatorn.</span><span class="sxs-lookup"><span data-stu-id="02664-117">One used for connecting to the local emulator and one for managing secrets within the emulator.</span></span> <span data-ttu-id="02664-118">Certifikatet som du vill exportera är anslutningscertifikatet med namnet ”DocumentDBEmulatorCertificate”.</span><span class="sxs-lookup"><span data-stu-id="02664-118">The certificate you want to export is the connection certificate with the friendly name "DocumentDBEmulatorCertificate".</span></span>

<span data-ttu-id="02664-119">Båda certifikaten kan återskapas genom att klicka på **återställa Data** enligt nedan från Azure Cosmos DB-emulatorn som körs i Windows-enheten.</span><span class="sxs-lookup"><span data-stu-id="02664-119">Both certificates can be regenerated by clicking **Reset Data** as shown below from Azure Cosmos DB Emulator running in the Windows Tray.</span></span> <span data-ttu-id="02664-120">Om du återskapa certifikatet och har installerat dem till certifikatarkivet Java eller använda dem någon annanstans måste du uppdatera dem, annars ditt program inte längre att ansluta till den lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="02664-120">If you regenerate the certificates and have installed them into the Java certificate store or used them elsewhere you will need to update them, otherwise your application will no longer connect to the local emulator.</span></span>

![Återställ Azure lokala Cosmos-DB-emulatorn](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-to-export-the-azure-cosmos-db-ssl-certificate"></a><span data-ttu-id="02664-122">Så här exporterar du Azure Cosmos DB SSL-certifikatet</span><span class="sxs-lookup"><span data-stu-id="02664-122">How to export the Azure Cosmos DB SSL certificate</span></span>

1. <span data-ttu-id="02664-123">Starta Windows Certifikathanteraren genom att köra certlm.msc och navigera till mappen-> personliga certifikat och öppna certifikatet med namnet **DocumentDbEmulatorCertificate**.</span><span class="sxs-lookup"><span data-stu-id="02664-123">Start the Windows Certificate manager by running certlm.msc and navigate to the Personal->Certificates folder and open the certificate with the friendly name **DocumentDbEmulatorCertificate**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 1](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. <span data-ttu-id="02664-125">Klicka på **information** sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="02664-125">Click on **Details** then **OK**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 2](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. <span data-ttu-id="02664-127">Klicka på **kopiera till fil...** .</span><span class="sxs-lookup"><span data-stu-id="02664-127">Click **Copy to File...**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 3](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. <span data-ttu-id="02664-129">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="02664-129">Click **Next**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 4](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. <span data-ttu-id="02664-131">Klicka på **Nej, exportera inte privat nyckel**, klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="02664-131">Click **No, do not export private key**, then click **Next**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 5](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. <span data-ttu-id="02664-133">Klicka på **Base64-kodad X.509 (. CER)** och sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="02664-133">Click on **Base-64 encoded X.509 (.CER)** and then **Next**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 6](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. <span data-ttu-id="02664-135">Namnge certifikatet.</span><span class="sxs-lookup"><span data-stu-id="02664-135">Give the certificate a name.</span></span> <span data-ttu-id="02664-136">I det här fallet **documentdbemulatorcert** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="02664-136">In this case **documentdbemulatorcert** and then click **Next**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 7](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. <span data-ttu-id="02664-138">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="02664-138">Click **Finish**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 8](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-to-use-the-certificate-in-java"></a><span data-ttu-id="02664-140">Hur du använder certifikat i Java</span><span class="sxs-lookup"><span data-stu-id="02664-140">How to use the certificate in Java</span></span>

<span data-ttu-id="02664-141">När du kör Java-program eller MongoDB-program som använder Java-klienten är det lättare att installera certifikatet i certifikatarkivet Java standard än skicka den ”-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword = ”<password>” flaggor.</span><span class="sxs-lookup"><span data-stu-id="02664-141">When running Java applications or MongoDB applications that use the Java client it is easier to install the certificate into the Java default certificate store than passing the "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>" flags.</span></span> <span data-ttu-id="02664-142">Till exempel ingår [Java Demo programmet](https://localhost:8081/_explorer/index.html) beror på standardcertifikatlagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="02664-142">For example the included [Java Demo application](https://localhost:8081/_explorer/index.html) depends on the default certificate store.</span></span>

<span data-ttu-id="02664-143">Följ instruktionerna i den [att lägga till ett certifikat i Java Certifikatutfärdarens certifikatarkivet](https://docs.microsoft.com/azure/java-add-certificate-ca-store) importera X.509-certifikatet till certifikatarkivet standard Java.</span><span class="sxs-lookup"><span data-stu-id="02664-143">Follow the instructions in the [Adding a Certificate to the Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store) to import the X.509 certificate into the default Java certificate store.</span></span> <span data-ttu-id="02664-144">Kom kommer ihåg att du att arbeta i katalogen % JAVA_HOME % när du kör keytool.</span><span class="sxs-lookup"><span data-stu-id="02664-144">Keep in mind you will be working in the %JAVA_HOME% directory when running keytool.</span></span>

<span data-ttu-id="02664-145">En gång i ”CosmosDBEmulatorCertificate” SSL-certifikatet har installerats ditt program ska kunna ansluta och använda lokala Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="02664-145">Once the "CosmosDBEmulatorCertificate" SSL certificate is installed your application should be able to connect and use the local Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="02664-146">Om du fortfarande har problem kan du vill följa den [felsökning SSL/TLS-anslutningar](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) artikel.</span><span class="sxs-lookup"><span data-stu-id="02664-146">If you continue to have trouble you may want to follow the [Debugging SSL/TLS Connections](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) article.</span></span> <span data-ttu-id="02664-147">Det är mycket troligt att certifikatet inte har installerats i arkivet för %JAVA_HOME%/jre/lib/security/cacerts.</span><span class="sxs-lookup"><span data-stu-id="02664-147">It is very likely the certificate is not installed into the %JAVA_HOME%/jre/lib/security/cacerts store.</span></span> <span data-ttu-id="02664-148">För exempel om du har flera installerade versioner av Java ditt program kanske använder en annan cacerts store än det du uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="02664-148">For example if you have multiple installed versions of Java your application may be using a different cacerts store than the one you updated.</span></span>

## <a name="how-to-use-the-certificate-in-python"></a><span data-ttu-id="02664-149">Hur du använder certifikatet i Python</span><span class="sxs-lookup"><span data-stu-id="02664-149">How to use the certificate in Python</span></span>

<span data-ttu-id="02664-150">Som standard den [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) för DocumentDB-API: et inte kommer försöker och använda SSL-certifikatet när du ansluter till lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="02664-150">By default the [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) for the DocumentDB API will not try and use the SSL certificate when connecting to the local emulator.</span></span> <span data-ttu-id="02664-151">Om du kan följa exemplen i men du vill använda SSL-verifiering av [Python socket omslutningar](https://docs.python.org/2/library/ssl.html) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="02664-151">If however you want to use SSL validation you can follow the examples in the [Python socket wrappers](https://docs.python.org/2/library/ssl.html) documentation.</span></span>

## <a name="how-to-use-the-certificate-in-nodejs"></a><span data-ttu-id="02664-152">Hur du använder certifikatet i Node.js</span><span class="sxs-lookup"><span data-stu-id="02664-152">How to use the certificate in Node.js</span></span>

<span data-ttu-id="02664-153">Som standard den [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) för DocumentDB-API: et inte kommer försöker och använda SSL-certifikatet när du ansluter till lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="02664-153">By default the [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) for the DocumentDB API will not try and use the SSL certificate when connecting to the local emulator.</span></span> <span data-ttu-id="02664-154">Om du kan följa exemplen i men du vill använda SSL-verifiering av [Node.js dokumentationen](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="02664-154">If however you want to use SSL validation you can follow the examples in the [Node.js documentation](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="02664-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02664-155">Next steps</span></span>

<span data-ttu-id="02664-156">I den här självstudiekursen kommer du har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="02664-156">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="02664-157">Roterade certifikat</span><span class="sxs-lookup"><span data-stu-id="02664-157">Rotated certificates</span></span>
> * <span data-ttu-id="02664-158">Exportera SSL-certifikatet</span><span class="sxs-lookup"><span data-stu-id="02664-158">Exported the SSL certificate</span></span>
> * <span data-ttu-id="02664-159">Lärt dig hur du använder certifikat i Java, Python och Node.js</span><span class="sxs-lookup"><span data-stu-id="02664-159">Learned how to use the certificate in Java, Python and Node.js</span></span>

<span data-ttu-id="02664-160">Du kan gå vidare till avsnittet begrepp för mer information om Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="02664-160">You can now proceed to the Concepts section for more information about Cosmos DB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="02664-161">Global distribution</span><span class="sxs-lookup"><span data-stu-id="02664-161">Global distribution</span></span>](distribute-data-globally.md) 
