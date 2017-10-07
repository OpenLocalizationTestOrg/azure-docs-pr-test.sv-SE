---
title: "Självstudier: Azure Active Directory-integrering med Huddle | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Huddle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 0b2f6c4d839943cdd07699a1ff95dc8f90505699
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a><span data-ttu-id="a17c2-103">Självstudier: Azure Active Directory-integrering med Huddle</span><span class="sxs-lookup"><span data-stu-id="a17c2-103">Tutorial: Azure Active Directory integration with Huddle</span></span>

<span data-ttu-id="a17c2-104">I kursen får du lära dig hur toointegrate Huddle med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a17c2-104">In this tutorial, you learn how toointegrate Huddle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a17c2-105">Integrera Huddle med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a17c2-105">Integrating Huddle with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a17c2-106">Du kan styra i Azure AD som har åtkomst till tooHuddle</span><span class="sxs-lookup"><span data-stu-id="a17c2-106">You can control in Azure AD who has access tooHuddle</span></span>
- <span data-ttu-id="a17c2-107">Du kan aktivera din användare tooautomatically get inloggade tooHuddle (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a17c2-107">You can enable your users tooautomatically get signed-on tooHuddle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a17c2-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a17c2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a17c2-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a17c2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a17c2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a17c2-110">Prerequisites</span></span>

<span data-ttu-id="a17c2-111">tooconfigure Azure AD-integrering med Huddle, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a17c2-111">tooconfigure Azure AD integration with Huddle, you need hello following items:</span></span>

- <span data-ttu-id="a17c2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a17c2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a17c2-113">En Huddle enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a17c2-113">A Huddle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a17c2-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a17c2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a17c2-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a17c2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a17c2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a17c2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a17c2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a17c2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a17c2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a17c2-118">Scenario description</span></span>

<span data-ttu-id="a17c2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a17c2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a17c2-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a17c2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a17c2-121">Att lägga till Huddle från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a17c2-121">Adding Huddle from hello gallery</span></span>
2. <span data-ttu-id="a17c2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a17c2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-huddle-from-hello-gallery"></a><span data-ttu-id="a17c2-123">Att lägga till Huddle från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a17c2-123">Adding Huddle from hello gallery</span></span>
<span data-ttu-id="a17c2-124">tooconfigure hello integrering av Huddle till Azure AD, behöver du tooadd Huddle hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a17c2-124">tooconfigure hello integration of Huddle into Azure AD, you need tooadd Huddle from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a17c2-125">**tooadd Huddle från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a17c2-125">**tooadd Huddle from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a17c2-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a17c2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a17c2-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a17c2-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a17c2-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a17c2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a17c2-133">Skriv i sökrutan hello **Huddle**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-133">In hello search box, type **Huddle**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_search.png)

5. <span data-ttu-id="a17c2-135">Markera hello resultat på panelen **Huddle**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a17c2-135">In hello results panel, select **Huddle**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a17c2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a17c2-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="a17c2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Huddle baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a17c2-138">In this section, you configure and test Azure AD single sign-on with Huddle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a17c2-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Huddle är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a17c2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Huddle is tooa user in Azure AD.</span></span> <span data-ttu-id="a17c2-140">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i Huddle behov toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a17c2-140">In other words, a link relationship between an Azure AD user and hello related user in Huddle needs toobe established.</span></span>

<span data-ttu-id="a17c2-141">I Huddle, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a17c2-141">In Huddle, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a17c2-142">tooconfigure och testa Azure AD enkel inloggning med Huddle, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a17c2-142">tooconfigure and test Azure AD single sign-on with Huddle, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a17c2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a17c2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>

2. <span data-ttu-id="a17c2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a17c2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>

3. <span data-ttu-id="a17c2-145">**[Skapa en testanvändare Huddle](#creating-a-huddle-test-user)**  -toohave en motsvarighet för Britta Simon i Huddle som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a17c2-145">**[Creating a Huddle test user](#creating-a-huddle-test-user)** - toohave a counterpart of Britta Simon in Huddle that is linked toohello Azure AD representation of user.</span></span>

4. <span data-ttu-id="a17c2-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a17c2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>

5. <span data-ttu-id="a17c2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a17c2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a17c2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a17c2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a17c2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Huddle program.</span><span class="sxs-lookup"><span data-stu-id="a17c2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Huddle application.</span></span>

<span data-ttu-id="a17c2-150">**tooconfigure Azure AD enkel inloggning med Huddle, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="a17c2-150">**tooconfigure Azure AD single sign-on with Huddle, perform hello following steps:**</span></span>

1. <span data-ttu-id="a17c2-151">I hello Azure-portalen på hello **Huddle** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-151">In hello Azure portal, on hello **Huddle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a17c2-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a17c2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_samlbase.png)

3. <span data-ttu-id="a17c2-155">På hello **Huddle domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a17c2-155">On hello **Huddle Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_url.png)

    <span data-ttu-id="a17c2-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`http://<company name>.huddle.com`</span><span class="sxs-lookup"><span data-stu-id="a17c2-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://<company name>.huddle.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a17c2-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a17c2-158">This value is not real.</span></span> <span data-ttu-id="a17c2-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="a17c2-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="a17c2-160">Kontakta [Huddle klienten supportteamet](https://huddle.zendesk.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="a17c2-160">Contact [Huddle Client support team](https://huddle.zendesk.com) tooget this value.</span></span> 

4. <span data-ttu-id="a17c2-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a17c2-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_certificate.png) 

5. <span data-ttu-id="a17c2-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a17c2-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a17c2-165">På hello **Huddle Configuration** klickar du på **konfigurera Huddle** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a17c2-165">On hello **Huddle Configuration** section, click **Configure Huddle** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a17c2-166">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a17c2-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_configure.png) 
    
7. <span data-ttu-id="a17c2-168">tooconfigure enkel inloggning på Huddle sida, behöver du toosend hello hämtas **certifikat**, **SAML enkel inloggning Tjänstwebbadress**, och **SAML enhets-ID** för[Huddle klienten supportteamet](https://huddle.zendesk.com).</span><span class="sxs-lookup"><span data-stu-id="a17c2-168">tooconfigure single sign-on on Huddle side, you need toosend hello downloaded  **Certificate**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** too[Huddle Client support team](https://huddle.zendesk.com).</span></span> <span data-ttu-id="a17c2-169">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="a17c2-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>  
   
    >[!NOTE]
    > <span data-ttu-id="a17c2-170">Enkel inloggning måste du toobe aktiveras med hello Huddle supportteamet.</span><span class="sxs-lookup"><span data-stu-id="a17c2-170">Single sign-on needs toobe enabled by hello Huddle support team.</span></span> <span data-ttu-id="a17c2-171">Du får ett meddelande när hello konfigurationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="a17c2-171">You get a notification when hello configuration has been completed.</span></span> 
    > 

> [!TIP]
> <span data-ttu-id="a17c2-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a17c2-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a17c2-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a17c2-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a17c2-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a17c2-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
   
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a17c2-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a17c2-175">Creating an Azure AD test user</span></span>

<span data-ttu-id="a17c2-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a17c2-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a17c2-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a17c2-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a17c2-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a17c2-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a17c2-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a17c2-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a17c2-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a17c2-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a17c2-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a17c2-187">a.</span><span class="sxs-lookup"><span data-stu-id="a17c2-187">a.</span></span> <span data-ttu-id="a17c2-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a17c2-189">b.</span><span class="sxs-lookup"><span data-stu-id="a17c2-189">b.</span></span> <span data-ttu-id="a17c2-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a17c2-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a17c2-191">c.</span><span class="sxs-lookup"><span data-stu-id="a17c2-191">c.</span></span> <span data-ttu-id="a17c2-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a17c2-193">d.</span><span class="sxs-lookup"><span data-stu-id="a17c2-193">d.</span></span> <span data-ttu-id="a17c2-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-194">Click **Create**.</span></span>
 
### <a name="creating-a-huddle-test-user"></a><span data-ttu-id="a17c2-195">Skapa en testanvändare Huddle</span><span class="sxs-lookup"><span data-stu-id="a17c2-195">Creating a Huddle test user</span></span>

<span data-ttu-id="a17c2-196">tooenable Azure AD-användare toolog i tooHuddle, måste de etableras i Huddle.</span><span class="sxs-lookup"><span data-stu-id="a17c2-196">tooenable Azure AD users toolog in tooHuddle, they must be provisioned into Huddle.</span></span> <span data-ttu-id="a17c2-197">Hello gäller Huddle är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a17c2-197">In hello case of Huddle, provisioning is a manual task.</span></span>

<span data-ttu-id="a17c2-198">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="a17c2-198">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="a17c2-199">Logga in tooyour **Huddle** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="a17c2-199">Log in tooyour **Huddle** company site as administrator.</span></span>
2. <span data-ttu-id="a17c2-200">Klicka på **arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-200">Click **Workspace**.</span></span>
3. <span data-ttu-id="a17c2-201">Klicka på **personer \> bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-201">Click **People \> Invite People**.</span></span>
   
   <span data-ttu-id="a17c2-202">![Personer](./media/active-directory-saas-huddle-tutorial/IC787838.png "personer")</span><span class="sxs-lookup"><span data-stu-id="a17c2-202">![People](./media/active-directory-saas-huddle-tutorial/IC787838.png "People")</span></span>

4. <span data-ttu-id="a17c2-203">I hello **skapa en ny inbjudan** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a17c2-203">In hello **Create a new invitation** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="a17c2-204">![Ny inbjudan](./media/active-directory-saas-huddle-tutorial/IC787839.png "ny inbjudan")</span><span class="sxs-lookup"><span data-stu-id="a17c2-204">![New Invitation](./media/active-directory-saas-huddle-tutorial/IC787839.png "New Invitation")</span></span>
   
   <span data-ttu-id="a17c2-205">a.</span><span class="sxs-lookup"><span data-stu-id="a17c2-205">a.</span></span> <span data-ttu-id="a17c2-206">I hello **Välj ett team tooinvite personer toojoin** väljer **team**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-206">In hello **Choose a team tooinvite people toojoin** list, select **team**.</span></span>

   <span data-ttu-id="a17c2-207">b.</span><span class="sxs-lookup"><span data-stu-id="a17c2-207">b.</span></span> <span data-ttu-id="a17c2-208">Typen hello **e-postadress** för en giltig Azure AD-kontot du vill ha tooprovision för**ange e-postadress för personer som tooinvite** textruta.</span><span class="sxs-lookup"><span data-stu-id="a17c2-208">Type hello **Email Address** of a valid Azure AD account you want tooprovision in too**Enter email address for people you'd like tooinvite** textbox.</span></span>

   <span data-ttu-id="a17c2-209">c.</span><span class="sxs-lookup"><span data-stu-id="a17c2-209">c.</span></span> <span data-ttu-id="a17c2-210">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-210">Click **Invite**.</span></span>   
   
    >[!NOTE]
    > <span data-ttu-id="a17c2-211">hello Azure AD-kontot innehavaren får ett e-postmeddelande inklusive en länk tooconfirm hello innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="a17c2-211">hello Azure AD account holder will receive an email including a link tooconfirm hello account before it becomes active.</span></span> 
    > 

>[!NOTE]
><span data-ttu-id="a17c2-212">Du kan använda något annat Huddle användarens konto skapas verktyg eller API: er som tillhandahålls av Huddle tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a17c2-212">You can use any other Huddle user account creation tools or APIs provided by Huddle tooprovision Azure AD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a17c2-213">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a17c2-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a17c2-214">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHuddle.</span><span class="sxs-lookup"><span data-stu-id="a17c2-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHuddle.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a17c2-216">**tooassign Britta Simon tooHuddle utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a17c2-216">**tooassign Britta Simon tooHuddle, perform hello following steps:**</span></span>

1. <span data-ttu-id="a17c2-217">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a17c2-219">Välj i listan med program hello **Huddle**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-219">In hello applications list, select **Huddle**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_app.png) 

3. <span data-ttu-id="a17c2-221">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a17c2-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a17c2-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a17c2-223">Click **Add** button.</span></span> <span data-ttu-id="a17c2-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a17c2-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a17c2-226">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a17c2-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a17c2-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a17c2-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a17c2-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a17c2-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a17c2-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a17c2-229">Testing single sign-on</span></span>

<span data-ttu-id="a17c2-230">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a17c2-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a17c2-231">När du klickar på hello Huddle panelen i hello åtkomstpanelen ska du automatiskt hämta inloggningssidan för Huddle program.</span><span class="sxs-lookup"><span data-stu-id="a17c2-231">When you click hello Huddle tile in hello Access Panel, you should get automatically login page of Huddle application.</span></span>
<span data-ttu-id="a17c2-232">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a17c2-232">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a17c2-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a17c2-233">Additional resources</span></span>

* [<span data-ttu-id="a17c2-234">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a17c2-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a17c2-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a17c2-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_04.png
[100]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_203.png
