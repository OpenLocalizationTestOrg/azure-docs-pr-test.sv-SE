---
title: "Självstudier: Azure Active Directory-integrering med Skilljar | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Skilljar."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c572f556-98a3-48e6-8e4c-e634b7a2ba70
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: f0f433d82a0b5510ec568ab610863bcade047697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skilljar"></a><span data-ttu-id="7c60d-103">Självstudier: Azure Active Directory-integrering med Skilljar</span><span class="sxs-lookup"><span data-stu-id="7c60d-103">Tutorial: Azure Active Directory integration with Skilljar</span></span>

<span data-ttu-id="7c60d-104">I kursen får du lära dig hur toointegrate Skilljar med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7c60d-104">In this tutorial, you learn how toointegrate Skilljar with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7c60d-105">Integrera Skilljar med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7c60d-105">Integrating Skilljar with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7c60d-106">Du kan styra i Azure AD som har åtkomst till tooSkilljar</span><span class="sxs-lookup"><span data-stu-id="7c60d-106">You can control in Azure AD who has access tooSkilljar</span></span>
- <span data-ttu-id="7c60d-107">Du kan aktivera din användare tooautomatically get inloggade tooSkilljar (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7c60d-107">You can enable your users tooautomatically get signed-on tooSkilljar (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7c60d-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7c60d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7c60d-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7c60d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c60d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7c60d-110">Prerequisites</span></span>

<span data-ttu-id="7c60d-111">tooconfigure Azure AD-integrering med Skilljar, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7c60d-111">tooconfigure Azure AD integration with Skilljar, you need hello following items:</span></span>

- <span data-ttu-id="7c60d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7c60d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7c60d-113">En Skilljar enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7c60d-113">A Skilljar single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7c60d-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7c60d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7c60d-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7c60d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7c60d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7c60d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7c60d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c60d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7c60d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7c60d-118">Scenario description</span></span>
<span data-ttu-id="7c60d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7c60d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7c60d-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7c60d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7c60d-121">Att lägga till Skilljar från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7c60d-121">Adding Skilljar from hello gallery</span></span>
2. <span data-ttu-id="7c60d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7c60d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skilljar-from-hello-gallery"></a><span data-ttu-id="7c60d-123">Att lägga till Skilljar från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7c60d-123">Adding Skilljar from hello gallery</span></span>
<span data-ttu-id="7c60d-124">tooconfigure hello integrering av Skilljar i Azure AD, behöver du tooadd Skilljar hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="7c60d-124">tooconfigure hello integration of Skilljar into Azure AD, you need tooadd Skilljar from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7c60d-125">**tooadd Skilljar från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7c60d-125">**tooadd Skilljar from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c60d-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7c60d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7c60d-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7c60d-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7c60d-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7c60d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7c60d-133">Skriv i sökrutan hello **Skilljar**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-133">In hello search box, type **Skilljar**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_search.png)

5. <span data-ttu-id="7c60d-135">Markera hello resultat på panelen **Skilljar**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="7c60d-135">In hello results panel, select **Skilljar**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7c60d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7c60d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7c60d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Skilljar baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7c60d-138">In this section, you configure and test Azure AD single sign-on with Skilljar based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7c60d-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Skilljar är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7c60d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Skilljar is tooa user in Azure AD.</span></span> <span data-ttu-id="7c60d-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Skilljar toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="7c60d-140">In other words, a link relationship between an Azure AD user and hello related user in Skilljar needs toobe established.</span></span>

