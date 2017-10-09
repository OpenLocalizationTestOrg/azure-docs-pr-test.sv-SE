---
title: "Självstudier: Azure Active Directory-integrering med eDigitalResearch | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och eDigitalResearch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c6b66ea0-16ba-45b4-b550-e81c56262b1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 6dd3cafb25ef8ede3a4c16902ed8da69cb7b715f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-edigitalresearch"></a><span data-ttu-id="5f391-103">Självstudier: Azure Active Directory-integrering med eDigitalResearch</span><span class="sxs-lookup"><span data-stu-id="5f391-103">Tutorial: Azure Active Directory integration with eDigitalResearch</span></span>

<span data-ttu-id="5f391-104">I kursen får du lära dig hur toointegrate eDigitalResearch med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5f391-104">In this tutorial, you learn how toointegrate eDigitalResearch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5f391-105">Integrera eDigitalResearch med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5f391-105">Integrating eDigitalResearch with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5f391-106">Du kan styra i Azure AD som har åtkomst till tooeDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="5f391-106">You can control in Azure AD who has access tooeDigitalResearch.</span></span>
- <span data-ttu-id="5f391-107">Du kan låta dina användare tooautomatically get inloggade tooeDigitalResearch (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="5f391-107">You can enable your users tooautomatically get signed-on tooeDigitalResearch (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="5f391-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5f391-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="5f391-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5f391-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f391-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5f391-110">Prerequisites</span></span>

<span data-ttu-id="5f391-111">tooconfigure Azure AD-integrering med eDigitalResearch, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5f391-111">tooconfigure Azure AD integration with eDigitalResearch, you need hello following items:</span></span>

- <span data-ttu-id="5f391-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5f391-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5f391-113">En eDigitalResearch enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="5f391-113">A eDigitalResearch single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5f391-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5f391-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5f391-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5f391-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5f391-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5f391-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5f391-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5f391-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5f391-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5f391-118">Scenario description</span></span>
<span data-ttu-id="5f391-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5f391-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5f391-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5f391-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5f391-121">Att lägga till eDigitalResearch från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5f391-121">Adding eDigitalResearch from hello gallery</span></span>
2. <span data-ttu-id="5f391-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5f391-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-edigitalresearch-from-hello-gallery"></a><span data-ttu-id="5f391-123">Att lägga till eDigitalResearch från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5f391-123">Adding eDigitalResearch from hello gallery</span></span>
<span data-ttu-id="5f391-124">tooconfigure hello integrering av eDigitalResearch i Azure AD, behöver du tooadd eDigitalResearch hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5f391-124">tooconfigure hello integration of eDigitalResearch into Azure AD, you need tooadd eDigitalResearch from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5f391-125">**tooadd eDigitalResearch från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5f391-125">**tooadd eDigitalResearch from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f391-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5f391-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="5f391-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5f391-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5f391-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5f391-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="5f391-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5f391-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="5f391-133">Skriv i sökrutan hello **eDigitalResearch**väljer **eDigitalResearch** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5f391-133">In hello search box, type **eDigitalResearch**, select **eDigitalResearch** from result panel then click **Add** button tooadd hello application.</span></span>

    ![eDigitalResearch i hello resultatlistan](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5f391-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5f391-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="5f391-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med eDigitalResearch baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5f391-136">In this section, you configure and test Azure AD single sign-on with eDigitalResearch based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5f391-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i eDigitalResearch är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f391-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in eDigitalResearch is tooa user in Azure AD.</span></span> <span data-ttu-id="5f391-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i eDigitalResearch toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5f391-138">In other words, a link relationship between an Azure AD user and hello related user in eDigitalResearch needs toobe established.</span></span>

<span data-ttu-id="5f391-139">I eDigitalResearch, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5f391-139">In eDigitalResearch, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5f391-140">tooconfigure och testa Azure AD enkel inloggning med eDigitalResearch, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5f391-140">tooconfigure and test Azure AD single sign-on with eDigitalResearch, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5f391-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5f391-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5f391-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f391-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5f391-143">**[Skapa en testanvändare eDigitalResearch](#create-a-edigitalresearch-test-user)**  -toohave en motsvarighet för Britta Simon i eDigitalResearch som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5f391-143">**[Create a eDigitalResearch test user](#create-a-edigitalresearch-test-user)** - toohave a counterpart of Britta Simon in eDigitalResearch that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5f391-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5f391-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5f391-145">**[Testa enkel inloggning](#test-single-sign-on)**  tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5f391-145">**[Test single sign-on](#test-single-sign-on)**  tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="5f391-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5f391-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="5f391-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt eDigitalResearch program.</span><span class="sxs-lookup"><span data-stu-id="5f391-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your eDigitalResearch application.</span></span>

<span data-ttu-id="5f391-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med eDigitalResearch:**</span><span class="sxs-lookup"><span data-stu-id="5f391-148">**tooconfigure Azure AD single sign-on with eDigitalResearch, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f391-149">I hello Azure-portalen på hello **eDigitalResearch** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5f391-149">In hello Azure portal, on hello **eDigitalResearch** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="5f391-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5f391-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_samlbase.png)

3. <span data-ttu-id="5f391-153">På hello **eDigitalResearch domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5f391-153">On hello **eDigitalResearch Domain and URLs** section, perform hello following steps:</span></span>

    ![eDigitalResearch domän URL: er och enkel inloggning information](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_url.png)

    <span data-ttu-id="5f391-155">a.</span><span class="sxs-lookup"><span data-stu-id="5f391-155">a.</span></span> <span data-ttu-id="5f391-156">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company-name>.edigitalresearch.com`</span><span class="sxs-lookup"><span data-stu-id="5f391-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company-name>.edigitalresearch.com`</span></span>

    <span data-ttu-id="5f391-157">b.</span><span class="sxs-lookup"><span data-stu-id="5f391-157">b.</span></span> <span data-ttu-id="5f391-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company-name>.edigitalresearch.com/login/consume`</span><span class="sxs-lookup"><span data-stu-id="5f391-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company-name>.edigitalresearch.com/login/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5f391-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="5f391-159">These values are not real.</span></span> <span data-ttu-id="5f391-160">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="5f391-160">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="5f391-161">Kontakta [eDigitalResearch supportteamet](http://www.maruedr.com/contact) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="5f391-161">Contact [eDigitalResearch support team](http://www.maruedr.com/contact) tooget these values.</span></span>
 


4. <span data-ttu-id="5f391-162">På hello **SAML-signeringscertifikat** klickar du på **certifikat Base(64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="5f391-162">On hello **SAML Signing Certificate** section, click **Certificate Base(64)** and then save hello certificate file on your computer.</span></span>

    <span data-ttu-id="5f391-163">!</span><span class="sxs-lookup"><span data-stu-id="5f391-163">!</span></span>![länk för hämtning av hello-certifikat](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_certificate.png) 

5. <span data-ttu-id="5f391-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5f391-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5f391-167">På hello **eDigitalResearch Configuration** klickar du på **konfigurera eDigitalResearch** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5f391-167">On hello **eDigitalResearch Configuration** section, click **Configure eDigitalResearch** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5f391-168">Kopiera hello **Sign-Out URL, SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5f391-168">Copy hello **Sign-Out URL, SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![eDigitalResearch konfiguration](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_configure.png) 

7. <span data-ttu-id="5f391-170">tooconfigure enkel inloggning på **eDigitalResearch** sida, behöver du toosend hello hämtas **certifikatfil (Base64)**, **SAML enhets-ID**, och  **URL för utloggning** för[eDigitalResearch supportteamet](http://www.maruedr.com/contact).</span><span class="sxs-lookup"><span data-stu-id="5f391-170">tooconfigure single sign-on on **eDigitalResearch** side, you need toosend hello downloaded **Certificate (Base64) File**, **SAML Entity ID**, and **Sign-Out URL** too[eDigitalResearch support team](http://www.maruedr.com/contact).</span></span> <span data-ttu-id="5f391-171">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="5f391-171">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="5f391-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="5f391-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5f391-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="5f391-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5f391-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5f391-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5f391-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f391-175">Create an Azure AD test user</span></span>

<span data-ttu-id="5f391-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f391-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="5f391-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5f391-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f391-179">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="5f391-179">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="5f391-181">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5f391-181">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="5f391-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5f391-183">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="5f391-185">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5f391-185">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_04.png)

    <span data-ttu-id="5f391-187">a.</span><span class="sxs-lookup"><span data-stu-id="5f391-187">a.</span></span> <span data-ttu-id="5f391-188">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5f391-188">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5f391-189">b.</span><span class="sxs-lookup"><span data-stu-id="5f391-189">b.</span></span> <span data-ttu-id="5f391-190">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f391-190">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="5f391-191">c.</span><span class="sxs-lookup"><span data-stu-id="5f391-191">c.</span></span> <span data-ttu-id="5f391-192">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="5f391-192">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="5f391-193">d.</span><span class="sxs-lookup"><span data-stu-id="5f391-193">d.</span></span> <span data-ttu-id="5f391-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5f391-194">Click **Create**.</span></span>
  
### <a name="create-a-edigitalresearch-test-user"></a><span data-ttu-id="5f391-195">Skapa en testanvändare eDigitalResearch</span><span class="sxs-lookup"><span data-stu-id="5f391-195">Create a eDigitalResearch test user</span></span>

<span data-ttu-id="5f391-196">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="5f391-196">hello objective of this section is toocreate a user called Britta Simon in eDigitalResearch.</span></span> 

<span data-ttu-id="5f391-197">Arbeta med hello [eDigitalResearch supportteamet](http://www.maruedr.com/contact) tooget användare som har skapats.</span><span class="sxs-lookup"><span data-stu-id="5f391-197">Work with hello [eDigitalResearch support team](http://www.maruedr.com/contact) tooget users created.</span></span>       
    
 > [!NOTE]
 > <span data-ttu-id="5f391-198">hello Azure Active Directory användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="5f391-198">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="5f391-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f391-199">Assign hello Azure AD test user</span></span>

<span data-ttu-id="5f391-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooeDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="5f391-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooeDigitalResearch.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="5f391-202">**tooassign Britta Simon tooeDigitalResearch utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5f391-202">**tooassign Britta Simon tooeDigitalResearch, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f391-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5f391-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5f391-205">Välj i listan med program hello **eDigitalResearch**.</span><span class="sxs-lookup"><span data-stu-id="5f391-205">In hello applications list, select **eDigitalResearch**.</span></span>

    ![Hej eDigitalResearch länken i listan med program hello](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_app.png)  

3. <span data-ttu-id="5f391-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5f391-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="5f391-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5f391-209">Click **Add** button.</span></span> <span data-ttu-id="5f391-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5f391-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="5f391-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5f391-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5f391-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5f391-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5f391-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5f391-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5f391-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5f391-215">Test single sign-on</span></span>

<span data-ttu-id="5f391-216">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5f391-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5f391-217">Du bör få automatiskt inloggade tooyour eDigitalResearch programmet när du klickar på hello eDigitalResearch panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5f391-217">When you click hello eDigitalResearch tile in hello Access Panel, you should get automatically signed-on tooyour eDigitalResearch application.</span></span>
<span data-ttu-id="5f391-218">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5f391-218">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5f391-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5f391-219">Additional resources</span></span>

* [<span data-ttu-id="5f391-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5f391-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5f391-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5f391-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_203.png

