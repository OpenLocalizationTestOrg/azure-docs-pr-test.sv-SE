---
title: "Rubrik-baserad autentisering med PingAccess för Azure AD Application Proxy | Microsoft Docs"
description: "Publicera program med PingAccess och App-Proxy som stöder huvud-baserad autentisering."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 58034ab8830cf655199875b448948ea14dc04a70
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a><span data-ttu-id="1851c-103">Rubrik-baserad autentisering för enkel inloggning med Application Proxy och PingAccess</span><span class="sxs-lookup"><span data-stu-id="1851c-103">Header-based authentication for single sign-on with Application Proxy and PingAccess</span></span>

<span data-ttu-id="1851c-104">Azure Active Directory Application Proxy och PingAccess samarbetar tillsammans för att ge åtkomst till fler program med Azure Active Directory-kunder.</span><span class="sxs-lookup"><span data-stu-id="1851c-104">Azure Active Directory Application Proxy and PingAccess have partnered together to provide Azure Active Directory customers with access to even more applications.</span></span> <span data-ttu-id="1851c-105">PingAccess expanderar den [befintliga Application Proxy-erbjudanden](active-directory-application-proxy-get-started.md) med enkel inloggning åtkomst till program som använder huvuden för autentisering.</span><span class="sxs-lookup"><span data-stu-id="1851c-105">PingAccess expands the [existing Application Proxy offerings](active-directory-application-proxy-get-started.md) to include single sign-on access to applications that use headers for authentication.</span></span>

## <a name="what-is-pingaccess-for-azure-ad"></a><span data-ttu-id="1851c-106">Vad är PingAccess för Azure AD?</span><span class="sxs-lookup"><span data-stu-id="1851c-106">What is PingAccess for Azure AD?</span></span>

<span data-ttu-id="1851c-107">PingAccess för Azure Active Directory är en uppsättning PingAccess som gör att du kan ge användare åtkomst till och enkel inloggning till program som använder huvuden för autentisering.</span><span class="sxs-lookup"><span data-stu-id="1851c-107">PingAccess for Azure Active Directory is an offering of PingAccess that enables you to give users access and single sign-on to applications that use headers for authentication.</span></span> <span data-ttu-id="1851c-108">Programmet Proxy behandlar apparna som någon annan, med hjälp av Azure AD för att autentisera åtkomst och skicka trafik via kopplingstjänsten.</span><span class="sxs-lookup"><span data-stu-id="1851c-108">Application Proxy treats these apps like any other, using Azure AD to authenticate access and then passing traffic through the connector service.</span></span> <span data-ttu-id="1851c-109">PingAccess placeras framför apparna och översätter åtkomst-token från Azure AD till ett sidhuvud så att programmet tar emot autentiseringen i det format som den kan läsa.</span><span class="sxs-lookup"><span data-stu-id="1851c-109">PingAccess sits in front of the apps and translates the access token from Azure AD into a header so that the application receives the authentication in the format it can read.</span></span>

<span data-ttu-id="1851c-110">Användarna kommer inte se något annat när de loggar in att använda din företags-appar.</span><span class="sxs-lookup"><span data-stu-id="1851c-110">Your users won’t notice anything different when they sign in to use your corporate apps.</span></span> <span data-ttu-id="1851c-111">De kan fortsätta att arbeta från var som helst på vilken enhet som helst.</span><span class="sxs-lookup"><span data-stu-id="1851c-111">They can still work from anywhere on any device.</span></span> 

<span data-ttu-id="1851c-112">Eftersom Application Proxy-kopplingar dirigera remote trafik till alla appar oavsett deras autentiseringstyp, kommer de fortsätta att belastningsutjämna automatiskt, samt.</span><span class="sxs-lookup"><span data-stu-id="1851c-112">Since the Application Proxy connectors direct remote traffic to all apps regardless of their authentication type, they’ll continue to load balance automatically, as well.</span></span>

