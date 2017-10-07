---
title: "Självstudier: Azure Active Directory-integrering med Alcumus Info Exchange | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Alcumus Info Exchange."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d26034b8-f0d5-4f65-aa56-0fc168ceec8c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 4ef9f4d654b6c451db44f929bdad1016304168b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a><span data-ttu-id="eae6a-103">Självstudier: Azure Active Directory-integrering med Alcumus Info Exchange</span><span class="sxs-lookup"><span data-stu-id="eae6a-103">Tutorial: Azure Active Directory integration with Alcumus Info Exchange</span></span>

<span data-ttu-id="eae6a-104">I kursen får du lära dig hur toointegrate Alcumus Info Exchange med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="eae6a-104">In this tutorial, you learn how toointegrate Alcumus Info Exchange with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eae6a-105">Integrera Alcumus Info Exchange med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="eae6a-105">Integrating Alcumus Info Exchange with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="eae6a-106">Du kan styra i Azure AD som har åtkomst tooAlcumus Info Exchange</span><span class="sxs-lookup"><span data-stu-id="eae6a-106">You can control in Azure AD who has access tooAlcumus Info Exchange</span></span>
- <span data-ttu-id="eae6a-107">Du kan aktivera din användare tooautomatically get inloggade tooAlcumus Info Exchange (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="eae6a-107">You can enable your users tooautomatically get signed-on tooAlcumus Info Exchange (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="eae6a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="eae6a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="eae6a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eae6a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eae6a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="eae6a-110">Prerequisites</span></span>

<span data-ttu-id="eae6a-111">tooconfigure Azure AD-integrering med Alcumus Info Exchange måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="eae6a-111">tooconfigure Azure AD integration with Alcumus Info Exchange, you need hello following items:</span></span>

- <span data-ttu-id="eae6a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="eae6a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="eae6a-113">En Alcumus Info Exchange enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="eae6a-113">An Alcumus Info Exchange single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eae6a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="eae6a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="eae6a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="eae6a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="eae6a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="eae6a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="eae6a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eae6a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eae6a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="eae6a-118">Scenario description</span></span>
<span data-ttu-id="eae6a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="eae6a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="eae6a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="eae6a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eae6a-121">Att lägga till Alcumus Info Exchange från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="eae6a-121">Adding Alcumus Info Exchange from hello gallery</span></span>
2. <span data-ttu-id="eae6a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eae6a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-alcumus-info-exchange-from-hello-gallery"></a><span data-ttu-id="eae6a-123">Att lägga till Alcumus Info Exchange från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="eae6a-123">Adding Alcumus Info Exchange from hello gallery</span></span>
<span data-ttu-id="eae6a-124">tooconfigure hello integrering av Alcumus Info Exchange i Azure AD, behöver du tooadd Alcumus Info Exchange hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="eae6a-124">tooconfigure hello integration of Alcumus Info Exchange into Azure AD, you need tooadd Alcumus Info Exchange from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="eae6a-125">**tooadd Alcumus Info Exchange från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="eae6a-125">**tooadd Alcumus Info Exchange from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="eae6a-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="eae6a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="eae6a-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="eae6a-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="eae6a-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eae6a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="eae6a-133">Skriv i sökrutan hello **Alcumus Info Exchange**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-133">In hello search box, type **Alcumus Info Exchange**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_search.png)

5. <span data-ttu-id="eae6a-135">Markera hello resultat på panelen **Alcumus Info Exchange**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="eae6a-135">In hello results panel, select **Alcumus Info Exchange**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="eae6a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eae6a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="eae6a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Alcumus Info Exchange baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="eae6a-138">In this section, you configure and test Azure AD single sign-on with Alcumus Info Exchange based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="eae6a-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Alcumus Info Exchange är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eae6a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Alcumus Info Exchange is tooa user in Azure AD.</span></span> <span data-ttu-id="eae6a-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Alcumus Info Exchange toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="eae6a-140">In other words, a link relationship between an Azure AD user and hello related user in Alcumus Info Exchange needs toobe established.</span></span>

<span data-ttu-id="eae6a-141">Tilldela hello värdet hello i Alcumus Info Exchange **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="eae6a-141">In Alcumus Info Exchange, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="eae6a-142">tooconfigure och testa Azure AD enkel inloggning med Alcumus Info Exchange, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="eae6a-142">tooconfigure and test Azure AD single sign-on with Alcumus Info Exchange, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="eae6a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="eae6a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="eae6a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eae6a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eae6a-145">**[Skapa en testanvändare Alcumus Info Exchange](#creating-an-alcumus-info-exchange-test-user)**  -toohave en motsvarighet för Britta Simon i Alcumus Info Exchange som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="eae6a-145">**[Creating an Alcumus Info Exchange test user](#creating-an-alcumus-info-exchange-test-user)** - toohave a counterpart of Britta Simon in Alcumus Info Exchange that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="eae6a-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="eae6a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eae6a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="eae6a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="eae6a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eae6a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="eae6a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Alcumus Info Exchange-program.</span><span class="sxs-lookup"><span data-stu-id="eae6a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Alcumus Info Exchange application.</span></span>

<span data-ttu-id="eae6a-150">**tooconfigure Azure AD enkel inloggning med Alcumus Info Exchange utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="eae6a-150">**tooconfigure Azure AD single sign-on with Alcumus Info Exchange, perform hello following steps:**</span></span>

1. <span data-ttu-id="eae6a-151">I hello Azure-portalen på hello **Alcumus Info Exchange** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-151">In hello Azure portal, on hello **Alcumus Info Exchange** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="eae6a-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="eae6a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_samlbase.png)

3. <span data-ttu-id="eae6a-155">På hello **Alcumus information Exchange-domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="eae6a-155">On hello **Alcumus Info Exchange Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_url.png)

    <span data-ttu-id="eae6a-157">a.</span><span class="sxs-lookup"><span data-stu-id="eae6a-157">a.</span></span> <span data-ttu-id="eae6a-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.info-exchange.com`</span><span class="sxs-lookup"><span data-stu-id="eae6a-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.info-exchange.com`</span></span>

    <span data-ttu-id="eae6a-159">b.</span><span class="sxs-lookup"><span data-stu-id="eae6a-159">b.</span></span> <span data-ttu-id="eae6a-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.info-exchange.com/Auth/`</span><span class="sxs-lookup"><span data-stu-id="eae6a-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.info-exchange.com/Auth/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="eae6a-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="eae6a-161">These values are not real.</span></span> <span data-ttu-id="eae6a-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="eae6a-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="eae6a-163">Kontakta [Alcumus Info Exchange supportteamet](mailto:helpdesk@alcumusgroup.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="eae6a-163">Contact [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com) tooget these values.</span></span>
 
4. <span data-ttu-id="eae6a-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="eae6a-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_certificate.png) 

5. <span data-ttu-id="eae6a-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="eae6a-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="eae6a-168">tooconfigure enkel inloggning på **Alcumus Info Exchange** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Alcumus Info Exchange supportteamet](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="eae6a-168">tooconfigure single sign-on on **Alcumus Info Exchange** side, you need toosend hello downloaded **Metadata XML** too[Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

> [!TIP]
> <span data-ttu-id="eae6a-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="eae6a-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="eae6a-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="eae6a-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="eae6a-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="eae6a-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="eae6a-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="eae6a-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="eae6a-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eae6a-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="eae6a-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="eae6a-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="eae6a-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="eae6a-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="eae6a-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="eae6a-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eae6a-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="eae6a-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="eae6a-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="eae6a-184">a.</span><span class="sxs-lookup"><span data-stu-id="eae6a-184">a.</span></span> <span data-ttu-id="eae6a-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="eae6a-186">b.</span><span class="sxs-lookup"><span data-stu-id="eae6a-186">b.</span></span> <span data-ttu-id="eae6a-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="eae6a-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="eae6a-188">c.</span><span class="sxs-lookup"><span data-stu-id="eae6a-188">c.</span></span> <span data-ttu-id="eae6a-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="eae6a-190">d.</span><span class="sxs-lookup"><span data-stu-id="eae6a-190">d.</span></span> <span data-ttu-id="eae6a-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-191">Click **Create**.</span></span>
 
### <a name="creating-an-alcumus-info-exchange-test-user"></a><span data-ttu-id="eae6a-192">Skapa en testanvändare Alcumus Info Exchange</span><span class="sxs-lookup"><span data-stu-id="eae6a-192">Creating an Alcumus Info Exchange test user</span></span>

<span data-ttu-id="eae6a-193">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Alcumus Info Exchange.</span><span class="sxs-lookup"><span data-stu-id="eae6a-193">hello objective of this section is toocreate a user called Britta Simon in Alcumus Info Exchange.</span></span>

<span data-ttu-id="eae6a-194">toocreate en användare som kallas Britta Simon i Alcumus Info Exchange, kontakta hello [Alcumus Info Exchange supportteamet](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="eae6a-194">toocreate a user called Britta Simon in Alcumus Info Exchange, Contact hello [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="eae6a-195">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="eae6a-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="eae6a-196">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAlcumus Info Exchange.</span><span class="sxs-lookup"><span data-stu-id="eae6a-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAlcumus Info Exchange.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="eae6a-198">**tooassign Britta Simon tooAlcumus Info Exchange, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="eae6a-198">**tooassign Britta Simon tooAlcumus Info Exchange, perform hello following steps:**</span></span>

1. <span data-ttu-id="eae6a-199">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="eae6a-201">Välj i listan med program hello **Alcumus Info Exchange**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-201">In hello applications list, select **Alcumus Info Exchange**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_app.png) 

3. <span data-ttu-id="eae6a-203">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="eae6a-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="eae6a-205">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="eae6a-205">Click **Add** button.</span></span> <span data-ttu-id="eae6a-206">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eae6a-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="eae6a-208">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="eae6a-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="eae6a-209">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eae6a-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="eae6a-210">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eae6a-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="eae6a-211">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eae6a-211">Testing single sign-on</span></span>

<span data-ttu-id="eae6a-212">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="eae6a-212">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="eae6a-213">När du klickar på hello Alcumus Info Exchange-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Alcumus information Exchange-program.</span><span class="sxs-lookup"><span data-stu-id="eae6a-213">When you click hello Alcumus Info Exchange tile in hello Access Panel, you should get automatically signed-on tooyour Alcumus Info Exchange application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eae6a-214">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="eae6a-214">Additional resources</span></span>

* [<span data-ttu-id="eae6a-215">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eae6a-215">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eae6a-216">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="eae6a-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png

