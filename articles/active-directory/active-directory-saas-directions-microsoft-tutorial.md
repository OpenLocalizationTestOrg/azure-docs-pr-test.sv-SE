---
title: "Självstudier: Azure Active Directory-integrering med Microsoft instruktioner | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och anvisningarna på Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 59a8b856fd2dc75a37e9bb8f46ced066bcd0fd56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a><span data-ttu-id="b9d16-103">Självstudier: Azure Active Directory-integrering med Microsoft instruktioner</span><span class="sxs-lookup"><span data-stu-id="b9d16-103">Tutorial: Azure Active Directory integration with Directions on Microsoft</span></span>

<span data-ttu-id="b9d16-104">I kursen får du lära dig hur toointegrate anvisningarna på Microsoft med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b9d16-104">In this tutorial, you learn how toointegrate Directions on Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b9d16-105">Integrera anvisningarna på Microsoft med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b9d16-105">Integrating Directions on Microsoft with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b9d16-106">Du kan styra i Azure AD som har åtkomst till tooDirections på Microsoft</span><span class="sxs-lookup"><span data-stu-id="b9d16-106">You can control in Azure AD who has access tooDirections on Microsoft</span></span>
- <span data-ttu-id="b9d16-107">Du kan aktivera din användare tooautomatically get inloggade tooDirections på Microsoft (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b9d16-107">You can enable your users tooautomatically get signed-on tooDirections on Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b9d16-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b9d16-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b9d16-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b9d16-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9d16-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b9d16-110">Prerequisites</span></span>

<span data-ttu-id="b9d16-111">tooconfigure Azure AD-integrering med Microsoft instruktioner måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b9d16-111">tooconfigure Azure AD integration with Directions on Microsoft, you need hello following items:</span></span>

- <span data-ttu-id="b9d16-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b9d16-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b9d16-113">En anvisningarna på Microsoft enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b9d16-113">A Directions on Microsoft single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b9d16-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b9d16-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b9d16-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b9d16-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b9d16-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b9d16-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b9d16-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b9d16-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b9d16-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b9d16-118">Scenario description</span></span>
<span data-ttu-id="b9d16-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b9d16-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b9d16-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b9d16-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b9d16-121">Lägger till anvisningarna på Microsoft från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b9d16-121">Adding Directions on Microsoft from hello gallery</span></span>
2. <span data-ttu-id="b9d16-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b9d16-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-directions-on-microsoft-from-hello-gallery"></a><span data-ttu-id="b9d16-123">Lägger till anvisningarna på Microsoft från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b9d16-123">Adding Directions on Microsoft from hello gallery</span></span>
<span data-ttu-id="b9d16-124">tooconfigure hello integrering av anvisningar i Microsoft till Azure AD, behöver du tooadd anvisningarna på Microsoft hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b9d16-124">tooconfigure hello integration of Directions on Microsoft into Azure AD, you need tooadd Directions on Microsoft from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b9d16-125">**tooadd instruktionerna på Microsoft från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b9d16-125">**tooadd Directions on Microsoft from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9d16-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b9d16-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b9d16-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b9d16-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b9d16-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9d16-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b9d16-133">Skriv i sökrutan hello **riktningar i Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-133">In hello search box, type **Directions on Microsoft**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_search.png)

5. <span data-ttu-id="b9d16-135">Markera hello resultat på panelen **riktningar i Microsoft**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b9d16-135">In hello results panel, select **Directions on Microsoft**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b9d16-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b9d16-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b9d16-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med instruktioner Microsoft utifrån en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b9d16-138">In this section, you configure and test Azure AD single sign-on with Directions on Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b9d16-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i anvisningarna på Microsoft är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b9d16-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Directions on Microsoft is tooa user in Azure AD.</span></span> <span data-ttu-id="b9d16-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i anvisningarna i Microsoft toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b9d16-140">In other words, a link relationship between an Azure AD user and hello related user in Directions on Microsoft needs toobe established.</span></span>

<span data-ttu-id="b9d16-141">Tilldela i anvisningarna i Microsoft hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b9d16-141">In Directions on Microsoft, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b9d16-142">tooconfigure och testa Azure AD enkel inloggning med instruktionerna på Microsoft, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b9d16-142">tooconfigure and test Azure AD single sign-on with Directions on Microsoft, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b9d16-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b9d16-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b9d16-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b9d16-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b9d16-145">**[Skapa en anvisningarna på Microsoft testanvändare](#creating-a-directions-on-microsoft-test-user)**  -toohave en motsvarighet för Britta Simon i anvisningarna på Microsoft som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b9d16-145">**[Creating a Directions on Microsoft test user](#creating-a-directions-on-microsoft-test-user)** - toohave a counterpart of Britta Simon in Directions on Microsoft that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b9d16-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b9d16-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b9d16-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b9d16-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b9d16-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b9d16-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b9d16-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din riktningar på Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="b9d16-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Directions on Microsoft application.</span></span>

<span data-ttu-id="b9d16-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Microsoft, instruktioner:**</span><span class="sxs-lookup"><span data-stu-id="b9d16-150">**tooconfigure Azure AD single sign-on with Directions on Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9d16-151">I hello Azure-portalen på hello **riktningar i Microsoft** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-151">In hello Azure portal, on hello **Directions on Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b9d16-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b9d16-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_samlbase.png)

3. <span data-ttu-id="b9d16-155">På hello **anvisningarna på URL: er och Microsoft Domain** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b9d16-155">On hello **Directions on Microsoft Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_url.png)

    <span data-ttu-id="b9d16-157">a.</span><span class="sxs-lookup"><span data-stu-id="b9d16-157">a.</span></span> <span data-ttu-id="b9d16-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="b9d16-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    <span data-ttu-id="b9d16-159">b.</span><span class="sxs-lookup"><span data-stu-id="b9d16-159">b.</span></span> <span data-ttu-id="b9d16-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="b9d16-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE] 
    > <span data-ttu-id="b9d16-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b9d16-161">These values are not real.</span></span> <span data-ttu-id="b9d16-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="b9d16-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b9d16-163">Kontakta [anvisningarna på Microsoft Client supportteam](mailto:service@DirectionsOnMicrosoft.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="b9d16-163">Contact [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="b9d16-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="b9d16-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_certificate.png) 

5. <span data-ttu-id="b9d16-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b9d16-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b9d16-168">tooconfigure enkel inloggning på **riktningar i Microsoft** sida, behöver du toosend hello hämtas **XML-Metadata för** för[anvisningarna på Microsoft supportteam](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="b9d16-168">tooconfigure single sign-on on **Directions on Microsoft** side, you need toosend hello downloaded **Metadata XML** too[Directions on Microsoft support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="b9d16-169">tooenable hello anvisningarna på Microsoft stöder team toolocate ditt federerade medlemskap, inkludera företagets information i din e-post.</span><span class="sxs-lookup"><span data-stu-id="b9d16-169">tooenable hello Directions on Microsoft support team toolocate your federated site membership, include your company information in your email.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="b9d16-170">Enkel inloggning för anvisningar i Microsoft måste aktiveras av hello toobe [anvisningarna på Microsoft Client supportteam](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="b9d16-170">Single sign-on for Directions on Microsoft needs toobe enabled by hello [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="b9d16-171">Du får ett meddelande när enkel inloggning har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="b9d16-171">You will receive a notification when single sign-on has been enabled.</span></span>

> [!TIP]
> <span data-ttu-id="b9d16-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="b9d16-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b9d16-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b9d16-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b9d16-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b9d16-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b9d16-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9d16-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="b9d16-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b9d16-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b9d16-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b9d16-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9d16-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b9d16-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b9d16-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b9d16-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9d16-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b9d16-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b9d16-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b9d16-187">a.</span><span class="sxs-lookup"><span data-stu-id="b9d16-187">a.</span></span> <span data-ttu-id="b9d16-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b9d16-189">b.</span><span class="sxs-lookup"><span data-stu-id="b9d16-189">b.</span></span> <span data-ttu-id="b9d16-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b9d16-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b9d16-191">c.</span><span class="sxs-lookup"><span data-stu-id="b9d16-191">c.</span></span> <span data-ttu-id="b9d16-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b9d16-193">d.</span><span class="sxs-lookup"><span data-stu-id="b9d16-193">d.</span></span> <span data-ttu-id="b9d16-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-194">Click **Create**.</span></span>
 
### <a name="creating-a-directions-on-microsoft-test-user"></a><span data-ttu-id="b9d16-195">Skapa en anvisningarna på Microsoft testanvändare</span><span class="sxs-lookup"><span data-stu-id="b9d16-195">Creating a Directions on Microsoft test user</span></span>

<span data-ttu-id="b9d16-196">Det finns inga uppgiften för du tooconfigure användaretablering tooDirections på Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9d16-196">There is no action item for you tooconfigure user provisioning tooDirections on Microsoft.</span></span>  

<span data-ttu-id="b9d16-197">När en tilldelad användare försöker toolog i tooDirections på Microsoft hello åtkomst panelen, kontrollerar anvisningarna på Microsoft om hello användaren finns.</span><span class="sxs-lookup"><span data-stu-id="b9d16-197">When an assigned user tries toolog in tooDirections on Microsoft using hello access panel, Directions on Microsoft checks whether hello user exists.</span></span> <span data-ttu-id="b9d16-198">Om det finns inget användarkonto ännu, skapas den automatiskt av instruktionerna på Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9d16-198">If there is no user account available yet, it is automatically created by Directions on Microsoft.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b9d16-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9d16-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b9d16-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDirections på Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9d16-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDirections on Microsoft.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b9d16-202">**tooassign Britta Simon tooDirections på Microsoft, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="b9d16-202">**tooassign Britta Simon tooDirections on Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9d16-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b9d16-205">Välj i listan med program hello **riktningar i Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-205">In hello applications list, select **Directions on Microsoft**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_app.png) 

3. <span data-ttu-id="b9d16-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b9d16-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b9d16-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b9d16-209">Click **Add** button.</span></span> <span data-ttu-id="b9d16-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9d16-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b9d16-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b9d16-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b9d16-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9d16-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b9d16-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9d16-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b9d16-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b9d16-215">Testing single sign-on</span></span>

<span data-ttu-id="b9d16-216">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b9d16-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="b9d16-217">När du klickar på hello anvisningarna på Microsoft-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour anvisningarna på Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="b9d16-217">When you click hello Directions on Microsoft tile in hello Access Panel, you should get automatically signed-on tooyour Directions on Microsoft application.</span></span>

<span data-ttu-id="b9d16-218">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b9d16-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b9d16-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b9d16-219">Additional resources</span></span>

* [<span data-ttu-id="b9d16-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b9d16-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b9d16-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b9d16-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_203.png

