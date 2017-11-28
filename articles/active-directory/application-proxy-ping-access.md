---
title: "aaaHeader-baserad autentisering med PingAccess för Azure AD Application Proxy | Microsoft Docs"
description: Publicera program med PingAccess och App-Proxy toosupport huvud-baserad autentisering.
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
ms.openlocfilehash: 38fe3e7a41a71f4ae6c75f014e44c722f773bd22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a><span data-ttu-id="6e1af-103">Rubrik-baserad autentisering för enkel inloggning med Application Proxy och PingAccess</span><span class="sxs-lookup"><span data-stu-id="6e1af-103">Header-based authentication for single sign-on with Application Proxy and PingAccess</span></span>

<span data-ttu-id="6e1af-104">Azure Active Directory Application Proxy och PingAccess samarbetar tillsammans tooprovide Azure Active Directory-kunder med åtkomst tooeven fler program.</span><span class="sxs-lookup"><span data-stu-id="6e1af-104">Azure Active Directory Application Proxy and PingAccess have partnered together tooprovide Azure Active Directory customers with access tooeven more applications.</span></span> <span data-ttu-id="6e1af-105">PingAccess expanderar hello [befintliga Application Proxy-erbjudanden](active-directory-application-proxy-get-started.md) tooinclude åtkomst för enkel inloggning tooapplications som använder huvuden för autentisering.</span><span class="sxs-lookup"><span data-stu-id="6e1af-105">PingAccess expands hello [existing Application Proxy offerings](active-directory-application-proxy-get-started.md) tooinclude single sign-on access tooapplications that use headers for authentication.</span></span>

## <a name="what-is-pingaccess-for-azure-ad"></a><span data-ttu-id="6e1af-106">Vad är PingAccess för Azure AD?</span><span class="sxs-lookup"><span data-stu-id="6e1af-106">What is PingAccess for Azure AD?</span></span>

<span data-ttu-id="6e1af-107">PingAccess för Azure Active Directory är ett erbjudande PingAccess som aktiverar du toogive användare åtkomst med enkel inloggning tooapplications som använder huvuden för autentisering.</span><span class="sxs-lookup"><span data-stu-id="6e1af-107">PingAccess for Azure Active Directory is an offering of PingAccess that enables you toogive users access and single sign-on tooapplications that use headers for authentication.</span></span> <span data-ttu-id="6e1af-108">Programproxy behandlas dessa appar som helst, med hjälp av Azure AD tooauthenticate åtkomst och skicka trafik via hello kopplingstjänsten.</span><span class="sxs-lookup"><span data-stu-id="6e1af-108">Application Proxy treats these apps like any other, using Azure AD tooauthenticate access and then passing traffic through hello connector service.</span></span> <span data-ttu-id="6e1af-109">PingAccess placeras framför hello appar och översätter hello åtkomst-token från Azure AD till ett sidhuvud så att programmet hello tar emot hello autentisering i hello-format som den kan läsa.</span><span class="sxs-lookup"><span data-stu-id="6e1af-109">PingAccess sits in front of hello apps and translates hello access token from Azure AD into a header so that hello application receives hello authentication in hello format it can read.</span></span>

<span data-ttu-id="6e1af-110">Användarna kommer inte se något annat när de loggar in toouse ditt företags-appar.</span><span class="sxs-lookup"><span data-stu-id="6e1af-110">Your users won’t notice anything different when they sign in toouse your corporate apps.</span></span> <span data-ttu-id="6e1af-111">De kan fortsätta att arbeta från var som helst på vilken enhet som helst.</span><span class="sxs-lookup"><span data-stu-id="6e1af-111">They can still work from anywhere on any device.</span></span> 

<span data-ttu-id="6e1af-112">Eftersom hello Application Proxy kopplingar direkt remote trafik tooall appar oavsett deras autentiseringstyp, kommer de fortsätta tooload saldo automatiskt, samt.</span><span class="sxs-lookup"><span data-stu-id="6e1af-112">Since hello Application Proxy connectors direct remote traffic tooall apps regardless of their authentication type, they’ll continue tooload balance automatically, as well.</span></span>

