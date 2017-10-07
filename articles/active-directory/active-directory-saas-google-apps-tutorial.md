---
title: "Självstudier: Azure Active Directory-integrering med Google Apps i Azure | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2093b5ab605ec0d7bbefe7a78e1eede34d756f53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a><span data-ttu-id="747fc-103">Självstudier: Azure Active Directory-integrering med Google Apps</span><span class="sxs-lookup"><span data-stu-id="747fc-103">Tutorial: Azure Active Directory integration with Google Apps</span></span>

<span data-ttu-id="747fc-104">I kursen får du lära dig hur toointegrate Google Apps med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="747fc-104">In this tutorial, you learn how toointegrate Google Apps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="747fc-105">Integrera Google Apps med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="747fc-105">Integrating Google Apps with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="747fc-106">Du kan styra i Azure AD som har åtkomst tooGoogle appar</span><span class="sxs-lookup"><span data-stu-id="747fc-106">You can control in Azure AD who has access tooGoogle Apps</span></span>
- <span data-ttu-id="747fc-107">Du kan aktivera din användare tooautomatically get inloggade tooGoogle appar (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="747fc-107">You can enable your users tooautomatically get signed-on tooGoogle Apps (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="747fc-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="747fc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="747fc-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="747fc-109">If you want tooknow more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="747fc-110">Krav</span><span class="sxs-lookup"><span data-stu-id="747fc-110">Prerequisites</span></span>

<span data-ttu-id="747fc-111">tooconfigure Azure AD-integrering med Google Apps behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="747fc-111">tooconfigure Azure AD integration with Google Apps, you need hello following items:</span></span>

- <span data-ttu-id="747fc-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="747fc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="747fc-113">En Google Apps enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="747fc-113">A Google Apps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="747fc-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="747fc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="747fc-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="747fc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="747fc-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="747fc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="747fc-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="747fc-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="747fc-118">Videosjälvstudie</span><span class="sxs-lookup"><span data-stu-id="747fc-118">Video tutorial</span></span>
<span data-ttu-id="747fc-119">Hur tooEnable enkel inloggning tooGoogle appar i två minuter:</span><span class="sxs-lookup"><span data-stu-id="747fc-119">How tooEnable Single Sign-On tooGoogle Apps in 2 Minutes:</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a><span data-ttu-id="747fc-120">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="747fc-120">Frequently Asked Questions</span></span>
1. <span data-ttu-id="747fc-121">**F: är Chromebooks och andra enheter som Chrome kompatibla med Azure AD enkel inloggning?**</span><span class="sxs-lookup"><span data-stu-id="747fc-121">**Q: Are Chromebooks and other Chrome devices compatible with Azure AD single sign-on?**</span></span>
   
    <span data-ttu-id="747fc-122">A: användare kan Ja, kan toosign till sina Chromebook enheter med hjälp av autentiseringsuppgifterna Azure AD.</span><span class="sxs-lookup"><span data-stu-id="747fc-122">A: Yes, users are able toosign into their Chromebook devices using their Azure AD credentials.</span></span> <span data-ttu-id="747fc-123">Se den här [artikel stöd för Google Apps](https://support.google.com/chrome/a/answer/6060880) för information om varför användare kan uppmanas att autentiseringsuppgifter två gånger.</span><span class="sxs-lookup"><span data-stu-id="747fc-123">See this [Google Apps support article](https://support.google.com/chrome/a/answer/6060880) for information on why users may get prompted for credentials twice.</span></span>

2. <span data-ttu-id="747fc-124">**F: om jag aktiverar enkel inloggning, kommer användarna att kunna toouse toosign sina Azure AD-autentiseringsuppgifterna i alla Google-produkten, till exempel Google klassrum, GMail, Google Drive, YouTube och så vidare?**</span><span class="sxs-lookup"><span data-stu-id="747fc-124">**Q: If I enable single sign-on, will users be able toouse their Azure AD credentials toosign into any Google product, such as Google Classroom, GMail, Google Drive, YouTube, and so on?**</span></span>
   
    <span data-ttu-id="747fc-125">S: Ja, beroende på [vilka Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) du välja tooenable eller inaktivera för din organisation.</span><span class="sxs-lookup"><span data-stu-id="747fc-125">A: Yes, depending on [which Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) you choose tooenable or disable for your organization.</span></span>

3. <span data-ttu-id="747fc-126">**F: kan jag aktivera enkel inloggning för en delmängd av användarna Google Apps?**</span><span class="sxs-lookup"><span data-stu-id="747fc-126">**Q: Can I enable single sign-on for only a subset of my Google Apps users?**</span></span>
   
    <span data-ttu-id="747fc-127">A: Aktivera enkel inloggning omedelbart kräver inte alla dina Google Apps användare tooauthenticate med sina autentiseringsuppgifter för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="747fc-127">A: No, turning on single sign-on immediately requires all your Google Apps users tooauthenticate with their Azure AD credentials.</span></span> <span data-ttu-id="747fc-128">Eftersom Google Apps inte stöder att ha flera identitetsleverantörer, hello identitetsleverantören för Google Apps-miljö kan antingen vara Azure AD eller Google-- men inte båda på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="747fc-128">Because Google Apps doesn't support having multiple identity providers, hello identity provider for your Google Apps environment can either be Azure AD or Google -- but not both at hello same time.</span></span>

4. <span data-ttu-id="747fc-129">**F: om en användare har loggat in via Windows är de automatiskt autentisera tooGoogle appar utan ett lösenord uppmanas?**</span><span class="sxs-lookup"><span data-stu-id="747fc-129">**Q: If a user is signed in through Windows, are they automatically authenticate tooGoogle Apps without getting prompted for a password?**</span></span>
   
    <span data-ttu-id="747fc-130">S: finns två alternativ för att aktivera det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="747fc-130">A: There are two options for enabling this scenario.</span></span> <span data-ttu-id="747fc-131">Först användare kan logga in på Windows 10-enheter via [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="747fc-131">First, users could sign into Windows 10 devices via [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span> <span data-ttu-id="747fc-132">Du kan också användare kan logga in på Windows-enheter som är ansluten till domänen tooan lokala Active Directory som har aktiverats för enkel inloggning tooAzure AD via en [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) distribution.</span><span class="sxs-lookup"><span data-stu-id="747fc-132">Alternatively, users could sign into Windows devices that are domain-joined tooan on-premises Active Directory that has been enabled for single sign-on tooAzure AD via an [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) deployment.</span></span> <span data-ttu-id="747fc-133">Båda alternativen kräver tooperform hello stegen i hello följa kursen tooenable enkel inloggning mellan Azure AD och Google Apps.</span><span class="sxs-lookup"><span data-stu-id="747fc-133">Both options require you tooperform hello steps in hello following tutorial tooenable single sign-on between Azure AD and Google Apps.</span></span>

## <a name="scenario-description"></a><span data-ttu-id="747fc-134">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="747fc-134">Scenario description</span></span>
<span data-ttu-id="747fc-135">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="747fc-135">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="747fc-136">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="747fc-136">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="747fc-137">Lägga till Google Apps från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="747fc-137">Adding Google Apps from hello gallery</span></span>
2. <span data-ttu-id="747fc-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="747fc-138">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-google-apps-from-hello-gallery"></a><span data-ttu-id="747fc-139">Lägga till Google Apps från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="747fc-139">Adding Google Apps from hello gallery</span></span>
<span data-ttu-id="747fc-140">tooconfigure hello integrering av Google Apps i Azure AD, behöver du tooadd Google Apps hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="747fc-140">tooconfigure hello integration of Google Apps into Azure AD, you need tooadd Google Apps from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="747fc-141">**tooadd Google Apps från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="747fc-141">**tooadd Google Apps from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="747fc-142">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="747fc-142">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="747fc-144">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="747fc-144">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="747fc-145">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="747fc-145">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="747fc-147">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="747fc-147">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="747fc-149">Skriv i sökrutan hello **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="747fc-149">In hello search box, type **Google Apps**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. <span data-ttu-id="747fc-151">Markera hello resultat på panelen **Google Apps**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="747fc-151">In hello results panel, select **Google Apps**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="747fc-153">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="747fc-153">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="747fc-154">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Google Apps baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="747fc-154">In this section, you configure and test Azure AD single sign-on with Google Apps based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="747fc-155">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Google Apps är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="747fc-155">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Google Apps is tooa user in Azure AD.</span></span> <span data-ttu-id="747fc-156">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Google Apps toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="747fc-156">In other words, a link relationship between an Azure AD user and hello related user in Google Apps needs toobe established.</span></span>

<span data-ttu-id="747fc-157">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Google Apps.</span><span class="sxs-lookup"><span data-stu-id="747fc-157">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Google Apps.</span></span>

<span data-ttu-id="747fc-158">tooconfigure och testa Azure AD enkel inloggning med Google Apps behöver toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="747fc-158">tooconfigure and test Azure AD single sign-on with Google Apps, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="747fc-159">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="747fc-159">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="747fc-160">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="747fc-160">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="747fc-161">**[Skapa en testanvändare för Google Apps](#creating-a-google-apps-test-user)**  -toohave en motsvarighet för Britta Simon i Google-appar som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="747fc-161">**[Creating a Google Apps test user](#creating-a-google-apps-test-user)** - toohave a counterpart of Britta Simon in Google Apps that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="747fc-162">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="747fc-162">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="747fc-163">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="747fc-163">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="747fc-164">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="747fc-164">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="747fc-165">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Google Apps-program.</span><span class="sxs-lookup"><span data-stu-id="747fc-165">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Google Apps application.</span></span>

<span data-ttu-id="747fc-166">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Google Apps:**</span><span class="sxs-lookup"><span data-stu-id="747fc-166">**tooconfigure Azure AD single sign-on with Google Apps, perform hello following steps:**</span></span>

1. <span data-ttu-id="747fc-167">I hello Azure-portalen på hello **Google Apps** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="747fc-167">In hello Azure portal, on hello **Google Apps** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="747fc-169">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="747fc-169">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. <span data-ttu-id="747fc-171">På hello **Google Apps-domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="747fc-171">On hello **Google Apps Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    <span data-ttu-id="747fc-173">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://mail.google.com/a/<yourdomain>`</span><span class="sxs-lookup"><span data-stu-id="747fc-173">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://mail.google.com/a/<yourdomain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="747fc-174">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="747fc-174">This value is not real.</span></span> <span data-ttu-id="747fc-175">Uppdatera hello värde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="747fc-175">Update hello value with hello actual Sign-on URL.</span></span> <span data-ttu-id="747fc-176">Kontakta hello [Google supportteamet](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="747fc-176">contact hello [Google support team](https://www.google.com/contact/).</span></span>
 
4. <span data-ttu-id="747fc-177">På hello **SAML-signeringscertifikat** klickar du på **certifikat** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="747fc-177">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. <span data-ttu-id="747fc-179">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="747fc-179">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="747fc-181">På hello **Google Apps-konfiguration** klickar du på **konfigurera Google Apps** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="747fc-181">On hello **Google Apps Configuration** section, click **Configure Google Apps** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="747fc-182">Kopiera hello **Sign-Out URL, SAML enkel inloggning URL: en och ändra URL: en lösenord** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="747fc-182">Copy hello **Sign-Out URL, SAML Single Sign-On Service URL and Change password URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. <span data-ttu-id="747fc-184">Öppna en ny flik i webbläsaren och logga in på hello [Google Apps-administrationskonsolen](http://admin.google.com/) med ditt administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="747fc-184">Open a new tab in your browser, and sign into hello [Google Apps Admin Console](http://admin.google.com/) using your administrator account.</span></span>

8. <span data-ttu-id="747fc-185">Klicka på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="747fc-185">Click **Security**.</span></span> <span data-ttu-id="747fc-186">Om du inte ser hello länken, det kan vara dolt under hello **fler kontroller** menyn längst hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="747fc-186">If you don't see hello link, it may be hidden under hello **More Controls** menu at hello bottom of hello screen.</span></span>
   
    ![Klicka på Security.][10]

9. <span data-ttu-id="747fc-188">På hello **säkerhet** klickar du på **Konfigurera enkel inloggning (SSO).**</span><span class="sxs-lookup"><span data-stu-id="747fc-188">On hello **Security** page, click **Set up single sign-on (SSO).**</span></span>
   
    ![Klicka på enkel inloggning.][11]

10. <span data-ttu-id="747fc-190">Utför följande konfigurationsändringar hello:</span><span class="sxs-lookup"><span data-stu-id="747fc-190">Perform hello following configuration changes:</span></span>
   
    ![Konfigurera enkel inloggning][12]
   
    <span data-ttu-id="747fc-192">a.</span><span class="sxs-lookup"><span data-stu-id="747fc-192">a.</span></span> <span data-ttu-id="747fc-193">Välj **installationsprogrammet SSO med tredjeparts identitetsprovider**.</span><span class="sxs-lookup"><span data-stu-id="747fc-193">Select **Setup SSO with third-party identity provider**.</span></span>

    <span data-ttu-id="747fc-194">b.</span><span class="sxs-lookup"><span data-stu-id="747fc-194">b.</span></span> <span data-ttu-id="747fc-195">I den **inloggning Sidadress** fältet i Google Apps, klistra in hello värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="747fc-195">In the **Sign-in page URL** field in Google Apps, paste hello value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="747fc-196">c.</span><span class="sxs-lookup"><span data-stu-id="747fc-196">c.</span></span> <span data-ttu-id="747fc-197">I hello **URL för utloggning** fältet i Google Apps, klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="747fc-197">In hello **Sign-out page URL** field in Google Apps, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="747fc-198">d.</span><span class="sxs-lookup"><span data-stu-id="747fc-198">d.</span></span> <span data-ttu-id="747fc-199">I hello **ändra lösenord URL** fältet i Google Apps, klistra in hello värdet för **ändra lösenord URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="747fc-199">In hello **Change password URL** field in Google Apps, paste hello value of **Change password URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="747fc-200">e.</span><span class="sxs-lookup"><span data-stu-id="747fc-200">e.</span></span> <span data-ttu-id="747fc-201">I Google Apps för hello **verifieringscertifikat**, överför hello certifikat som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="747fc-201">In Google Apps, for hello **Verification certificate**, upload hello certificate that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="747fc-202">f.</span><span class="sxs-lookup"><span data-stu-id="747fc-202">f.</span></span> <span data-ttu-id="747fc-203">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="747fc-203">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="747fc-204">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="747fc-204">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="747fc-205">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="747fc-205">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="747fc-206">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="747fc-206">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="747fc-207">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="747fc-207">Creating an Azure AD test user</span></span>
<span data-ttu-id="747fc-208">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="747fc-208">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="747fc-210">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="747fc-210">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="747fc-211">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="747fc-211">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="747fc-213">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="747fc-213">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="747fc-215">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="747fc-215">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="747fc-217">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="747fc-217">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="747fc-219">a.</span><span class="sxs-lookup"><span data-stu-id="747fc-219">a.</span></span> <span data-ttu-id="747fc-220">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="747fc-220">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="747fc-221">b.</span><span class="sxs-lookup"><span data-stu-id="747fc-221">b.</span></span> <span data-ttu-id="747fc-222">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="747fc-222">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="747fc-223">c.</span><span class="sxs-lookup"><span data-stu-id="747fc-223">c.</span></span> <span data-ttu-id="747fc-224">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="747fc-224">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="747fc-225">d.</span><span class="sxs-lookup"><span data-stu-id="747fc-225">d.</span></span> <span data-ttu-id="747fc-226">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="747fc-226">Click **Create**.</span></span>
 
### <a name="creating-a-google-apps-test-user"></a><span data-ttu-id="747fc-227">Skapa en testanvändare för Google Apps</span><span class="sxs-lookup"><span data-stu-id="747fc-227">Creating a Google Apps test user</span></span>

<span data-ttu-id="747fc-228">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Google Apps programvara.</span><span class="sxs-lookup"><span data-stu-id="747fc-228">hello objective of this section is toocreate a user called Britta Simon in Google Apps Software.</span></span> <span data-ttu-id="747fc-229">Google Apps har stöd för automatisk etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="747fc-229">Google Apps supports auto provisioning, which is by default enabled.</span></span> <span data-ttu-id="747fc-230">Det finns inga åtgärder i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="747fc-230">There is no action for you in this section.</span></span> <span data-ttu-id="747fc-231">Om en användare inte redan finns i Google Apps programvara, skapas en ny när du försöker tooaccess Google Apps programvara.</span><span class="sxs-lookup"><span data-stu-id="747fc-231">If a user doesn't already exist in Google Apps Software, a new one is created when you attempt tooaccess Google Apps Software.</span></span>

>[!NOTE] 
><span data-ttu-id="747fc-232">Om du behöver toocreate en användare manuellt kan kontakta hello [Google supportteamet](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="747fc-232">If you need toocreate a user manually, contact hello [Google support team](https://www.google.com/contact/).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="747fc-233">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="747fc-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="747fc-234">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooGoogle appar.</span><span class="sxs-lookup"><span data-stu-id="747fc-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGoogle Apps.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="747fc-236">**tooassign Britta Simon tooGoogle appar, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="747fc-236">**tooassign Britta Simon tooGoogle Apps, perform hello following steps:**</span></span>

1. <span data-ttu-id="747fc-237">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="747fc-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="747fc-239">Välj i listan med program hello **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="747fc-239">In hello applications list, select **Google Apps**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. <span data-ttu-id="747fc-241">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="747fc-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="747fc-243">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="747fc-243">Click **Add** button.</span></span> <span data-ttu-id="747fc-244">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="747fc-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="747fc-246">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="747fc-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="747fc-247">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="747fc-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="747fc-248">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="747fc-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="747fc-249">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="747fc-249">Testing single sign-on</span></span>

<span data-ttu-id="747fc-250">I det här avsnittet tootest enkel inloggning inställningarna, öppna hello åtkomstpanelen på [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md)sedan logga in på hello testkonto och klicka på **Google Apps** panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="747fc-250">In this section, tootest your single sign-on settings, open hello Access Panel at [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), then sign into hello test account, and click **Google Apps** tile in hello Access Panel.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="747fc-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="747fc-251">Additional resources</span></span>

* [<span data-ttu-id="747fc-252">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="747fc-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="747fc-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="747fc-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="747fc-254">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="747fc-254">Configure User Provisioning</span></span>](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png