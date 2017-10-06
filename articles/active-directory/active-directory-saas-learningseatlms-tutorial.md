---
title: "Självstudier: Azure Active Directory-integrering med Learning klient LMS | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Learning klient LMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bb056fcf-4135-478e-85b1-5015d1f07b85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: jeedes
ms.openlocfilehash: dc08aa444b85f35a4458768ac560ec663baa1c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learning-seat-lms"></a><span data-ttu-id="f9832-103">Självstudier: Azure Active Directory-integrering med Learning klient LMS</span><span class="sxs-lookup"><span data-stu-id="f9832-103">Tutorial: Azure Active Directory integration with Learning Seat LMS</span></span>

<span data-ttu-id="f9832-104">I kursen får du lära dig hur toointegrate Learning klient LMS med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f9832-104">In this tutorial, you learn how toointegrate Learning Seat LMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f9832-105">Integrera Learning klient LMS med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f9832-105">Integrating Learning Seat LMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f9832-106">Du kan styra i Azure AD som har åtkomst tooLearning klient LMS</span><span class="sxs-lookup"><span data-stu-id="f9832-106">You can control in Azure AD who has access tooLearning Seat LMS</span></span>
- <span data-ttu-id="f9832-107">Du kan aktivera din användare tooautomatically get inloggade tooLearning klient LMS (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f9832-107">You can enable your users tooautomatically get signed-on tooLearning Seat LMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f9832-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f9832-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f9832-109">Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f9832-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="f9832-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f9832-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9832-111">Krav</span><span class="sxs-lookup"><span data-stu-id="f9832-111">Prerequisites</span></span>

<span data-ttu-id="f9832-112">tooconfigure Azure AD-integrering med Learning klient LMS måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f9832-112">tooconfigure Azure AD integration with Learning Seat LMS, you need hello following items:</span></span>

- <span data-ttu-id="f9832-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f9832-113">An Azure AD subscription</span></span>
- <span data-ttu-id="f9832-114">En Learning klient LMS enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f9832-114">A Learning Seat LMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f9832-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f9832-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f9832-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f9832-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f9832-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f9832-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f9832-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f9832-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f9832-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f9832-119">Scenario description</span></span>
<span data-ttu-id="f9832-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f9832-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f9832-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f9832-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f9832-122">Att lägga till Learning klient LMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f9832-122">Adding Learning Seat LMS from hello gallery</span></span>
2. <span data-ttu-id="f9832-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f9832-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learning-seat-lms-from-hello-gallery"></a><span data-ttu-id="f9832-124">Att lägga till Learning klient LMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f9832-124">Adding Learning Seat LMS from hello gallery</span></span>
<span data-ttu-id="f9832-125">tooconfigure hello integrering av Learning klient LMS i Azure AD, behöver du tooadd Learning klient LMS hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f9832-125">tooconfigure hello integration of Learning Seat LMS into Azure AD, you need tooadd Learning Seat LMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f9832-126">**tooadd Learning klient LMS från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f9832-126">**tooadd Learning Seat LMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f9832-127">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f9832-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f9832-129">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f9832-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f9832-130">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f9832-130">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f9832-132">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9832-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f9832-134">Skriv i sökrutan hello **Learning klient LMS**.</span><span class="sxs-lookup"><span data-stu-id="f9832-134">In hello search box, type **Learning Seat LMS**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_search.png)

5. <span data-ttu-id="f9832-136">Markera hello resultat på panelen **Learning klient LMS**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f9832-136">In hello results panel, select **Learning Seat LMS**, and then click **Add** button tooadd hello application.</span></span>


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f9832-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f9832-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f9832-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Learning klient LMS baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f9832-138">In this section, you configure and test Azure AD single sign-on with Learning Seat LMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f9832-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Learning klient LMS är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f9832-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Learning Seat LMS is tooa user in Azure AD.</span></span> <span data-ttu-id="f9832-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Learning klient LMS toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="f9832-140">In other words, a link relationship between an Azure AD user and hello related user in Learning Seat LMS needs toobe established.</span></span>

<span data-ttu-id="f9832-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Learning klient LMS.</span><span class="sxs-lookup"><span data-stu-id="f9832-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Learning Seat LMS.</span></span>

<span data-ttu-id="f9832-142">tooconfigure och testa Azure AD enkel inloggning med Learning klient LMS, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f9832-142">tooconfigure and test Azure AD single sign-on with Learning Seat LMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f9832-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f9832-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f9832-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f9832-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f9832-145">**[Skapa en testanvändare Learning klient LMS](#creating-a-learnconnect-test-user)**  -toohave en motsvarighet för Britta Simon i Learning klient LMS som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f9832-145">**[Creating a Learning Seat LMS test user](#creating-a-learnconnect-test-user)** - toohave a counterpart of Britta Simon in Learning Seat LMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f9832-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f9832-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f9832-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f9832-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f9832-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f9832-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f9832-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Learning klient LMS.</span><span class="sxs-lookup"><span data-stu-id="f9832-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Learning Seat LMS application.</span></span>

<span data-ttu-id="f9832-150">**tooconfigure Azure AD enkel inloggning med Learning klient LMS utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f9832-150">**tooconfigure Azure AD single sign-on with Learning Seat LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="f9832-151">I hello Azure-portalen på hello **Learning klient LMS** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f9832-151">In hello Azure portal, on hello **Learning Seat LMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f9832-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f9832-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_samlbase.png)

3. <span data-ttu-id="f9832-155">På hello **Learning klient LMS domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="f9832-155">On hello **Learning Seat LMS Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_url.png)

    <span data-ttu-id="f9832-157">a.</span><span class="sxs-lookup"><span data-stu-id="f9832-157">a.</span></span> <span data-ttu-id="f9832-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.learningseatlms.com`</span><span class="sxs-lookup"><span data-stu-id="f9832-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.learningseatlms.com`</span></span>

    <span data-ttu-id="f9832-159">b.</span><span class="sxs-lookup"><span data-stu-id="f9832-159">b.</span></span> <span data-ttu-id="f9832-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`</span><span class="sxs-lookup"><span data-stu-id="f9832-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`</span></span>

