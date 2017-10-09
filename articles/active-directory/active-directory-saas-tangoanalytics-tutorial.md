---
title: "Självstudier: Azure Active Directory-integrering med Tango Analytics | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Tango analyser."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2f7555d3-e9ba-40b2-9b3a-2f0ab38a4c08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 2076397e746235692f07241650d5556afb3c3d28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tango-analytics"></a><span data-ttu-id="50e72-103">Självstudier: Azure Active Directory-integrering med Tango Analytics</span><span class="sxs-lookup"><span data-stu-id="50e72-103">Tutorial: Azure Active Directory integration with Tango Analytics</span></span>

<span data-ttu-id="50e72-104">I kursen får du lära dig hur toointegrate Tango Analytics med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="50e72-104">In this tutorial, you learn how toointegrate Tango Analytics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="50e72-105">Integrera Tango Analytics med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="50e72-105">Integrating Tango Analytics with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="50e72-106">Du kan styra i Azure AD som har åtkomst tooTango Analytics</span><span class="sxs-lookup"><span data-stu-id="50e72-106">You can control in Azure AD who has access tooTango Analytics</span></span>
- <span data-ttu-id="50e72-107">Du kan aktivera din användare tooautomatically get inloggade tooTango Analytics (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="50e72-107">You can enable your users tooautomatically get signed-on tooTango Analytics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="50e72-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="50e72-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="50e72-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="50e72-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="50e72-110">Krav</span><span class="sxs-lookup"><span data-stu-id="50e72-110">Prerequisites</span></span>

<span data-ttu-id="50e72-111">tooconfigure Azure AD-integrering med Tango Analytics behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="50e72-111">tooconfigure Azure AD integration with Tango Analytics, you need hello following items:</span></span>

- <span data-ttu-id="50e72-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="50e72-112">An Azure AD subscription</span></span>
- <span data-ttu-id="50e72-113">En Tango Analytics enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="50e72-113">A Tango Analytics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="50e72-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="50e72-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="50e72-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="50e72-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="50e72-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="50e72-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="50e72-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="50e72-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="50e72-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="50e72-118">Scenario description</span></span>
<span data-ttu-id="50e72-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="50e72-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="50e72-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="50e72-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="50e72-121">Att lägga till Tango Analytics från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="50e72-121">Adding Tango Analytics from hello gallery</span></span>
2. <span data-ttu-id="50e72-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="50e72-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tango-analytics-from-hello-gallery"></a><span data-ttu-id="50e72-123">Att lägga till Tango Analytics från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="50e72-123">Adding Tango Analytics from hello gallery</span></span>
<span data-ttu-id="50e72-124">tooconfigure hello integrering av Tango Analytics till Azure AD, behöver du tooadd Tango Analytics hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="50e72-124">tooconfigure hello integration of Tango Analytics into Azure AD, you need tooadd Tango Analytics from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="50e72-125">**tooadd Tango Analytics från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="50e72-125">**tooadd Tango Analytics from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="50e72-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="50e72-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="50e72-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="50e72-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="50e72-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="50e72-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="50e72-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="50e72-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="50e72-133">Skriv i sökrutan hello **Tango Analytics**.</span><span class="sxs-lookup"><span data-stu-id="50e72-133">In hello search box, type **Tango Analytics**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_search.png)

5. <span data-ttu-id="50e72-135">Markera hello resultat på panelen **Tango Analytics**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="50e72-135">In hello results panel, select **Tango Analytics**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="50e72-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="50e72-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="50e72-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Tango Analytics baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="50e72-138">In this section, you configure and test Azure AD single sign-on with Tango Analytics based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="50e72-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Tango Analytics är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="50e72-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tango Analytics is tooa user in Azure AD.</span></span> <span data-ttu-id="50e72-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Tango Analytics toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="50e72-140">In other words, a link relationship between an Azure AD user and hello related user in Tango Analytics needs toobe established.</span></span>

<span data-ttu-id="50e72-141">Tilldela hello värdet hello i Tango Analytics **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="50e72-141">In Tango Analytics, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="50e72-142">tooconfigure och testa Azure AD enkel inloggning med Tango Analytics, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="50e72-142">tooconfigure and test Azure AD single sign-on with Tango Analytics, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="50e72-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="50e72-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="50e72-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="50e72-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="50e72-145">**[Skapa en testanvändare Tango Analytics](#creating-a-tango-analytics-test-user)**  -toohave en motsvarighet för Britta Simon i Tango Analytics som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="50e72-145">**[Creating a Tango Analytics test user](#creating-a-tango-analytics-test-user)** - toohave a counterpart of Britta Simon in Tango Analytics that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="50e72-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="50e72-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="50e72-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="50e72-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="50e72-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="50e72-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="50e72-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Tango Analytics.</span><span class="sxs-lookup"><span data-stu-id="50e72-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tango Analytics application.</span></span>

<span data-ttu-id="50e72-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Tango Analytics:**</span><span class="sxs-lookup"><span data-stu-id="50e72-150">**tooconfigure Azure AD single sign-on with Tango Analytics, perform hello following steps:**</span></span>

1. <span data-ttu-id="50e72-151">I hello Azure-portalen på hello **Tango Analytics** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="50e72-151">In hello Azure portal, on hello **Tango Analytics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="50e72-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="50e72-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_samlbase.png)

