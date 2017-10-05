---
title: "Azure Lagringsutforskaren felsökningsguiden | Microsoft Docs"
description: "Översikt över de två felsökning funktion i Azure"
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
ms.openlocfilehash: 470b2d87ffdc4769bb2963df7dea646901469e00
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a><span data-ttu-id="4e761-103">Azure Lagringsutforskaren felsökningsguiden</span><span class="sxs-lookup"><span data-stu-id="4e761-103">Azure Storage Explorer troubleshooting guide</span></span>

## <a name="introduction"></a><span data-ttu-id="4e761-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="4e761-104">Introduction</span></span>

<span data-ttu-id="4e761-105">Microsoft Azure Lagringsutforskaren (förhandsversion) är en fristående app som gör det enkelt att arbeta med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="4e761-105">Microsoft Azure Storage Explorer (Preview) is a stand-alone app that enables you to easily work with Azure Storage data on Windows, macOS and Linux.</span></span> <span data-ttu-id="4e761-106">Appen kan ansluta toStorage konton finns i Azure, statliga moln och Azure-stacken.</span><span class="sxs-lookup"><span data-stu-id="4e761-106">The app can connect toStorage accounts hosted on Azure, Sovereign Clouds, and Azure Stack.</span></span>

<span data-ttu-id="4e761-107">Den här guiden beskrivs lösningar på vanliga problem som visas i Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="4e761-107">This guide summarizes solutions for common issues seen in Storage Explorer.</span></span>

## <a name="sign-in-issues"></a><span data-ttu-id="4e761-108">Logga in problem</span><span class="sxs-lookup"><span data-stu-id="4e761-108">Sign in issues</span></span>

<span data-ttu-id="4e761-109">Innan du fortsätter försök att starta om programmet och se om problemen kan åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="4e761-109">Before you continue, try restarting your application and see whether the problems can be fixed.</span></span>

### <a name="error-self-signed-certificate-in-certificate-chain"></a><span data-ttu-id="4e761-110">Fel: Självsignerat certifikat i certifikatkedjan</span><span class="sxs-lookup"><span data-stu-id="4e761-110">Error: Self-Signed Certificate in Certificate Chain</span></span>

<span data-ttu-id="4e761-111">Det finns flera skäl till varför detta fel kan uppstå och de två vanligaste orsakerna är följande:</span><span class="sxs-lookup"><span data-stu-id="4e761-111">There are several reasons why you may encounter this error, and the most common two reasons are as follows:</span></span>

1. <span data-ttu-id="4e761-112">Appen är anslutna via en ”transparent proxy”, vilket innebär att en server (till exempel företagets servern) avlyssna HTTPS-trafik, att dekryptera den och kryptera den med hjälp av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="4e761-112">The app is connected through a “transparent proxy”, which means a server (such as your company server) is intercepting HTTPS traffic, decrypting it, and then encrypting it using a self-signed certificate.</span></span>

2. <span data-ttu-id="4e761-113">Du kör ett program, till exempel antivirusprogram, vilket är att injicera en självsignerat SSL-certifikatet till HTTPS-meddelanden som visas.</span><span class="sxs-lookup"><span data-stu-id="4e761-113">You are running an application, such as antivirus software, which is injecting a self-signed SSL certificate into the HTTPS messages that you receive.</span></span>

<span data-ttu-id="4e761-114">När Lagringsutforskaren påträffar ett problem, kan den inte längre vet om det mottagna meddelandet HTTPS har ändrats.</span><span class="sxs-lookup"><span data-stu-id="4e761-114">When Storage Explorer encounters one of the issues, it can no longer know whether the received HTTPS message is tampered.</span></span> <span data-ttu-id="4e761-115">Om du har en kopia av det självsignerade certifikatet, kan du låta Lagringsutforskaren litar på den.</span><span class="sxs-lookup"><span data-stu-id="4e761-115">If you have a copy of the self-signed certificate, you can let Storage Explorer trust it.</span></span> <span data-ttu-id="4e761-116">Om du är osäker på som är att injicera certifikatet, Följ dessa steg för att hitta den:</span><span class="sxs-lookup"><span data-stu-id="4e761-116">If you are unsure of who is injecting the certificate, follow these steps to find it:</span></span>