## <a name="how-do-i-get-access"></a><span data-ttu-id="6e1af-113">Hur skaffar åtkomst?</span><span class="sxs-lookup"><span data-stu-id="6e1af-113">How do I get access?</span></span>

<span data-ttu-id="6e1af-114">Eftersom det här scenariot erbjuds via ett partnerskap mellan Azure Active Directory och PingAccess, behöver du licenser för båda tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="6e1af-114">Since this scenario is offered through a partnership between Azure Active Directory and PingAccess, you need licenses for both services.</span></span> <span data-ttu-id="6e1af-115">Azure Active Directory Premium prenumerationer innehåller emellertid en grundläggande PingAccess licens som täcker in too20 program.</span><span class="sxs-lookup"><span data-stu-id="6e1af-115">However, Azure Active Directory Premium subscriptions include a basic PingAccess license that covers up too20 applications.</span></span> <span data-ttu-id="6e1af-116">Om du behöver toopublish fler än 20 huvud-baserade program, kan du köpa ytterligare en licens från PingAccess.</span><span class="sxs-lookup"><span data-stu-id="6e1af-116">If you need toopublish more than 20 header-based applications, you can purchase an additional license from PingAccess.</span></span> 

<span data-ttu-id="6e1af-117">Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="6e1af-117">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="publish-your-application-in-azure"></a><span data-ttu-id="6e1af-118">Publicera programmet i Azure</span><span class="sxs-lookup"><span data-stu-id="6e1af-118">Publish your application in Azure</span></span>

<span data-ttu-id="6e1af-119">Den här artikeln är avsedd för personer som publicerar en app med det här scenariot för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="6e1af-119">This article is intended for people who are publishing an app with this scenario for hello first time.</span></span> <span data-ttu-id="6e1af-120">Det går igenom hur tooget igång med både program- och PingAccess, dessutom toohello publishing steg.</span><span class="sxs-lookup"><span data-stu-id="6e1af-120">It walks through how tooget started with both Application and PingAccess, in addition toohello publishing steps.</span></span> <span data-ttu-id="6e1af-121">Om du redan har konfigurerat båda tjänsterna men vill en uppdaterare på hello publicering steg kan du hoppa över hello kopplingsinstallationen och vidare för[lägga till din app tooAzure AD med Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).</span><span class="sxs-lookup"><span data-stu-id="6e1af-121">If you’ve already configured both services but want a refresher on hello publishing steps, you can skip hello connector installation and move on too[Add your app tooAzure AD with Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).</span></span>

>[!NOTE]
><span data-ttu-id="6e1af-122">Eftersom det här scenariot är en koppling mellan Azure AD och PingAccess, vissa hello anvisningar finns på hello Ping identitet plats.</span><span class="sxs-lookup"><span data-stu-id="6e1af-122">Since this scenario is a partnership between Azure AD and PingAccess, some of hello instructions exist on hello Ping Identity site.</span></span>

### <a name="install-an-application-proxy-connector"></a><span data-ttu-id="6e1af-123">Installera en Application Proxy connector</span><span class="sxs-lookup"><span data-stu-id="6e1af-123">Install an Application Proxy connector</span></span>

<span data-ttu-id="6e1af-124">Om du redan har Application Proxy är aktiverat och har en koppling installeras, kan du hoppa över det här avsnittet och flyttar på för[lägga till din app tooAzure AD med Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).</span><span class="sxs-lookup"><span data-stu-id="6e1af-124">If you already have Application Proxy enabled, and have a connector installed, you can skip this section and move on too[Add your app tooAzure AD with Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).</span></span>

