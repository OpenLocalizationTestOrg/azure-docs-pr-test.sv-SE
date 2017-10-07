---
title: "Självstudier: Azure Active Directory-integrering med JIRA SAML SSO av Microsoft | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och JIRA SAML SSO av Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0dc847b9-eec4-4c31-845e-0144ddedc4a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 178c4c040d9939bca271ac185ca5c2feb14f1247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a><span data-ttu-id="140b0-103">Självstudier: Azure Active Directory-integrering med JIRA SAML SSO av Microsoft</span><span class="sxs-lookup"><span data-stu-id="140b0-103">Tutorial: Azure Active Directory integration with JIRA SAML SSO by Microsoft</span></span>

<span data-ttu-id="140b0-104">I kursen får du lära dig hur toointegrate JIRA SAML SSO av Microsoft med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="140b0-104">In this tutorial, you learn how toointegrate JIRA SAML SSO by Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="140b0-105">Integrera JIRA SAML SSO av Microsoft med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="140b0-105">Integrating JIRA SAML SSO by Microsoft with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="140b0-106">Du kan styra i Azure AD som har åtkomst tooJIRA SAML SSO av Microsoft</span><span class="sxs-lookup"><span data-stu-id="140b0-106">You can control in Azure AD who has access tooJIRA SAML SSO by Microsoft</span></span>
- <span data-ttu-id="140b0-107">Du kan aktivera din användare tooautomatically get inloggade tooJIRA SAML SSO av Microsoft (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="140b0-107">You can enable your users tooautomatically get signed-on tooJIRA SAML SSO by Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="140b0-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="140b0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="140b0-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="140b0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="140b0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="140b0-110">Prerequisites</span></span>

<span data-ttu-id="140b0-111">tooconfigure Azure AD-integrering med JIRA SAML SSO av Microsoft, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="140b0-111">tooconfigure Azure AD integration with JIRA SAML SSO by Microsoft, you need hello following items:</span></span>

- <span data-ttu-id="140b0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="140b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="140b0-113">JIRA server installerat program på en Windows 64-bitars server (lokalt eller på hello molnet IaaS-infrastruktur)</span><span class="sxs-lookup"><span data-stu-id="140b0-113">JIRA server application installed on a Windows 64-bit server (on premise or on hello cloud IaaS infrastructure)</span></span>
- <span data-ttu-id="140b0-114">JIRA server är HTTPS aktiverat</span><span class="sxs-lookup"><span data-stu-id="140b0-114">JIRA server is HTTPS enabled</span></span>
- <span data-ttu-id="140b0-115">Obs hello stöds versioner för plugin-programmet för JIRA enligt under avsnittet.</span><span class="sxs-lookup"><span data-stu-id="140b0-115">Note hello supported versions for JIRA Plugin are mentioned in below section.</span></span>
- <span data-ttu-id="140b0-116">JIRA servern kan nås på internet särskilt tooAzure AD inloggningen sidan för verifiering och ska kunna tooreceive hello-token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="140b0-116">JIRA server is reachable on internet particularly tooAzure AD Login page for authentication and should able tooreceive hello token from Azure AD</span></span>
- <span data-ttu-id="140b0-117">Admin-autentiseringsuppgifter anges i JIRA</span><span class="sxs-lookup"><span data-stu-id="140b0-117">Admin credentials are set up in JIRA</span></span>
- <span data-ttu-id="140b0-118">WebSudo har inaktiverats i JIRA</span><span class="sxs-lookup"><span data-stu-id="140b0-118">WebSudo is disabled in JIRA</span></span>
- <span data-ttu-id="140b0-119">Testanvändare som skapats i hello JIRA serverprogram</span><span class="sxs-lookup"><span data-stu-id="140b0-119">Test user created in hello JIRA server application</span></span>

> [!NOTE]
> <span data-ttu-id="140b0-120">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö för JIRA.</span><span class="sxs-lookup"><span data-stu-id="140b0-120">tootest hello steps in this tutorial, we do not recommend using a production environment of JIRA.</span></span> <span data-ttu-id="140b0-121">Testa hello integration först i utvecklings- eller mellanlagring av programmet hello-miljön och Använd hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="140b0-121">Test hello integration first in development or staging environment of hello application and then use hello production environment.</span></span>

<span data-ttu-id="140b0-122">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="140b0-122">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="140b0-123">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="140b0-123">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="140b0-124">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="140b0-124">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="supported-versions-of-jira"></a><span data-ttu-id="140b0-125">Versioner som stöds av JIRA</span><span class="sxs-lookup"><span data-stu-id="140b0-125">Supported versions of JIRA</span></span> 

<span data-ttu-id="140b0-126">Från och med nu stöds följande versioner av JIRA:</span><span class="sxs-lookup"><span data-stu-id="140b0-126">As of now, following versions of JIRA are supported:</span></span>

- <span data-ttu-id="140b0-127">JIRA Core- och programvara: 6.0 too7.2.0</span><span class="sxs-lookup"><span data-stu-id="140b0-127">JIRA Core and Software: 6.0 too7.2.0</span></span>
- <span data-ttu-id="140b0-128">JIRA helpdesk: 3.0 too3.2</span><span class="sxs-lookup"><span data-stu-id="140b0-128">JIRA Service Desk: 3.0 too3.2</span></span>

## <a name="scenario-description"></a><span data-ttu-id="140b0-129">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="140b0-129">Scenario description</span></span>
<span data-ttu-id="140b0-130">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="140b0-130">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="140b0-131">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="140b0-131">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="140b0-132">Att lägga till JIRA SAML SSO av Microsoft från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="140b0-132">Adding JIRA SAML SSO by Microsoft from hello gallery</span></span>
2. <span data-ttu-id="140b0-133">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="140b0-133">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jira-saml-sso-by-microsoft-from-hello-gallery"></a><span data-ttu-id="140b0-134">Att lägga till JIRA SAML SSO av Microsoft från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="140b0-134">Adding JIRA SAML SSO by Microsoft from hello gallery</span></span>
<span data-ttu-id="140b0-135">tooconfigure hello-integrering av JIRA SAML SSO av Microsoft Azure AD, behöver du tooadd JIRA SAML SSO av Microsoft hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="140b0-135">tooconfigure hello integration of JIRA SAML SSO by Microsoft into Azure AD, you need tooadd JIRA SAML SSO by Microsoft from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="140b0-136">**tooadd JIRA SAML SSO av Microsoft från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="140b0-136">**tooadd JIRA SAML SSO by Microsoft from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="140b0-137">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="140b0-137">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="140b0-139">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="140b0-139">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="140b0-140">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="140b0-140">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="140b0-142">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="140b0-142">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="140b0-144">Skriv i sökrutan hello **JIRA SAML SSO av Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="140b0-144">In hello search box, type **JIRA SAML SSO by Microsoft**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_search.png)

