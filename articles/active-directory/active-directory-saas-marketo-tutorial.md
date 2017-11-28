---
title: "Självstudier: Azure Active Directory-integrering med Marketo | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Marketo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 87f88cde4f027f99a83c1ab3b318247bb4d658ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a><span data-ttu-id="a6a25-103">Självstudier: Azure Active Directory-integrering med Marketo</span><span class="sxs-lookup"><span data-stu-id="a6a25-103">Tutorial: Azure Active Directory integration with Marketo</span></span>

<span data-ttu-id="a6a25-104">I kursen får du lära dig hur toointegrate Marketo med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a6a25-104">In this tutorial, you learn how toointegrate Marketo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a6a25-105">Integrera Marketo med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a6a25-105">Integrating Marketo with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a6a25-106">Du kan styra i Azure AD som har åtkomst till tooMarketo</span><span class="sxs-lookup"><span data-stu-id="a6a25-106">You can control in Azure AD who has access tooMarketo</span></span>
- <span data-ttu-id="a6a25-107">Du kan aktivera din användare tooautomatically get inloggade tooMarketo (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a6a25-107">You can enable your users tooautomatically get signed-on tooMarketo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a6a25-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a6a25-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a6a25-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a6a25-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6a25-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a6a25-110">Prerequisites</span></span>

<span data-ttu-id="a6a25-111">tooconfigure Azure AD-integrering med Marketo, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a6a25-111">tooconfigure Azure AD integration with Marketo, you need hello following items:</span></span>

- <span data-ttu-id="a6a25-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a6a25-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a6a25-113">En Marketo enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="a6a25-113">A Marketo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a6a25-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a6a25-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a6a25-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a6a25-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a6a25-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a6a25-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a6a25-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a6a25-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a6a25-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a6a25-118">Scenario description</span></span>
<span data-ttu-id="a6a25-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a6a25-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a6a25-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a6a25-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a6a25-121">Att lägga till Marketo från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a6a25-121">Adding Marketo from hello gallery</span></span>
2. <span data-ttu-id="a6a25-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a6a25-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-marketo-from-hello-gallery"></a><span data-ttu-id="a6a25-123">Att lägga till Marketo från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a6a25-123">Adding Marketo from hello gallery</span></span>
<span data-ttu-id="a6a25-124">tooconfigure hello integrering av Marketo i Azure AD, behöver du tooadd Marketo hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a6a25-124">tooconfigure hello integration of Marketo into Azure AD, you need tooadd Marketo from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a6a25-125">**tooadd Marketo från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a6a25-125">**tooadd Marketo from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6a25-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a6a25-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a6a25-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a6a25-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a6a25-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a6a25-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a6a25-133">Skriv i sökrutan hello **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-133">In hello search box, type **Marketo**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. <span data-ttu-id="a6a25-135">Markera hello resultat på panelen **Marketo**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a6a25-135">In hello results panel, select **Marketo**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a6a25-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a6a25-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a6a25-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Marketo baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a6a25-138">In this section, you configure and test Azure AD single sign-on with Marketo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a6a25-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Marketo är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a6a25-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Marketo is tooa user in Azure AD.</span></span> <span data-ttu-id="a6a25-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Marketo toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a6a25-140">In other words, a link relationship between an Azure AD user and hello related user in Marketo needs toobe established.</span></span>

<span data-ttu-id="a6a25-141">I Marketo, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a6a25-141">In Marketo, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a6a25-142">tooconfigure och testa Azure AD enkel inloggning med Marketo, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a6a25-142">tooconfigure and test Azure AD single sign-on with Marketo, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a6a25-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a6a25-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a6a25-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a6a25-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a6a25-145">**[Skapa en testanvändare Marketo](#creating-a-marketo-test-user)**  -toohave en motsvarighet för Britta Simon i Marketo som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a6a25-145">**[Creating a Marketo test user](#creating-a-marketo-test-user)** - toohave a counterpart of Britta Simon in Marketo that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a6a25-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a6a25-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a6a25-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a6a25-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a6a25-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a6a25-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a6a25-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Marketo-program.</span><span class="sxs-lookup"><span data-stu-id="a6a25-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Marketo application.</span></span>

<span data-ttu-id="a6a25-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Marketo:**</span><span class="sxs-lookup"><span data-stu-id="a6a25-150">**tooconfigure Azure AD single sign-on with Marketo, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6a25-151">I hello Azure-portalen på hello **Marketo** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-151">In hello Azure portal, on hello **Marketo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a6a25-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a6a25-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. <span data-ttu-id="a6a25-155">På hello **Marketo domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a6a25-155">On hello **Marketo Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    <span data-ttu-id="a6a25-157">a.</span><span class="sxs-lookup"><span data-stu-id="a6a25-157">a.</span></span> <span data-ttu-id="a6a25-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://saml.marketo.com/sp`</span><span class="sxs-lookup"><span data-stu-id="a6a25-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://saml.marketo.com/sp`</span></span>

    <span data-ttu-id="a6a25-159">b.</span><span class="sxs-lookup"><span data-stu-id="a6a25-159">b.</span></span> <span data-ttu-id="a6a25-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://login.marketo.com/saml/assertion/\<munchkinid\>`</span><span class="sxs-lookup"><span data-stu-id="a6a25-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://login.marketo.com/saml/assertion/\<munchkinid\>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a6a25-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a6a25-161">These values are not real.</span></span> <span data-ttu-id="a6a25-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="a6a25-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="a6a25-163">Kontakta [Marketo supportteamet](http://investors.marketo.com/contactus.cfm) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a6a25-163">Contact [Marketo support team](http://investors.marketo.com/contactus.cfm) tooget these values.</span></span>
 
4. <span data-ttu-id="a6a25-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a6a25-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. <span data-ttu-id="a6a25-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a6a25-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a6a25-168">På hello **Marketo Configuration** klickar du på **konfigurera Marketo** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a6a25-168">On hello **Marketo Configuration** section, click **Configure Marketo** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a6a25-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a6a25-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. <span data-ttu-id="a6a25-171">tooget Munchkin Id för programmet, logga in med administratörsautentiseringsuppgifter tooMarketo och utföra följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="a6a25-171">tooget Munchkin Id of your application, log in tooMarketo using admin credentials and perform following actions:</span></span>
   
    <span data-ttu-id="a6a25-172">a.</span><span class="sxs-lookup"><span data-stu-id="a6a25-172">a.</span></span> <span data-ttu-id="a6a25-173">Logga in tooMarketo app med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a6a25-173">Log in tooMarketo app using admin credentials.</span></span>
   
    <span data-ttu-id="a6a25-174">b.</span><span class="sxs-lookup"><span data-stu-id="a6a25-174">b.</span></span> <span data-ttu-id="a6a25-175">Klicka på hello **Admin** knappen hello övre navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="a6a25-175">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="a6a25-177">c.</span><span class="sxs-lookup"><span data-stu-id="a6a25-177">c.</span></span> <span data-ttu-id="a6a25-178">Navigera toohello integrering-menyn och klicka på hello **Munchkin länk**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-178">Navigate toohello Integration menu and click hello **Munchkin link**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    <span data-ttu-id="a6a25-180">d.</span><span class="sxs-lookup"><span data-stu-id="a6a25-180">d.</span></span> <span data-ttu-id="a6a25-181">Kopiera hello Munchkin-Id som visas på hello-skärmen och slutföra Reply-URL i guiden för konfiguration av hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a6a25-181">Copy hello Munchkin Id shown on hello screen and complete your Reply URL in hello Azure AD configuration wizard.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. <span data-ttu-id="a6a25-183">tooconfigure hello SSO i hello program så hello nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="a6a25-183">tooconfigure hello SSO in hello application, follow hello below steps:</span></span>
   
    <span data-ttu-id="a6a25-184">a.</span><span class="sxs-lookup"><span data-stu-id="a6a25-184">a.</span></span> <span data-ttu-id="a6a25-185">Logga in tooMarketo app med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a6a25-185">Log in tooMarketo app using admin credentials.</span></span>
   
    <span data-ttu-id="a6a25-186">b.</span><span class="sxs-lookup"><span data-stu-id="a6a25-186">b.</span></span> <span data-ttu-id="a6a25-187">Klicka på hello **Admin** knappen hello övre navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="a6a25-187">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="a6a25-189">c.</span><span class="sxs-lookup"><span data-stu-id="a6a25-189">c.</span></span> <span data-ttu-id="a6a25-190">Navigera toohello integrering-menyn och klicka på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-190">Navigate toohello Integration menu and click **Single Sign On**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    <span data-ttu-id="a6a25-192">d.</span><span class="sxs-lookup"><span data-stu-id="a6a25-192">d.</span></span> <span data-ttu-id="a6a25-193">tooenable hello SAML-inställningar, klickar du på **redigera** knappen.</span><span class="sxs-lookup"><span data-stu-id="a6a25-193">tooenable hello SAML Settings, click **Edit** button.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    <span data-ttu-id="a6a25-195">e.</span><span class="sxs-lookup"><span data-stu-id="a6a25-195">e.</span></span> <span data-ttu-id="a6a25-196">**Aktiverad** inställningar för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a6a25-196">**Enabled** Single Sign-On settings.</span></span>
   
    <span data-ttu-id="a6a25-197">f.</span><span class="sxs-lookup"><span data-stu-id="a6a25-197">f.</span></span> <span data-ttu-id="a6a25-198">Klistra in hello **SAML enhets-ID**, i hello **utfärdaren ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="a6a25-198">Paste hello **SAML Entity ID**, in hello **Issuer ID** textbox.</span></span>
   
    <span data-ttu-id="a6a25-199">g.</span><span class="sxs-lookup"><span data-stu-id="a6a25-199">g.</span></span> <span data-ttu-id="a6a25-200">I hello **enhets-ID** textruta ange hello URL: en som `http://saml.marketo.com/sp`.</span><span class="sxs-lookup"><span data-stu-id="a6a25-200">In hello **Entity ID** textbox, enter hello URL as `http://saml.marketo.com/sp`.</span></span>
   
    <span data-ttu-id="a6a25-201">h.</span><span class="sxs-lookup"><span data-stu-id="a6a25-201">h.</span></span> <span data-ttu-id="a6a25-202">Välj hello plats-ID som **namnidentifierare elementet**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-202">Select hello User ID Location as **Name Identifier element**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > <span data-ttu-id="a6a25-204">Om ditt användar-ID inte UPN-värde kan ändra hello värdet hello attribut på fliken.</span><span class="sxs-lookup"><span data-stu-id="a6a25-204">If your User Identifier is not UPN value then change hello value in hello Attribute tab.</span></span>
   
    <span data-ttu-id="a6a25-205">Jag.</span><span class="sxs-lookup"><span data-stu-id="a6a25-205">i.</span></span> <span data-ttu-id="a6a25-206">Överför hello-certifikatet som du har hämtat från Azure AD-konfigurationsguiden.</span><span class="sxs-lookup"><span data-stu-id="a6a25-206">Upload hello certificate, which you have downloaded from Azure AD configuration wizard.</span></span> <span data-ttu-id="a6a25-207">**Spara** hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="a6a25-207">**Save** hello settings.</span></span>
   
    <span data-ttu-id="a6a25-208">j.</span><span class="sxs-lookup"><span data-stu-id="a6a25-208">j.</span></span> <span data-ttu-id="a6a25-209">Redigera inställningar för hello omdirigera felsidor.</span><span class="sxs-lookup"><span data-stu-id="a6a25-209">Edit hello Redirect Pages settings.</span></span>
   
    <span data-ttu-id="a6a25-210">k.</span><span class="sxs-lookup"><span data-stu-id="a6a25-210">k.</span></span> <span data-ttu-id="a6a25-211">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** i hello **inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a6a25-211">Paste hello **SAML Single Sign-On Service URL** in hello **Login URL** textbox.</span></span>
   
    <span data-ttu-id="a6a25-212">l.</span><span class="sxs-lookup"><span data-stu-id="a6a25-212">l.</span></span> <span data-ttu-id="a6a25-213">Klistra in hello **Sign-Out URL** i hello **logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a6a25-213">Paste hello **Sign-Out URL** in hello **Logout URL** textbox.</span></span>
   
    <span data-ttu-id="a6a25-214">m.</span><span class="sxs-lookup"><span data-stu-id="a6a25-214">m.</span></span> <span data-ttu-id="a6a25-215">I hello **fel URL**, kopiera ditt **Marketo instans-URL** och på **spara** knappen toosave inställningar.</span><span class="sxs-lookup"><span data-stu-id="a6a25-215">In hello **Error URL**, copy your **Marketo instance URL** and click **Save** button toosave settings.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. <span data-ttu-id="a6a25-217">tooenable hello enkel inloggning för användare, fullständig hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="a6a25-217">tooenable hello SSO for users, complete hello following actions:</span></span>
   
    <span data-ttu-id="a6a25-218">a.</span><span class="sxs-lookup"><span data-stu-id="a6a25-218">a.</span></span> <span data-ttu-id="a6a25-219">Logga in tooMarketo app med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a6a25-219">Log in tooMarketo app using admin credentials.</span></span>
   
    <span data-ttu-id="a6a25-220">b.</span><span class="sxs-lookup"><span data-stu-id="a6a25-220">b.</span></span> <span data-ttu-id="a6a25-221">Klicka på hello **Admin** knappen hello övre navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="a6a25-221">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="a6a25-223">c.</span><span class="sxs-lookup"><span data-stu-id="a6a25-223">c.</span></span> <span data-ttu-id="a6a25-224">Navigera toohello **säkerhet** -menyn och klicka på **inloggningsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-224">Navigate toohello **Security** menu and click **Login Settings**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    <span data-ttu-id="a6a25-226">d.</span><span class="sxs-lookup"><span data-stu-id="a6a25-226">d.</span></span> <span data-ttu-id="a6a25-227">Kontrollera hello **kräver SSO** alternativet och **spara** hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="a6a25-227">Check hello **Require SSO** option and **Save** hello settings.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> <span data-ttu-id="a6a25-229">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a6a25-229">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a6a25-230">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a6a25-230">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a6a25-231">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a6a25-231">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a6a25-232">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a6a25-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="a6a25-233">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a6a25-233">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a6a25-235">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a6a25-235">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6a25-236">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a6a25-236">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a6a25-238">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-238">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a6a25-240">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a6a25-240">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a6a25-242">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a6a25-242">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a6a25-244">a.</span><span class="sxs-lookup"><span data-stu-id="a6a25-244">a.</span></span> <span data-ttu-id="a6a25-245">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-245">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a6a25-246">b.</span><span class="sxs-lookup"><span data-stu-id="a6a25-246">b.</span></span> <span data-ttu-id="a6a25-247">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a6a25-247">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a6a25-248">c.</span><span class="sxs-lookup"><span data-stu-id="a6a25-248">c.</span></span> <span data-ttu-id="a6a25-249">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-249">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a6a25-250">d.</span><span class="sxs-lookup"><span data-stu-id="a6a25-250">d.</span></span> <span data-ttu-id="a6a25-251">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-251">Click **Create**.</span></span>
 
### <a name="creating-a-marketo-test-user"></a><span data-ttu-id="a6a25-252">Skapa en testanvändare Marketo</span><span class="sxs-lookup"><span data-stu-id="a6a25-252">Creating a Marketo test user</span></span>

<span data-ttu-id="a6a25-253">I det här avsnittet skapar du en användare som kallas Britta Simon i Marketo.</span><span class="sxs-lookup"><span data-stu-id="a6a25-253">In this section, you create a user called Britta Simon in Marketo.</span></span> <span data-ttu-id="a6a25-254">Följ dessa steg toocreate en användare i Marketo-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a6a25-254">follow these steps toocreate a user in Marketo platform.</span></span>

1. <span data-ttu-id="a6a25-255">Logga in tooMarketo app med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a6a25-255">Log in tooMarketo app using admin credentials.</span></span>

2. <span data-ttu-id="a6a25-256">Klicka på hello **Admin** knappen hello övre navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="a6a25-256">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. <span data-ttu-id="a6a25-258">Navigera toohello **säkerhet** -menyn och klicka på **användare och roller**</span><span class="sxs-lookup"><span data-stu-id="a6a25-258">Navigate toohello **Security** menu and click **Users & Roles**</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. <span data-ttu-id="a6a25-260">Klicka på hello **bjuda in nya användare** länk på fliken för hello-användare</span><span class="sxs-lookup"><span data-stu-id="a6a25-260">Click hello **Invite New User** link on hello Users tab</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. <span data-ttu-id="a6a25-262">I följande information hello bjuda in nya användare guiden fill hello</span><span class="sxs-lookup"><span data-stu-id="a6a25-262">In hello Invite New User wizard fill hello following information</span></span>
   
    <span data-ttu-id="a6a25-263">a.</span><span class="sxs-lookup"><span data-stu-id="a6a25-263">a.</span></span> <span data-ttu-id="a6a25-264">Ange hello användare **e-post** adress i hello textruta</span><span class="sxs-lookup"><span data-stu-id="a6a25-264">Enter hello user **Email** address in hello textbox</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    <span data-ttu-id="a6a25-266">b.</span><span class="sxs-lookup"><span data-stu-id="a6a25-266">b.</span></span> <span data-ttu-id="a6a25-267">Ange hello **Förnamn** i hello textruta</span><span class="sxs-lookup"><span data-stu-id="a6a25-267">Enter hello **First Name** in hello textbox</span></span>
   
    <span data-ttu-id="a6a25-268">c.</span><span class="sxs-lookup"><span data-stu-id="a6a25-268">c.</span></span> <span data-ttu-id="a6a25-269">Ange hello **efternamn** i hello textruta</span><span class="sxs-lookup"><span data-stu-id="a6a25-269">Enter hello **Last Name**  in hello textbox</span></span>
   
    <span data-ttu-id="a6a25-270">d.</span><span class="sxs-lookup"><span data-stu-id="a6a25-270">d.</span></span> <span data-ttu-id="a6a25-271">Klicka på **Nästa**</span><span class="sxs-lookup"><span data-stu-id="a6a25-271">Click **Next**</span></span>

6. <span data-ttu-id="a6a25-272">I hello **behörigheter** fliken, väljer hello **userRoles** och på **nästa**</span><span class="sxs-lookup"><span data-stu-id="a6a25-272">In hello **Permissions** tab, select hello **userRoles** and click **Next**</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. <span data-ttu-id="a6a25-274">Klicka på hello **skicka** knappen toosend hello användarinbjudan</span><span class="sxs-lookup"><span data-stu-id="a6a25-274">Click hello **Send** button toosend hello user invitation</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. <span data-ttu-id="a6a25-276">Användaren får hello e-postmeddelande och har tooclick hello länka och ändra hello lösenord tooactivate hello konto.</span><span class="sxs-lookup"><span data-stu-id="a6a25-276">User receives hello email notification and has tooclick hello link and change hello password tooactivate hello account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a6a25-277">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a6a25-277">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a6a25-278">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMarketo.</span><span class="sxs-lookup"><span data-stu-id="a6a25-278">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMarketo.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a6a25-280">**tooassign Britta Simon tooMarketo utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a6a25-280">**tooassign Britta Simon tooMarketo, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6a25-281">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-281">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a6a25-283">Välj i listan med program hello **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-283">In hello applications list, select **Marketo**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. <span data-ttu-id="a6a25-285">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a6a25-285">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a6a25-287">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a6a25-287">Click **Add** button.</span></span> <span data-ttu-id="a6a25-288">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a6a25-288">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a6a25-290">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a6a25-290">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a6a25-291">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a6a25-291">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a6a25-292">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a6a25-292">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a6a25-293">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a6a25-293">Testing single sign-on</span></span>

<span data-ttu-id="a6a25-294">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a6a25-294">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a6a25-295">Du bör få automatiskt inloggade tooyour Marketo programmet när du klickar på hello Marketo-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a6a25-295">When you click hello Marketo tile in hello Access Panel, you should get automatically signed-on tooyour Marketo application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6a25-296">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a6a25-296">Additional resources</span></span>

* [<span data-ttu-id="a6a25-297">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a6a25-297">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a6a25-298">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a6a25-298">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