## <a name="how-do-i-get-access"></a><span data-ttu-id="1851c-113">Hur skaffar åtkomst?</span><span class="sxs-lookup"><span data-stu-id="1851c-113">How do I get access?</span></span>

<span data-ttu-id="1851c-114">Eftersom det här scenariot erbjuds via ett partnerskap mellan Azure Active Directory och PingAccess, behöver du licenser för båda tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="1851c-114">Since this scenario is offered through a partnership between Azure Active Directory and PingAccess, you need licenses for both services.</span></span> <span data-ttu-id="1851c-115">Azure Active Directory Premium prenumerationer innehåller emellertid en grundläggande PingAccess licens som täcker upp till 20 program.</span><span class="sxs-lookup"><span data-stu-id="1851c-115">However, Azure Active Directory Premium subscriptions include a basic PingAccess license that covers up to 20 applications.</span></span> <span data-ttu-id="1851c-116">Om du vill publicera fler än 20 huvud-baserade program kan du köpa ytterligare en licens från PingAccess.</span><span class="sxs-lookup"><span data-stu-id="1851c-116">If you need to publish more than 20 header-based applications, you can purchase an additional license from PingAccess.</span></span> 

<span data-ttu-id="1851c-117">Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="1851c-117">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="publish-your-application-in-azure"></a><span data-ttu-id="1851c-118">Publicera programmet i Azure</span><span class="sxs-lookup"><span data-stu-id="1851c-118">Publish your application in Azure</span></span>

<span data-ttu-id="1851c-119">Den här artikeln är avsedd för personer som publicerar en app med det här scenariot för första gången.</span><span class="sxs-lookup"><span data-stu-id="1851c-119">This article is intended for people who are publishing an app with this scenario for the first time.</span></span> <span data-ttu-id="1851c-120">Det går igenom hur du kommer igång med både program- och PingAccess, utöver publicering stegen.</span><span class="sxs-lookup"><span data-stu-id="1851c-120">It walks through how to get started with both Application and PingAccess, in addition to the publishing steps.</span></span> <span data-ttu-id="1851c-121">Om du redan har konfigurerat båda tjänsterna men vill en uppdaterare för publishing steg du kan hoppa över kopplingsinstallationen av och gå vidare till [lägga till din app i Azure AD med Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).</span><span class="sxs-lookup"><span data-stu-id="1851c-121">If you’ve already configured both services but want a refresher on the publishing steps, you can skip the connector installation and move on to [Add your app to Azure AD with Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).</span></span>

>[!NOTE]
><span data-ttu-id="1851c-122">Eftersom det här scenariot är en koppling mellan Azure AD och PingAccess, vissa anvisningarna finns på webbplatsen Ping identitet.</span><span class="sxs-lookup"><span data-stu-id="1851c-122">Since this scenario is a partnership between Azure AD and PingAccess, some of the instructions exist on the Ping Identity site.</span></span>

### <a name="install-an-application-proxy-connector"></a><span data-ttu-id="1851c-123">Installera en Application Proxy connector</span><span class="sxs-lookup"><span data-stu-id="1851c-123">Install an Application Proxy connector</span></span>

<span data-ttu-id="1851c-124">Om du redan har Application Proxy är aktiverat och har en koppling installeras, kan du hoppa över det här avsnittet och gå vidare till [lägga till din app i Azure AD med Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).</span><span class="sxs-lookup"><span data-stu-id="1851c-124">If you already have Application Proxy enabled, and have a connector installed, you can skip this section and move on to [Add your app to Azure AD with Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).</span></span>

