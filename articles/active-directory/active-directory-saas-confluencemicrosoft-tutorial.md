---
title: "Självstudier: Azure Active Directory-integrering med antal samverkande SAML SSO av Microsoft | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och växer samman SAML SSO av Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ace23800e3908c8125052b4a2edcaae71f19935d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a><span data-ttu-id="bb37c-103">Självstudier: Azure Active Directory-integrering med antal samverkande SAML SSO av Microsoft</span><span class="sxs-lookup"><span data-stu-id="bb37c-103">Tutorial: Azure Active Directory integration with Confluence SAML SSO by Microsoft</span></span>

<span data-ttu-id="bb37c-104">I kursen får du lära dig hur toointegrate växer samman SAML SSO av Microsoft med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="bb37c-104">In this tutorial, you learn how toointegrate Confluence SAML SSO by Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bb37c-105">Integrera växer samman SAML SSO av Microsoft med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bb37c-105">Integrating Confluence SAML SSO by Microsoft with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bb37c-106">Du kan styra i Azure AD som har åtkomst tooConfluence SAML SSO av Microsoft</span><span class="sxs-lookup"><span data-stu-id="bb37c-106">You can control in Azure AD who has access tooConfluence SAML SSO by Microsoft</span></span>
- <span data-ttu-id="bb37c-107">Du kan aktivera din användare tooautomatically get inloggade tooConfluence SAML SSO av Microsoft (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="bb37c-107">You can enable your users tooautomatically get signed-on tooConfluence SAML SSO by Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bb37c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bb37c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bb37c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bb37c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb37c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bb37c-110">Prerequisites</span></span>

<span data-ttu-id="bb37c-111">tooconfigure Azure AD-integrering med antal samverkande SAML SSO av Microsoft, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="bb37c-111">tooconfigure Azure AD integration with Confluence SAML SSO by Microsoft, you need hello following items:</span></span>

- <span data-ttu-id="bb37c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bb37c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bb37c-113">Antal samverkande server installerat program på en Windows 64-bitars server (lokalt eller på hello molnet IaaS-infrastruktur)</span><span class="sxs-lookup"><span data-stu-id="bb37c-113">Confluence server application installed on a Windows 64-bit server (on premise or on hello cloud IaaS infrastructure)</span></span>
- <span data-ttu-id="bb37c-114">Antal samverkande servern är HTTPS-aktiverade</span><span class="sxs-lookup"><span data-stu-id="bb37c-114">Confluence server is HTTPS enabled</span></span>
- <span data-ttu-id="bb37c-115">Obs hello stöds versioner för antal samverkande plugin-programmet enligt under avsnittet.</span><span class="sxs-lookup"><span data-stu-id="bb37c-115">Note hello supported versions for Confluence Plugin are mentioned in below section.</span></span>
- <span data-ttu-id="bb37c-116">Antal samverkande servern kan nås på internet särskilt tooAzure AD inloggningen sidan för verifiering och ska kunna tooreceive hello-token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb37c-116">Confluence server is reachable on internet particularly tooAzure AD Login page for authentication and should able tooreceive hello token from Azure AD</span></span>
- <span data-ttu-id="bb37c-117">Admin-autentiseringsuppgifter anges i antal samverkande</span><span class="sxs-lookup"><span data-stu-id="bb37c-117">Admin credentials are set up in Confluence</span></span>
- <span data-ttu-id="bb37c-118">WebSudo har inaktiverats i antal samverkande</span><span class="sxs-lookup"><span data-stu-id="bb37c-118">WebSudo is disabled in Confluence</span></span>
- <span data-ttu-id="bb37c-119">Testanvändare som skapats i hello växer samman serverprogram</span><span class="sxs-lookup"><span data-stu-id="bb37c-119">Test user created in hello Confluence server application</span></span>

> [!NOTE]
> <span data-ttu-id="bb37c-120">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö för växer samman.</span><span class="sxs-lookup"><span data-stu-id="bb37c-120">tootest hello steps in this tutorial, we do not recommend using a production environment of Confluence.</span></span> <span data-ttu-id="bb37c-121">Testa hello integration först i utvecklings- eller mellanlagring av programmet hello-miljön och Använd hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bb37c-121">Test hello integration first in development or staging environment of hello application and then use hello production environment.</span></span>

<span data-ttu-id="bb37c-122">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bb37c-122">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bb37c-123">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="bb37c-123">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bb37c-124">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bb37c-124">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="supported-versions-of-confluence"></a><span data-ttu-id="bb37c-125">Antal samverkande versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="bb37c-125">Supported versions of Confluence</span></span> 

<span data-ttu-id="bb37c-126">Från och med nu stöds följande antal samverkande-versioner:</span><span class="sxs-lookup"><span data-stu-id="bb37c-126">As of now, following versions of Confluence are supported:</span></span>

- <span data-ttu-id="bb37c-127">Antal samverkande: 5.0 too5.10</span><span class="sxs-lookup"><span data-stu-id="bb37c-127">Confluence: 5.0 too5.10</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bb37c-128">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="bb37c-128">Scenario description</span></span>
<span data-ttu-id="bb37c-129">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="bb37c-129">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bb37c-130">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="bb37c-130">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bb37c-131">Att lägga till växer samman SAML SSO av Microsoft från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="bb37c-131">Adding Confluence SAML SSO by Microsoft from hello gallery</span></span>
2. <span data-ttu-id="bb37c-132">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bb37c-132">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-confluence-saml-sso-by-microsoft-from-hello-gallery"></a><span data-ttu-id="bb37c-133">Att lägga till växer samman SAML SSO av Microsoft från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="bb37c-133">Adding Confluence SAML SSO by Microsoft from hello gallery</span></span>
<span data-ttu-id="bb37c-134">tooconfigure hello-integrering av antal samverkande SAML SSO av Microsoft Azure AD, behöver du tooadd växer samman SAML SSO av Microsoft hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="bb37c-134">tooconfigure hello integration of Confluence SAML SSO by Microsoft into Azure AD, you need tooadd Confluence SAML SSO by Microsoft from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bb37c-135">**tooadd växer samman SAML SSO av Microsoft från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bb37c-135">**tooadd Confluence SAML SSO by Microsoft from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb37c-136">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bb37c-136">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bb37c-138">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-138">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bb37c-139">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-139">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="bb37c-141">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb37c-141">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="bb37c-143">Skriv i sökrutan hello **växer samman SAML SSO av Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-143">In hello search box, type **Confluence SAML SSO by Microsoft**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. <span data-ttu-id="bb37c-145">Markera hello resultat på panelen **växer samman SAML SSO av Microsoft**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="bb37c-145">In hello results panel, select **Confluence SAML SSO by Microsoft**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bb37c-147">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bb37c-147">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bb37c-148">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med antal samverkande SAML SSO av Microsoft baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="bb37c-148">In this section, you configure and test Azure AD single sign-on with Confluence SAML SSO by Microsoft based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bb37c-149">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren växer samman SAML SSO av Microsoft är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bb37c-149">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Confluence SAML SSO by Microsoft is tooa user in Azure AD.</span></span> <span data-ttu-id="bb37c-150">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren växer samman SAML SSO av Microsoft toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="bb37c-150">In other words, a link relationship between an Azure AD user and hello related user in Confluence SAML SSO by Microsoft needs toobe established.</span></span>

<span data-ttu-id="bb37c-151">Antal samverkande SAML SSO av Microsoft, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-151">In Confluence SAML SSO by Microsoft, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bb37c-152">tooconfigure och testa Azure AD enkel inloggning med antal samverkande SAML SSO av Microsoft, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="bb37c-152">tooconfigure and test Azure AD single sign-on with Confluence SAML SSO by Microsoft, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bb37c-153">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-153">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bb37c-154">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bb37c-154">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bb37c-155">**[Skapa ett antal samverkande SAML SSO av Microsoft testanvändare](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  -toohave en motsvarighet för Britta Simon växer samman SAML SSO från Microsoft som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="bb37c-155">**[Creating a Confluence SAML SSO by Microsoft test user](#creating-a-confluence-saml-sso-by-microsoft-test-user)** - toohave a counterpart of Britta Simon in Confluence SAML SSO by Microsoft that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bb37c-156">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bb37c-156">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bb37c-157">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="bb37c-157">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bb37c-158">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bb37c-158">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bb37c-159">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din växer samman SAML SSO av Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="bb37c-159">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Confluence SAML SSO by Microsoft application.</span></span>

<span data-ttu-id="bb37c-160">**tooconfigure Azure AD enkel inloggning med antal samverkande SAML SSO av Microsoft, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="bb37c-160">**tooconfigure Azure AD single sign-on with Confluence SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb37c-161">I hello Azure-portalen på hello **växer samman SAML SSO av Microsoft** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-161">In hello Azure portal, on hello **Confluence SAML SSO by Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="bb37c-163">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bb37c-163">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. <span data-ttu-id="bb37c-165">På hello **växer samman SAML SSO av URL: er och Microsoft Domain** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb37c-165">On hello **Confluence SAML SSO by Microsoft Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    <span data-ttu-id="bb37c-167">a.</span><span class="sxs-lookup"><span data-stu-id="bb37c-167">a.</span></span> <span data-ttu-id="bb37c-168">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="bb37c-168">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    <span data-ttu-id="bb37c-169">b.</span><span class="sxs-lookup"><span data-stu-id="bb37c-169">b.</span></span> <span data-ttu-id="bb37c-170">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain:port>/`</span><span class="sxs-lookup"><span data-stu-id="bb37c-170">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<domain:port>/`</span></span>

    <span data-ttu-id="bb37c-171">c.</span><span class="sxs-lookup"><span data-stu-id="bb37c-171">c.</span></span> <span data-ttu-id="bb37c-172">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="bb37c-172">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bb37c-173">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="bb37c-173">These values are not real.</span></span> <span data-ttu-id="bb37c-174">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="bb37c-174">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="bb37c-175">Porten är valfria om det är en namngiven URL.</span><span class="sxs-lookup"><span data-stu-id="bb37c-175">Port is optional in case it’s a named URL.</span></span> <span data-ttu-id="bb37c-176">Dessa värden tas emot under hello konfigurationen av antal samverkande plugin som beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-176">These values are received during hello configuration of Confluence plugin, which is explained later in hello tutorial.</span></span>
 

4. <span data-ttu-id="bb37c-177">toogenerate hello **Metadata** url, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb37c-177">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="bb37c-178">a.</span><span class="sxs-lookup"><span data-stu-id="bb37c-178">a.</span></span> <span data-ttu-id="bb37c-179">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-179">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    <span data-ttu-id="bb37c-181">b.</span><span class="sxs-lookup"><span data-stu-id="bb37c-181">b.</span></span> <span data-ttu-id="bb37c-182">Klicka på **slutpunkter** tooopen **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb37c-182">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    <span data-ttu-id="bb37c-184">c.</span><span class="sxs-lookup"><span data-stu-id="bb37c-184">c.</span></span> <span data-ttu-id="bb37c-185">Klicka på hello Kopiera knappen toocopy **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="bb37c-185">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    <span data-ttu-id="bb37c-187">d.</span><span class="sxs-lookup"><span data-stu-id="bb37c-187">d.</span></span> <span data-ttu-id="bb37c-188">Gå nu toohello egenskapssida **växer samman SAML SSO av Microsoft** och kopiera hello **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="bb37c-188">Now go toohello property page of **Confluence SAML SSO by Microsoft** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    <span data-ttu-id="bb37c-190">e.</span><span class="sxs-lookup"><span data-stu-id="bb37c-190">e.</span></span> <span data-ttu-id="bb37c-191">Generera hello **URL för tjänstmetadata** med hello följer mönstret: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` och kopiera det här värdet i anteckningar eftersom den används senare för att konfigurera hello hello plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="bb37c-191">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` and copy this value in notepad as it is used later for hello configuration of hello plugin.</span></span>

5. <span data-ttu-id="bb37c-192">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-192">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bb37c-194">Kontakta [Microsoft](mailto:waadpartners@microsoft.com) med hello följande information för hello växer samman plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="bb37c-194">Contact [Microsoft](mailto:waadpartners@microsoft.com) with hello following information for hello Confluence plugin.</span></span>
    
    *   <span data-ttu-id="bb37c-195">Kundnamn:</span><span class="sxs-lookup"><span data-stu-id="bb37c-195">Customer Name:</span></span>
    *   <span data-ttu-id="bb37c-196">Namn på primär domän:</span><span class="sxs-lookup"><span data-stu-id="bb37c-196">Primary domain name:</span></span>
    *   <span data-ttu-id="bb37c-197">Azure AD Premium: Ja/Nej (plugin-programmet blir tillgängliga tooall hello kund lediga, grundläggande och Premium-SKU)</span><span class="sxs-lookup"><span data-stu-id="bb37c-197">Azure AD Premium: Yes/No (Plugin will be available tooall     hello customer Free, Basic, and Premium SKU)</span></span>
    *   <span data-ttu-id="bb37c-198">Antalet användare som kommer att använda den här integreringen:</span><span class="sxs-lookup"><span data-stu-id="bb37c-198">Number of users who will be using this integration:</span></span>
    *   <span data-ttu-id="bb37c-199">Antal samverkande Version:</span><span class="sxs-lookup"><span data-stu-id="bb37c-199">Confluence Version:</span></span>
    *   <span data-ttu-id="bb37c-200">Kommentarer:</span><span class="sxs-lookup"><span data-stu-id="bb37c-200">Comments:</span></span>

7. <span data-ttu-id="bb37c-201">Logga in tooyour växer samman instans som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="bb37c-201">In a different web browser window, log in tooyour Confluence instance as an administrator.</span></span>

8. <span data-ttu-id="bb37c-202">Hovra över kugge och klicka på hello **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-202">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

9. <span data-ttu-id="bb37c-204">Klicka under tillägg fliken avsnitt **Hantera tillägg**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-204">Under Add-ons tab section, click **Manage add-ons**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon72.png)

10. <span data-ttu-id="bb37c-206">Ladda upp hello plugin-program som tillhandahålls av Microsoft manuellt.</span><span class="sxs-lookup"><span data-stu-id="bb37c-206">Manually upload hello plugin provided by Microsoft.</span></span> <span data-ttu-id="bb37c-207">När hello plugin-programmet installeras, visas det i **användaren installerat** tillägg avsnitt i **Hantera tillägg** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bb37c-207">Once hello plugin is installed, it appears in **User Installed** add-ons section of **Manage Add-on** section.</span></span>

11. <span data-ttu-id="bb37c-208">Klicka på **konfigurera** tooconfigure hello nytt plugin-program.</span><span class="sxs-lookup"><span data-stu-id="bb37c-208">Click **Configure** tooconfigure hello new plugin.</span></span>

12. <span data-ttu-id="bb37c-209">Utför följande steg på konfigurationssidan:</span><span class="sxs-lookup"><span data-stu-id="bb37c-209">Perform following steps on configuration page:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon5.png)
 
    <span data-ttu-id="bb37c-211">a.</span><span class="sxs-lookup"><span data-stu-id="bb37c-211">a.</span></span> <span data-ttu-id="bb37c-212">I **URL för tjänstmetadata** klistra in hello **URL för tjänstmetadata** genereras från Azure AD och klicka på hello **lösa** knappen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-212">In **Metadata URL** paste hello **Metadata URL** generated from Azure AD and click hello **Resolve** button.</span></span> <span data-ttu-id="bb37c-213">Den läser hello IdP metadata-URL och fyller i informationen för alla hello-fält.</span><span class="sxs-lookup"><span data-stu-id="bb37c-213">It reads hello IdP metadata URL and populates all hello fields information.</span></span>

    > [!Note]
    > <span data-ttu-id="bb37c-214">Standardplatsen för SAML användar-ID är namnidentifierare.</span><span class="sxs-lookup"><span data-stu-id="bb37c-214">Default SAML User ID location is Name Identifier.</span></span> <span data-ttu-id="bb37c-215">Du kan ändra det här attributet tooan-alternativet och ange hello lämpliga attributets namn.</span><span class="sxs-lookup"><span data-stu-id="bb37c-215">You can change this tooan attribute option and enter hello appropriate attribute name.</span></span>

    > [!TIP]
    > <span data-ttu-id="bb37c-216">Kontrollera att det finns bara ett certifikat mappas mot hello app så att det finns inga fel vid matchning av hello metadata.</span><span class="sxs-lookup"><span data-stu-id="bb37c-216">Ensure that there is only one certificate mapped against hello app so that there is no error in resolving hello metadata.</span></span> <span data-ttu-id="bb37c-217">Om det finns flera certifikat på lösa hello metadata får administratör ett fel.</span><span class="sxs-lookup"><span data-stu-id="bb37c-217">If there are multiple certificates, upon resolving hello metadata, admin gets an error.</span></span>
    
    <span data-ttu-id="bb37c-218">b.</span><span class="sxs-lookup"><span data-stu-id="bb37c-218">b.</span></span> <span data-ttu-id="bb37c-219">Kopiera hello **identifierare, svars-URL: en och URL: en inloggning** värden och klistra in dem i **identifierare, svars-URL: en och URL: en inloggning** textrutor respektive i **växer samman SAML SSO av URL: er och Microsoft Domain**  avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-219">Copy hello **Identifier, Reply URL and Sign on URL** values and paste them in **Identifier, Reply URL and Sign on URL** textboxes respectively in **Confluence SAML SSO by Microsoft Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="bb37c-220">c.</span><span class="sxs-lookup"><span data-stu-id="bb37c-220">c.</span></span> <span data-ttu-id="bb37c-221">I **knappen inloggningsnamnet** hello typnamn knappen organisationen vill hello användare toosee på inloggningsskärmen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-221">In **Login Button Name** type hello name of button your organization wants hello users toosee on login screen.</span></span>

    <span data-ttu-id="bb37c-222">d.</span><span class="sxs-lookup"><span data-stu-id="bb37c-222">d.</span></span> <span data-ttu-id="bb37c-223">I **SAML användar-ID platser** väljer du antingen **användar-ID är i hello NameIdentifier elementet i hello ämne instruktionen** eller **användar-ID är i ett element med attributet**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-223">In **SAML User ID Locations** select either **User ID is in hello NameIdentifier element of hello Subject statement** or **User ID is in an Attribute element**.</span></span>  <span data-ttu-id="bb37c-224">Detta ID har toobe hello växer samman användar-id. Om hello användar-id inte matchas sedan kan inte användare toolog i.</span><span class="sxs-lookup"><span data-stu-id="bb37c-224">This ID has toobe hello Confluence user id. If hello user id is not matched, then system will not allow users toolog in.</span></span> 
    
    <span data-ttu-id="bb37c-225">e.</span><span class="sxs-lookup"><span data-stu-id="bb37c-225">e.</span></span> <span data-ttu-id="bb37c-226">Om du väljer **användar-ID är i ett element med attributet** alternativet i **attributnamn** textruta hello-typnamn för hello-attribut som där användar-Id förväntas.</span><span class="sxs-lookup"><span data-stu-id="bb37c-226">If you select **User ID is in an Attribute element** option, then in **Attribute name** textbox type hello name of hello attribute where User Id is expected.</span></span> 

    <span data-ttu-id="bb37c-227">f.</span><span class="sxs-lookup"><span data-stu-id="bb37c-227">f.</span></span> <span data-ttu-id="bb37c-228">Om du använder hello federerade domänen (t.ex. AD FS etc.) med Azure AD, klicka på hello **aktivera identifiering av startsfär** och konfigurera hello **domännamn**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-228">If you are using hello federated domain (like ADFS etc.) with Azure AD, then click on hello **Enable Home Realm Discovery** option and configure hello **Domain Name**.</span></span>
    
    <span data-ttu-id="bb37c-229">g.</span><span class="sxs-lookup"><span data-stu-id="bb37c-229">g.</span></span> <span data-ttu-id="bb37c-230">I **domännamn** typen hello domännamn här vid hello ADFS-baserade inloggningen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-230">In **Domain Name** type hello domain name here in case of hello ADFS-based login.</span></span>

    <span data-ttu-id="bb37c-231">h.</span><span class="sxs-lookup"><span data-stu-id="bb37c-231">h.</span></span> <span data-ttu-id="bb37c-232">Kontrollera **aktivera enkel inloggning ut** om du vill toolog ut från Azure AD när en användare loggar från växer samman.</span><span class="sxs-lookup"><span data-stu-id="bb37c-232">Check **Enable Single Sign out** if you wish toolog out from Azure AD when a user logs out from Confluence.</span></span> 

    <span data-ttu-id="bb37c-233">Jag.</span><span class="sxs-lookup"><span data-stu-id="bb37c-233">i.</span></span> <span data-ttu-id="bb37c-234">Klicka på **spara** knappen toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="bb37c-234">Click **Save** button toosave hello settings.</span></span>