5. <span data-ttu-id="140b0-146">Markera hello resultat på panelen **JIRA SAML SSO av Microsoft**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="140b0-146">In hello results panel, select **JIRA SAML SSO by Microsoft**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="140b0-148">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="140b0-148">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="140b0-149">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med JIRA SAML SSO av Microsoft baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="140b0-149">In this section, you configure and test Azure AD single sign-on with JIRA SAML SSO by Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="140b0-150">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren JIRA SAML SSO av Microsoft är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="140b0-150">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in JIRA SAML SSO by Microsoft is tooa user in Azure AD.</span></span> <span data-ttu-id="140b0-151">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren JIRA SAML SSO av Microsoft toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="140b0-151">In other words, a link relationship between an Azure AD user and hello related user in JIRA SAML SSO by Microsoft needs toobe established.</span></span>

<span data-ttu-id="140b0-152">JIRA SAML SSO av Microsoft, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="140b0-152">In JIRA SAML SSO by Microsoft, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="140b0-153">tooconfigure och testa Azure AD enkel inloggning med JIRA SAML SSO av Microsoft, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="140b0-153">tooconfigure and test Azure AD single sign-on with JIRA SAML SSO by Microsoft, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="140b0-154">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="140b0-154">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="140b0-155">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="140b0-155">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="140b0-156">**[Skapa en JIRA SAML SSO av Microsoft testanvändare](#creating-a-jira-saml-sso-by-microsoft-test-user)**  -toohave en motsvarighet för Britta Simon JIRA SAML SSO från Microsoft som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="140b0-156">**[Creating a JIRA SAML SSO by Microsoft test user](#creating-a-jira-saml-sso-by-microsoft-test-user)** - toohave a counterpart of Britta Simon in JIRA SAML SSO by Microsoft that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="140b0-157">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="140b0-157">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="140b0-158">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="140b0-158">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="140b0-159">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="140b0-159">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="140b0-160">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din JIRA SAML SSO av Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="140b0-160">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your JIRA SAML SSO by Microsoft application.</span></span>

<span data-ttu-id="140b0-161">**tooconfigure Azure AD enkel inloggning med JIRA SAML SSO av Microsoft, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="140b0-161">**tooconfigure Azure AD single sign-on with JIRA SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="140b0-162">I hello Azure-portalen på hello **JIRA SAML SSO av Microsoft** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="140b0-162">In hello Azure portal, on hello **JIRA SAML SSO by Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="140b0-164">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="140b0-164">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_samlbase.png)

