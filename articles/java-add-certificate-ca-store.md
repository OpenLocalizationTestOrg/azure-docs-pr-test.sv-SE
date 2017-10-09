---
title: aaaAdd ett certifikatarkiv toohello Java CA | Microsoft Docs
description: "Lär dig hur tooadd ett certifikat (certifikatutfärdare) certifikat toohello Java CA-certifikat (cacerts) lagrar för Twilio-tjänsten eller Azure Service Bus."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 030e43129580023942dee662e72d2f443167f308
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-certificate-toohello-java-ca-certificates-store"></a>Lägga till ett certifikat toohello Java CA-certifikat i lagret
hello följande steg visar hur tooadd ett certifikat (certifikatutfärdare) certifikat toohello Java CA-certifikat (cacerts) lagrar. hello-exempel som används för hello CA-certifikat krävs av hello Twilio-tjänsten. Informationen som ges senare i hello avsnittet beskriver hur tooinstall hello CA-certifikatet för hello Azure Service Bus. 

Du kan använda keytool tooadd hello Certifikatutfärdarens certifikat tidigare toozipping din JDK och lägga till den tooyour Azure-projekt **approot** mappen, eller om du kan köra en Azure uppstart aktivitet som använder keytool tooadd hello certifikat. Det här exemplet förutsätter att du ska lägga till en CA-certifikat tidigare toohello JDK som zippade. Dessutom en viss CA-certifikat ska användas i hello exempel, men hello stegen för att hämta en annan certifikatutfärdare och importerar den till hello cacerts store är liknande.

## <a name="tooadd-a-certificate-toohello-cacerts-store"></a>tooadd ett certifikat toohello cacerts lagra
1. Vid en kommandotolk som har angetts tooyour JDK **jdk\jre\lib\security** mapp, kör hello följande toosee vilka certifikat som är installerade:
   
    `keytool -list -keystore cacerts`
   
    Du uppmanas för hello lagra lösenord. hello standardlösenordet är **changeit**. (Om du vill toochange hello lösenord finns hello i dokumentationen på <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) Det här exemplet förutsätter att hello-certifikat med MD5 fingeravtryck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 inte visas och du vill tooimport it (särskilt certifikatet krävs av hello Twilio API-tjänsten).
2. Skaffa hello certifikat från hello listan över certifikat som anges i [GeoTrust rotcertifikat](http://www.geotrust.com/resources/root-certificates/). Högerklicka på hello länk för hello certifikat med serienumret 35:DE:F4:CF och spara den toohello **jdk\jre\lib\security** mapp. För det här exemplet, den har sparats tooa fil med namnet **Equifax\_Secure\_certifikat\_Authority.cer**.
3. Importera hello certifikat via hello följande kommando:
   
    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`
   
    När du uppmanas till detta tootrust det här certifikatet om hello certifikat har MD5 fingeravtryck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, svara genom att skriva **y**.
4. Kör hello efter kommandot tooensure hello Certifikatutfärdarens certifikat har importerats:
   
    `keytool -list -keystore cacerts`
5. ZIP-hello JDK och lägga till den tooyour Azure-projekt **approot** mapp.

Information om keytool finns <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure rotcertifikat
Ditt program som använder Azure-tjänster (till exempel Azure Service Bus) måste tootrust hello Baltimore CyberTrust-rotcertifikatet. (Från 15 April 2013 Azure började migrera från hello GTE CyberTrust globala Root toohello Baltimore CyberTrust Root. Den här migreringen tog flera månader toocomplete.)

hello certifikat kan vara installerad i din cacerts store, Baltimore så Spara toorun hello **keytool-lista** kommandot första toosee om den redan finns.

Om du behöver tooadd Hej Baltimore CyberTrust Root, den har serienummer 02:00:00:b9 och SHA1 fingeravtryck d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Det kan hämtas från <https://cacert.omniroot.com/bc2025.crt>, sparade tooa lokal fil med tillägget **.cer**, och sedan importeras med **keytool** som ovan.

## <a name="next-steps"></a>Nästa steg
Mer information om hello rotcertifikat som används av Azure finns [Azure Root Certificate migrering](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Läs mer om Java [Azure för Java-utvecklare](/java/azure).

