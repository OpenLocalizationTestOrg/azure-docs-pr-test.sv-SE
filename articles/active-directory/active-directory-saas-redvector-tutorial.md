---
title: "Självstudier: Azure Active Directory-integrering med RedVector | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och RedVector."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 99042f39-0ab2-475b-8df8-3016d7f875e9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 4b866911769dcd1a5e6fe978f2c42e680215b4ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-redvector"></a><span data-ttu-id="24cc2-103">Självstudier: Azure Active Directory-integrering med RedVector</span><span class="sxs-lookup"><span data-stu-id="24cc2-103">Tutorial: Azure Active Directory integration with RedVector</span></span>

<span data-ttu-id="24cc2-104">I kursen får du lära dig hur toointegrate RedVector med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="24cc2-104">In this tutorial, you learn how toointegrate RedVector with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="24cc2-105">Integrera RedVector med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="24cc2-105">Integrating RedVector with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="24cc2-106">Du kan styra i Azure AD som har åtkomst till tooRedVector</span><span class="sxs-lookup"><span data-stu-id="24cc2-106">You can control in Azure AD who has access tooRedVector</span></span>
- <span data-ttu-id="24cc2-107">Du kan aktivera din användare tooautomatically get inloggade tooRedVector (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="24cc2-107">You can enable your users tooautomatically get signed-on tooRedVector (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="24cc2-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="24cc2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="24cc2-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="24cc2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24cc2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="24cc2-110">Prerequisites</span></span>

<span data-ttu-id="24cc2-111">tooconfigure Azure AD-integrering med RedVector, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="24cc2-111">tooconfigure Azure AD integration with RedVector, you need hello following items:</span></span>

- <span data-ttu-id="24cc2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="24cc2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="24cc2-113">En RedVector enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="24cc2-113">A RedVector single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="24cc2-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="24cc2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="24cc2-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="24cc2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="24cc2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="24cc2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="24cc2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="24cc2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="24cc2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="24cc2-118">Scenario description</span></span>
<span data-ttu-id="24cc2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="24cc2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="24cc2-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="24cc2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="24cc2-121">Att lägga till RedVector från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="24cc2-121">Adding RedVector from hello gallery</span></span>
2. <span data-ttu-id="24cc2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="24cc2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-redvector-from-hello-gallery"></a><span data-ttu-id="24cc2-123">Att lägga till RedVector från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="24cc2-123">Adding RedVector from hello gallery</span></span>
<span data-ttu-id="24cc2-124">tooconfigure hello integrering av RedVector i Azure AD, behöver du tooadd RedVector hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="24cc2-124">tooconfigure hello integration of RedVector into Azure AD, you need tooadd RedVector from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="24cc2-125">**tooadd RedVector från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="24cc2-125">**tooadd RedVector from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="24cc2-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="24cc2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="24cc2-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="24cc2-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="24cc2-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24cc2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="24cc2-133">Skriv i sökrutan hello **RedVector**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-133">In hello search box, type **RedVector**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_search.png)

5. <span data-ttu-id="24cc2-135">Markera hello resultat på panelen **RedVector**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="24cc2-135">In hello results panel, select **RedVector**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="24cc2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="24cc2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="24cc2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RedVector baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="24cc2-138">In this section, you configure and test Azure AD single sign-on with RedVector based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="24cc2-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i RedVector är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="24cc2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RedVector is tooa user in Azure AD.</span></span> <span data-ttu-id="24cc2-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i RedVector toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="24cc2-140">In other words, a link relationship between an Azure AD user and hello related user in RedVector needs toobe established.</span></span>

<span data-ttu-id="24cc2-141">I RedVector, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="24cc2-141">In RedVector, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="24cc2-142">tooconfigure och testa Azure AD enkel inloggning med RedVector, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="24cc2-142">tooconfigure and test Azure AD single sign-on with RedVector, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="24cc2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="24cc2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="24cc2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="24cc2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="24cc2-145">**[Skapa en testanvändare RedVector](#creating-a-redvector-test-user)**  -toohave en motsvarighet för Britta Simon i RedVector som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="24cc2-145">**[Creating a RedVector test user](#creating-a-redvector-test-user)** - toohave a counterpart of Britta Simon in RedVector that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="24cc2-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="24cc2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="24cc2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="24cc2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="24cc2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="24cc2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="24cc2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt RedVector program.</span><span class="sxs-lookup"><span data-stu-id="24cc2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RedVector application.</span></span>

<span data-ttu-id="24cc2-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med RedVector:**</span><span class="sxs-lookup"><span data-stu-id="24cc2-150">**tooconfigure Azure AD single sign-on with RedVector, perform hello following steps:**</span></span>

1. <span data-ttu-id="24cc2-151">I hello Azure-portalen på hello **RedVector** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-151">In hello Azure portal, on hello **RedVector** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="24cc2-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="24cc2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_samlbase.png)

3. <span data-ttu-id="24cc2-155">På hello **RedVector domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="24cc2-155">On hello **RedVector Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_url.png)

    <span data-ttu-id="24cc2-157">a.</span><span class="sxs-lookup"><span data-stu-id="24cc2-157">a.</span></span> <span data-ttu-id="24cc2-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://sso2.redvector.com/adfs/<Companyname>`</span><span class="sxs-lookup"><span data-stu-id="24cc2-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://sso2.redvector.com/adfs/<Companyname>`</span></span>

    <span data-ttu-id="24cc2-159">b.</span><span class="sxs-lookup"><span data-stu-id="24cc2-159">b.</span></span> <span data-ttu-id="24cc2-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<Companyname>.redvector.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="24cc2-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<Companyname>.redvector.com/saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="24cc2-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="24cc2-161">These values are not real.</span></span> <span data-ttu-id="24cc2-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="24cc2-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="24cc2-163">Kontakta [RedVector klienten supportteamet](mailto:sso@redvector.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="24cc2-163">Contact [RedVector Client support team](mailto:sso@redvector.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="24cc2-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="24cc2-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_certificate.png) 

5. <span data-ttu-id="24cc2-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="24cc2-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="24cc2-168">På hello **RedVector Configuration** klickar du på **konfigurera RedVector** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="24cc2-168">On hello **RedVector Configuration** section, click **Configure RedVector** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="24cc2-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="24cc2-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_configure.png) 