3. <span data-ttu-id="140b0-166">På hello **JIRA SAML SSO av URL: er och Microsoft Domain** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="140b0-166">On hello **JIRA SAML SSO by Microsoft Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_url.png)

    <span data-ttu-id="140b0-168">a.</span><span class="sxs-lookup"><span data-stu-id="140b0-168">a.</span></span> <span data-ttu-id="140b0-169">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="140b0-169">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    <span data-ttu-id="140b0-170">b.</span><span class="sxs-lookup"><span data-stu-id="140b0-170">b.</span></span> <span data-ttu-id="140b0-171">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain:port>/`</span><span class="sxs-lookup"><span data-stu-id="140b0-171">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<domain:port>/`</span></span>

    <span data-ttu-id="140b0-172">c.</span><span class="sxs-lookup"><span data-stu-id="140b0-172">c.</span></span> <span data-ttu-id="140b0-173">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="140b0-173">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="140b0-174">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="140b0-174">These values are not real.</span></span> <span data-ttu-id="140b0-175">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="140b0-175">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="140b0-176">Porten är valfria om det är en namngiven URL.</span><span class="sxs-lookup"><span data-stu-id="140b0-176">Port is optional in case it’s a named URL.</span></span> <span data-ttu-id="140b0-177">Dessa värden tas emot under hello konfigurationen av Jira plugin som beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="140b0-177">These values are received during hello configuration of Jira plugin, which is explained later in hello tutorial.</span></span>
 
4. <span data-ttu-id="140b0-178">toogenerate hello **Metadata** url, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="140b0-178">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="140b0-179">a.</span><span class="sxs-lookup"><span data-stu-id="140b0-179">a.</span></span> <span data-ttu-id="140b0-180">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="140b0-180">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/appregistrations.png)
   
    <span data-ttu-id="140b0-182">b.</span><span class="sxs-lookup"><span data-stu-id="140b0-182">b.</span></span> <span data-ttu-id="140b0-183">Klicka på **slutpunkter** tooopen **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="140b0-183">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/endpointicon.png)

    <span data-ttu-id="140b0-185">c.</span><span class="sxs-lookup"><span data-stu-id="140b0-185">c.</span></span> <span data-ttu-id="140b0-186">Klicka på hello Kopiera knappen toocopy **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="140b0-186">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/endpoint.png)
     
    <span data-ttu-id="140b0-188">d.</span><span class="sxs-lookup"><span data-stu-id="140b0-188">d.</span></span> <span data-ttu-id="140b0-189">Gå nu toohello egenskapssida **JIRA SAML SSO av Microsoft** och kopiera hello **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="140b0-189">Now go toohello property page of **JIRA SAML SSO by Microsoft** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/appid.png)

    <span data-ttu-id="140b0-191">e.</span><span class="sxs-lookup"><span data-stu-id="140b0-191">e.</span></span> <span data-ttu-id="140b0-192">Generera hello **URL för tjänstmetadata** med hello följer mönstret: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` och kopiera det här värdet i anteckningar eftersom den används senare för att konfigurera hello hello plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="140b0-192">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` and copy this value in notepad as it is used later for hello configuration of hello plugin.</span></span>