1. <span data-ttu-id="4e761-117">Installera öppna SSL</span><span class="sxs-lookup"><span data-stu-id="4e761-117">Install Open SSL</span></span>

    - <span data-ttu-id="4e761-118">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (någon av de lätta versionerna bör vara tillräckligt)</span><span class="sxs-lookup"><span data-stu-id="4e761-118">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (any of the light versions should be sufficient)</span></span>

    - <span data-ttu-id="4e761-119">Mac- och Linux: ska ingå i ditt operativsystem</span><span class="sxs-lookup"><span data-stu-id="4e761-119">Mac and Linux: should be included with your operating system</span></span>

2. <span data-ttu-id="4e761-120">Kör öppna SSL</span><span class="sxs-lookup"><span data-stu-id="4e761-120">Run Open SSL</span></span>

    - <span data-ttu-id="4e761-121">Windows: öppna installationskatalogen, klicka på **/bin/**, och dubbelklicka sedan på **openssl.exe**.</span><span class="sxs-lookup"><span data-stu-id="4e761-121">Windows: open the installation directory, click **/bin/**, and then double-click **openssl.exe**.</span></span>
    - <span data-ttu-id="4e761-122">Mac- och Linux: kör **openssl** från en terminal.</span><span class="sxs-lookup"><span data-stu-id="4e761-122">Mac and Linux: run **openssl** from a terminal.</span></span>

3. <span data-ttu-id="4e761-123">Köra s_client - showcerts-ansluta microsoft.com:443</span><span class="sxs-lookup"><span data-stu-id="4e761-123">Execute s_client -showcerts -connect microsoft.com:443</span></span>

4. <span data-ttu-id="4e761-124">Leta efter självsignerade certifikat.</span><span class="sxs-lookup"><span data-stu-id="4e761-124">Look for self-signed certificates.</span></span> <span data-ttu-id="4e761-125">Om du inte vet vilket självsignerat leta efter överallt ämne (”s:”) och utfärdare (”i:”) är samma.</span><span class="sxs-lookup"><span data-stu-id="4e761-125">If you are unsure which are self-signed, look for anywhere the subject ("s:") and issuer ("i:") are the same.</span></span>

5. <span data-ttu-id="4e761-126">När du har hittat självsignerade certifikat för var och en, kopiera och klistra in allt från och med **---BEGIN CERTIFICATE---** till **---END CERTIFICATE---** till en ny .cer-fil.</span><span class="sxs-lookup"><span data-stu-id="4e761-126">When you have found any self-signed certificates, for each one, copy and paste everything from and including **-----BEGIN CERTIFICATE-----** to **-----END CERTIFICATE-----** to a new .cer file.</span></span>

6. <span data-ttu-id="4e761-127">Öppna Lagringsutforskaren, klicka på **redigera** > **SSL-certifikat** > **Importera certifikat**, och sedan använda filväljaren för att söka efter och välj Öppna CER-filen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="4e761-127">Open Storage Explorer, click **Edit** > **SSL Certificates** > **Import Certificates**, and then use the file picker to find, select, and open the .cer files that you created.</span></span>

<span data-ttu-id="4e761-128">Om du inte hittar någon självsignerade certifikat med hjälp av stegen ovan kan du kontakta oss genom verktyget feedback för mer hjälp.</span><span class="sxs-lookup"><span data-stu-id="4e761-128">If you cannot find any self-signed certificates using the above steps, contact us through the feedback tool for more help.</span></span>

### <a name="unable-to-retrieve-subscriptions"></a><span data-ttu-id="4e761-129">Det gick inte att hämta prenumerationer</span><span class="sxs-lookup"><span data-stu-id="4e761-129">Unable to retrieve subscriptions</span></span>

<span data-ttu-id="4e761-130">Om det inte går att hämta dina prenumerationer när du har loggat in, Följ dessa steg för att felsöka problemet:</span><span class="sxs-lookup"><span data-stu-id="4e761-130">If you are unable to retrieve your subscriptions after you successfully sign in, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="4e761-131">Kontrollera att ditt konto har åtkomst till prenumerationerna genom att logga in på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="4e761-131">Verify that your account has access to the subscriptions by signing into the Azure Portal.</span></span>

- <span data-ttu-id="4e761-132">Kontrollera att du har loggat in med rätt miljön (Azure, Azure Kina, Tyskland Azure, Azure som tillhör amerikanska myndigheter eller anpassad miljö-/ Azure-stacken).</span><span class="sxs-lookup"><span data-stu-id="4e761-132">Make sure that you have signed in using the correct environment (Azure, Azure China, Azure Germany, Azure US Government, or Custom Environment/Azure Stack).</span></span>

- <span data-ttu-id="4e761-133">Om du är bakom en proxyserver, se till att du har konfigurerat korrekt Lagringsutforskaren proxy.</span><span class="sxs-lookup"><span data-stu-id="4e761-133">If you are behind a proxy, make sure that you have configured the Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="4e761-134">Försök att ta bort och readding kontot.</span><span class="sxs-lookup"><span data-stu-id="4e761-134">Try removing and readding the account.</span></span>

- <span data-ttu-id="4e761-135">Försök ta bort följande filer från rotkatalogen (det vill säga C:\Users\ContosoUser) och sedan lägga till kontot igen:</span><span class="sxs-lookup"><span data-stu-id="4e761-135">Try deleting the following files from your root directory (that is, C:\Users\ContosoUser), and then re-adding the account:</span></span>

    - <span data-ttu-id="4e761-136">.adalcache</span><span class="sxs-lookup"><span data-stu-id="4e761-136">.adalcache</span></span>

    - <span data-ttu-id="4e761-137">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="4e761-137">.devaccounts</span></span>

    - <span data-ttu-id="4e761-138">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="4e761-138">.extaccounts</span></span>

- <span data-ttu-id="4e761-139">Titta på utvecklingsverktygen konsolen (genom att trycka på F12) när du loggar in för eventuella felmeddelanden:</span><span class="sxs-lookup"><span data-stu-id="4e761-139">Watch the developer tools console (by pressing F12) when you are signing in for any error messages:</span></span>

![Utvecklingsverktyg](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-to-see-the-authentication-page"></a><span data-ttu-id="4e761-141">Det gick inte att visas på autentiseringssidan</span><span class="sxs-lookup"><span data-stu-id="4e761-141">Unable to see the authentication page</span></span>

<span data-ttu-id="4e761-142">Om det inte går att visa autentiseringssidan, Följ dessa steg för att felsöka problemet:</span><span class="sxs-lookup"><span data-stu-id="4e761-142">If you are unable to see the authentication page, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="4e761-143">Beroende på anslutningen, kan det ta en stund för inloggningssidan om du vill läsa in vänta minst en minut innan du stänger dialogrutan autentisering.</span><span class="sxs-lookup"><span data-stu-id="4e761-143">Depending on the speed of your connection, it may take a while for the sign-in page to load, wait at least one minute before closing the authentication dialog box.</span></span>

- <span data-ttu-id="4e761-144">Om du är bakom en proxyserver, se till att du har konfigurerat korrekt Lagringsutforskaren proxy.</span><span class="sxs-lookup"><span data-stu-id="4e761-144">If you are behind a proxy, make sure that you have configured the Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="4e761-145">Visa developer-konsolen genom att trycka på F12.</span><span class="sxs-lookup"><span data-stu-id="4e761-145">View the developer console by pressing the F12 key.</span></span> <span data-ttu-id="4e761-146">Titta på svar från developer-konsolen och se om du hittar en ledtråd för varför autentisering fungerar inte.</span><span class="sxs-lookup"><span data-stu-id="4e761-146">Watch the responses from the developer console and see whether you can find any clue for why authentication not working.</span></span>

### <a name="cannot-remove-account"></a><span data-ttu-id="4e761-147">Det går inte att ta bort kontot</span><span class="sxs-lookup"><span data-stu-id="4e761-147">Cannot remove account</span></span>

<span data-ttu-id="4e761-148">Om det inte går att ta bort ett konto, eller om länken återautentisera inte göra något, Följ dessa steg för att felsöka problemet:</span><span class="sxs-lookup"><span data-stu-id="4e761-148">If you are unable to remove an account, or if the reauthenticate link does not do anything, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="4e761-149">Försök ta bort följande filer från rotkatalogen och readding kontot:</span><span class="sxs-lookup"><span data-stu-id="4e761-149">Try deleting the following files from your root directory, and then readding the account:</span></span>

    - <span data-ttu-id="4e761-150">.adalcache</span><span class="sxs-lookup"><span data-stu-id="4e761-150">.adalcache</span></span>

    - <span data-ttu-id="4e761-151">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="4e761-151">.devaccounts</span></span>

    - <span data-ttu-id="4e761-152">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="4e761-152">.extaccounts</span></span>

- <span data-ttu-id="4e761-153">Om du vill ta bort SAS kopplade lagringsresurser, ta bort följande filer:</span><span class="sxs-lookup"><span data-stu-id="4e761-153">If you want to remove SAS attached Storage resources, delete the following files:</span></span>

    - <span data-ttu-id="4e761-154">%AppData%/StorageExplorer mapp för Windows</span><span class="sxs-lookup"><span data-stu-id="4e761-154">%AppData%/StorageExplorer folder for Windows</span></span>

    - <span data-ttu-id="4e761-155">/Users/ < ditt_namn >/bibliotek/Flersvalsstart SUpport/StorageExplorer för Mac</span><span class="sxs-lookup"><span data-stu-id="4e761-155">/Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer for Mac</span></span>

    - <span data-ttu-id="4e761-156">~/.config/StorageExplorer för Linux</span><span class="sxs-lookup"><span data-stu-id="4e761-156">~/.config/StorageExplorer for Linux</span></span>

> [!NOTE]
>  <span data-ttu-id="4e761-157">Du måste ange dina autentiseringsuppgifter igen om du tar bort dessa filer.</span><span class="sxs-lookup"><span data-stu-id="4e761-157">You will have to reenter all your credentials if you delete these files.</span></span>

## <a name="proxy-issues"></a><span data-ttu-id="4e761-158">Proxy-problem</span><span class="sxs-lookup"><span data-stu-id="4e761-158">Proxy issues</span></span>

<span data-ttu-id="4e761-159">Kontrollera först att följande information som du angett är korrekta:</span><span class="sxs-lookup"><span data-stu-id="4e761-159">First, make sure that the following information you entered are all correct:</span></span>

- <span data-ttu-id="4e761-160">Proxy-URL och portnummer</span><span class="sxs-lookup"><span data-stu-id="4e761-160">The proxy URL and port number</span></span>

- <span data-ttu-id="4e761-161">Användarnamn och lösenord om det krävs av proxy</span><span class="sxs-lookup"><span data-stu-id="4e761-161">Username and password if required by the proxy</span></span>

### <a name="common-solutions"></a><span data-ttu-id="4e761-162">Vanliga lösningar</span><span class="sxs-lookup"><span data-stu-id="4e761-162">Common solutions</span></span>

<span data-ttu-id="4e761-163">Följ stegen nedan för att felsöka dem. Om du fortfarande har problem:</span><span class="sxs-lookup"><span data-stu-id="4e761-163">If you are still experiencing issues, follow these steps to troubleshoot them:</span></span>

- <span data-ttu-id="4e761-164">Om du kan ansluta till Internet utan att använda proxyservern kan du kontrollera att Lagringsutforskaren fungerar utan aktiverade proxyinställningar.</span><span class="sxs-lookup"><span data-stu-id="4e761-164">If you can connect to the Internet without using your proxy, verify that Storage Explorer works without proxy settings enabled.</span></span> <span data-ttu-id="4e761-165">Om så är fallet kan det vara ett problem med proxyinställningarna.</span><span class="sxs-lookup"><span data-stu-id="4e761-165">If this is the case, there may be an issue with your proxy settings.</span></span> <span data-ttu-id="4e761-166">Arbeta med din proxy-administratör att identifiera problem.</span><span class="sxs-lookup"><span data-stu-id="4e761-166">Work with your proxy administrator to identify the problems.</span></span>

- <span data-ttu-id="4e761-167">Kontrollera att andra program med hjälp av proxyservern fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="4e761-167">Verify that other applications using the proxy server work as expected.</span></span>

- <span data-ttu-id="4e761-168">Kontrollera att du kan ansluta till Microsoft Azure-portalen med hjälp av webbläsaren</span><span class="sxs-lookup"><span data-stu-id="4e761-168">Verify that you can connect to the Microsoft Azure portal using your web browser</span></span>

- <span data-ttu-id="4e761-169">Kontrollera att du kan få svar från dina Tjänsteslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="4e761-169">Verify that you can receive responses from your service endpoints.</span></span> <span data-ttu-id="4e761-170">Ange en slutpunkt-URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="4e761-170">Enter one of your endpoint URLs into your browser.</span></span> <span data-ttu-id="4e761-171">Om du kan ansluta bör du få ett InvalidQueryParameterValue eller liknande XML-svaret.</span><span class="sxs-lookup"><span data-stu-id="4e761-171">If you can connect, you should receive an InvalidQueryParameterValue or similar XML response.</span></span>

- <span data-ttu-id="4e761-172">Om någon annan också använder Lagringsutforskaren med proxyservern, kontrollera att de kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="4e761-172">If someone else is also using Storage Explorer with your proxy server, verify that they can connect.</span></span> <span data-ttu-id="4e761-173">Om de kan ansluta kan du behöva kontakta din proxy server-administratör.</span><span class="sxs-lookup"><span data-stu-id="4e761-173">If they can connect, you may have to contact your proxy server admin.</span></span>

### <a name="tools-for-diagnosing-issues"></a><span data-ttu-id="4e761-174">Verktyg för att diagnostisera problem</span><span class="sxs-lookup"><span data-stu-id="4e761-174">Tools for diagnosing issues</span></span>

<span data-ttu-id="4e761-175">Om du har nätverk verktyg, till exempel Fiddler för Windows kan du diagnostisera felet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4e761-175">If you have networking tools, such as Fiddler for Windows, you may be able to diagnose the problems as follows:</span></span>

- <span data-ttu-id="4e761-176">Om du behöver gå igenom din proxyserver måste du kanske konfigurera nätverk verktyget för att ansluta till proxyservern.</span><span class="sxs-lookup"><span data-stu-id="4e761-176">If you have to work through your proxy, you may have to configure your networking tool to connect through the proxy.</span></span>

- <span data-ttu-id="4e761-177">Kontrollera portnumret som används av ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="4e761-177">Check the port number used by your networking tool.</span></span>

- <span data-ttu-id="4e761-178">Ange den lokala värd-URL och portnummer för verktyget nätverk som proxyinställningarna i Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="4e761-178">Enter the local host URL and the networking tool's port number as proxy settings in Storage Explorer.</span></span> <span data-ttu-id="4e761-179">Om den här isdone korrekt, ditt nätverk verktyget börjar loggning nätverksförfrågningar som görs av Lagringsutforskaren till hanterings- och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="4e761-179">If this isdone correctly, your networking tool starts logging network requests made by Storage Explorer to management and service endpoints.</span></span> <span data-ttu-id="4e761-180">Ange till exempel https://cawablobgrs.blob.core.windows.net/ för blob-slutpunkten i en webbläsare och du får ett svar som liknar följande, vilket tyder på resursen finns, även om du inte kommer åt den.</span><span class="sxs-lookup"><span data-stu-id="4e761-180">For example, enter https://cawablobgrs.blob.core.windows.net/ for your blob endpoint in a browser, and you will receive a response resembles the following, which suggests the resource exists, although you cannot access it.</span></span>

![kodexempel](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a><span data-ttu-id="4e761-182">Kontakta serveradministratören för proxy</span><span class="sxs-lookup"><span data-stu-id="4e761-182">Contact proxy server admin</span></span>

<span data-ttu-id="4e761-183">Om proxyinställningarna är korrekta, du kan behöva kontakta administratören proxy-server och</span><span class="sxs-lookup"><span data-stu-id="4e761-183">If your proxy settings are correct, you may have to contact your proxy server admin, and</span></span>

- <span data-ttu-id="4e761-184">Se till att proxyservern inte blockerar trafik till Azure hanterings- eller resurs-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="4e761-184">Make sure that your proxy does not block traffic to Azure management or resource endpoints.</span></span>

- <span data-ttu-id="4e761-185">Kontrollera autentiseringsprotokollet som används av proxyservern.</span><span class="sxs-lookup"><span data-stu-id="4e761-185">Verify the authentication protocol used by your proxy server.</span></span> <span data-ttu-id="4e761-186">Lagringsutforskaren stöder för närvarande inte NTLM-proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="4e761-186">Storage Explorer does not currently support NTLM proxies.</span></span>

## <a name="unable-to-retrieve-children-error-message"></a><span data-ttu-id="4e761-187">”Det gick inte att hämta underordnade” felmeddelande</span><span class="sxs-lookup"><span data-stu-id="4e761-187">"Unable to Retrieve Children" error message</span></span>

<span data-ttu-id="4e761-188">Om du är ansluten till Azure via en proxyserver, kontrollerar du att proxyinställningarna är korrekta.</span><span class="sxs-lookup"><span data-stu-id="4e761-188">If you are connected to Azure through a proxy, verify that your proxy settings are correct.</span></span> <span data-ttu-id="4e761-189">Om du har beviljats åtkomst till en resurs från ägaren av prenumerationen eller konto, kontrollera att du har läst eller visa en lista med behörigheter för resursen.</span><span class="sxs-lookup"><span data-stu-id="4e761-189">If you were granted access to a resource from the owner of the subscription or account, verify that you have read or list permissions for that resource.</span></span>

### <a name="issues-with-sas-url"></a><span data-ttu-id="4e761-190">Problem med SAS-URL</span><span class="sxs-lookup"><span data-stu-id="4e761-190">Issues with SAS URL</span></span>
<span data-ttu-id="4e761-191">Om du ansluter till en tjänst med hjälp av en SAS-URL och det här felet:</span><span class="sxs-lookup"><span data-stu-id="4e761-191">If you are connecting to a service using a SAS URL and experiencing this error:</span></span>

- <span data-ttu-id="4e761-192">Kontrollera att URL: en ger tillräcklig behörighet för att läsa eller visa en lista med resurser.</span><span class="sxs-lookup"><span data-stu-id="4e761-192">Verify that the URL provides the necessary permissions to read or list resources.</span></span>

- <span data-ttu-id="4e761-193">Kontrollera att URL: en inte har gått ut.</span><span class="sxs-lookup"><span data-stu-id="4e761-193">Verify that the URL has not expired.</span></span>

- <span data-ttu-id="4e761-194">Om SAS-URL är baserat på en åtkomstprincip, kontrollerar du att åtkomstprincipen inte har återkallats.</span><span class="sxs-lookup"><span data-stu-id="4e761-194">If the SAS URL is based on an access policy, verify that the access policy has not been revoked.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e761-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4e761-195">Next steps</span></span>

<span data-ttu-id="4e761-196">Om ingen av lösningarna som fungerar för dig skicka din via verktyget feedback med din e-post och så många detaljer om problem som du kan, så att vi kan kontakta dig om hur du löser problemet.</span><span class="sxs-lookup"><span data-stu-id="4e761-196">If none of the solutions work for you, submit your issue through the feedback tool with your email and as many details about the issue included as you can, so that we can contact you for fixing the issue.</span></span>

<span data-ttu-id="4e761-197">Gör detta genom att klicka på **hjälp** -menyn och klicka sedan på **skicka Feedback**.</span><span class="sxs-lookup"><span data-stu-id="4e761-197">To do this, click **Help** menu, and then click **Send Feedback**.</span></span>

![Feedback](./media/storage-explorer-troubleshooting/4022503_en_1.png)
