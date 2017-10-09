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
# <a name="adding-a-certificate-toohello-java-ca-certificates-store"></a><span data-ttu-id="27daf-103">Lägga till ett certifikat toohello Java CA-certifikat i lagret</span><span class="sxs-lookup"><span data-stu-id="27daf-103">Adding a Certificate toohello Java CA Certificates Store</span></span>
<span data-ttu-id="27daf-104">hello följande steg visar hur tooadd ett certifikat (certifikatutfärdare) certifikat toohello Java CA-certifikat (cacerts) lagrar.</span><span class="sxs-lookup"><span data-stu-id="27daf-104">hello following steps show you how tooadd a certificate authority (CA) certificate toohello Java CA certificate (cacerts) store.</span></span> <span data-ttu-id="27daf-105">hello-exempel som används för hello CA-certifikat krävs av hello Twilio-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="27daf-105">hello example used is for hello CA certificate required by hello Twilio service.</span></span> <span data-ttu-id="27daf-106">Informationen som ges senare i hello avsnittet beskriver hur tooinstall hello CA-certifikatet för hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="27daf-106">Information provided later in hello topic describes how tooinstall hello CA certificate for hello Azure Service Bus.</span></span> 

<span data-ttu-id="27daf-107">Du kan använda keytool tooadd hello Certifikatutfärdarens certifikat tidigare toozipping din JDK och lägga till den tooyour Azure-projekt **approot** mappen, eller om du kan köra en Azure uppstart aktivitet som använder keytool tooadd hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="27daf-107">You can use keytool tooadd hello CA certificate prior toozipping your JDK and adding it tooyour Azure project's **approot** folder, or you could run an Azure start-up task that uses keytool tooadd hello certificate.</span></span> <span data-ttu-id="27daf-108">Det här exemplet förutsätter att du ska lägga till en CA-certifikat tidigare toohello JDK som zippade.</span><span class="sxs-lookup"><span data-stu-id="27daf-108">This example assumes you will add a CA certificate prior toohello JDK being zipped.</span></span> <span data-ttu-id="27daf-109">Dessutom en viss CA-certifikat ska användas i hello exempel, men hello stegen för att hämta en annan certifikatutfärdare och importerar den till hello cacerts store är liknande.</span><span class="sxs-lookup"><span data-stu-id="27daf-109">Also, a specific CA certificate will be used in hello example, but hello steps of obtaining a different CA certificate and importing it into hello cacerts store would be similar.</span></span>

## <a name="tooadd-a-certificate-toohello-cacerts-store"></a><span data-ttu-id="27daf-110">tooadd ett certifikat toohello cacerts lagra</span><span class="sxs-lookup"><span data-stu-id="27daf-110">tooadd a certificate toohello cacerts store</span></span>
1. <span data-ttu-id="27daf-111">Vid en kommandotolk som har angetts tooyour JDK **jdk\jre\lib\security** mapp, kör hello följande toosee vilka certifikat som är installerade:</span><span class="sxs-lookup"><span data-stu-id="27daf-111">At a command prompt that is set tooyour JDK's **jdk\jre\lib\security** folder, run hello following toosee what certificates are installed:</span></span>
   
    `keytool -list -keystore cacerts`
   
    <span data-ttu-id="27daf-112">Du uppmanas för hello lagra lösenord.</span><span class="sxs-lookup"><span data-stu-id="27daf-112">You'll be prompted for hello store password.</span></span> <span data-ttu-id="27daf-113">hello standardlösenordet är **changeit**.</span><span class="sxs-lookup"><span data-stu-id="27daf-113">hello default password is **changeit**.</span></span> <span data-ttu-id="27daf-114">(Om du vill toochange hello lösenord finns hello i dokumentationen på <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) Det här exemplet förutsätter att hello-certifikat med MD5 fingeravtryck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 inte visas och du vill tooimport it (särskilt certifikatet krävs av hello Twilio API-tjänsten).</span><span class="sxs-lookup"><span data-stu-id="27daf-114">(If you want toochange hello password, see hello keytool documentation at <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) This example assumes that hello certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 is not listed, and that you want tooimport it (this particular certificate is needed by hello Twilio API service).</span></span>