> [!TIP]
> <span data-ttu-id="bb37c-235">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="bb37c-235">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bb37c-236">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="bb37c-236">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bb37c-237">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bb37c-237">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bb37c-238">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb37c-238">Creating an Azure AD test user</span></span>
<span data-ttu-id="bb37c-239">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bb37c-239">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="bb37c-241">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bb37c-241">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb37c-242">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bb37c-242">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bb37c-244">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-244">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bb37c-246">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb37c-246">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bb37c-248">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb37c-248">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bb37c-250">a.</span><span class="sxs-lookup"><span data-stu-id="bb37c-250">a.</span></span> <span data-ttu-id="bb37c-251">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-251">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bb37c-252">b.</span><span class="sxs-lookup"><span data-stu-id="bb37c-252">b.</span></span> <span data-ttu-id="bb37c-253">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bb37c-253">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bb37c-254">c.</span><span class="sxs-lookup"><span data-stu-id="bb37c-254">c.</span></span> <span data-ttu-id="bb37c-255">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-255">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bb37c-256">d.</span><span class="sxs-lookup"><span data-stu-id="bb37c-256">d.</span></span> <span data-ttu-id="bb37c-257">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-257">Click **Create**.</span></span>
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a><span data-ttu-id="bb37c-258">Skapa ett antal samverkande SAML SSO av Microsoft testanvändare</span><span class="sxs-lookup"><span data-stu-id="bb37c-258">Creating a Confluence SAML SSO by Microsoft test user</span></span>