5. <span data-ttu-id="140b0-193">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="140b0-193">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="140b0-195">Kontakta [Microsoft](mailto:waadpartners@microsoft.com) med hello följande information för hello JIRA plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="140b0-195">Contact [Microsoft](mailto:waadpartners@microsoft.com) with hello following information for hello JIRA plugin.</span></span>
    
    *   <span data-ttu-id="140b0-196">Kundnamn:</span><span class="sxs-lookup"><span data-stu-id="140b0-196">Customer Name:</span></span>
    *   <span data-ttu-id="140b0-197">Namn på primär domän:</span><span class="sxs-lookup"><span data-stu-id="140b0-197">Primary domain name:</span></span>
    *   <span data-ttu-id="140b0-198">Azure AD Premium: Ja/Nej (plugin-programmet blir tillgängliga tooall hello kund lediga, grundläggande och Premium-SKU)</span><span class="sxs-lookup"><span data-stu-id="140b0-198">Azure AD Premium: Yes/No (Plugin will be available tooall     hello customer Free, Basic, and Premium SKU)</span></span>
    *   <span data-ttu-id="140b0-199">Antalet användare som kommer att använda den här integreringen:</span><span class="sxs-lookup"><span data-stu-id="140b0-199">Number of users who will be using this integration:</span></span>
    *   <span data-ttu-id="140b0-200">JIRA Version:</span><span class="sxs-lookup"><span data-stu-id="140b0-200">JIRA Version:</span></span>
    *   <span data-ttu-id="140b0-201">Kommentarer:</span><span class="sxs-lookup"><span data-stu-id="140b0-201">Comments:</span></span>

7. <span data-ttu-id="140b0-202">Logga in tooyour JIRA-instans som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="140b0-202">In a different web browser window, log in tooyour JIRA instance as an administrator.</span></span>

8. <span data-ttu-id="140b0-203">Hovra över kugge och klicka på hello **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="140b0-203">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/addon1.png)

9. <span data-ttu-id="140b0-205">Klicka under tillägg fliken avsnitt **Hantera tillägg**.</span><span class="sxs-lookup"><span data-stu-id="140b0-205">Under Add-ons tab section, click **Manage add-ons**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/addon7.png)

10. <span data-ttu-id="140b0-207">Ladda upp hello plugin-program som tillhandahålls av Microsoft manuellt.</span><span class="sxs-lookup"><span data-stu-id="140b0-207">Manually upload hello plugin provided by Microsoft.</span></span> <span data-ttu-id="140b0-208">När hello plugin-programmet installeras, visas det i **användaren installerat** tillägg avsnitt i **Hantera tillägg** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="140b0-208">Once hello plugin is installed, it appears in **User Installed** add-ons section of **Manage Add-on** section.</span></span>

11. <span data-ttu-id="140b0-209">Klicka på **konfigurera** tooconfigure hello nytt plugin-program.</span><span class="sxs-lookup"><span data-stu-id="140b0-209">Click **Configure** tooconfigure hello new plugin.</span></span>