3. <span data-ttu-id="50e72-155">På hello **Tango Analytics domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="50e72-155">On hello **Tango Analytics Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_url.png)

    <span data-ttu-id="50e72-157">a.</span><span class="sxs-lookup"><span data-stu-id="50e72-157">a.</span></span> <span data-ttu-id="50e72-158">I hello **identifierare** textruta hello TYPVÄRDE`TACORE_SSO`</span><span class="sxs-lookup"><span data-stu-id="50e72-158">In hello **Identifier** textbox, type hello value `TACORE_SSO`</span></span>

    <span data-ttu-id="50e72-159">b.</span><span class="sxs-lookup"><span data-stu-id="50e72-159">b.</span></span> <span data-ttu-id="50e72-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://mts.tangoanalytics.com/saml2/sp/acs/post`</span><span class="sxs-lookup"><span data-stu-id="50e72-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://mts.tangoanalytics.com/saml2/sp/acs/post`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="50e72-161">hello Reply URL-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="50e72-161">hello Reply URL value is not real.</span></span> <span data-ttu-id="50e72-162">Uppdatera den med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="50e72-162">Update this with hello actual Reply URL.</span></span> <span data-ttu-id="50e72-163">Kontakta [Tango Analytics supportteam](mailto:support@tangoanalytics.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="50e72-163">Contact [Tango Analytics support team](mailto:support@tangoanalytics.com) tooget this value.</span></span>

4. <span data-ttu-id="50e72-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="50e72-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_certificate.png) 

5. <span data-ttu-id="50e72-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="50e72-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="50e72-168">tooconfigure enkel inloggning på **Tango Analytics** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Tango Analytics supportteam](mailto:support@tangoanalytics.com).</span><span class="sxs-lookup"><span data-stu-id="50e72-168">tooconfigure single sign-on on **Tango Analytics** side, you need toosend hello downloaded **Metadata XML** too[Tango Analytics support team](mailto:support@tangoanalytics.com).</span></span> <span data-ttu-id="50e72-169">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="50e72-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="50e72-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="50e72-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="50e72-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="50e72-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="50e72-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="50e72-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="50e72-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="50e72-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="50e72-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="50e72-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="50e72-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="50e72-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="50e72-177">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="50e72-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="50e72-179">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="50e72-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="50e72-181">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="50e72-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="50e72-183">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="50e72-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="50e72-185">a.</span><span class="sxs-lookup"><span data-stu-id="50e72-185">a.</span></span> <span data-ttu-id="50e72-186">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="50e72-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="50e72-187">b.</span><span class="sxs-lookup"><span data-stu-id="50e72-187">b.</span></span> <span data-ttu-id="50e72-188">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="50e72-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="50e72-189">c.</span><span class="sxs-lookup"><span data-stu-id="50e72-189">c.</span></span> <span data-ttu-id="50e72-190">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="50e72-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="50e72-191">d.</span><span class="sxs-lookup"><span data-stu-id="50e72-191">d.</span></span> <span data-ttu-id="50e72-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="50e72-192">Click **Create**.</span></span>
 
### <a name="creating-a-tango-analytics-test-user"></a><span data-ttu-id="50e72-193">Skapa en testanvändare Tango Analytics</span><span class="sxs-lookup"><span data-stu-id="50e72-193">Creating a Tango Analytics test user</span></span>

<span data-ttu-id="50e72-194">I det här avsnittet skapar du en användare som kallas Britta Simon i Tango Analytics.</span><span class="sxs-lookup"><span data-stu-id="50e72-194">In this section, you create a user called Britta Simon in Tango Analytics.</span></span> <span data-ttu-id="50e72-195">Arbeta med [Tango Analytics supportteam](mailto:support@tangoanalytics.com) att lägga till hello användare i hello Tango Analytics platform.</span><span class="sxs-lookup"><span data-stu-id="50e72-195">Work with [Tango Analytics support team](mailto:support@tangoanalytics.com) to add hello users in hello Tango Analytics platform.</span></span> <span data-ttu-id="50e72-196">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="50e72-196">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="50e72-197">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="50e72-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="50e72-198">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTango Analytics.</span><span class="sxs-lookup"><span data-stu-id="50e72-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTango Analytics.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="50e72-200">**tooassign Britta Simon tooTango Analytics, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="50e72-200">**tooassign Britta Simon tooTango Analytics, perform hello following steps:**</span></span>

1. <span data-ttu-id="50e72-201">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="50e72-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="50e72-203">Välj i listan med program hello **Tango Analytics**.</span><span class="sxs-lookup"><span data-stu-id="50e72-203">In hello applications list, select **Tango Analytics**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_app.png) 

3. <span data-ttu-id="50e72-205">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="50e72-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="50e72-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="50e72-207">Click **Add** button.</span></span> <span data-ttu-id="50e72-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="50e72-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="50e72-210">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="50e72-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="50e72-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="50e72-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="50e72-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="50e72-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="50e72-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="50e72-213">Testing single sign-on</span></span>

<span data-ttu-id="50e72-214">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="50e72-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="50e72-215">Du bör få automatiskt inloggade tooyour Tango analysprogram när du klickar på hello Tango Analytics panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="50e72-215">When you click hello Tango Analytics tile in hello Access Panel, you should get automatically signed-on tooyour Tango Analytics application.</span></span>
<span data-ttu-id="50e72-216">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="50e72-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="50e72-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="50e72-217">Additional resources</span></span>

* [<span data-ttu-id="50e72-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="50e72-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="50e72-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="50e72-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_203.png