<span data-ttu-id="7c60d-141">I Skilljar, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7c60d-141">In Skilljar, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7c60d-142">tooconfigure och testa Azure AD enkel inloggning med Skilljar, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7c60d-142">tooconfigure and test Azure AD single sign-on with Skilljar, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7c60d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7c60d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7c60d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7c60d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7c60d-145">**[Skapa en testanvändare Skilljar](#creating-a-skilljar-test-user)**  -toohave en motsvarighet för Britta Simon i Skilljar som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7c60d-145">**[Creating a Skilljar test user](#creating-a-skilljar-test-user)** - toohave a counterpart of Britta Simon in Skilljar that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7c60d-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7c60d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7c60d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7c60d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7c60d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7c60d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7c60d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Skilljar program.</span><span class="sxs-lookup"><span data-stu-id="7c60d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Skilljar application.</span></span>

<span data-ttu-id="7c60d-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Skilljar:**</span><span class="sxs-lookup"><span data-stu-id="7c60d-150">**tooconfigure Azure AD single sign-on with Skilljar, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c60d-151">I hello Azure-portalen på hello **Skilljar** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-151">In hello Azure portal, on hello **Skilljar** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7c60d-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7c60d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_samlbase.png)

3. <span data-ttu-id="7c60d-155">På hello **Skilljar domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7c60d-155">On hello **Skilljar Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_url.png)

    <span data-ttu-id="7c60d-157">a.</span><span class="sxs-lookup"><span data-stu-id="7c60d-157">a.</span></span> <span data-ttu-id="7c60d-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.skilljar.com/`</span><span class="sxs-lookup"><span data-stu-id="7c60d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.skilljar.com/`</span></span>

    <span data-ttu-id="7c60d-159">b.</span><span class="sxs-lookup"><span data-stu-id="7c60d-159">b.</span></span> <span data-ttu-id="7c60d-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.skilljar.com/`</span><span class="sxs-lookup"><span data-stu-id="7c60d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.skilljar.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7c60d-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7c60d-161">These values are not real.</span></span> <span data-ttu-id="7c60d-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="7c60d-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7c60d-163">Kontakta [Skilljar klienten supportteamet](http://support.skilljar.com/hc/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7c60d-163">Contact [Skilljar Client support team](http://support.skilljar.com/hc/) tooget these values.</span></span> 
 
4. <span data-ttu-id="7c60d-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="7c60d-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_certificate.png) 

5. <span data-ttu-id="7c60d-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7c60d-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7c60d-168">tooconfigure enkel inloggning på **Skilljar** sida, behöver du toosend hello hämtas **XML-Metadata för**, och **namn Format identifierarvärde - urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress** för[Skilljar supportteamet](http://support.skilljar.com/hc/).</span><span class="sxs-lookup"><span data-stu-id="7c60d-168">tooconfigure single sign-on on **Skilljar** side, you need toosend hello downloaded **Metadata XML**, and **Name Identifier Format Value - urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress** too[Skilljar support team](http://support.skilljar.com/hc/).</span></span> <span data-ttu-id="7c60d-169">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="7c60d-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="7c60d-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="7c60d-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7c60d-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="7c60d-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7c60d-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7c60d-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7c60d-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c60d-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="7c60d-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7c60d-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7c60d-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7c60d-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c60d-177">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7c60d-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7c60d-179">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7c60d-181">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7c60d-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7c60d-183">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7c60d-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7c60d-185">a.</span><span class="sxs-lookup"><span data-stu-id="7c60d-185">a.</span></span> <span data-ttu-id="7c60d-186">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7c60d-187">b.</span><span class="sxs-lookup"><span data-stu-id="7c60d-187">b.</span></span> <span data-ttu-id="7c60d-188">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7c60d-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7c60d-189">c.</span><span class="sxs-lookup"><span data-stu-id="7c60d-189">c.</span></span> <span data-ttu-id="7c60d-190">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7c60d-191">d.</span><span class="sxs-lookup"><span data-stu-id="7c60d-191">d.</span></span> <span data-ttu-id="7c60d-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-192">Click **Create**.</span></span>
 
### <a name="creating-a-skilljar-test-user"></a><span data-ttu-id="7c60d-193">Skapa en testanvändare Skilljar</span><span class="sxs-lookup"><span data-stu-id="7c60d-193">Creating a Skilljar test user</span></span>

<span data-ttu-id="7c60d-194">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Skilljar.</span><span class="sxs-lookup"><span data-stu-id="7c60d-194">hello objective of this section is toocreate a user called Britta Simon in Skilljar.</span></span> <span data-ttu-id="7c60d-195">Skilljar stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="7c60d-195">Skilljar supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="7c60d-196">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="7c60d-196">There is no action item for you in this section.</span></span> <span data-ttu-id="7c60d-197">En ny användare skapas under ett försök tooaccess Skilljar om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="7c60d-197">A new user is created during an attempt tooaccess Skilljar if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="7c60d-198">Om du behöver toocreate en användare manuellt, måste toocontact hello [Skilljar supportteamet](http://support.skilljar.com/hc/).</span><span class="sxs-lookup"><span data-stu-id="7c60d-198">If you need toocreate a user manually, you need toocontact hello [Skilljar support team](http://support.skilljar.com/hc/).</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7c60d-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c60d-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7c60d-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSkilljar.</span><span class="sxs-lookup"><span data-stu-id="7c60d-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSkilljar.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7c60d-202">**tooassign Britta Simon tooSkilljar utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7c60d-202">**tooassign Britta Simon tooSkilljar, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c60d-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7c60d-205">Välj i listan med program hello **Skilljar**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-205">In hello applications list, select **Skilljar**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_app.png) 

3. <span data-ttu-id="7c60d-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7c60d-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7c60d-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7c60d-209">Click **Add** button.</span></span> <span data-ttu-id="7c60d-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7c60d-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7c60d-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="7c60d-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7c60d-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7c60d-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7c60d-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7c60d-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7c60d-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7c60d-215">Testing single sign-on</span></span>

<span data-ttu-id="7c60d-216">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7c60d-216">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="7c60d-217">Du bör få automatiskt inloggade tooyour Skilljar programmet när du klickar på hello Skilljar panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7c60d-217">When you click hello Skilljar tile in hello Access Panel, you should get automatically signed-on tooyour Skilljar application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c60d-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7c60d-218">Additional resources</span></span>

* [<span data-ttu-id="7c60d-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7c60d-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7c60d-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7c60d-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_203.png