12. <span data-ttu-id="140b0-210">Utför följande steg på konfigurationssidan:</span><span class="sxs-lookup"><span data-stu-id="140b0-210">Perform following steps on configuration page:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/addon5.png)
 
    <span data-ttu-id="140b0-212">a.</span><span class="sxs-lookup"><span data-stu-id="140b0-212">a.</span></span> <span data-ttu-id="140b0-213">I **URL för tjänstmetadata** klistra in hello **URL för tjänstmetadata** genereras från Azure AD och klicka på hello **lösa** knappen.</span><span class="sxs-lookup"><span data-stu-id="140b0-213">In **Metadata URL** paste hello **Metadata URL** generated from Azure AD and click hello **Resolve** button.</span></span> <span data-ttu-id="140b0-214">Den läser hello IdP metadata-URL och fyller i informationen för alla hello-fält.</span><span class="sxs-lookup"><span data-stu-id="140b0-214">It reads hello IdP metadata URL and populates all hello fields information.</span></span>

    > [!Note]
    > <span data-ttu-id="140b0-215">Standardplatsen för SAML användar-ID är namnidentifierare.</span><span class="sxs-lookup"><span data-stu-id="140b0-215">Default SAML User ID location is Name Identifier.</span></span> <span data-ttu-id="140b0-216">Du kan ändra det här attributet tooan-alternativet och ange hello lämpliga attributets namn.</span><span class="sxs-lookup"><span data-stu-id="140b0-216">You can change this tooan attribute option and enter hello appropriate attribute name.</span></span>

    > [!TIP]
    > <span data-ttu-id="140b0-217">Kontrollera att det finns bara ett certifikat mappas mot hello app så att det finns inga fel vid matchning av hello metadata.</span><span class="sxs-lookup"><span data-stu-id="140b0-217">Ensure that there is only one certificate mapped against hello app so that there is no error in resolving hello metadata.</span></span> <span data-ttu-id="140b0-218">Om det finns flera certifikat på lösa hello metadata får administratör ett fel.</span><span class="sxs-lookup"><span data-stu-id="140b0-218">If there are multiple certificates, upon resolving hello metadata, admin gets an error.</span></span>
    
    <span data-ttu-id="140b0-219">b.</span><span class="sxs-lookup"><span data-stu-id="140b0-219">b.</span></span> <span data-ttu-id="140b0-220">Kopiera hello **identifierare, svars-URL: en och URL: en inloggning** värden och klistra in dem i **identifierare, svars-URL: en och URL: en inloggning** respektive i textrutorna **JIRA SAML SSO av Microsoft-Domain och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="140b0-220">Copy hello **Identifier, Reply URL and Sign on URL** values and paste them in **Identifier, Reply URL and Sign on URL** textboxes respectively in **JIRA SAML SSO by Microsoft Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="140b0-221">c.</span><span class="sxs-lookup"><span data-stu-id="140b0-221">c.</span></span> <span data-ttu-id="140b0-222">I **knappen inloggningsnamnet** hello typnamn knappen organisationen vill hello användare toosee på inloggningsskärmen.</span><span class="sxs-lookup"><span data-stu-id="140b0-222">In **Login Button Name** type hello name of button your organization wants hello users toosee on login screen.</span></span>

    <span data-ttu-id="140b0-223">d.</span><span class="sxs-lookup"><span data-stu-id="140b0-223">d.</span></span> <span data-ttu-id="140b0-224">I **SAML användar-ID platser** väljer du antingen **användar-ID är i hello NameIdentifier elementet i hello ämne instruktionen** eller **användar-ID är i ett element med attributet**.</span><span class="sxs-lookup"><span data-stu-id="140b0-224">In **SAML User ID Locations** select either **User ID is in hello NameIdentifier element of hello Subject statement** or **User ID is in an Attribute element**.</span></span>  <span data-ttu-id="140b0-225">Detta ID har toobe hello JIRA användar-id. Om hello användar-id inte matchas sedan kan inte användare toolog i.</span><span class="sxs-lookup"><span data-stu-id="140b0-225">This ID has toobe hello JIRA user id. If hello user id is not matched, then system will not allow users toolog in.</span></span> 
    
    <span data-ttu-id="140b0-226">e.</span><span class="sxs-lookup"><span data-stu-id="140b0-226">e.</span></span> <span data-ttu-id="140b0-227">Om du väljer **användar-ID är i ett element med attributet** alternativet i **attributnamn** textruta hello-typnamn för hello-attribut som där användar-Id förväntas.</span><span class="sxs-lookup"><span data-stu-id="140b0-227">If you select **User ID is in an Attribute element** option, then in **Attribute name** textbox type hello name of hello attribute where User Id is expected.</span></span> 

    <span data-ttu-id="140b0-228">f.</span><span class="sxs-lookup"><span data-stu-id="140b0-228">f.</span></span> <span data-ttu-id="140b0-229">Om du använder hello federerade domänen (t.ex. AD FS etc.) med Azure AD, klicka på hello **aktivera identifiering av startsfär** och konfigurera hello **domännamn**.</span><span class="sxs-lookup"><span data-stu-id="140b0-229">If you are using hello federated domain (like ADFS etc.) with Azure AD, then click on hello **Enable Home Realm Discovery** option and configure hello **Domain Name**.</span></span>
    
    <span data-ttu-id="140b0-230">g.</span><span class="sxs-lookup"><span data-stu-id="140b0-230">g.</span></span> <span data-ttu-id="140b0-231">I **domännamn** typen hello domännamn här vid hello ADFS-baserade inloggningen.</span><span class="sxs-lookup"><span data-stu-id="140b0-231">In **Domain Name** type hello domain name here in case of hello ADFS-based login.</span></span>

    <span data-ttu-id="140b0-232">h.</span><span class="sxs-lookup"><span data-stu-id="140b0-232">h.</span></span> <span data-ttu-id="140b0-233">Kontrollera **aktivera enkel inloggning ut** om du vill toolog ut från Azure AD när en användare loggar från JIRA.</span><span class="sxs-lookup"><span data-stu-id="140b0-233">Check **Enable Single Sign out** if you wish toolog out from Azure AD when a user logs out from JIRA.</span></span> 

    <span data-ttu-id="140b0-234">Jag.</span><span class="sxs-lookup"><span data-stu-id="140b0-234">i.</span></span> <span data-ttu-id="140b0-235">Klicka på **spara** knappen toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="140b0-235">Click **Save** button toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="140b0-236">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="140b0-236">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="140b0-237">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="140b0-237">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="140b0-238">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="140b0-238">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="140b0-239">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="140b0-239">Creating an Azure AD test user</span></span>