<span data-ttu-id="bb37c-259">tooenable Azure AD-användare toolog i tooConfluence på lokal server de måste etableras i antal samverkande SAML SSO av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bb37c-259">tooenable Azure AD users toolog in tooConfluence on premise server, they must be provisioned into Confluence SAML SSO by Microsoft.</span></span> <span data-ttu-id="bb37c-260">Antal samverkande SAML SSO av Microsoft är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="bb37c-260">For Confluence SAML SSO by Microsoft, provisioning is a manual task.</span></span>

<span data-ttu-id="bb37c-261">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="bb37c-261">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb37c-262">Logga in tooyour växer samman på lokal server som administratör.</span><span class="sxs-lookup"><span data-stu-id="bb37c-262">Log in tooyour Confluence on premise server as an administrator.</span></span>

2. <span data-ttu-id="bb37c-263">Hovra över kugge och klicka på hello **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-263">Hover on cog and click hello **User management**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. <span data-ttu-id="bb37c-265">Under avsnittet för användare, klickar du på **lägga till användare** fliken. På hello **”Lägg till en användare”** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb37c-265">Under Users section, click **Add users** tab. On hello **“Add a User”** dialog page, perform hello following steps:</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    <span data-ttu-id="bb37c-267">a.</span><span class="sxs-lookup"><span data-stu-id="bb37c-267">a.</span></span> <span data-ttu-id="bb37c-268">I hello **användarnamn** textruta hello e-post för användare som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bb37c-268">In hello **Username** textbox, type hello email of user like Britta Simon.</span></span>

    <span data-ttu-id="bb37c-269">b.</span><span class="sxs-lookup"><span data-stu-id="bb37c-269">b.</span></span> <span data-ttu-id="bb37c-270">I hello **fullständiga namn** textruta typen hello användarens fullständiga namn som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bb37c-270">In hello **Full Name** textbox, type hello full name of user like Britta Simon.</span></span>

    <span data-ttu-id="bb37c-271">c.</span><span class="sxs-lookup"><span data-stu-id="bb37c-271">c.</span></span> <span data-ttu-id="bb37c-272">I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="bb37c-272">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="bb37c-273">d.</span><span class="sxs-lookup"><span data-stu-id="bb37c-273">d.</span></span> <span data-ttu-id="bb37c-274">I hello **lösenord** textruta hello lösenordstyp för Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bb37c-274">In hello **Password** textbox, type hello password for Britta Simon.</span></span>

    <span data-ttu-id="bb37c-275">e.</span><span class="sxs-lookup"><span data-stu-id="bb37c-275">e.</span></span> <span data-ttu-id="bb37c-276">Klicka på **Bekräfta lösenord** hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="bb37c-276">Click **Confirm Password** reenter hello password.</span></span>
    
    <span data-ttu-id="bb37c-277">f.</span><span class="sxs-lookup"><span data-stu-id="bb37c-277">f.</span></span> <span data-ttu-id="bb37c-278">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-278">Click **Add** button.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="bb37c-279">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb37c-279">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="bb37c-280">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooConfluence SAML SSO av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bb37c-280">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooConfluence SAML SSO by Microsoft.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="bb37c-282">**tooassign Britta Simon tooConfluence SAML SSO av Microsoft, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="bb37c-282">**tooassign Britta Simon tooConfluence SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb37c-283">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-283">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="bb37c-285">Välj i listan med program hello **växer samman SAML SSO av Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-285">In hello applications list, select **Confluence SAML SSO by Microsoft**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. <span data-ttu-id="bb37c-287">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="bb37c-287">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="bb37c-289">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-289">Click **Add** button.</span></span> <span data-ttu-id="bb37c-290">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb37c-290">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="bb37c-292">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="bb37c-292">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bb37c-293">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb37c-293">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bb37c-294">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb37c-294">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bb37c-295">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bb37c-295">Testing single sign-on</span></span>

<span data-ttu-id="bb37c-296">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="bb37c-296">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="bb37c-297">När du klickar på hello växer samman SAML SSO av Microsoft-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour växer samman SAML SSO av Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="bb37c-297">When you click hello Confluence SAML SSO by Microsoft tile in hello Access Panel, you should get automatically signed-on tooyour Confluence SAML SSO by Microsoft application.</span></span>
<span data-ttu-id="bb37c-298">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bb37c-298">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb37c-299">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bb37c-299">Additional resources</span></span>

* [<span data-ttu-id="bb37c-300">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bb37c-300">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bb37c-301">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bb37c-301">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png

