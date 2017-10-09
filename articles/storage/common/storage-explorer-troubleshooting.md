---
title: "aaaAzure Lagringsutforskaren felsökningsguiden | Microsoft Docs"
description: "Översikt över hello två felsökningsfunktionen Azure"
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: delhan
ms.openlocfilehash: 5152f70418707d65c0a4bce9a916336829956219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a><span data-ttu-id="420e6-103">Azure Lagringsutforskaren felsökningsguiden</span><span class="sxs-lookup"><span data-stu-id="420e6-103">Azure Storage Explorer troubleshooting guide</span></span>

## <a name="introduction"></a><span data-ttu-id="420e6-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="420e6-104">Introduction</span></span>

<span data-ttu-id="420e6-105">Microsoft Azure Lagringsutforskaren (förhandsversion) är en fristående app som du kan använda tooeasily fungerar med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="420e6-105">Microsoft Azure Storage Explorer (Preview) is a stand-alone app that enables you tooeasily work with Azure Storage data on Windows, macOS and Linux.</span></span> <span data-ttu-id="420e6-106">hello app kan ansluta toStorage konton finns i Azure, statliga moln och Azure-stacken.</span><span class="sxs-lookup"><span data-stu-id="420e6-106">hello app can connect toStorage accounts hosted on Azure, Sovereign Clouds, and Azure Stack.</span></span>

<span data-ttu-id="420e6-107">Den här guiden beskrivs lösningar på vanliga problem som visas i Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="420e6-107">This guide summarizes solutions for common issues seen in Storage Explorer.</span></span>

## <a name="sign-in-issues"></a><span data-ttu-id="420e6-108">Logga in problem</span><span class="sxs-lookup"><span data-stu-id="420e6-108">Sign in issues</span></span>

<span data-ttu-id="420e6-109">Innan du fortsätter försök att starta om programmet och se om hello problem kan åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="420e6-109">Before you continue, try restarting your application and see whether hello problems can be fixed.</span></span>

### <a name="error-self-signed-certificate-in-certificate-chain"></a><span data-ttu-id="420e6-110">Fel: Självsignerat certifikat i certifikatkedjan</span><span class="sxs-lookup"><span data-stu-id="420e6-110">Error: Self-Signed Certificate in Certificate Chain</span></span>

<span data-ttu-id="420e6-111">Det finns flera skäl till varför detta fel kan uppstå och två vanligaste orsakerna till hello är följande:</span><span class="sxs-lookup"><span data-stu-id="420e6-111">There are several reasons why you may encounter this error, and hello most common two reasons are as follows:</span></span>

1. <span data-ttu-id="420e6-112">hello app är anslutna via en ”transparent proxy”, vilket innebär att en server (till exempel företagets servern) avlyssna HTTPS-trafik, att dekryptera den och kryptera den med hjälp av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="420e6-112">hello app is connected through a “transparent proxy”, which means a server (such as your company server) is intercepting HTTPS traffic, decrypting it, and then encrypting it using a self-signed certificate.</span></span>

2. <span data-ttu-id="420e6-113">Du kör ett program, till exempel antivirusprogram, vilket är att injicera ett självsignerat SSL-certifikat i hello HTTPS-meddelanden som visas.</span><span class="sxs-lookup"><span data-stu-id="420e6-113">You are running an application, such as antivirus software, which is injecting a self-signed SSL certificate into hello HTTPS messages that you receive.</span></span>

<span data-ttu-id="420e6-114">När Lagringsutforskaren påträffar ett problem med hello, kan den inte längre vet om emot HTTPS hälsningsmeddelande har ändrats.</span><span class="sxs-lookup"><span data-stu-id="420e6-114">When Storage Explorer encounters one of hello issues, it can no longer know whether hello received HTTPS message is tampered.</span></span> <span data-ttu-id="420e6-115">Om du har en kopia av hello självsignerade certifikat, kan du låta Lagringsutforskaren litar på den.</span><span class="sxs-lookup"><span data-stu-id="420e6-115">If you have a copy of hello self-signed certificate, you can let Storage Explorer trust it.</span></span> <span data-ttu-id="420e6-116">Om du är osäker på som är att injicera hello certifikatet följer du dessa steg toofind den:</span><span class="sxs-lookup"><span data-stu-id="420e6-116">If you are unsure of who is injecting hello certificate, follow these steps toofind it:</span></span>

