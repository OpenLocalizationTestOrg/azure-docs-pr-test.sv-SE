---
title: "Självstudier: Azure Active Directory-integrering med Condeco | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Condeco."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4601c17d-ad93-4865-8885-b378c4bbe82b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 20c57ff0466c28d4fb69f73bc8b5a479715eba0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-condeco"></a><span data-ttu-id="fb9f2-103">Självstudier: Azure Active Directory-integrering med Condeco</span><span class="sxs-lookup"><span data-stu-id="fb9f2-103">Tutorial: Azure Active Directory integration with Condeco</span></span>

<span data-ttu-id="fb9f2-104">I kursen får du lära dig hur toointegrate Condeco med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="fb9f2-104">In this tutorial, you learn how toointegrate Condeco with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fb9f2-105">Integrera Condeco med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="fb9f2-105">Integrating Condeco with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fb9f2-106">Du kan styra i Azure AD som har åtkomst till tooCondeco</span><span class="sxs-lookup"><span data-stu-id="fb9f2-106">You can control in Azure AD who has access tooCondeco</span></span>
- <span data-ttu-id="fb9f2-107">Du kan aktivera din användare tooautomatically get inloggade tooCondeco (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="fb9f2-107">You can enable your users tooautomatically get signed-on tooCondeco (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fb9f2-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fb9f2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="fb9f2-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fb9f2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb9f2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="fb9f2-110">Prerequisites</span></span>

<span data-ttu-id="fb9f2-111">tooconfigure Azure AD-integrering med Condeco, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="fb9f2-111">tooconfigure Azure AD integration with Condeco, you need hello following items:</span></span>

- <span data-ttu-id="fb9f2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fb9f2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fb9f2-113">En Condeco enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="fb9f2-113">A Condeco single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fb9f2-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fb9f2-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="fb9f2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fb9f2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fb9f2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fb9f2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fb9f2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="fb9f2-118">Scenario description</span></span>
<span data-ttu-id="fb9f2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fb9f2-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="fb9f2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fb9f2-121">Att lägga till Condeco från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fb9f2-121">Adding Condeco from hello gallery</span></span>
2. <span data-ttu-id="fb9f2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fb9f2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-condeco-from-hello-gallery"></a><span data-ttu-id="fb9f2-123">Att lägga till Condeco från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fb9f2-123">Adding Condeco from hello gallery</span></span>
<span data-ttu-id="fb9f2-124">tooconfigure hello integrering av Condeco i Azure AD, behöver du tooadd Condeco hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-124">tooconfigure hello integration of Condeco into Azure AD, you need tooadd Condeco from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fb9f2-125">**tooadd Condeco från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fb9f2-125">**tooadd Condeco from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fb9f2-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fb9f2-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fb9f2-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="fb9f2-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="fb9f2-133">Skriv i sökrutan hello **Condeco**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-133">In hello search box, type **Condeco**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_search.png)

5. <span data-ttu-id="fb9f2-135">Markera hello resultat på panelen **Condeco**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-135">In hello results panel, select **Condeco**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fb9f2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fb9f2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fb9f2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Condeco baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-138">In this section, you configure and test Azure AD single sign-on with Condeco based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="fb9f2-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Condeco är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Condeco is tooa user in Azure AD.</span></span> <span data-ttu-id="fb9f2-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Condeco toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-140">In other words, a link relationship between an Azure AD user and hello related user in Condeco needs toobe established.</span></span>

<span data-ttu-id="fb9f2-141">I Condeco, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-141">In Condeco, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="fb9f2-142">tooconfigure och testa Azure AD enkel inloggning med Condeco, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="fb9f2-142">tooconfigure and test Azure AD single sign-on with Condeco, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fb9f2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fb9f2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fb9f2-145">**[Skapa en testanvändare Condeco](#creating-a-condeco-test-user)**  -toohave en motsvarighet för Britta Simon i Condeco som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-145">**[Creating a Condeco test user](#creating-a-condeco-test-user)** - toohave a counterpart of Britta Simon in Condeco that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fb9f2-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fb9f2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fb9f2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fb9f2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fb9f2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Condeco program.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Condeco application.</span></span>

<span data-ttu-id="fb9f2-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Condeco:**</span><span class="sxs-lookup"><span data-stu-id="fb9f2-150">**tooconfigure Azure AD single sign-on with Condeco, perform hello following steps:**</span></span>

1. <span data-ttu-id="fb9f2-151">I hello Azure-portalen på hello **Condeco** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-151">In hello Azure portal, on hello **Condeco** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="fb9f2-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_samlbase.png)

3. <span data-ttu-id="fb9f2-155">På hello **Condeco domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fb9f2-155">On hello **Condeco Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_url.png)

    <span data-ttu-id="fb9f2-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.condecosoftware.com`</span><span class="sxs-lookup"><span data-stu-id="fb9f2-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.condecosoftware.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fb9f2-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-158">This value is not real.</span></span> <span data-ttu-id="fb9f2-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="fb9f2-160">Kontakta [Condeco klienten supportteamet](mailTo:supportna@condecosoftware.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-160">Contact [Condeco Client support team](mailTo:supportna@condecosoftware.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="fb9f2-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_certificate.png) 

5. <span data-ttu-id="fb9f2-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-condeco-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fb9f2-165">tooconfigure enkel inloggning på **Condeco** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Condeco supportteamet](mailTo:supportna@condecosoftware.com).</span><span class="sxs-lookup"><span data-stu-id="fb9f2-165">tooconfigure single sign-on on **Condeco** side, you need toosend hello downloaded **Metadata XML** too[Condeco support team](mailTo:supportna@condecosoftware.com).</span></span> <span data-ttu-id="fb9f2-166">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="fb9f2-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="fb9f2-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fb9f2-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fb9f2-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fb9f2-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fb9f2-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb9f2-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="fb9f2-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="fb9f2-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fb9f2-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fb9f2-174">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-condeco-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fb9f2-176">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-condeco-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fb9f2-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-condeco-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fb9f2-180">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fb9f2-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-condeco-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fb9f2-182">a.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-182">a.</span></span> <span data-ttu-id="fb9f2-183">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fb9f2-184">b.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-184">b.</span></span> <span data-ttu-id="fb9f2-185">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fb9f2-186">c.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-186">c.</span></span> <span data-ttu-id="fb9f2-187">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="fb9f2-188">d.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-188">d.</span></span> <span data-ttu-id="fb9f2-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-189">Click **Create**.</span></span>
 
### <a name="creating-a-condeco-test-user"></a><span data-ttu-id="fb9f2-190">Skapa en testanvändare Condeco</span><span class="sxs-lookup"><span data-stu-id="fb9f2-190">Creating a Condeco test user</span></span>

<span data-ttu-id="fb9f2-191">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Condeco.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-191">hello objective of this section is toocreate a user called Britta Simon in Condeco.</span></span> <span data-ttu-id="fb9f2-192">Condeco stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-192">Condeco supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="fb9f2-193">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-193">There is no action item for you in this section.</span></span> <span data-ttu-id="fb9f2-194">En ny användare skapas under ett försök tooaccess Condeco om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-194">A new user is created during an attempt tooaccess Condeco if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="fb9f2-195">Om du behöver toocreate en användare manuellt, måste toocontact hello [Condeco supportteamet](mailTo:supportna@condecosoftware.com).</span><span class="sxs-lookup"><span data-stu-id="fb9f2-195">If you need toocreate a user manually, you need toocontact hello [Condeco support team](mailTo:supportna@condecosoftware.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="fb9f2-196">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="fb9f2-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="fb9f2-197">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCondeco.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCondeco.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="fb9f2-199">**tooassign Britta Simon tooCondeco utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fb9f2-199">**tooassign Britta Simon tooCondeco, perform hello following steps:**</span></span>

1. <span data-ttu-id="fb9f2-200">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="fb9f2-202">Välj i listan med program hello **Condeco**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-202">In hello applications list, select **Condeco**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-condeco-tutorial/tutorial_condeco_app.png) 

3. <span data-ttu-id="fb9f2-204">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="fb9f2-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-206">Click **Add** button.</span></span> <span data-ttu-id="fb9f2-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="fb9f2-209">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fb9f2-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fb9f2-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fb9f2-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fb9f2-212">Testing single sign-on</span></span>

<span data-ttu-id="fb9f2-213">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="fb9f2-214">Du bör få automatiskt inloggade tooyour Condeco programmet när du klickar på hello Condeco panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="fb9f2-214">When you click hello Condeco tile in hello Access Panel, you should get automatically signed-on tooyour Condeco application.</span></span>
<span data-ttu-id="fb9f2-215">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fb9f2-215">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fb9f2-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="fb9f2-216">Additional resources</span></span>

* [<span data-ttu-id="fb9f2-217">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fb9f2-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fb9f2-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fb9f2-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-condeco-tutorial/tutorial_general_203.png

