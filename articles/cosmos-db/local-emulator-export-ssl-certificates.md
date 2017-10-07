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
# <a name="export-hello-azure-cosmos-db-emulator-certificates-for-use-with-java-python-and-nodejs"></a>Exportera hello Azure Cosmos DB emulatorn certifikat för användning med Java, Python och Node.js

[**Hämta hello emulatorn**](https://aka.ms/cosmosdb-emulator)

hello Azure Cosmos DB emulatorn tillhandahåller en lokal miljö som emulerar hello Azure DB som Cosmos-tjänsten för utveckling, inklusive dess användning av SSL-anslutningar. Det här exemplet visar hur tooexport hello SSL-certifikat för användning i språk och körningar som inte integreras med hello Windows certifikatarkiv, till exempel Java som använder sin egen [certifikatarkiv](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) och Python som använder [socket omslutningar](https://docs.python.org/2/library/ssl.html) och Node.js som använder [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback). Du kan läsa mer om hello emulatorn i [Använd hello Azure Cosmos DB-emulatorn för utveckling och testning](./local-emulator.md).

Den här kursen ingår hello följande uppgifter:

> [!div class="checklist"]
> * Rotera certifikat
> * Exportera SSL-certifikat
> * Lär dig hur toouse hello certifikat i Java, Python och Node.js

## <a name="certification-rotation"></a>Certifikatutfärdare rotation

Certifikat i hello Azure Cosmos DB lokala emulatorn har genererats hello första gången hello-emulatorn kör. Det finns två certifikat. En som används för att ansluta toohello lokala emulatorn och en för att hantera hemligheter inom hello-emulatorn. hello-certifikat som du vill tooexport är hello anslutningscertifikatet med hello eget namn ”DocumentDBEmulatorCertificate”.

Båda certifikaten kan återskapas genom att klicka på **återställa Data** enligt nedan från Azure Cosmos DB-emulatorn i hello Windows fack. Om du återskapa hello certifikat och har installerat dem till certifikatarkivet för hello Java eller använda dem någon annanstans måste tooupdate dem, annars ditt program inte längre ansluta toohello lokala emulatorn.

![Återställ Azure lokala Cosmos-DB-emulatorn](./media/local-emulator-export-ssl-certificates/database-local-emulator-reset-data.png)

## <a name="how-tooexport-hello-azure-cosmos-db-ssl-certificate"></a>Hur tooexport hello Azure Cosmos DB SSL-certifikat

1. Starta hello Windows Certificate manager genom att köra certlm.msc och navigera toohello personliga certifikat-mapp och öppna hello certifikat med hello eget namn -> **DocumentDbEmulatorCertificate**.

    ![Azure DB Cosmos lokala emulatorn exportera steg 1](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-1.png)

2. Klicka på **information** sedan **OK**.

    ![Azure DB Cosmos lokala emulatorn exportera steg 2](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-2.png)

3. Klicka på **kopiera tooFile...** .

    ![Azure DB Cosmos lokala emulatorn exportera steg 3](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-3.png)

4. Klicka på **Nästa**.

    ![Azure DB Cosmos lokala emulatorn exportera steg 4](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-4.png)

5. Klicka på **Nej, exportera inte privat nyckel**, klicka på **nästa**.

    ![Azure DB Cosmos lokala emulatorn exportera steg 5](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-5.png)

6. Klicka på **Base64-kodad X.509 (. CER)** och sedan **nästa**.

    ![Azure DB Cosmos lokala emulatorn exportera steg 6](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-6.png)

7. Namnge hello certifikat. I det här fallet **documentdbemulatorcert** och klicka sedan på **nästa**.

    ![Azure DB Cosmos lokala emulatorn exportera steg 7](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-7.png)

8. Klicka på **Slutför**.

    ![Azure DB Cosmos lokala emulatorn exportera steg 8](./media/local-emulator-export-ssl-certificates/database-local-emulator-export-step-8.png)

## <a name="how-toouse-hello-certificate-in-java"></a>Hur toouse hello certifikat i Java

När du kör Java-program eller MongoDB-program som använder hello Java-klienten är det enklare tooinstall hello certifikatet till certifikatarkivet för hello Java standard än passera hello ”-Djavax.net.ssl.trustStore=<keystore> - Djavax.net.ssl.trustStorePassword= ”<password>” flaggor. Till exempel hello ingår [Java Demo programmet](https://localhost:8081/_explorer/index.html) beror på hello standardcertifikatlagringsplatsen.

Följ anvisningarna för hello i hello [att lägga till ett certifikat toohello Java Certifikatutfärdarens certifikatarkivet](https://docs.microsoft.com/azure/java-add-certificate-ca-store) tooimport hello X.509-certifikatet till certifikatarkivet för hello standard Java. Kom kommer ihåg att du att arbeta i katalogen för hello % JAVA_HOME % när du kör keytool.

En gång hello ”CosmosDBEmulatorCertificate” SSL-certifikatet har installerats ditt program bör vara kan tooconnect och Använd hello lokala Azure Cosmos DB-emulatorn. Om du fortsätter toohave problem du toofollow hello [felsökning SSL/TLS-anslutningar](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) artikel. Det är mycket troligt hello certifikatet inte har installerats i hello %JAVA_HOME%/jre/lib/security/cacerts Windows store. För exempel om du har flera installerade versioner av Java ditt program kanske använder en annan cacerts store än hello något du uppdaterade.

## <a name="how-toouse-hello-certificate-in-python"></a>Hur toouse hello certifikat i Python

Som standard hello [Python SDK(version 2.0.0 or higher)](documentdb-sdk-python.md) för hello kommer DocumentDB API inte försöker och använda hello SSL-certifikat när du ansluter toohello lokala emulatorn. Om du vill toouse SSL-verifiering kan du följa hello exemplen i hello [Python socket omslutningar](https://docs.python.org/2/library/ssl.html) dokumentation.

## <a name="how-toouse-hello-certificate-in-nodejs"></a>Hur toouse hello certifikat i Node.js

Som standard hello [Node.js SDK(version 1.10.1 or higher)](documentdb-sdk-node.md) för hello kommer DocumentDB API inte försöker och använda hello SSL-certifikat när du ansluter toohello lokala emulatorn. Om du vill toouse SSL-verifiering kan du följa hello exemplen i hello [Node.js dokumentationen](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback).

## <a name="next-steps"></a>Nästa steg

I den här kursen har du gjort hello följande:

> [!div class="checklist"]
> * Roterade certifikat
> * Exporterade hello SSL-certifikat
> * Lärt dig hur toouse hello certifikat i Java, Python och Node.js

Du kan nu fortsätta toohello begrepp finns mer information om Cosmos DB.

> [!div class="nextstepaction"]
> [Global distribution](distribute-data-globally.md) 