1. <span data-ttu-id="420e6-117">Installera öppna SSL</span><span class="sxs-lookup"><span data-stu-id="420e6-117">Install Open SSL</span></span>

    - <span data-ttu-id="420e6-118">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (alla hello lätta versionerna bör vara tillräckligt)</span><span class="sxs-lookup"><span data-stu-id="420e6-118">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (any of hello light versions should be sufficient)</span></span>

    - <span data-ttu-id="420e6-119">Mac- och Linux: ska ingå i ditt operativsystem</span><span class="sxs-lookup"><span data-stu-id="420e6-119">Mac and Linux: should be included with your operating system</span></span>

2. <span data-ttu-id="420e6-120">Kör öppna SSL</span><span class="sxs-lookup"><span data-stu-id="420e6-120">Run Open SSL</span></span>

    - <span data-ttu-id="420e6-121">Windows: öppna hello installationskatalogen, klicka på **/bin/**, och dubbelklicka sedan på **openssl.exe**.</span><span class="sxs-lookup"><span data-stu-id="420e6-121">Windows: open hello installation directory, click **/bin/**, and then double-click **openssl.exe**.</span></span>
    - <span data-ttu-id="420e6-122">Mac- och Linux: kör **openssl** från en terminal.</span><span class="sxs-lookup"><span data-stu-id="420e6-122">Mac and Linux: run **openssl** from a terminal.</span></span>

3. <span data-ttu-id="420e6-123">Köra s_client - showcerts-ansluta microsoft.com:443</span><span class="sxs-lookup"><span data-stu-id="420e6-123">Execute s_client -showcerts -connect microsoft.com:443</span></span>

4. <span data-ttu-id="420e6-124">Leta efter självsignerade certifikat.</span><span class="sxs-lookup"><span data-stu-id="420e6-124">Look for self-signed certificates.</span></span> <span data-ttu-id="420e6-125">Om du inte vet vilket är självsignerade letar var som helst hello ämne (”s:”) och utfärdaren (”i:”) är hello samma.</span><span class="sxs-lookup"><span data-stu-id="420e6-125">If you are unsure which are self-signed, look for anywhere hello subject ("s:") and issuer ("i:") are hello same.</span></span>

5. <span data-ttu-id="420e6-126">När du har hittat självsignerade certifikat för var och en, kopiera och klistra in allt från och med **---BEGIN CERTIFICATE---** för**---END CERTIFICATE---** tooa nya .cer-fil.</span><span class="sxs-lookup"><span data-stu-id="420e6-126">When you have found any self-signed certificates, for each one, copy and paste everything from and including **-----BEGIN CERTIFICATE-----** too**-----END CERTIFICATE-----** tooa new .cer file.</span></span>

6. <span data-ttu-id="420e6-127">Öppna Lagringsutforskaren, klicka på **redigera** > **SSL-certifikat** > **Importera certifikat**, och sedan använda hello filen Väljaren toofind, Välj, och öppna hello CER-filer som du skapade.</span><span class="sxs-lookup"><span data-stu-id="420e6-127">Open Storage Explorer, click **Edit** > **SSL Certificates** > **Import Certificates**, and then use hello file picker toofind, select, and open hello .cer files that you created.</span></span>

<span data-ttu-id="420e6-128">Om du inte hittar någon självsignerade certifikat med hjälp av hello senare steg kan du kontakta oss via hello feedbackverktyg för mer hjälp.</span><span class="sxs-lookup"><span data-stu-id="420e6-128">If you cannot find any self-signed certificates using hello above steps, contact us through hello feedback tool for more help.</span></span>

### <a name="unable-tooretrieve-subscriptions"></a><span data-ttu-id="420e6-129">Det går inte tooretrieve prenumerationer</span><span class="sxs-lookup"><span data-stu-id="420e6-129">Unable tooretrieve subscriptions</span></span>

<span data-ttu-id="420e6-130">Om du tooretrieve dina prenumerationer när du logga in, följer du dessa steg tootroubleshoot problemet:</span><span class="sxs-lookup"><span data-stu-id="420e6-130">If you are unable tooretrieve your subscriptions after you successfully sign in, follow these steps tootroubleshoot this issue:</span></span>

- <span data-ttu-id="420e6-131">Kontrollera att ditt konto har åtkomst toohello prenumerationer loggar in på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="420e6-131">Verify that your account has access toohello subscriptions by signing into hello Azure Portal.</span></span>

- <span data-ttu-id="420e6-132">Kontrollera att du har loggat in med hjälp av hello rätt miljö (Azure, Azure Kina, Tyskland Azure, Azure som tillhör amerikanska myndigheter eller anpassad miljö-/ Azure-stacken).</span><span class="sxs-lookup"><span data-stu-id="420e6-132">Make sure that you have signed in using hello correct environment (Azure, Azure China, Azure Germany, Azure US Government, or Custom Environment/Azure Stack).</span></span>

- <span data-ttu-id="420e6-133">Om du är bakom en proxyserver kan du kontrollera att du har konfigurerat korrekt hello Lagringsutforskaren proxy.</span><span class="sxs-lookup"><span data-stu-id="420e6-133">If you are behind a proxy, make sure that you have configured hello Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="420e6-134">Försök att ta bort och readding hello-konto.</span><span class="sxs-lookup"><span data-stu-id="420e6-134">Try removing and readding hello account.</span></span>

- <span data-ttu-id="420e6-135">Försök att ta bort hello följande filer från din rotkatalog (det vill säga C:\Users\ContosoUser) och sedan lägga till hello konto:</span><span class="sxs-lookup"><span data-stu-id="420e6-135">Try deleting hello following files from your root directory (that is, C:\Users\ContosoUser), and then re-adding hello account:</span></span>

    - <span data-ttu-id="420e6-136">.adalcache</span><span class="sxs-lookup"><span data-stu-id="420e6-136">.adalcache</span></span>

    - <span data-ttu-id="420e6-137">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="420e6-137">.devaccounts</span></span>

    - <span data-ttu-id="420e6-138">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="420e6-138">.extaccounts</span></span>

- <span data-ttu-id="420e6-139">Titta på hello developer-konsolen (genom att trycka på F12) när du loggar in för eventuella felmeddelanden:</span><span class="sxs-lookup"><span data-stu-id="420e6-139">Watch hello developer tools console (by pressing F12) when you are signing in for any error messages:</span></span>

![Utvecklingsverktyg](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-toosee-hello-authentication-page"></a><span data-ttu-id="420e6-141">Det går inte toosee hello autentiseringssidan</span><span class="sxs-lookup"><span data-stu-id="420e6-141">Unable toosee hello authentication page</span></span>

<span data-ttu-id="420e6-142">Om du toosee hello autentiseringssidan, följer du dessa steg tootroubleshoot problemet:</span><span class="sxs-lookup"><span data-stu-id="420e6-142">If you are unable toosee hello authentication page, follow these steps tootroubleshoot this issue:</span></span>

- <span data-ttu-id="420e6-143">Beroende på anslutningens hello hastighet, kan det ta en stund innan hello-inloggningssida tooload, vänta minst en minut innan du stänger dialogrutan hello.</span><span class="sxs-lookup"><span data-stu-id="420e6-143">Depending on hello speed of your connection, it may take a while for hello sign-in page tooload, wait at least one minute before closing hello authentication dialog box.</span></span>

- <span data-ttu-id="420e6-144">Om du är bakom en proxyserver kan du kontrollera att du har konfigurerat korrekt hello Lagringsutforskaren proxy.</span><span class="sxs-lookup"><span data-stu-id="420e6-144">If you are behind a proxy, make sure that you have configured hello Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="420e6-145">Visa hello developer-konsolen genom att trycka på F12 för hello.</span><span class="sxs-lookup"><span data-stu-id="420e6-145">View hello developer console by pressing hello F12 key.</span></span> <span data-ttu-id="420e6-146">Titta på hello svar från hello developer-konsolen och se om du hittar en ledtråd för varför autentisering fungerar inte.</span><span class="sxs-lookup"><span data-stu-id="420e6-146">Watch hello responses from hello developer console and see whether you can find any clue for why authentication not working.</span></span>

### <a name="cannot-remove-account"></a><span data-ttu-id="420e6-147">Det går inte att ta bort kontot</span><span class="sxs-lookup"><span data-stu-id="420e6-147">Cannot remove account</span></span>

<span data-ttu-id="420e6-148">Om du tooremove ett konto, eller om hello återautentisera länken inte göra något, följer du dessa steg tootroubleshoot problemet:</span><span class="sxs-lookup"><span data-stu-id="420e6-148">If you are unable tooremove an account, or if hello reauthenticate link does not do anything, follow these steps tootroubleshoot this issue:</span></span>

- <span data-ttu-id="420e6-149">Försök att ta bort hello följande filer från din rotkatalog och readding hello konto:</span><span class="sxs-lookup"><span data-stu-id="420e6-149">Try deleting hello following files from your root directory, and then readding hello account:</span></span>

    - <span data-ttu-id="420e6-150">.adalcache</span><span class="sxs-lookup"><span data-stu-id="420e6-150">.adalcache</span></span>

    - <span data-ttu-id="420e6-151">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="420e6-151">.devaccounts</span></span>

    - <span data-ttu-id="420e6-152">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="420e6-152">.extaccounts</span></span>

- <span data-ttu-id="420e6-153">Om du vill tooremove SAS kopplade lagringsresurser, tar du bort hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="420e6-153">If you want tooremove SAS attached Storage resources, delete hello following files:</span></span>

    - <span data-ttu-id="420e6-154">%AppData%/StorageExplorer mapp för Windows</span><span class="sxs-lookup"><span data-stu-id="420e6-154">%AppData%/StorageExplorer folder for Windows</span></span>

    - <span data-ttu-id="420e6-155">/Users/ < ditt_namn >/bibliotek/Flersvalsstart SUpport/StorageExplorer för Mac</span><span class="sxs-lookup"><span data-stu-id="420e6-155">/Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer for Mac</span></span>

    - <span data-ttu-id="420e6-156">~/.config/StorageExplorer för Linux</span><span class="sxs-lookup"><span data-stu-id="420e6-156">~/.config/StorageExplorer for Linux</span></span>

> [!NOTE]
>  <span data-ttu-id="420e6-157">Du måste tooreenter dina autentiseringsuppgifter om du tar bort dessa filer.</span><span class="sxs-lookup"><span data-stu-id="420e6-157">You will have tooreenter all your credentials if you delete these files.</span></span>

## <a name="proxy-issues"></a><span data-ttu-id="420e6-158">Proxy-problem</span><span class="sxs-lookup"><span data-stu-id="420e6-158">Proxy issues</span></span>

<span data-ttu-id="420e6-159">Kontrollera först att hello följande information som du angett är korrekta:</span><span class="sxs-lookup"><span data-stu-id="420e6-159">First, make sure that hello following information you entered are all correct:</span></span>

- <span data-ttu-id="420e6-160">Hej Proxyserver och port tal</span><span class="sxs-lookup"><span data-stu-id="420e6-160">hello proxy URL and port number</span></span>

- <span data-ttu-id="420e6-161">Användarnamn och lösenord om det krävs av hello-proxy</span><span class="sxs-lookup"><span data-stu-id="420e6-161">Username and password if required by hello proxy</span></span>

### <a name="common-solutions"></a><span data-ttu-id="420e6-162">Vanliga lösningar</span><span class="sxs-lookup"><span data-stu-id="420e6-162">Common solutions</span></span>

<span data-ttu-id="420e6-163">Om du fortfarande har problem, följer du dessa steg tootroubleshoot dem:</span><span class="sxs-lookup"><span data-stu-id="420e6-163">If you are still experiencing issues, follow these steps tootroubleshoot them:</span></span>

- <span data-ttu-id="420e6-164">Om du kan ansluta toohello Internet utan att använda proxyservern kan du kontrollera att Lagringsutforskaren fungerar utan aktiverade proxyinställningar.</span><span class="sxs-lookup"><span data-stu-id="420e6-164">If you can connect toohello Internet without using your proxy, verify that Storage Explorer works without proxy settings enabled.</span></span> <span data-ttu-id="420e6-165">Om så är fallet hello kan det finnas ett problem med proxyinställningarna.</span><span class="sxs-lookup"><span data-stu-id="420e6-165">If this is hello case, there may be an issue with your proxy settings.</span></span> <span data-ttu-id="420e6-166">Arbeta med dina proxy administratören tooidentify hello problem.</span><span class="sxs-lookup"><span data-stu-id="420e6-166">Work with your proxy administrator tooidentify hello problems.</span></span>

- <span data-ttu-id="420e6-167">Kontrollera att andra program som använder hello proxyserver fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="420e6-167">Verify that other applications using hello proxy server work as expected.</span></span>

- <span data-ttu-id="420e6-168">Kontrollera att du kan ansluta med hjälp av webbläsaren toohello Microsoft Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="420e6-168">Verify that you can connect toohello Microsoft Azure portal using your web browser</span></span>

- <span data-ttu-id="420e6-169">Kontrollera att du kan få svar från dina Tjänsteslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="420e6-169">Verify that you can receive responses from your service endpoints.</span></span> <span data-ttu-id="420e6-170">Ange en slutpunkt-URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="420e6-170">Enter one of your endpoint URLs into your browser.</span></span> <span data-ttu-id="420e6-171">Om du kan ansluta bör du få ett InvalidQueryParameterValue eller liknande XML-svaret.</span><span class="sxs-lookup"><span data-stu-id="420e6-171">If you can connect, you should receive an InvalidQueryParameterValue or similar XML response.</span></span>

- <span data-ttu-id="420e6-172">Om någon annan också använder Lagringsutforskaren med proxyservern, kontrollera att de kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="420e6-172">If someone else is also using Storage Explorer with your proxy server, verify that they can connect.</span></span> <span data-ttu-id="420e6-173">Om de kan ansluta kanske toocontact proxy server administratören.</span><span class="sxs-lookup"><span data-stu-id="420e6-173">If they can connect, you may have toocontact your proxy server admin.</span></span>

### <a name="tools-for-diagnosing-issues"></a><span data-ttu-id="420e6-174">Verktyg för att diagnostisera problem</span><span class="sxs-lookup"><span data-stu-id="420e6-174">Tools for diagnosing issues</span></span>

<span data-ttu-id="420e6-175">Om du har nätverk verktyg, till exempel Fiddler för Windows kan kanske du kan toodiagnose hello problem på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="420e6-175">If you have networking tools, such as Fiddler for Windows, you may be able toodiagnose hello problems as follows:</span></span>

- <span data-ttu-id="420e6-176">Om du har toowork via din proxyserver kan kanske du tooconfigure ditt nätverk verktyget tooconnect via hello proxy.</span><span class="sxs-lookup"><span data-stu-id="420e6-176">If you have toowork through your proxy, you may have tooconfigure your networking tool tooconnect through hello proxy.</span></span>

- <span data-ttu-id="420e6-177">Kontrollera hello-portnumret som används av ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="420e6-177">Check hello port number used by your networking tool.</span></span>

- <span data-ttu-id="420e6-178">Ange hello lokala Värdadressen och hello nätverk verktygets portnummer som proxyinställningarna i Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="420e6-178">Enter hello local host URL and hello networking tool's port number as proxy settings in Storage Explorer.</span></span> <span data-ttu-id="420e6-179">Om den här isdone korrekt, ditt nätverk verktyget börjar loggning nätverksbegäranden med Lagringsutforskaren toomanagement och tjänsten slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="420e6-179">If this isdone correctly, your networking tool starts logging network requests made by Storage Explorer toomanagement and service endpoints.</span></span> <span data-ttu-id="420e6-180">Ange till exempel https://cawablobgrs.blob.core.windows.net/ för blob-slutpunkten i en webbläsare och du får ett svar liknar hello följande, vilket tyder på hello resursen finns, även om du inte kommer åt den.</span><span class="sxs-lookup"><span data-stu-id="420e6-180">For example, enter https://cawablobgrs.blob.core.windows.net/ for your blob endpoint in a browser, and you will receive a response resembles hello following, which suggests hello resource exists, although you cannot access it.</span></span>

![kodexempel](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a><span data-ttu-id="420e6-182">Kontakta serveradministratören för proxy</span><span class="sxs-lookup"><span data-stu-id="420e6-182">Contact proxy server admin</span></span>

<span data-ttu-id="420e6-183">Om proxyinställningarna är rätt måste du kanske toocontact administratören proxy-server och</span><span class="sxs-lookup"><span data-stu-id="420e6-183">If your proxy settings are correct, you may have toocontact your proxy server admin, and</span></span>

- <span data-ttu-id="420e6-184">Se till att proxyservern inte blockerar trafik tooAzure hanterings- eller slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="420e6-184">Make sure that your proxy does not block traffic tooAzure management or resource endpoints.</span></span>

- <span data-ttu-id="420e6-185">Kontrollera hello autentiseringsprotokoll som används av proxyservern.</span><span class="sxs-lookup"><span data-stu-id="420e6-185">Verify hello authentication protocol used by your proxy server.</span></span> <span data-ttu-id="420e6-186">Lagringsutforskaren stöder för närvarande inte NTLM-proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="420e6-186">Storage Explorer does not currently support NTLM proxies.</span></span>

## <a name="unable-tooretrieve-children-error-message"></a><span data-ttu-id="420e6-187">Felmeddelandet ”Det gick inte tooRetrieve underordnade”</span><span class="sxs-lookup"><span data-stu-id="420e6-187">"Unable tooRetrieve Children" error message</span></span>

<span data-ttu-id="420e6-188">Om du är ansluten tooAzure via en proxyserver kan du verifiera att proxyinställningarna är korrekta.</span><span class="sxs-lookup"><span data-stu-id="420e6-188">If you are connected tooAzure through a proxy, verify that your proxy settings are correct.</span></span> <span data-ttu-id="420e6-189">Om du har beviljat åtkomst tooa resurs från hello ägare hello prenumeration eller konto, kontrollera att du har läst eller visa en lista med behörigheter för resursen.</span><span class="sxs-lookup"><span data-stu-id="420e6-189">If you were granted access tooa resource from hello owner of hello subscription or account, verify that you have read or list permissions for that resource.</span></span>

### <a name="issues-with-sas-url"></a><span data-ttu-id="420e6-190">Problem med SAS-URL</span><span class="sxs-lookup"><span data-stu-id="420e6-190">Issues with SAS URL</span></span>
<span data-ttu-id="420e6-191">Om du ansluter tooa tjänsten med hjälp av en SAS-URL och det här felet:</span><span class="sxs-lookup"><span data-stu-id="420e6-191">If you are connecting tooa service using a SAS URL and experiencing this error:</span></span>

- <span data-ttu-id="420e6-192">Kontrollera att hello URL innehåller hello behörighet tooread eller lista resurser.</span><span class="sxs-lookup"><span data-stu-id="420e6-192">Verify that hello URL provides hello necessary permissions tooread or list resources.</span></span>

- <span data-ttu-id="420e6-193">Kontrollera att hello URL inte har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="420e6-193">Verify that hello URL has not expired.</span></span>

- <span data-ttu-id="420e6-194">Om hello SAS-URL är baserat på en åtkomstprincip, kontrollerar du att hello åtkomstprincip inte har återkallats.</span><span class="sxs-lookup"><span data-stu-id="420e6-194">If hello SAS URL is based on an access policy, verify that hello access policy has not been revoked.</span></span>

## <a name="next-steps"></a><span data-ttu-id="420e6-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="420e6-195">Next steps</span></span>

<span data-ttu-id="420e6-196">Om ingen av hello lösningar fungerar för dig skicka din via hello feedbackverktyg med din e-post och så många detaljer om hello problem som du kan, så att vi kan kontakta dig för att åtgärda hello problem.</span><span class="sxs-lookup"><span data-stu-id="420e6-196">If none of hello solutions work for you, submit your issue through hello feedback tool with your email and as many details about hello issue included as you can, so that we can contact you for fixing hello issue.</span></span>

<span data-ttu-id="420e6-197">toodo, genom att välja **hjälp** -menyn och klicka sedan på **skicka Feedback**.</span><span class="sxs-lookup"><span data-stu-id="420e6-197">toodo this, click **Help** menu, and then click **Send Feedback**.</span></span>

![Feedback](./media/storage-explorer-troubleshooting/4022503_en_1.png)