7. <span data-ttu-id="24cc2-171">tooconfigure enkel inloggning på **RedVector** sida, behöver du toosend hello hämtas **certifikat (Base64)** och **SAML inloggning tjänst-URL för enkel** för[RedVector supportteamet](mailto:sso@redvector.com).</span><span class="sxs-lookup"><span data-stu-id="24cc2-171">tooconfigure single sign-on on **RedVector** side, you need toosend hello downloaded **Certificate (Base64)** and **SAML Single Sign-On Service URL** too[RedVector support team](mailto:sso@redvector.com).</span></span> <span data-ttu-id="24cc2-172">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="24cc2-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="24cc2-173">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="24cc2-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="24cc2-174">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="24cc2-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="24cc2-175">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="24cc2-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="24cc2-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="24cc2-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="24cc2-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="24cc2-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="24cc2-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="24cc2-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="24cc2-180">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="24cc2-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="24cc2-182">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="24cc2-184">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24cc2-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="24cc2-186">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="24cc2-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="24cc2-188">a.</span><span class="sxs-lookup"><span data-stu-id="24cc2-188">a.</span></span> <span data-ttu-id="24cc2-189">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="24cc2-190">b.</span><span class="sxs-lookup"><span data-stu-id="24cc2-190">b.</span></span> <span data-ttu-id="24cc2-191">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="24cc2-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="24cc2-192">c.</span><span class="sxs-lookup"><span data-stu-id="24cc2-192">c.</span></span> <span data-ttu-id="24cc2-193">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="24cc2-194">d.</span><span class="sxs-lookup"><span data-stu-id="24cc2-194">d.</span></span> <span data-ttu-id="24cc2-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-195">Click **Create**.</span></span>
 
### <a name="creating-a-redvector-test-user"></a><span data-ttu-id="24cc2-196">Skapa en testanvändare RedVector</span><span class="sxs-lookup"><span data-stu-id="24cc2-196">Creating a RedVector test user</span></span>

<span data-ttu-id="24cc2-197">I det här avsnittet skapar du en användare som kallas Britta Simon i RedVector.</span><span class="sxs-lookup"><span data-stu-id="24cc2-197">In this section, you create a user called Britta Simon in RedVector.</span></span> <span data-ttu-id="24cc2-198">Om du inte vet hur tooadd Britta Simon i RedVector, fungerar med [RedVector supportteamet](mailto:sso@redvector.com) tooadd hello testanvändare och aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="24cc2-198">If you don't know how tooadd Britta Simon in RedVector, Work with [RedVector support team](mailto:sso@redvector.com) tooadd hello test user and enable SSO.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="24cc2-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="24cc2-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="24cc2-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRedVector.</span><span class="sxs-lookup"><span data-stu-id="24cc2-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRedVector.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="24cc2-202">**tooassign Britta Simon tooRedVector utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="24cc2-202">**tooassign Britta Simon tooRedVector, perform hello following steps:**</span></span>

1. <span data-ttu-id="24cc2-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="24cc2-205">Välj i listan med program hello **RedVector**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-205">In hello applications list, select **RedVector**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_app.png) 

3. <span data-ttu-id="24cc2-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="24cc2-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="24cc2-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="24cc2-209">Click **Add** button.</span></span> <span data-ttu-id="24cc2-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24cc2-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="24cc2-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="24cc2-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="24cc2-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24cc2-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="24cc2-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24cc2-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="24cc2-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="24cc2-215">Testing single sign-on</span></span>

<span data-ttu-id="24cc2-216">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="24cc2-216">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="24cc2-217">När du klickar på hello RedVector panelen i hello åtkomstpanelen får automatiskt inloggade tooyour RedVector program...</span><span class="sxs-lookup"><span data-stu-id="24cc2-217">When you click hello RedVector tile in hello Access Panel, you should get automatically signed-on tooyour RedVector application..</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24cc2-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="24cc2-218">Additional resources</span></span>

* [<span data-ttu-id="24cc2-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="24cc2-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="24cc2-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="24cc2-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_203.png