<span data-ttu-id="1851c-125">Application Proxy connector är en Windows Server-tjänst som dirigerar trafik från fjärranslutna anställda till publicerade appar.</span><span class="sxs-lookup"><span data-stu-id="1851c-125">The Application Proxy connector is a Windows Server service that directs the traffic from your remote employees to your published apps.</span></span> <span data-ttu-id="1851c-126">Mer detaljerad Installationsinstruktioner finns [aktivera Application Proxy på Azure-portalen](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="1851c-126">For more detailed installation instructions, see [Enable Application Proxy in the Azure portal](active-directory-application-proxy-enable.md).</span></span>

1. <span data-ttu-id="1851c-127">Logga in på den [Azure-portalen](https://portal.azure.com) som global administratör.</span><span class="sxs-lookup"><span data-stu-id="1851c-127">Sign in to the [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="1851c-128">Välj **Azure Active Directory** > **programproxy**.</span><span class="sxs-lookup"><span data-stu-id="1851c-128">Select **Azure Active Directory** > **Application proxy**.</span></span>
3. <span data-ttu-id="1851c-129">Välj **hämta anslutning** att starta Application Proxy connector hämtningen.</span><span class="sxs-lookup"><span data-stu-id="1851c-129">Select **Download Connector** to start the Application Proxy connector download.</span></span> <span data-ttu-id="1851c-130">Följ installationsanvisningarna.</span><span class="sxs-lookup"><span data-stu-id="1851c-130">Follow the installation instructions.</span></span>

   ![Aktivera Application Proxy och hämtar connector](./media/application-proxy-ping-access/install-connector.png)

4. <span data-ttu-id="1851c-132">Hämtar anslutningen automatiskt aktivera Application Proxy för din katalog, men om inte du väljer **aktivera Application Proxy**.</span><span class="sxs-lookup"><span data-stu-id="1851c-132">Downloading the connector should automatically enable Application Proxy for your directory, but if not you can select **Enable Application Proxy**.</span></span>


### <a name="add-your-app-to-azure-ad-with-application-proxy"></a><span data-ttu-id="1851c-133">Lägg till din app i Azure AD med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="1851c-133">Add your app to Azure AD with Application Proxy</span></span>

<span data-ttu-id="1851c-134">Det finns två åtgärder du måste vidta i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1851c-134">There are two actions you need to take in the Azure portal.</span></span> <span data-ttu-id="1851c-135">Först måste du publicera ditt program med programproxy.</span><span class="sxs-lookup"><span data-stu-id="1851c-135">First, you need to publish your application with Application Proxy.</span></span> <span data-ttu-id="1851c-136">Måste du samla in information om den app som du kan använda under PingAccess steg.</span><span class="sxs-lookup"><span data-stu-id="1851c-136">Then, you need to collect some information about the app that you can use during the PingAccess steps.</span></span>

<span data-ttu-id="1851c-137">Följ dessa steg om du vill publicera en app.</span><span class="sxs-lookup"><span data-stu-id="1851c-137">Follow these steps to publish your app.</span></span> <span data-ttu-id="1851c-138">En mer detaljerad genomgång av steg 1 – 8, se [publicera program med Azure AD Application Proxy](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1851c-138">For a more detailed walkthrough of steps 1-8, see [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

1. <span data-ttu-id="1851c-139">Om du inte i det sista avsnittet, logga in på den [Azure-portalen](https://portal.azure.com) som global administratör.</span><span class="sxs-lookup"><span data-stu-id="1851c-139">If you didn't in the last section, sign in to the [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="1851c-140">Välj **Azure Active Directory** > **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1851c-140">Select **Azure Active Directory** > **Enterprise applications**.</span></span>
3. <span data-ttu-id="1851c-141">Välj **Lägg till** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="1851c-141">Select **Add** at the top of the blade.</span></span>
4. <span data-ttu-id="1851c-142">Välj **lokalt program**.</span><span class="sxs-lookup"><span data-stu-id="1851c-142">Select **On-premises application**.</span></span>
5. <span data-ttu-id="1851c-143">Fyll i de obligatoriska fälten med information om den nya appen.</span><span class="sxs-lookup"><span data-stu-id="1851c-143">Fill out the required fields with information about your new app.</span></span> <span data-ttu-id="1851c-144">Använd följande riktlinjer för inställningarna:</span><span class="sxs-lookup"><span data-stu-id="1851c-144">Use the following guidance for the settings:</span></span>
   - <span data-ttu-id="1851c-145">**Intern URL**: normalt du ange en URL som tar dig till inloggningssidan för appens när du är på företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="1851c-145">**Internal URL**: Normally you provide the URL that takes you to the app’s sign in page when you’re on the corporate network.</span></span> <span data-ttu-id="1851c-146">I det här scenariot måste kopplingen behandla PingAccess-proxy som sidan främre i appen.</span><span class="sxs-lookup"><span data-stu-id="1851c-146">For this scenario the connector needs to treat the PingAccess proxy as the front page of the app.</span></span> <span data-ttu-id="1851c-147">Använd följande format: `https://<host name of your PA server>:<port>`.</span><span class="sxs-lookup"><span data-stu-id="1851c-147">Use this format: `https://<host name of your PA server>:<port>`.</span></span> <span data-ttu-id="1851c-148">Porten är 3000 som standard, men du kan konfigurera i PingAccess.</span><span class="sxs-lookup"><span data-stu-id="1851c-148">The port is 3000 by default, but you can configure it in PingAccess.</span></span>
   - <span data-ttu-id="1851c-149">**Förautentiseringsmetoden**: Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1851c-149">**Pre-authentication method**: Azure Active Directory</span></span>
   - <span data-ttu-id="1851c-150">**Översätta URL: en i sidhuvuden**: Nej</span><span class="sxs-lookup"><span data-stu-id="1851c-150">**Translate URL in Headers**: No</span></span>

   >[!NOTE]
   ><span data-ttu-id="1851c-151">Om det här är ditt första program Använd port 3000 att starta och gå tillbaka till att uppdatera den här inställningen om du ändrar konfigurationen PingAccess.</span><span class="sxs-lookup"><span data-stu-id="1851c-151">If this is your first application, use port 3000 to start and come back to update this setting if you change your PingAccess configuration.</span></span> <span data-ttu-id="1851c-152">Om det här är en app i andra behöver detta matcha lyssnare som du har konfigurerat i PingAccess.</span><span class="sxs-lookup"><span data-stu-id="1851c-152">If this is your second or later app, this will need to match the Listener you’ve configured in PingAccess.</span></span> <span data-ttu-id="1851c-153">Lär dig mer om [lyssnare i PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span><span class="sxs-lookup"><span data-stu-id="1851c-153">Learn more about [listeners in PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span></span>

6. <span data-ttu-id="1851c-154">Välj **Lägg till** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="1851c-154">Select **Add** at the bottom of the blade.</span></span> <span data-ttu-id="1851c-155">Programmet har lagts till och snabb start-menyn öppnas.</span><span class="sxs-lookup"><span data-stu-id="1851c-155">Your application is added, and the quick start menu opens.</span></span>
7. <span data-ttu-id="1851c-156">Snabb startmenyn väljer du **tilldela en användare för att testa**, och Lägg till minst en användare till programmet.</span><span class="sxs-lookup"><span data-stu-id="1851c-156">In the quick start menu, select **Assign a user for testing**, and add at least one user to the application.</span></span> <span data-ttu-id="1851c-157">Kontrollera att det här testet kontot har åtkomst till lokala program.</span><span class="sxs-lookup"><span data-stu-id="1851c-157">Make sure this test account has access to the on-premises application.</span></span>
8. <span data-ttu-id="1851c-158">Välj **tilldela** spara Användartilldelning test.</span><span class="sxs-lookup"><span data-stu-id="1851c-158">Select **Assign** to save the test user assignment.</span></span>
9. <span data-ttu-id="1851c-159">På bladet hantering väljer **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1851c-159">On the app management blade, select **Single sign-on**.</span></span>
10. <span data-ttu-id="1851c-160">Välj **huvud-baserade inloggning** från den nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="1851c-160">Choose **Header-based sign-on** from the drop-down menu.</span></span> <span data-ttu-id="1851c-161">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1851c-161">Select **Save**.</span></span>

   >[!TIP]
   ><span data-ttu-id="1851c-162">Om det här är första gången du använder huvud-baserade enkel inloggning måste du installera PingAccess.</span><span class="sxs-lookup"><span data-stu-id="1851c-162">If this is your first time using header-based single sign-on, you need to install PingAccess.</span></span> <span data-ttu-id="1851c-163">Kontrollera att din Azure-prenumeration associeras automatiskt med PingAccess installationen genom att använda länken på den här sidan för enkel inloggning för att hämta PingAccess.</span><span class="sxs-lookup"><span data-stu-id="1851c-163">To make sure your Azure subscription is automatically associated with your PingAccess installation, use the link on this single sign-on page to download PingAccess.</span></span> <span data-ttu-id="1851c-164">Du kan öppna hämtningsplatsen nu eller försöka till den här sidan igen senare.</span><span class="sxs-lookup"><span data-stu-id="1851c-164">You can open the download site now, or come back to this page later.</span></span> 

   ![Välj huvud-baserade inloggning](./media/application-proxy-ping-access/sso-header.PNG)

11. <span data-ttu-id="1851c-166">Stäng bladet för Enterprise-program eller rulla längst till vänster om du vill gå tillbaka till Azure Active Directory-menyn.</span><span class="sxs-lookup"><span data-stu-id="1851c-166">Close the Enterprise applications blade or scroll all the way to the left to return to the Azure Active Directory menu.</span></span>
12. <span data-ttu-id="1851c-167">Välj **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="1851c-167">Select **App registrations**.</span></span>

   ![Välj App-registreringar](./media/application-proxy-ping-access/app-registrations.png)

13. <span data-ttu-id="1851c-169">Välj den app som du just lagt till, sedan **Reply URL: er**.</span><span class="sxs-lookup"><span data-stu-id="1851c-169">Select the app you just added, then **Reply URLs**.</span></span>

   ![Välj svars-URL: er](./media/application-proxy-ping-access/reply-urls.png)

14. <span data-ttu-id="1851c-171">Kontrollera om det finns externa URL: en som tilldelats din app i steg 5 i Reply URL-listan.</span><span class="sxs-lookup"><span data-stu-id="1851c-171">Check to see if the external URL that you assigned to your app in step 5 is in the Reply URLs list.</span></span> <span data-ttu-id="1851c-172">Om den inte gör du det nu.</span><span class="sxs-lookup"><span data-stu-id="1851c-172">If it’s not, add it now.</span></span>
15. <span data-ttu-id="1851c-173">På inställningsbladet väljer **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="1851c-173">On the app settings blade, select **Required permissions**.</span></span>

  ![Välj behörigheter som krävs](./media/application-proxy-ping-access/required-permissions.png)

16. <span data-ttu-id="1851c-175">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1851c-175">Select **Add**.</span></span> <span data-ttu-id="1851c-176">API: et, Välj **Windows Azure Active Directory**, sedan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="1851c-176">For the API, choose **Windows Azure Active Directory**, then **Select**.</span></span> <span data-ttu-id="1851c-177">Behörigheter, Välj **läsa och skriva alla program** och **logga in och Läs användarprofil**, sedan **Välj** och **klar**.</span><span class="sxs-lookup"><span data-stu-id="1851c-177">For the permissions, choose **Read and write all applications** and **Sign in and read user profile**, then **Select** and **Done**.</span></span>  

  ![Välj behörigheter](./media/application-proxy-ping-access/select-permissions.png)

### <a name="collect-information-for-the-pingaccess-steps"></a><span data-ttu-id="1851c-179">Samla in information för stegen PingAccess</span><span class="sxs-lookup"><span data-stu-id="1851c-179">Collect information for the PingAccess steps</span></span>

1. <span data-ttu-id="1851c-180">På inställningsbladet din app väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1851c-180">On your app settings blade, select **Properties**.</span></span> 

  ![Välj egenskaper](./media/application-proxy-ping-access/properties.png)

2. <span data-ttu-id="1851c-182">Spara den **program-Id** värde.</span><span class="sxs-lookup"><span data-stu-id="1851c-182">Save the **Application Id** value.</span></span> <span data-ttu-id="1851c-183">Detta används för klient-ID när du konfigurerar PingAccess.</span><span class="sxs-lookup"><span data-stu-id="1851c-183">This is used for the client ID when you configure PingAccess.</span></span>
3. <span data-ttu-id="1851c-184">På inställningsbladet väljer **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="1851c-184">On the app settings blade, select **Keys**.</span></span>

  ![Välj nycklar](./media/application-proxy-ping-access/Keys.png)

4. <span data-ttu-id="1851c-186">Skapa en nyckel genom att ange en beskrivning av nyckeln och väljer ett förfallodatum nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="1851c-186">Create a key by entering a key description and choosing an expiration date from the drop-down menu.</span></span>
5. <span data-ttu-id="1851c-187">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1851c-187">Select **Save**.</span></span> <span data-ttu-id="1851c-188">Ett GUID som visas i den **värdet** fältet.</span><span class="sxs-lookup"><span data-stu-id="1851c-188">A GUID appears in the **Value** field.</span></span>

  <span data-ttu-id="1851c-189">Spara det här värdet nu, eftersom du inte kommer att kunna se den igen när du har stängt det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="1851c-189">Save this value now, as you won’t be able to see it again after you close this window.</span></span>

  ![Skapa en ny nyckel](./media/application-proxy-ping-access/create-keys.png)

6. <span data-ttu-id="1851c-191">Stäng appen registreringar bladet eller rulla åt vänster gå tillbaka till Azure Active Directory-menyn.</span><span class="sxs-lookup"><span data-stu-id="1851c-191">Close the App registrations blade or scroll all the way to the left to return to the Azure Active Directory menu.</span></span>
7. <span data-ttu-id="1851c-192">Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1851c-192">Select **Properties**.</span></span>
8. <span data-ttu-id="1851c-193">Spara den **katalog-ID** GUID.</span><span class="sxs-lookup"><span data-stu-id="1851c-193">Save the **Directory ID** GUID.</span></span>

### <a name="optional---update-graphapi-to-send-custom-fields"></a><span data-ttu-id="1851c-194">Valfritt: uppdatera GraphAPI att skicka anpassade fält</span><span class="sxs-lookup"><span data-stu-id="1851c-194">Optional - Update GraphAPI to send custom fields</span></span>

<span data-ttu-id="1851c-195">En lista över säkerhetstoken som Azure AD skickar för autentisering, se [Azure AD tokenreferens](./develop/active-directory-token-and-claims.md).</span><span class="sxs-lookup"><span data-stu-id="1851c-195">For a list of security tokens that Azure AD sends for authentication, see [Azure AD token reference](./develop/active-directory-token-and-claims.md).</span></span> <span data-ttu-id="1851c-196">Om du behöver ett anpassat anspråk som skickar andra token kan använda GraphAPI för att ange fältet app *acceptMappedClaims* till **SANT**.</span><span class="sxs-lookup"><span data-stu-id="1851c-196">If you need a custom claim that sends other tokens, use GraphAPI to set the app field *acceptMappedClaims* to **True**.</span></span> <span data-ttu-id="1851c-197">Du kan använda Azure AD Graph-Utforskaren eller MS Graph för att göra den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1851c-197">You can use either Azure AD Graph Explorer or MS Graph to make this configuration.</span></span> 

<span data-ttu-id="1851c-198">Det här exemplet använder diagram Explorer:</span><span class="sxs-lookup"><span data-stu-id="1851c-198">This example uses Graph Explorer:</span></span>

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

## <a name="download-pingaccess-and-configure-your-app"></a><span data-ttu-id="1851c-199">Hämta PingAccess och konfigurera din app</span><span class="sxs-lookup"><span data-stu-id="1851c-199">Download PingAccess and configure your app</span></span>

<span data-ttu-id="1851c-200">Nu när du har slutfört alla steg för Azure Active Directory-installationen, kan du gå vidare till Konfigurera PingAccess.</span><span class="sxs-lookup"><span data-stu-id="1851c-200">Now that you've completed all the Azure Active Directory setup steps, you can move on to configuring PingAccess.</span></span> 

<span data-ttu-id="1851c-201">Detaljerade anvisningar för PingAccess en del av det här scenariot fortsätter i dokumentationen för Ping identitet [konfigurera PingAccess för Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span><span class="sxs-lookup"><span data-stu-id="1851c-201">The detailed steps for the PingAccess part of this scenario continue in the Ping Identity documentation, [Configure PingAccess for Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span></span>

<span data-ttu-id="1851c-202">Dessa steg vägleder dig genom processen att skaffa en PingAccess-konto om du inte redan har ett, installerar PingAccess-servern och skapa en Azure AD OIDC Provider-anslutning med katalog-ID som du kopierade från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1851c-202">Those steps walk you through the process of getting a PingAccess account if you don't already have one, installing the PingAccess Server, and creating an Azure AD OIDC Provider connection with the Directory ID that you copied from the Azure portal.</span></span> <span data-ttu-id="1851c-203">Sedan kan du använda program-ID och nyckel värden, för att skapa en webbsessionen på PingAccess.</span><span class="sxs-lookup"><span data-stu-id="1851c-203">Then, you use the Application ID and Key values to create a Web Session on PingAccess.</span></span> <span data-ttu-id="1851c-204">Sedan kan du konfigurera identitetsmappning och skapa en virtuell värd, plats och program.</span><span class="sxs-lookup"><span data-stu-id="1851c-204">After that, you can set up identity mapping and create a virtual host, site, and application.</span></span>

### <a name="test-your-app"></a><span data-ttu-id="1851c-205">Testa din app</span><span class="sxs-lookup"><span data-stu-id="1851c-205">Test your app</span></span>

<span data-ttu-id="1851c-206">När du har slutfört de här stegen kan ska din app vara igång.</span><span class="sxs-lookup"><span data-stu-id="1851c-206">When you've completed all these steps, your app should be up and running.</span></span> <span data-ttu-id="1851c-207">Prova genom att öppna en webbläsare och gå till den externa URL som du skapade när du har publicerat appen i Azure.</span><span class="sxs-lookup"><span data-stu-id="1851c-207">To test it, open a browser and navigate to the external URL that you created when you published the app in Azure.</span></span> <span data-ttu-id="1851c-208">Logga in med kontot test som du tilldelade till appen.</span><span class="sxs-lookup"><span data-stu-id="1851c-208">Sign in with the test account that you assigned to the app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1851c-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1851c-209">Next steps</span></span>

- [<span data-ttu-id="1851c-210">Konfigurera PingAccess för Azure AD</span><span class="sxs-lookup"><span data-stu-id="1851c-210">Configure PingAccess for Azure AD</span></span>](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [<span data-ttu-id="1851c-211">Hur tillhandahåller Azure AD Application Proxy enkel inloggning?</span><span class="sxs-lookup"><span data-stu-id="1851c-211">How does Azure AD Application Proxy provide single sign-on?</span></span>](application-proxy-sso-overview.md)
- [<span data-ttu-id="1851c-212">Felsöka Application Proxy</span><span class="sxs-lookup"><span data-stu-id="1851c-212">Troubleshoot Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)
