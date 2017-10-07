---
title: aaaExport hello Azure Cosmos DB emulatorn certifikat | Microsoft Docs
description: "När du utvecklar i språk och körningar som inte använder hello Windows certifikatarkiv du behöver tooexport och hantera hello SSL-certifikat. Den här posten ger steg för steg-instruktioner."
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
ms.openlocfilehash: db56cda856fccf93d71ae5b21c4090ccb9aa40a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="export-hello-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a><span data-ttu-id="8a39c-105">Exportera hello Azure Cosmos DB emulatorn certifikat för användning med Java, Python och Node.js</span><span class="sxs-lookup"><span data-stu-id="8a39c-105">Export hello Azure Cosmos DB Emulator certificates for use with Java, Python, and Node.js</span></span>

[<span data-ttu-id="8a39c-106">**Hämta hello emulatorn**</span><span class="sxs-lookup"><span data-stu-id="8a39c-106">**Download hello Emulator**</span></span>](https://aka.ms/cosmosdb-emulator)

<span data-ttu-id="8a39c-107">hello Azure Cosmos DB emulatorn tillhandahåller en lokal miljö som emulerar hello Azure DB som Cosmos-tjänsten för utveckling, inklusive dess användning av SSL-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="8a39c-107">hello Azure Cosmos DB Emulator provides a local environment that emulates hello Azure Cosmos DB service for development purposes including its use of SSL connections.</span></span> <span data-ttu-id="8a39c-108">Det här exemplet visar hur tooexport hello SSL-certifikat för användning i språk och körningar som inte integreras med hello Windows certifikatarkiv, till exempel Java som använder sin egen [certifikatarkiv](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) och Python som använder [socket omslutningar](https://docs.python.org/2/library/ssl.html) och Node.js som använder [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="8a39c-108">This post demonstrates how tooexport hello SSL certificates for use in languages and runtimes that do not integrate with hello Windows Certificate Store such as Java which uses its own [certificate store](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) and Python which uses [socket wrappers](https://docs.python.org/2/library/ssl.html) and Node.js which uses [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span> <span data-ttu-id="8a39c-109">Du kan läsa mer om hello emulatorn i [Använd hello Azure Cosmos DB-emulatorn för utveckling och testning](./local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="8a39c-109">You can read more about hello emulator in [Use hello Azure Cosmos DB Emulator for development and testing](./local-emulator.md).</span></span>

<span data-ttu-id="8a39c-110">Den här kursen ingår hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="8a39c-110">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8a39c-111">Rotera certifikat</span><span class="sxs-lookup"><span data-stu-id="8a39c-111">Rotating certificates</span></span>
> * <span data-ttu-id="8a39c-112">Exportera SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="8a39c-112">Exporting SSL certificate</span></span>
> * <span data-ttu-id="8a39c-113">Lär dig hur toouse hello certifikat i Java, Python och Node.js</span><span class="sxs-lookup"><span data-stu-id="8a39c-113">Learning how toouse hello certificate in Java, Python, and Node.js</span></span>

## <a name="certification-rotation"></a><span data-ttu-id="8a39c-114">Certifikatutfärdare rotation</span><span class="sxs-lookup"><span data-stu-id="8a39c-114">Certification rotation</span></span>

<span data-ttu-id="8a39c-115">Certifikat i hello Azure Cosmos DB lokala emulatorn har genererats hello första gången hello-emulatorn kör.</span><span class="sxs-lookup"><span data-stu-id="8a39c-115">Certificates in hello Azure Cosmos DB Local Emulator are generated hello first time hello emulator is run.</span></span> <span data-ttu-id="8a39c-116">Det finns två certifikat.</span><span class="sxs-lookup"><span data-stu-id="8a39c-116">There are two certificates.</span></span> <span data-ttu-id="8a39c-117">En som används för att ansluta toohello lokala emulatorn och en för att hantera hemligheter inom hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="8a39c-117">One used for connecting toohello local emulator and one for managing secrets within hello emulator.</span></span> <span data-ttu-id="8a39c-118">hello-certifikat som du vill tooexport är hello anslutningscertifikatet med hello eget namn ”DocumentDBEmulatorCertificate”.</span><span class="sxs-lookup"><span data-stu-id="8a39c-118">hello certificate you want tooexport is hello connection certificate with hello friendly name "DocumentDBEmulatorCertificate".</span></span>

<span data-ttu-id="8a39c-119">Båda certifikaten kan återskapas genom att klicka på **återställa Data** enligt nedan från Azure Cosmos DB-emulatorn i hello Windows fack.</span><span class="sxs-lookup"><span data-stu-id="8a39c-119">Both certificates can be regenerated by clicking **Reset Data** as shown below from Azure Cosmos DB Emulator running in hello Windows Tray.</span></span> <span data-ttu-id="8a39c-120">Om du återskapa hello certifikat och har installerat dem till certifikatarkivet för hello Java eller använda dem någon annanstans måste tooupdate dem, annars ditt program inte längre ansluta toohello lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="8a39c-120">If you regenerate hello certificates and have installed them into hello Java certificate store or used them elsewhere you will need tooupdate them, otherwise your application will no longer connect toohello local emulator.</span></span>

![Återställ Azure lokala Cosmos-DB-emulatorn](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-tooexport-hello-azure-cosmos-db-ssl-certificate"></a><span data-ttu-id="8a39c-122">Hur tooexport hello Azure Cosmos DB SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="8a39c-122">How tooexport hello Azure Cosmos DB SSL certificate</span></span>

1. <span data-ttu-id="8a39c-123">Starta hello Windows Certificate manager genom att köra certlm.msc och navigera toohello personliga certifikat-mapp och öppna hello certifikat med hello eget namn -> **DocumentDbEmulatorCertificate**.</span><span class="sxs-lookup"><span data-stu-id="8a39c-123">Start hello Windows Certificate manager by running certlm.msc and navigate toohello Personal->Certificates folder and open hello certificate with hello friendly name **DocumentDbEmulatorCertificate**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 1](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. <span data-ttu-id="8a39c-125">Klicka på **information** sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a39c-125">Click on **Details** then **OK**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 2](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. <span data-ttu-id="8a39c-127">Klicka på **kopiera tooFile...** .</span><span class="sxs-lookup"><span data-stu-id="8a39c-127">Click **Copy tooFile...**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 3](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. <span data-ttu-id="8a39c-129">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8a39c-129">Click **Next**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 4](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. <span data-ttu-id="8a39c-131">Klicka på **Nej, exportera inte privat nyckel**, klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8a39c-131">Click **No, do not export private key**, then click **Next**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 5](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. <span data-ttu-id="8a39c-133">Klicka på **Base64-kodad X.509 (. CER)** och sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8a39c-133">Click on **Base-64 encoded X.509 (.CER)** and then **Next**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 6](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. <span data-ttu-id="8a39c-135">Namnge hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="8a39c-135">Give hello certificate a name.</span></span> <span data-ttu-id="8a39c-136">I det här fallet **documentdbemulatorcert** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8a39c-136">In this case **documentdbemulatorcert** and then click **Next**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 7](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. <span data-ttu-id="8a39c-138">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="8a39c-138">Click **Finish**.</span></span>

    ![Azure DB Cosmos lokala emulatorn exportera steg 8](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-toouse-hello-certificate-in-java"></a><span data-ttu-id="8a39c-140">Hur toouse hello certifikat i Java</span><span class="sxs-lookup"><span data-stu-id="8a39c-140">How toouse hello certificate in Java</span></span>

<span data-ttu-id="8a39c-141">När du kör Java-program eller MongoDB-program som använder hello Java-klienten är det enklare tooinstall hello certifikatet till certifikatarkivet för hello Java standard än passera hello ”-Djavax.net.ssl.trustStore=<keystore> - Djavax.net.ssl.trustStorePassword= ”<password>” flaggor.</span><span class="sxs-lookup"><span data-stu-id="8a39c-141">When running Java applications or MongoDB applications that use hello Java client it is easier tooinstall hello certificate into hello Java default certificate store than passing hello "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>" flags.</span></span> <span data-ttu-id="8a39c-142">Till exempel hello ingår [Java Demo programmet](https://localhost:8081/_explorer/index.html) beror på hello standardcertifikatlagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="8a39c-142">For example hello included [Java Demo application](https://localhost:8081/_explorer/index.html) depends on hello default certificate store.</span></span>

<span data-ttu-id="8a39c-143">Följ anvisningarna för hello i hello [att lägga till ett certifikat toohello Java Certifikatutfärdarens certifikatarkivet](https://docs.microsoft.com/azure/java-add-certificate-ca-store) tooimport hello X.509-certifikatet till certifikatarkivet för hello standard Java.</span><span class="sxs-lookup"><span data-stu-id="8a39c-143">Follow hello instructions in hello [Adding a Certificate toohello Java CA Certificates Store](https://docs.microsoft.com/azure/java-add-certificate-ca-store) tooimport hello X.509 certificate into hello default Java certificate store.</span></span> <span data-ttu-id="8a39c-144">Kom kommer ihåg att du att arbeta i katalogen för hello % JAVA_HOME % när du kör keytool.</span><span class="sxs-lookup"><span data-stu-id="8a39c-144">Keep in mind you will be working in hello %JAVA_HOME% directory when running keytool.</span></span>

<span data-ttu-id="8a39c-145">En gång hello ”CosmosDBEmulatorCertificate” SSL-certifikatet har installerats ditt program bör vara kan tooconnect och Använd hello lokala Azure Cosmos DB-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="8a39c-145">Once hello "CosmosDBEmulatorCertificate" SSL certificate is installed your application should be able tooconnect and use hello local Azure Cosmos DB Emulator.</span></span> <span data-ttu-id="8a39c-146">Om du fortsätter toohave problem du toofollow hello [felsökning SSL/TLS-anslutningar](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) artikel.</span><span class="sxs-lookup"><span data-stu-id="8a39c-146">If you continue toohave trouble you may want toofollow hello [Debugging SSL/TLS Connections](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) article.</span></span> <span data-ttu-id="8a39c-147">Det är mycket troligt hello certifikatet inte har installerats i hello %JAVA_HOME%/jre/lib/security/cacerts Windows store.</span><span class="sxs-lookup"><span data-stu-id="8a39c-147">It is very likely hello certificate is not installed into hello %JAVA_HOME%/jre/lib/security/cacerts store.</span></span> <span data-ttu-id="8a39c-148">För exempel om du har flera installerade versioner av Java ditt program kanske använder en annan cacerts store än hello något du uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="8a39c-148">For example if you have multiple installed versions of Java your application may be using a different cacerts store than hello one you updated.</span></span>

## <a name="how-toouse-hello-certificate-in-python"></a><span data-ttu-id="8a39c-149">Hur toouse hello certifikat i Python</span><span class="sxs-lookup"><span data-stu-id="8a39c-149">How toouse hello certificate in Python</span></span>

<span data-ttu-id="8a39c-150">Som standard hello [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) för hello kommer DocumentDB API inte försöker och använda hello SSL-certifikat när du ansluter toohello lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="8a39c-150">By default hello [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) for hello DocumentDB API will not try and use hello SSL certificate when connecting toohello local emulator.</span></span> <span data-ttu-id="8a39c-151">Om du vill toouse SSL-verifiering kan du följa hello exemplen i hello [Python socket omslutningar](https://docs.python.org/2/library/ssl.html) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8a39c-151">If however you want toouse SSL validation you can follow hello examples in hello [Python socket wrappers](https://docs.python.org/2/library/ssl.html) documentation.</span></span>

## <a name="how-toouse-hello-certificate-in-nodejs"></a><span data-ttu-id="8a39c-152">Hur toouse hello certifikat i Node.js</span><span class="sxs-lookup"><span data-stu-id="8a39c-152">How toouse hello certificate in Node.js</span></span>

<span data-ttu-id="8a39c-153">Som standard hello [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) för hello kommer DocumentDB API inte försöker och använda hello SSL-certifikat när du ansluter toohello lokala emulatorn.</span><span class="sxs-lookup"><span data-stu-id="8a39c-153">By default hello [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) for hello DocumentDB API will not try and use hello SSL certificate when connecting toohello local emulator.</span></span> <span data-ttu-id="8a39c-154">Om du vill toouse SSL-verifiering kan du följa hello exemplen i hello [Node.js dokumentationen](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span><span class="sxs-lookup"><span data-stu-id="8a39c-154">If however you want toouse SSL validation you can follow hello examples in hello [Node.js documentation](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a39c-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a39c-155">Next steps</span></span>

<span data-ttu-id="8a39c-156">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="8a39c-156">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8a39c-157">Roterade certifikat</span><span class="sxs-lookup"><span data-stu-id="8a39c-157">Rotated certificates</span></span>
> * <span data-ttu-id="8a39c-158">Exporterade hello SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="8a39c-158">Exported hello SSL certificate</span></span>
> * <span data-ttu-id="8a39c-159">Lärt dig hur toouse hello certifikat i Java, Python och Node.js</span><span class="sxs-lookup"><span data-stu-id="8a39c-159">Learned how toouse hello certificate in Java, Python and Node.js</span></span>

<span data-ttu-id="8a39c-160">Du kan nu fortsätta toohello begrepp finns mer information om Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8a39c-160">You can now proceed toohello Concepts section for more information about Cosmos DB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8a39c-161">Global distribution</span><span class="sxs-lookup"><span data-stu-id="8a39c-161">Global distribution</span></span>](distribute-data-globally.md) 