<span data-ttu-id="6e1af-125">hello Application Proxy connector är en Windows Server-tjänst som dirigerar hello trafik från din fjärranslutna anställda tooyour publicerade appar.</span><span class="sxs-lookup"><span data-stu-id="6e1af-125">hello Application Proxy connector is a Windows Server service that directs hello traffic from your remote employees tooyour published apps.</span></span> <span data-ttu-id="6e1af-126">Mer detaljerad Installationsinstruktioner finns [aktivera Application Proxy hello Azure-portalen](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="6e1af-126">For more detailed installation instructions, see [Enable Application Proxy in hello Azure portal](active-directory-application-proxy-enable.md).</span></span>

1. <span data-ttu-id="6e1af-127">Logga in toohello [Azure-portalen](https://portal.azure.com) som global administratör.</span><span class="sxs-lookup"><span data-stu-id="6e1af-127">Sign in toohello [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="6e1af-128">Välj **Azure Active Directory** > **programproxy**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-128">Select **Azure Active Directory** > **Application proxy**.</span></span>
3. <span data-ttu-id="6e1af-129">Välj **hämta anslutning** toostart hello Application Proxy connector hämtning.</span><span class="sxs-lookup"><span data-stu-id="6e1af-129">Select **Download Connector** toostart hello Application Proxy connector download.</span></span> <span data-ttu-id="6e1af-130">Följ installationsanvisningarna hello.</span><span class="sxs-lookup"><span data-stu-id="6e1af-130">Follow hello installation instructions.</span></span>

   ![Aktivera Application Proxy och hämta hello connector](./media/application-proxy-ping-access/install-connector.png)

4. <span data-ttu-id="6e1af-132">Hämta hello-anslutningen automatiskt aktivera Application Proxy för din katalog, men om inte du väljer **aktivera Application Proxy**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-132">Downloading hello connector should automatically enable Application Proxy for your directory, but if not you can select **Enable Application Proxy**.</span></span>


### <a name="add-your-app-tooazure-ad-with-application-proxy"></a><span data-ttu-id="6e1af-133">Lägg till din app tooAzure AD med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="6e1af-133">Add your app tooAzure AD with Application Proxy</span></span>

<span data-ttu-id="6e1af-134">Det finns två åtgärder som du behöver tootake i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6e1af-134">There are two actions you need tootake in hello Azure portal.</span></span> <span data-ttu-id="6e1af-135">Du måste först toopublish ditt program med programproxy.</span><span class="sxs-lookup"><span data-stu-id="6e1af-135">First, you need toopublish your application with Application Proxy.</span></span> <span data-ttu-id="6e1af-136">Sedan måste toocollect viss information om hello-app som du kan använda under hello PingAccess steg.</span><span class="sxs-lookup"><span data-stu-id="6e1af-136">Then, you need toocollect some information about hello app that you can use during hello PingAccess steps.</span></span>

<span data-ttu-id="6e1af-137">Följ dessa steg toopublish din app.</span><span class="sxs-lookup"><span data-stu-id="6e1af-137">Follow these steps toopublish your app.</span></span> <span data-ttu-id="6e1af-138">En mer detaljerad genomgång av steg 1 – 8, se [publicera program med Azure AD Application Proxy](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6e1af-138">For a more detailed walkthrough of steps 1-8, see [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

1. <span data-ttu-id="6e1af-139">Om du inte i hello sista avsnittet, loggar du in toohello [Azure-portalen](https://portal.azure.com) som global administratör.</span><span class="sxs-lookup"><span data-stu-id="6e1af-139">If you didn't in hello last section, sign in toohello [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="6e1af-140">Välj **Azure Active Directory** > **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-140">Select **Azure Active Directory** > **Enterprise applications**.</span></span>
3. <span data-ttu-id="6e1af-141">Välj **Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="6e1af-141">Select **Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="6e1af-142">Välj **lokalt program**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-142">Select **On-premises application**.</span></span>
5. <span data-ttu-id="6e1af-143">Fyll i hello krävs fält med information om den nya appen.</span><span class="sxs-lookup"><span data-stu-id="6e1af-143">Fill out hello required fields with information about your new app.</span></span> <span data-ttu-id="6e1af-144">Använd följande riktlinjer för hello inställningar hello:</span><span class="sxs-lookup"><span data-stu-id="6e1af-144">Use hello following guidance for hello settings:</span></span>
   - <span data-ttu-id="6e1af-145">**Intern URL**: anger normalt hello-URL som tar dig logga toohello appen på sidan när du är på hello företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="6e1af-145">**Internal URL**: Normally you provide hello URL that takes you toohello app’s sign in page when you’re on hello corporate network.</span></span> <span data-ttu-id="6e1af-146">I det här scenariot måste hello connector tootreat hello PingAccess proxy som hello framsidan hello-appen.</span><span class="sxs-lookup"><span data-stu-id="6e1af-146">For this scenario hello connector needs tootreat hello PingAccess proxy as hello front page of hello app.</span></span> <span data-ttu-id="6e1af-147">Använd följande format: `https://<host name of your PA server>:<port>`.</span><span class="sxs-lookup"><span data-stu-id="6e1af-147">Use this format: `https://<host name of your PA server>:<port>`.</span></span> <span data-ttu-id="6e1af-148">hello porten är 3000 som standard, men du kan konfigurera i PingAccess.</span><span class="sxs-lookup"><span data-stu-id="6e1af-148">hello port is 3000 by default, but you can configure it in PingAccess.</span></span>
   - <span data-ttu-id="6e1af-149">**Förautentiseringsmetoden**: Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6e1af-149">**Pre-authentication method**: Azure Active Directory</span></span>
   - <span data-ttu-id="6e1af-150">**Översätta URL: en i sidhuvuden**: Nej</span><span class="sxs-lookup"><span data-stu-id="6e1af-150">**Translate URL in Headers**: No</span></span>

   >[!NOTE]
   ><span data-ttu-id="6e1af-151">Om det här är ditt första program använder port 3000 toostart och gå tillbaka tooupdate den här inställningen om du ändrar konfigurationen PingAccess.</span><span class="sxs-lookup"><span data-stu-id="6e1af-151">If this is your first application, use port 3000 toostart and come back tooupdate this setting if you change your PingAccess configuration.</span></span> <span data-ttu-id="6e1af-152">Om det här är en app i andra eller senare behöver detta toomatch hello lyssnare som du har konfigurerat i PingAccess.</span><span class="sxs-lookup"><span data-stu-id="6e1af-152">If this is your second or later app, this will need toomatch hello Listener you’ve configured in PingAccess.</span></span> <span data-ttu-id="6e1af-153">Lär dig mer om [lyssnare i PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span><span class="sxs-lookup"><span data-stu-id="6e1af-153">Learn more about [listeners in PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span></span>

6. <span data-ttu-id="6e1af-154">Välj **Lägg till** längst hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="6e1af-154">Select **Add** at hello bottom of hello blade.</span></span> <span data-ttu-id="6e1af-155">Programmet har lagts till och hello snabb start-menyn öppnas.</span><span class="sxs-lookup"><span data-stu-id="6e1af-155">Your application is added, and hello quick start menu opens.</span></span>
7. <span data-ttu-id="6e1af-156">Hello snabb start-menyn, Välj **tilldela en användare för att testa**, och Lägg till minst en användare toohello program.</span><span class="sxs-lookup"><span data-stu-id="6e1af-156">In hello quick start menu, select **Assign a user for testing**, and add at least one user toohello application.</span></span> <span data-ttu-id="6e1af-157">Kontrollera att det här testet kontot har åtkomst toohello lokalt program.</span><span class="sxs-lookup"><span data-stu-id="6e1af-157">Make sure this test account has access toohello on-premises application.</span></span>
8. <span data-ttu-id="6e1af-158">Välj **tilldela** toosave hello test Användartilldelning.</span><span class="sxs-lookup"><span data-stu-id="6e1af-158">Select **Assign** toosave hello test user assignment.</span></span>
9. <span data-ttu-id="6e1af-159">På bladet för hantering av hello appen, Välj **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-159">On hello app management blade, select **Single sign-on**.</span></span>
10. <span data-ttu-id="6e1af-160">Välj **huvud-baserade inloggning** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="6e1af-160">Choose **Header-based sign-on** from hello drop-down menu.</span></span> <span data-ttu-id="6e1af-161">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-161">Select **Save**.</span></span>

   >[!TIP]
   ><span data-ttu-id="6e1af-162">Om det här är första gången du använder huvud-baserade enkel inloggning måste tooinstall PingAccess.</span><span class="sxs-lookup"><span data-stu-id="6e1af-162">If this is your first time using header-based single sign-on, you need tooinstall PingAccess.</span></span> <span data-ttu-id="6e1af-163">toomake till din Azure-prenumeration associeras automatiskt med PingAccess installationen, Använd hello länk på den här sidan för enkel inloggning toodownload PingAccess.</span><span class="sxs-lookup"><span data-stu-id="6e1af-163">toomake sure your Azure subscription is automatically associated with your PingAccess installation, use hello link on this single sign-on page toodownload PingAccess.</span></span> <span data-ttu-id="6e1af-164">Du kan öppna hello hämtningsplats nu eller kommer tillbaka toothis sidan senare.</span><span class="sxs-lookup"><span data-stu-id="6e1af-164">You can open hello download site now, or come back toothis page later.</span></span> 

   ![Välj huvud-baserade inloggning](./media/application-proxy-ping-access/sso-header.PNG)

11. <span data-ttu-id="6e1af-166">Stäng hello Enterprise program bladet eller bläddra alla hello sätt toohello vänstra tooreturn toohello Azure Active Directory-menyn.</span><span class="sxs-lookup"><span data-stu-id="6e1af-166">Close hello Enterprise applications blade or scroll all hello way toohello left tooreturn toohello Azure Active Directory menu.</span></span>
12. <span data-ttu-id="6e1af-167">Välj **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-167">Select **App registrations**.</span></span>

   ![Välj App-registreringar](./media/application-proxy-ping-access/app-registrations.png)

13. <span data-ttu-id="6e1af-169">Välj hello-app som du just lagt till, sedan **Reply URL: er**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-169">Select hello app you just added, then **Reply URLs**.</span></span>

   ![Välj svars-URL: er](./media/application-proxy-ping-access/reply-urls.png)

14. <span data-ttu-id="6e1af-171">Kontrollera toosee om hello externa URL: en som du tilldelade tooyour app i steg 5 i hello Reply URL-listan.</span><span class="sxs-lookup"><span data-stu-id="6e1af-171">Check toosee if hello external URL that you assigned tooyour app in step 5 is in hello Reply URLs list.</span></span> <span data-ttu-id="6e1af-172">Om den inte gör du det nu.</span><span class="sxs-lookup"><span data-stu-id="6e1af-172">If it’s not, add it now.</span></span>
15. <span data-ttu-id="6e1af-173">På inställningsbladet för hello app väljer **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-173">On hello app settings blade, select **Required permissions**.</span></span>

  ![Välj behörigheter som krävs](./media/application-proxy-ping-access/required-permissions.png)

16. <span data-ttu-id="6e1af-175">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-175">Select **Add**.</span></span> <span data-ttu-id="6e1af-176">Hello API, Välj **Windows Azure Active Directory**, sedan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-176">For hello API, choose **Windows Azure Active Directory**, then **Select**.</span></span> <span data-ttu-id="6e1af-177">Hello behörigheter, Välj **läsa och skriva alla program** och **logga in och Läs användarprofil**, sedan **Välj** och **klar**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-177">For hello permissions, choose **Read and write all applications** and **Sign in and read user profile**, then **Select** and **Done**.</span></span>  

  ![Välj behörigheter](./media/application-proxy-ping-access/select-permissions.png)

### <a name="collect-information-for-hello-pingaccess-steps"></a><span data-ttu-id="6e1af-179">Samla in information för hello PingAccess steg</span><span class="sxs-lookup"><span data-stu-id="6e1af-179">Collect information for hello PingAccess steps</span></span>

1. <span data-ttu-id="6e1af-180">På inställningsbladet din app väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-180">On your app settings blade, select **Properties**.</span></span> 

  ![Välj egenskaper](./media/application-proxy-ping-access/properties.png)

2. <span data-ttu-id="6e1af-182">Spara hello **program-Id** värde.</span><span class="sxs-lookup"><span data-stu-id="6e1af-182">Save hello **Application Id** value.</span></span> <span data-ttu-id="6e1af-183">Detta används för hello klient-ID när du konfigurerar PingAccess.</span><span class="sxs-lookup"><span data-stu-id="6e1af-183">This is used for hello client ID when you configure PingAccess.</span></span>
3. <span data-ttu-id="6e1af-184">På inställningsbladet för hello app väljer **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-184">On hello app settings blade, select **Keys**.</span></span>

  ![Välj nycklar](./media/application-proxy-ping-access/Keys.png)

4. <span data-ttu-id="6e1af-186">Skapa en nyckel genom att ange en beskrivning av nyckeln och väljer ett förfallodatum hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="6e1af-186">Create a key by entering a key description and choosing an expiration date from hello drop-down menu.</span></span>
5. <span data-ttu-id="6e1af-187">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-187">Select **Save**.</span></span> <span data-ttu-id="6e1af-188">Ett GUID som visas i hello **värdet** fältet.</span><span class="sxs-lookup"><span data-stu-id="6e1af-188">A GUID appears in hello **Value** field.</span></span>

  <span data-ttu-id="6e1af-189">Spara det här värdet, som du inte kan toosee den igen när du stänga fönstret.</span><span class="sxs-lookup"><span data-stu-id="6e1af-189">Save this value now, as you won’t be able toosee it again after you close this window.</span></span>

  ![Skapa en ny nyckel](./media/application-proxy-ping-access/create-keys.png)

6. <span data-ttu-id="6e1af-191">Stäng hello App registreringar bladet eller bläddra alla hello sätt toohello vänstra tooreturn toohello Azure Active Directory-menyn.</span><span class="sxs-lookup"><span data-stu-id="6e1af-191">Close hello App registrations blade or scroll all hello way toohello left tooreturn toohello Azure Active Directory menu.</span></span>
7. <span data-ttu-id="6e1af-192">Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-192">Select **Properties**.</span></span>
8. <span data-ttu-id="6e1af-193">Spara hello **katalog-ID** GUID.</span><span class="sxs-lookup"><span data-stu-id="6e1af-193">Save hello **Directory ID** GUID.</span></span>

### <a name="optional---update-graphapi-toosend-custom-fields"></a><span data-ttu-id="6e1af-194">Valfritt: uppdatera GraphAPI toosend anpassade fält</span><span class="sxs-lookup"><span data-stu-id="6e1af-194">Optional - Update GraphAPI toosend custom fields</span></span>

<span data-ttu-id="6e1af-195">En lista över säkerhetstoken som Azure AD skickar för autentisering, se [Azure AD tokenreferens](./develop/active-directory-token-and-claims.md).</span><span class="sxs-lookup"><span data-stu-id="6e1af-195">For a list of security tokens that Azure AD sends for authentication, see [Azure AD token reference](./develop/active-directory-token-and-claims.md).</span></span> <span data-ttu-id="6e1af-196">Om du behöver ett anpassat anspråk som skickar andra token kan du använda GraphAPI tooset hello app fältet *acceptMappedClaims* för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="6e1af-196">If you need a custom claim that sends other tokens, use GraphAPI tooset hello app field *acceptMappedClaims* too**True**.</span></span> <span data-ttu-id="6e1af-197">Du kan använda Azure AD Graph-Utforskaren eller MS Graph toomake den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="6e1af-197">You can use either Azure AD Graph Explorer or MS Graph toomake this configuration.</span></span> 

<span data-ttu-id="6e1af-198">Det här exemplet använder diagram Explorer:</span><span class="sxs-lookup"><span data-stu-id="6e1af-198">This example uses Graph Explorer:</span></span>

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

## <a name="download-pingaccess-and-configure-your-app"></a><span data-ttu-id="6e1af-199">Hämta PingAccess och konfigurera din app</span><span class="sxs-lookup"><span data-stu-id="6e1af-199">Download PingAccess and configure your app</span></span>

<span data-ttu-id="6e1af-200">Nu när du har slutfört alla steg i hello Azure Active Directory-installationen, kan du gå vidare tooconfiguring PingAccess.</span><span class="sxs-lookup"><span data-stu-id="6e1af-200">Now that you've completed all hello Azure Active Directory setup steps, you can move on tooconfiguring PingAccess.</span></span> 

<span data-ttu-id="6e1af-201">hello detaljerade anvisningar för hello PingAccess en del av det här scenariot fortsätter i hello Ping Identity-dokumentation [konfigurera PingAccess för Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span><span class="sxs-lookup"><span data-stu-id="6e1af-201">hello detailed steps for hello PingAccess part of this scenario continue in hello Ping Identity documentation, [Configure PingAccess for Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span></span>

<span data-ttu-id="6e1af-202">Dessa steg beskriver hur du hello processen för att skaffa en PingAccess-konto om du inte redan har ett, installera hello PingAccess Server och skapa en Azure AD OIDC leverantörsanslutning med hello katalog-ID som du kopierade från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6e1af-202">Those steps walk you through hello process of getting a PingAccess account if you don't already have one, installing hello PingAccess Server, and creating an Azure AD OIDC Provider connection with hello Directory ID that you copied from hello Azure portal.</span></span> <span data-ttu-id="6e1af-203">Sedan kan du använda hello program-ID och nyckel värden toocreate en webbsessionen på PingAccess.</span><span class="sxs-lookup"><span data-stu-id="6e1af-203">Then, you use hello Application ID and Key values toocreate a Web Session on PingAccess.</span></span> <span data-ttu-id="6e1af-204">Sedan kan du konfigurera identitetsmappning och skapa en virtuell värd, plats och program.</span><span class="sxs-lookup"><span data-stu-id="6e1af-204">After that, you can set up identity mapping and create a virtual host, site, and application.</span></span>

### <a name="test-your-app"></a><span data-ttu-id="6e1af-205">Testa din app</span><span class="sxs-lookup"><span data-stu-id="6e1af-205">Test your app</span></span>

<span data-ttu-id="6e1af-206">När du har slutfört de här stegen kan ska din app vara igång.</span><span class="sxs-lookup"><span data-stu-id="6e1af-206">When you've completed all these steps, your app should be up and running.</span></span> <span data-ttu-id="6e1af-207">tootest, öppna en webbläsare och gå toohello externa URL: en som du skapade när du har publicerat hello app i Azure.</span><span class="sxs-lookup"><span data-stu-id="6e1af-207">tootest it, open a browser and navigate toohello external URL that you created when you published hello app in Azure.</span></span> <span data-ttu-id="6e1af-208">Logga in med hello testkonto du tilldelade toohello appen.</span><span class="sxs-lookup"><span data-stu-id="6e1af-208">Sign in with hello test account that you assigned toohello app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e1af-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6e1af-209">Next steps</span></span>

- [<span data-ttu-id="6e1af-210">Konfigurera PingAccess för Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e1af-210">Configure PingAccess for Azure AD</span></span>](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [<span data-ttu-id="6e1af-211">Hur tillhandahåller Azure AD Application Proxy enkel inloggning?</span><span class="sxs-lookup"><span data-stu-id="6e1af-211">How does Azure AD Application Proxy provide single sign-on?</span></span>](application-proxy-sso-overview.md)
- [<span data-ttu-id="6e1af-212">Felsöka Application Proxy</span><span class="sxs-lookup"><span data-stu-id="6e1af-212">Troubleshoot Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)