4. <span data-ttu-id="f9832-161">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="f9832-161">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_url2.png)

    <span data-ttu-id="f9832-163">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.learningseatlms.com`</span><span class="sxs-lookup"><span data-stu-id="f9832-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.learningseatlms.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="f9832-164">Dessa värden är inte hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="f9832-164">These values are not hello real values.</span></span> <span data-ttu-id="f9832-165">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="f9832-165">Update these values with hello actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="f9832-166">Kontakta [Learning klient supportteamet](http://help.learningseatlms.com/help) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f9832-166">Contact [Learning Seat support team](http://help.learningseatlms.com/help) tooget these values.</span></span> 

5. <span data-ttu-id="f9832-167">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="f9832-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_certificate.png) 

6. <span data-ttu-id="f9832-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f9832-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnconnect-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="f9832-171">tooconfigure enkel inloggning på **Learning klient LMS** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Learning klient supportteamet](http://help.learningseatlms.com/help).</span><span class="sxs-lookup"><span data-stu-id="f9832-171">tooconfigure single sign-on on **Learning Seat LMS** side, you need toosend hello downloaded **Metadata XML** too[Learning Seat support team](http://help.learningseatlms.com/help).</span></span>

> [!TIP]
> <span data-ttu-id="f9832-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="f9832-172">You can now read a concise version of these instructions inside hello [Azure  portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f9832-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f9832-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f9832-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f9832-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f9832-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f9832-175">Creating an Azure AD test user</span></span>

<span data-ttu-id="f9832-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f9832-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f9832-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f9832-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f9832-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f9832-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f9832-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f9832-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f9832-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9832-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f9832-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f9832-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f9832-187">a.</span><span class="sxs-lookup"><span data-stu-id="f9832-187">a.</span></span> <span data-ttu-id="f9832-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f9832-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f9832-189">b.</span><span class="sxs-lookup"><span data-stu-id="f9832-189">b.</span></span> <span data-ttu-id="f9832-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f9832-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f9832-191">c.</span><span class="sxs-lookup"><span data-stu-id="f9832-191">c.</span></span> <span data-ttu-id="f9832-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f9832-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f9832-193">d.</span><span class="sxs-lookup"><span data-stu-id="f9832-193">d.</span></span> <span data-ttu-id="f9832-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f9832-194">Click **Create**.</span></span>
 
### <a name="creating-a-learning-seat-lms-test-user"></a><span data-ttu-id="f9832-195">Skapa en Learning klient LMS testanvändare</span><span class="sxs-lookup"><span data-stu-id="f9832-195">Creating a Learning Seat LMS test user</span></span>

<span data-ttu-id="f9832-196">I det här avsnittet skapar du en användare som kallas Britta Simon i Learning klient LMS.</span><span class="sxs-lookup"><span data-stu-id="f9832-196">In this section, you create a user called Britta Simon in Learning Seat LMS.</span></span> <span data-ttu-id="f9832-197">Kontakta [Learning klient supportteamet](http://help.learningseatlms.com/help) med alla hello användaren information tooadd hello användare i hello Learning klient LMS program.</span><span class="sxs-lookup"><span data-stu-id="f9832-197">Contact [Learning Seat support team](http://help.learningseatlms.com/help) with all hello user information tooadd hello users in hello Learning Seat LMS application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f9832-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f9832-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f9832-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLearning klient LMS.</span><span class="sxs-lookup"><span data-stu-id="f9832-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLearning Seat LMS.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f9832-201">**tooassign Britta Simon tooLearning klient LMS utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f9832-201">**tooassign Britta Simon tooLearning Seat LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="f9832-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f9832-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f9832-204">Välj i listan med program hello **Learning klient LMS**.</span><span class="sxs-lookup"><span data-stu-id="f9832-204">In hello applications list, select **Learning Seat LMS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_app.png) 

3. <span data-ttu-id="f9832-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f9832-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f9832-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f9832-208">Click **Add** button.</span></span> <span data-ttu-id="f9832-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9832-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f9832-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f9832-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f9832-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9832-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f9832-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9832-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f9832-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f9832-214">Testing single sign-on</span></span>

<span data-ttu-id="f9832-215">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f9832-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> 

<span data-ttu-id="f9832-216">Klicka på hello Learning klient LMS panelen i hello åtkomstpanelen, kommer du att automatiskt inloggade tooyour Learning klient LMS program.</span><span class="sxs-lookup"><span data-stu-id="f9832-216">Click hello Learning Seat LMS tile in hello Access Panel, you will be automatically signed-on tooyour Learning Seat LMS application.</span></span> <span data-ttu-id="f9832-217">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="f9832-217">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f9832-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f9832-218">Additional resources</span></span>

* [<span data-ttu-id="f9832-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f9832-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f9832-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f9832-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_203.png