<span data-ttu-id="140b0-240">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="140b0-240">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="140b0-242">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="140b0-242">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="140b0-243">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="140b0-243">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="140b0-245">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="140b0-245">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="140b0-247">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="140b0-247">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="140b0-249">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="140b0-249">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="140b0-251">a.</span><span class="sxs-lookup"><span data-stu-id="140b0-251">a.</span></span> <span data-ttu-id="140b0-252">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="140b0-252">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="140b0-253">b.</span><span class="sxs-lookup"><span data-stu-id="140b0-253">b.</span></span> <span data-ttu-id="140b0-254">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="140b0-254">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="140b0-255">c.</span><span class="sxs-lookup"><span data-stu-id="140b0-255">c.</span></span> <span data-ttu-id="140b0-256">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="140b0-256">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="140b0-257">d.</span><span class="sxs-lookup"><span data-stu-id="140b0-257">d.</span></span> <span data-ttu-id="140b0-258">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="140b0-258">Click **Create**.</span></span>
 
### <a name="creating-a-jira-saml-sso-by-microsoft-test-user"></a><span data-ttu-id="140b0-259">Skapa en JIRA SAML SSO av Microsoft testanvändare</span><span class="sxs-lookup"><span data-stu-id="140b0-259">Creating a JIRA SAML SSO by Microsoft test user</span></span>

<span data-ttu-id="140b0-260">tooenable Azure AD-användare toolog i tooJIRA lokal server, måste de att etableras i JIRA SAML SSO av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="140b0-260">tooenable Azure AD users toolog in tooJIRA on-premise server, they must be provisioned into JIRA SAML SSO by Microsoft.</span></span> <span data-ttu-id="140b0-261">Etablering är en manuell aktivitet för JIRA SAML SSO av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="140b0-261">For JIRA SAML SSO by Microsoft, provisioning is a manual task.</span></span>

<span data-ttu-id="140b0-262">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="140b0-262">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="140b0-263">Logga in tooyour JIRA lokal server som administratör.</span><span class="sxs-lookup"><span data-stu-id="140b0-263">Log in tooyour JIRA on-premise server as an administrator.</span></span>