2. <span data-ttu-id="27daf-115">Skaffa hello certifikat från hello listan över certifikat som anges i [GeoTrust rotcertifikat](http://www.geotrust.com/resources/root-certificates/).</span><span class="sxs-lookup"><span data-stu-id="27daf-115">Obtain hello certificate from hello list of certificates listed at [GeoTrust Root Certificates](http://www.geotrust.com/resources/root-certificates/).</span></span> <span data-ttu-id="27daf-116">Högerklicka på hello länk för hello certifikat med serienumret 35:DE:F4:CF och spara den toohello **jdk\jre\lib\security** mapp.</span><span class="sxs-lookup"><span data-stu-id="27daf-116">Right-click hello link for hello certificate with serial number 35:DE:F4:CF and save it toohello **jdk\jre\lib\security** folder.</span></span> <span data-ttu-id="27daf-117">För det här exemplet, den har sparats tooa fil med namnet **Equifax\_Secure\_certifikat\_Authority.cer**.</span><span class="sxs-lookup"><span data-stu-id="27daf-117">For purposes of this example, it was saved tooa file named **Equifax\_Secure\_Certificate\_Authority.cer**.</span></span>
3. <span data-ttu-id="27daf-118">Importera hello certifikat via hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="27daf-118">Import hello certificate via hello following command:</span></span>
   
    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`
   
    <span data-ttu-id="27daf-119">När du uppmanas till detta tootrust det här certifikatet om hello certifikat har MD5 fingeravtryck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, svara genom att skriva **y**.</span><span class="sxs-lookup"><span data-stu-id="27daf-119">When prompted tootrust this certificate, if hello certificate has MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, respond by typing **y**.</span></span>
4. <span data-ttu-id="27daf-120">Kör hello efter kommandot tooensure hello Certifikatutfärdarens certifikat har importerats:</span><span class="sxs-lookup"><span data-stu-id="27daf-120">Run hello following command tooensure hello CA certificate has been successfully imported:</span></span>
   
    `keytool -list -keystore cacerts`
5. <span data-ttu-id="27daf-121">ZIP-hello JDK och lägga till den tooyour Azure-projekt **approot** mapp.</span><span class="sxs-lookup"><span data-stu-id="27daf-121">Zip hello JDK and add it tooyour Azure project's **approot** folder.</span></span>

<span data-ttu-id="27daf-122">Information om keytool finns <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span><span class="sxs-lookup"><span data-stu-id="27daf-122">For information about keytool, see <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.</span></span>

## <a name="azure-root-certificates"></a><span data-ttu-id="27daf-123">Azure rotcertifikat</span><span class="sxs-lookup"><span data-stu-id="27daf-123">Azure Root Certificates</span></span>
<span data-ttu-id="27daf-124">Ditt program som använder Azure-tjänster (till exempel Azure Service Bus) måste tootrust hello Baltimore CyberTrust-rotcertifikatet.</span><span class="sxs-lookup"><span data-stu-id="27daf-124">Your applications that use Azure services (such as Azure Service Bus) need tootrust hello Baltimore CyberTrust Root certificate.</span></span> <span data-ttu-id="27daf-125">(Från 15 April 2013 Azure började migrera från hello GTE CyberTrust globala Root toohello Baltimore CyberTrust Root.</span><span class="sxs-lookup"><span data-stu-id="27daf-125">(Beginning April 15, 2013, Azure began migrating from hello GTE CyberTrust Global Root toohello Baltimore CyberTrust Root.</span></span> <span data-ttu-id="27daf-126">Den här migreringen tog flera månader toocomplete.)</span><span class="sxs-lookup"><span data-stu-id="27daf-126">This migration took several months toocomplete.)</span></span>

<span data-ttu-id="27daf-127">hello certifikat kan vara installerad i din cacerts store, Baltimore så Spara toorun hello **keytool-lista** kommandot första toosee om den redan finns.</span><span class="sxs-lookup"><span data-stu-id="27daf-127">hello Baltimore certificate might already be installed in your cacerts store, so remember toorun hello **keytool -list** command first toosee if it already exists.</span></span>

<span data-ttu-id="27daf-128">Om du behöver tooadd Hej Baltimore CyberTrust Root, den har serienummer 02:00:00:b9 och SHA1 fingeravtryck d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74.</span><span class="sxs-lookup"><span data-stu-id="27daf-128">If you need tooadd hello Baltimore CyberTrust Root, it has serial number 02:00:00:b9 and SHA1 fingerprint d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74.</span></span> <span data-ttu-id="27daf-129">Det kan hämtas från <https://cacert.omniroot.com/bc2025.crt>, sparade tooa lokal fil med tillägget **.cer**, och sedan importeras med **keytool** som ovan.</span><span class="sxs-lookup"><span data-stu-id="27daf-129">It can be downloaded from <https://cacert.omniroot.com/bc2025.crt>, saved tooa local file with extension **.cer**, and then imported using **keytool** as shown above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27daf-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27daf-130">Next steps</span></span>
<span data-ttu-id="27daf-131">Mer information om hello rotcertifikat som används av Azure finns [Azure Root Certificate migrering](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).</span><span class="sxs-lookup"><span data-stu-id="27daf-131">For more information about hello root certificates used by Azure, see [Azure Root Certificate Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).</span></span>

<span data-ttu-id="27daf-132">Läs mer om Java [Azure för Java-utvecklare](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="27daf-132">For more information about Java, see [Azure for Java developers](/java/azure).</span></span>

