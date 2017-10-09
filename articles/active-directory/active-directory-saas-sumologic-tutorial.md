---
title: "Självstudier: Azure Active Directory-integrering med SumoLogic | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SumoLogic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 2ef1bd329f5db8899f0b57744e4c0f6eed1c532f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a><span data-ttu-id="b7ed6-103">Självstudier: Azure Active Directory-integrering med SumoLogic</span><span class="sxs-lookup"><span data-stu-id="b7ed6-103">Tutorial: Azure Active Directory integration with SumoLogic</span></span>

<span data-ttu-id="b7ed6-104">I kursen får du lära dig hur toointegrate SumoLogic med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b7ed6-104">In this tutorial, you learn how toointegrate SumoLogic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b7ed6-105">Integrera SumoLogic med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-105">Integrating SumoLogic with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b7ed6-106">Du kan styra i Azure AD som har åtkomst till tooSumoLogic</span><span class="sxs-lookup"><span data-stu-id="b7ed6-106">You can control in Azure AD who has access tooSumoLogic</span></span>
- <span data-ttu-id="b7ed6-107">Du kan aktivera din användare tooautomatically get inloggade tooSumoLogic (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b7ed6-107">You can enable your users tooautomatically get signed-on tooSumoLogic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b7ed6-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b7ed6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b7ed6-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b7ed6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7ed6-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b7ed6-110">Prerequisites</span></span>

<span data-ttu-id="b7ed6-111">tooconfigure Azure AD-integrering med SumoLogic, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-111">tooconfigure Azure AD integration with SumoLogic, you need hello following items:</span></span>

- <span data-ttu-id="b7ed6-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7ed6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b7ed6-113">En SumoLogic enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7ed6-113">A SumoLogic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b7ed6-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b7ed6-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b7ed6-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b7ed6-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b7ed6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b7ed6-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b7ed6-118">Scenario description</span></span>
<span data-ttu-id="b7ed6-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b7ed6-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b7ed6-121">Att lägga till SumoLogic från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b7ed6-121">Adding SumoLogic from hello gallery</span></span>
2. <span data-ttu-id="b7ed6-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7ed6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sumologic-from-hello-gallery"></a><span data-ttu-id="b7ed6-123">Att lägga till SumoLogic från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b7ed6-123">Adding SumoLogic from hello gallery</span></span>
<span data-ttu-id="b7ed6-124">tooconfigure hello integrering av SumoLogic i Azure AD, behöver du tooadd SumoLogic hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-124">tooconfigure hello integration of SumoLogic into Azure AD, you need tooadd SumoLogic from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b7ed6-125">**tooadd SumoLogic från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7ed6-125">**tooadd SumoLogic from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7ed6-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b7ed6-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b7ed6-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b7ed6-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b7ed6-133">Skriv i sökrutan hello **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-133">In hello search box, type **SumoLogic**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_search.png)

5. <span data-ttu-id="b7ed6-135">Markera hello resultat på panelen **SumoLogic**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-135">In hello results panel, select **SumoLogic**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b7ed6-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7ed6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b7ed6-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SumoLogic baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-138">In this section, you configure and test Azure AD single sign-on with SumoLogic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b7ed6-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SumoLogic är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SumoLogic is tooa user in Azure AD.</span></span> <span data-ttu-id="b7ed6-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SumoLogic toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-140">In other words, a link relationship between an Azure AD user and hello related user in SumoLogic needs toobe established.</span></span>

<span data-ttu-id="b7ed6-141">I SumoLogic, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-141">In SumoLogic, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b7ed6-142">tooconfigure och testa Azure AD enkel inloggning med SumoLogic, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-142">tooconfigure and test Azure AD single sign-on with SumoLogic, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b7ed6-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b7ed6-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b7ed6-145">**[Skapa en testanvändare SumoLogic](#creating-a-sumologic-test-user)**  -toohave en motsvarighet för Britta Simon i SumoLogic som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-145">**[Creating a SumoLogic test user](#creating-a-sumologic-test-user)** - toohave a counterpart of Britta Simon in SumoLogic that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b7ed6-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b7ed6-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b7ed6-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7ed6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b7ed6-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt SumoLogic program.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SumoLogic application.</span></span>

<span data-ttu-id="b7ed6-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SumoLogic:**</span><span class="sxs-lookup"><span data-stu-id="b7ed6-150">**tooconfigure Azure AD single sign-on with SumoLogic, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7ed6-151">I hello Azure-portalen på hello **SumoLogic** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-151">In hello Azure portal, on hello **SumoLogic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b7ed6-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_samlbase.png)

3. <span data-ttu-id="b7ed6-155">På hello **SumoLogic domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-155">On hello **SumoLogic Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_url.png)

    <span data-ttu-id="b7ed6-157">a.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-157">a.</span></span> <span data-ttu-id="b7ed6-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.SumoLogic.com`</span><span class="sxs-lookup"><span data-stu-id="b7ed6-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.SumoLogic.com`</span></span>

    <span data-ttu-id="b7ed6-159">b.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-159">b.</span></span> <span data-ttu-id="b7ed6-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > <span data-ttu-id="b7ed6-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-161">These values are not real.</span></span> <span data-ttu-id="b7ed6-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b7ed6-163">Kontakta [SumoLogic klienten supportteamet](https://www.sumologic.com/contact-us/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-163">Contact [SumoLogic Client support team](https://www.sumologic.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="b7ed6-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_certificate.png) 

5. <span data-ttu-id="b7ed6-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b7ed6-168">På hello **SumoLogic Configuration** klickar du på **konfigurera SumoLogic** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-168">On hello **SumoLogic Configuration** section, click **Configure SumoLogic** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b7ed6-169">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b7ed6-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_configure.png) 

7. <span data-ttu-id="b7ed6-171">Logga in tooyour SumoLogic företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-171">In a different web browser window, log in tooyour SumoLogic company site as an administrator.</span></span>

8. <span data-ttu-id="b7ed6-172">Gå för**hantera \> säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-172">Go too**Manage \> Security**.</span></span>
   
    <span data-ttu-id="b7ed6-173">![Hantera](./media/active-directory-saas-sumologic-tutorial/ic778556.png "hantera")</span><span class="sxs-lookup"><span data-stu-id="b7ed6-173">![Manage](./media/active-directory-saas-sumologic-tutorial/ic778556.png "Manage")</span></span>

9. <span data-ttu-id="b7ed6-174">Klicka på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-174">Click **SAML**.</span></span>
   
    <span data-ttu-id="b7ed6-175">![Globala säkerhetsinställningar](./media/active-directory-saas-sumologic-tutorial/ic778557.png "globala säkerhetsinställningar")</span><span class="sxs-lookup"><span data-stu-id="b7ed6-175">![Global security settings](./media/active-directory-saas-sumologic-tutorial/ic778557.png "Global security settings")</span></span>

10. <span data-ttu-id="b7ed6-176">Från hello **väljer en konfiguration eller skapa en ny** väljer **Azure AD**, och klicka sedan på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-176">From hello **Select a configuration or create a new one** list, select **Azure AD**, and then click **Configure**.</span></span>
   
    <span data-ttu-id="b7ed6-177">![Konfigurera SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "konfigurera SAML 2.0")</span><span class="sxs-lookup"><span data-stu-id="b7ed6-177">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "Configure SAML 2.0")</span></span>

11. <span data-ttu-id="b7ed6-178">På hello **konfigurera SAML 2.0** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-178">On hello **Configure SAML 2.0** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="b7ed6-179">![Konfigurera SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "konfigurera SAML 2.0")</span><span class="sxs-lookup"><span data-stu-id="b7ed6-179">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "Configure SAML 2.0")</span></span>
   
    <span data-ttu-id="b7ed6-180">a.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-180">a.</span></span> <span data-ttu-id="b7ed6-181">I hello **Konfigurationsnamnet** textruta typen **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-181">In hello **Configuration Name** textbox, type **Azure AD**.</span></span> 

    <span data-ttu-id="b7ed6-182">b.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-182">b.</span></span> <span data-ttu-id="b7ed6-183">Välj **felsökningsläge**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-183">Select **Debug Mode**.</span></span>

    <span data-ttu-id="b7ed6-184">c.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-184">c.</span></span> <span data-ttu-id="b7ed6-185">I hello **utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-185">In hello **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="b7ed6-186">d.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-186">d.</span></span> <span data-ttu-id="b7ed6-187">I hello **Authn begära URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-187">In hello **Authn Request URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b7ed6-188">e.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-188">e.</span></span> <span data-ttu-id="b7ed6-189">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in hello hela certifikatet till **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-189">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **X.509 Certificate** textbox.</span></span>

    <span data-ttu-id="b7ed6-190">f.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-190">f.</span></span> <span data-ttu-id="b7ed6-191">Som **e-attributet**väljer **Använd SAML ämne**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-191">As **Email Attribute**, select **Use SAML subject**.</span></span>  

    <span data-ttu-id="b7ed6-192">g.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-192">g.</span></span> <span data-ttu-id="b7ed6-193">Välj **SP initierade inloggningen Configuration**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-193">Select **SP initiated Login Configuration**.</span></span>

    <span data-ttu-id="b7ed6-194">h.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-194">h.</span></span> <span data-ttu-id="b7ed6-195">I hello **inloggningen sökvägen** textruta typen **Azure** och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-195">In hello **Login Path** textbox, type **Azure** and click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="b7ed6-196">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="b7ed6-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b7ed6-197">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b7ed6-198">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b7ed6-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b7ed6-199">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7ed6-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="b7ed6-200">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b7ed6-202">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7ed6-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7ed6-203">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b7ed6-205">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b7ed6-207">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b7ed6-209">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b7ed6-211">a.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-211">a.</span></span> <span data-ttu-id="b7ed6-212">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b7ed6-213">b.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-213">b.</span></span> <span data-ttu-id="b7ed6-214">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b7ed6-215">c.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-215">c.</span></span> <span data-ttu-id="b7ed6-216">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b7ed6-217">d.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-217">d.</span></span> <span data-ttu-id="b7ed6-218">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-218">Click **Create**.</span></span>
 
### <a name="creating-a-sumologic-test-user"></a><span data-ttu-id="b7ed6-219">Skapa en testanvändare SumoLogic</span><span class="sxs-lookup"><span data-stu-id="b7ed6-219">Creating a SumoLogic test user</span></span>

<span data-ttu-id="b7ed6-220">I ordning tooenable Azure AD-användare toolog i tooSumoLogic, måste de vara etablerade tooSumoLogic.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-220">In order tooenable Azure AD users toolog in tooSumoLogic, they must be provisioned tooSumoLogic.</span></span>  

* <span data-ttu-id="b7ed6-221">Hello gäller SumoLogic är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-221">In hello case of SumoLogic, provisioning is a manual task.</span></span>

<span data-ttu-id="b7ed6-222">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="b7ed6-222">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7ed6-223">Logga in tooyour **SumoLogic** klient.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-223">Log in tooyour **SumoLogic** tenant.</span></span>

2. <span data-ttu-id="b7ed6-224">Gå för**hantera \> användare**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-224">Go too**Manage \> Users**.</span></span>
   
    <span data-ttu-id="b7ed6-225">![Användare](./media/active-directory-saas-sumologic-tutorial/ic778561.png "användare")</span><span class="sxs-lookup"><span data-stu-id="b7ed6-225">![Users](./media/active-directory-saas-sumologic-tutorial/ic778561.png "Users")</span></span>

3. <span data-ttu-id="b7ed6-226">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-226">Click **Add**.</span></span>
   
    <span data-ttu-id="b7ed6-227">![Användare](./media/active-directory-saas-sumologic-tutorial/ic778562.png "användare")</span><span class="sxs-lookup"><span data-stu-id="b7ed6-227">![Users](./media/active-directory-saas-sumologic-tutorial/ic778562.png "Users")</span></span>

4. <span data-ttu-id="b7ed6-228">På hello **ny användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7ed6-228">On hello **New User** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="b7ed6-229">![Ny användare](./media/active-directory-saas-sumologic-tutorial/ic778563.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="b7ed6-229">![New User](./media/active-directory-saas-sumologic-tutorial/ic778563.png "New User")</span></span> 
 
    <span data-ttu-id="b7ed6-230">a.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-230">a.</span></span> <span data-ttu-id="b7ed6-231">Typen hello relaterad information på hello Azure AD-konto som du vill tooprovision till hello **Förnamn**, **efternamn**, och **e-post** textrutor.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-231">Type hello related information of hello Azure AD account you want tooprovision into hello **First Name**, **Last Name**, and **Email** textboxes.</span></span>
  
    <span data-ttu-id="b7ed6-232">b.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-232">b.</span></span> <span data-ttu-id="b7ed6-233">Välj en roll.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-233">Select a role.</span></span>
  
    <span data-ttu-id="b7ed6-234">c.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-234">c.</span></span> <span data-ttu-id="b7ed6-235">Som **Status**väljer **Active**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-235">As **Status**, select **Active**.</span></span>
  
    <span data-ttu-id="b7ed6-236">d.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-236">d.</span></span> <span data-ttu-id="b7ed6-237">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-237">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="b7ed6-238">Du kan använda något annat SumoLogic användarens konto skapas verktyg eller API: er som tillhandahålls av SumoLogic tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-238">You can use any other SumoLogic user account creation tools or APIs provided by SumoLogic tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b7ed6-239">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7ed6-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b7ed6-240">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSumoLogic.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSumoLogic.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b7ed6-242">**tooassign Britta Simon tooSumoLogic utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7ed6-242">**tooassign Britta Simon tooSumoLogic, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7ed6-243">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b7ed6-245">Välj i listan med program hello **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-245">In hello applications list, select **SumoLogic**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_app.png) 

3. <span data-ttu-id="b7ed6-247">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b7ed6-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-249">Click **Add** button.</span></span> <span data-ttu-id="b7ed6-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b7ed6-252">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b7ed6-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b7ed6-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b7ed6-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7ed6-255">Testing single sign-on</span></span>

<span data-ttu-id="b7ed6-256">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-256">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b7ed6-257">Du bör få automatiskt inloggade tooyour SumoLogic programmet när du klickar på hello SumoLogic panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7ed6-257">When you click hello SumoLogic tile in hello Access Panel, you should get automatically signed-on tooyour SumoLogic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7ed6-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b7ed6-258">Additional resources</span></span>

* [<span data-ttu-id="b7ed6-259">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b7ed6-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7ed6-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b7ed6-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_203.png