2. <span data-ttu-id="140b0-264">Hovra över kugge och klicka på hello **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="140b0-264">Hover on cog and click hello **User management**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-jiramicrosoft-tutorial/user1.png) 

3. <span data-ttu-id="140b0-266">Du är omdirigerade tooAdministrator åtkomst sidan tooenter **lösenord** och på **Bekräfta** knappen.</span><span class="sxs-lookup"><span data-stu-id="140b0-266">You are redirected tooAdministrator Access page tooenter **Password** and click **Confirm** button.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-jiramicrosoft-tutorial/user2.png) 

4. <span data-ttu-id="140b0-268">Under **Användarhantering** avsnittet klickar du på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="140b0-268">Under **User management** tab section, click **create user**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-jiramicrosoft-tutorial/user3.png) 

5. <span data-ttu-id="140b0-270">På hello **”skapa nya användare”** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="140b0-270">On hello **“Create new user”** dialog page, perform hello following steps:</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-jiramicrosoft-tutorial/user4.png) 

    <span data-ttu-id="140b0-272">a.</span><span class="sxs-lookup"><span data-stu-id="140b0-272">a.</span></span> <span data-ttu-id="140b0-273">I hello **e-postadress** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="140b0-273">In hello **Email address** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="140b0-274">b.</span><span class="sxs-lookup"><span data-stu-id="140b0-274">b.</span></span> <span data-ttu-id="140b0-275">I hello **fullständiga namn** textruta typen hello användarens som Britta Simon fullständiga namn.</span><span class="sxs-lookup"><span data-stu-id="140b0-275">In hello **Full Name** textbox, type full name of hello user like Britta Simon.</span></span>

    <span data-ttu-id="140b0-276">c.</span><span class="sxs-lookup"><span data-stu-id="140b0-276">c.</span></span> <span data-ttu-id="140b0-277">I hello **användarnamn** textruta hello e-post för användare som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="140b0-277">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="140b0-278">d.</span><span class="sxs-lookup"><span data-stu-id="140b0-278">d.</span></span> <span data-ttu-id="140b0-279">I hello **lösenord** textruta typen hello lösenord för användare.</span><span class="sxs-lookup"><span data-stu-id="140b0-279">In hello **Password** textbox, type hello password of user.</span></span>

    <span data-ttu-id="140b0-280">e.</span><span class="sxs-lookup"><span data-stu-id="140b0-280">e.</span></span> <span data-ttu-id="140b0-281">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="140b0-281">Click **Create user**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="140b0-282">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="140b0-282">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="140b0-283">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooJIRA SAML SSO av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="140b0-283">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJIRA SAML SSO by Microsoft.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="140b0-285">**tooassign Britta Simon tooJIRA SAML SSO av Microsoft, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="140b0-285">**tooassign Britta Simon tooJIRA SAML SSO by Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="140b0-286">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="140b0-286">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="140b0-288">Välj i listan med program hello **JIRA SAML SSO av Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="140b0-288">In hello applications list, select **JIRA SAML SSO by Microsoft**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_app.png) 

3. <span data-ttu-id="140b0-290">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="140b0-290">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="140b0-292">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="140b0-292">Click **Add** button.</span></span> <span data-ttu-id="140b0-293">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="140b0-293">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="140b0-295">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="140b0-295">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="140b0-296">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="140b0-296">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="140b0-297">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="140b0-297">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="140b0-298">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="140b0-298">Testing single sign-on</span></span>

<span data-ttu-id="140b0-299">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="140b0-299">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="140b0-300">När du klickar på hello JIRA SAML SSO av Microsoft-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour JIRA SAML SSO av Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="140b0-300">When you click hello JIRA SAML SSO by Microsoft tile in hello Access Panel, you should get automatically signed-on tooyour JIRA SAML SSO by Microsoft application.</span></span>
<span data-ttu-id="140b0-301">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="140b0-301">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="140b0-302">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="140b0-302">Additional resources</span></span>

* [<span data-ttu-id="140b0-303">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="140b0-303">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="140b0-304">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="140b0-304">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_203.png

