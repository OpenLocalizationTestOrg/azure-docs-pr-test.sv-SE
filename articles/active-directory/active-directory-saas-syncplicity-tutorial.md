---
title: "Självstudier: Azure Active Directory-integrering med Syncplicity | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Syncplicity."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 6148112a959232ed24d76d1c7b8773f06568fee7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a><span data-ttu-id="cf49e-103">Självstudier: Azure Active Directory-integrering med Syncplicity</span><span class="sxs-lookup"><span data-stu-id="cf49e-103">Tutorial: Azure Active Directory integration with Syncplicity</span></span>

<span data-ttu-id="cf49e-104">I kursen får du lära dig hur toointegrate Syncplicity med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cf49e-104">In this tutorial, you learn how toointegrate Syncplicity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cf49e-105">Integrera Syncplicity med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cf49e-105">Integrating Syncplicity with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cf49e-106">Du kan styra i Azure AD som har åtkomst till tooSyncplicity</span><span class="sxs-lookup"><span data-stu-id="cf49e-106">You can control in Azure AD who has access tooSyncplicity</span></span>
- <span data-ttu-id="cf49e-107">Du kan aktivera din användare tooautomatically get inloggade tooSyncplicity (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cf49e-107">You can enable your users tooautomatically get signed-on tooSyncplicity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cf49e-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cf49e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cf49e-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cf49e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf49e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cf49e-110">Prerequisites</span></span>

<span data-ttu-id="cf49e-111">tooconfigure Azure AD-integrering med Syncplicity, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="cf49e-111">tooconfigure Azure AD integration with Syncplicity, you need hello following items:</span></span>

- <span data-ttu-id="cf49e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cf49e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cf49e-113">En Syncplicity enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="cf49e-113">A Syncplicity single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cf49e-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cf49e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cf49e-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cf49e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cf49e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cf49e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cf49e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cf49e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cf49e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cf49e-118">Scenario description</span></span>
<span data-ttu-id="cf49e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cf49e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cf49e-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cf49e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cf49e-121">Att lägga till Syncplicity från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="cf49e-121">Adding Syncplicity from hello gallery</span></span>
2. <span data-ttu-id="cf49e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cf49e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-syncplicity-from-hello-gallery"></a><span data-ttu-id="cf49e-123">Att lägga till Syncplicity från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="cf49e-123">Adding Syncplicity from hello gallery</span></span>
<span data-ttu-id="cf49e-124">tooconfigure hello integrering av Syncplicity i Azure AD, behöver du tooadd Syncplicity hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="cf49e-124">tooconfigure hello integration of Syncplicity into Azure AD, you need tooadd Syncplicity from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cf49e-125">**tooadd Syncplicity från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cf49e-125">**tooadd Syncplicity from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf49e-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cf49e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cf49e-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cf49e-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cf49e-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cf49e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cf49e-133">Skriv i sökrutan hello **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-133">In hello search box, type **Syncplicity**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. <span data-ttu-id="cf49e-135">Markera hello resultat på panelen **Syncplicity**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="cf49e-135">In hello results panel, select **Syncplicity**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cf49e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cf49e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cf49e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Syncplicity baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cf49e-138">In this section, you configure and test Azure AD single sign-on with Syncplicity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cf49e-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Syncplicity är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cf49e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Syncplicity is tooa user in Azure AD.</span></span> <span data-ttu-id="cf49e-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Syncplicity toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="cf49e-140">In other words, a link relationship between an Azure AD user and hello related user in Syncplicity needs toobe established.</span></span>

<span data-ttu-id="cf49e-141">I Syncplicity, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-141">In Syncplicity, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cf49e-142">tooconfigure och testa Azure AD enkel inloggning med Syncplicity, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cf49e-142">tooconfigure and test Azure AD single sign-on with Syncplicity, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cf49e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cf49e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cf49e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cf49e-145">**[Skapa en testanvändare Syncplicity](#creating-a-syncplicity-test-user)**  -toohave en motsvarighet för Britta Simon i Syncplicity som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cf49e-145">**[Creating a Syncplicity test user](#creating-a-syncplicity-test-user)** - toohave a counterpart of Britta Simon in Syncplicity that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cf49e-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cf49e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cf49e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cf49e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cf49e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cf49e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cf49e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Syncplicity program.</span><span class="sxs-lookup"><span data-stu-id="cf49e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Syncplicity application.</span></span>

<span data-ttu-id="cf49e-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Syncplicity:**</span><span class="sxs-lookup"><span data-stu-id="cf49e-150">**tooconfigure Azure AD single sign-on with Syncplicity, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf49e-151">I hello Azure-portalen på hello **Syncplicity** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-151">In hello Azure portal, on hello **Syncplicity** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cf49e-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cf49e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. <span data-ttu-id="cf49e-155">På hello **Syncplicity domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cf49e-155">On hello **Syncplicity Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    <span data-ttu-id="cf49e-157">a.</span><span class="sxs-lookup"><span data-stu-id="cf49e-157">a.</span></span> <span data-ttu-id="cf49e-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.syncplicity.com`</span><span class="sxs-lookup"><span data-stu-id="cf49e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.syncplicity.com`</span></span>

    <span data-ttu-id="cf49e-159">b.</span><span class="sxs-lookup"><span data-stu-id="cf49e-159">b.</span></span> <span data-ttu-id="cf49e-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.syncplicity.com/sp`</span><span class="sxs-lookup"><span data-stu-id="cf49e-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.syncplicity.com/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cf49e-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cf49e-161">These values are not real.</span></span> <span data-ttu-id="cf49e-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="cf49e-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cf49e-163">Kontakta [Syncplicity klienten supportteamet](https://www.syncplicity.com/contact-us) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="cf49e-163">Contact [Syncplicity Client support team](https://www.syncplicity.com/contact-us) tooget these values.</span></span> 
 

4. <span data-ttu-id="cf49e-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="cf49e-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. <span data-ttu-id="cf49e-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cf49e-168">På hello **Syncplicity Configuration** klickar du på **konfigurera Syncplicity** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="cf49e-168">On hello **Syncplicity Configuration** section, click **Configure Syncplicity** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cf49e-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="cf49e-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. <span data-ttu-id="cf49e-171">Logga in tooyour **Syncplicity** klient.</span><span class="sxs-lookup"><span data-stu-id="cf49e-171">Sign in tooyour **Syncplicity** tenant.</span></span>

8. <span data-ttu-id="cf49e-172">Hello-menyn överst hello **admin**väljer **inställningar**, och klicka sedan på **anpassad domän och enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-172">In hello menu on hello top, click **admin**, select **settings**, and then click **Custom domain and single sign-on**.</span></span>
   
    <span data-ttu-id="cf49e-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span><span class="sxs-lookup"><span data-stu-id="cf49e-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span></span>

9. <span data-ttu-id="cf49e-174">På hello **enkel inloggning (SSO)** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cf49e-174">On hello **Single Sign-On (SSO)** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="cf49e-175">![Enkel inloggning \(enkel inloggning\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span><span class="sxs-lookup"><span data-stu-id="cf49e-175">![Single Sign-On \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span></span>   

    <span data-ttu-id="cf49e-176">a.</span><span class="sxs-lookup"><span data-stu-id="cf49e-176">a.</span></span> <span data-ttu-id="cf49e-177">I hello **anpassad domän** textruta hello-typnamn för din domän.</span><span class="sxs-lookup"><span data-stu-id="cf49e-177">In hello **Custom Domain** textbox, type hello name of your domain.</span></span>
  
    <span data-ttu-id="cf49e-178">b.</span><span class="sxs-lookup"><span data-stu-id="cf49e-178">b.</span></span> <span data-ttu-id="cf49e-179">Välj **aktiverat** som **enkel inloggning Status**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-179">Select **Enabled** as **Single Sign-On Status**.</span></span>

    <span data-ttu-id="cf49e-180">c.</span><span class="sxs-lookup"><span data-stu-id="cf49e-180">c.</span></span> <span data-ttu-id="cf49e-181">I hello **enhets-Id** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-181">In hello **Entity Id** textbox, Paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="cf49e-182">d.</span><span class="sxs-lookup"><span data-stu-id="cf49e-182">d.</span></span> <span data-ttu-id="cf49e-183">I hello **inloggning Sidadress** textruta klistra in hello **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-183">In hello **Sign-in page URL** textbox, Paste hello **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="cf49e-184">e.</span><span class="sxs-lookup"><span data-stu-id="cf49e-184">e.</span></span> <span data-ttu-id="cf49e-185">I hello **logga ut Sidadress** textruta klistra in hello **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-185">In hello **Logout page URL** textbox, Paste hello **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="cf49e-186">f.</span><span class="sxs-lookup"><span data-stu-id="cf49e-186">f.</span></span> <span data-ttu-id="cf49e-187">I **providern identitetscertifikat**, klickar du på **Välj fil**, och sedan ladda upp hello-certifikat som du har hämtat från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-187">In **Identity Provider Certificate**, click **Choose file**, and then upload hello certificate which you have downloaded from hello Azure portal.</span></span> 

    <span data-ttu-id="cf49e-188">g.</span><span class="sxs-lookup"><span data-stu-id="cf49e-188">g.</span></span> <span data-ttu-id="cf49e-189">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-189">Click **SAVE CHANGES**.</span></span>

> [!TIP]
> <span data-ttu-id="cf49e-190">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="cf49e-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cf49e-191">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="cf49e-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cf49e-192">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cf49e-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cf49e-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf49e-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="cf49e-194">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cf49e-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cf49e-196">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cf49e-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf49e-197">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cf49e-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cf49e-199">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cf49e-201">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cf49e-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cf49e-203">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cf49e-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cf49e-205">a.</span><span class="sxs-lookup"><span data-stu-id="cf49e-205">a.</span></span> <span data-ttu-id="cf49e-206">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cf49e-207">b.</span><span class="sxs-lookup"><span data-stu-id="cf49e-207">b.</span></span> <span data-ttu-id="cf49e-208">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cf49e-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cf49e-209">c.</span><span class="sxs-lookup"><span data-stu-id="cf49e-209">c.</span></span> <span data-ttu-id="cf49e-210">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cf49e-211">d.</span><span class="sxs-lookup"><span data-stu-id="cf49e-211">d.</span></span> <span data-ttu-id="cf49e-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-212">Click **Create**.</span></span>
 
### <a name="creating-a-syncplicity-test-user"></a><span data-ttu-id="cf49e-213">Skapa en testanvändare Syncplicity</span><span class="sxs-lookup"><span data-stu-id="cf49e-213">Creating a Syncplicity test user</span></span>
<span data-ttu-id="cf49e-214">För AAD användare toobe kan toosign i, måste de vara etablerade tooSyncplicity program.</span><span class="sxs-lookup"><span data-stu-id="cf49e-214">For AAD users toobe able toosign in, they must be provisioned tooSyncplicity application.</span></span> <span data-ttu-id="cf49e-215">Det här avsnittet beskrivs hur toocreate AAD användarkonton i Syncplicity.</span><span class="sxs-lookup"><span data-stu-id="cf49e-215">This section describes how toocreate AAD user accounts in Syncplicity.</span></span>

<span data-ttu-id="cf49e-216">**tooprovision konto tooSyncplicity för en användare utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cf49e-216">**tooprovision a user account tooSyncplicity, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf49e-217">Logga in tooyour **Syncplicity** klient (till exempel: `https://company.Syncplicity.com`).</span><span class="sxs-lookup"><span data-stu-id="cf49e-217">Log in tooyour **Syncplicity** tenant (for example: `https://company.Syncplicity.com`).</span></span>

2. <span data-ttu-id="cf49e-218">Klicka på **admin** och välj **användarkonton**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-218">Click **admin** and select **user accounts**.</span></span>

3. <span data-ttu-id="cf49e-219">Klicka på **lägga till en användare**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-219">Click **ADD A USER**.</span></span>
   
    <span data-ttu-id="cf49e-220">![Hantera användare](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="cf49e-220">![Manage Users](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Manage Users")</span></span>

4. <span data-ttu-id="cf49e-221">Typen hello **e-adresser** på ett AAD-konto som du vill tooprovision, Välj **användare** som **rollen**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-221">Type hello **Email addressess** of an AAD account you want tooprovision, select **User** as **Role**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="cf49e-222">![Kontoinformation](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "kontoinformation")</span><span class="sxs-lookup"><span data-stu-id="cf49e-222">![Account Information](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "Account Information")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="cf49e-223">hello AAD användare får ett e-postmeddelande inklusive en länk tooconfirm och aktivera hello-konto.</span><span class="sxs-lookup"><span data-stu-id="cf49e-223">hello AAD account holder  gets an email including a link tooconfirm and activate hello account.</span></span> 
    > 

5. <span data-ttu-id="cf49e-224">Välj en grupp i företaget som de nya användarna ska bli medlem i och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-224">Select a group in your company that your new user should become a member of, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="cf49e-225">![Gruppmedlemskap](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "gruppmedlemskap")</span><span class="sxs-lookup"><span data-stu-id="cf49e-225">![Group Membership](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "Group Membership")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="cf49e-226">Om det finns inga grupper visas, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-226">If there are no groups listed, click **NEXT**.</span></span> 
    > 

6. <span data-ttu-id="cf49e-227">Välj hello mappar du gillar tooplace under Syncplicitys kontroll på hello användares dator och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-227">Select hello folders you would like tooplace under Syncplicity’s control on hello user’s computer, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="cf49e-228">![Syncplicity mappar](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity mappar")</span><span class="sxs-lookup"><span data-stu-id="cf49e-228">![Syncplicity Folders](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity Folders")</span></span>

>[!NOTE]
><span data-ttu-id="cf49e-229">Du kan använda något annat Syncplicity användarens konto skapas verktyg eller API: er som tillhandahålls av Syncplicity tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="cf49e-229">You can use any other Syncplicity user account creation tools or APIs provided by Syncplicity tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cf49e-230">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf49e-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cf49e-231">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSyncplicity.</span><span class="sxs-lookup"><span data-stu-id="cf49e-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSyncplicity.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cf49e-233">**tooassign Britta Simon tooSyncplicity utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cf49e-233">**tooassign Britta Simon tooSyncplicity, perform hello following steps:**</span></span>

1. <span data-ttu-id="cf49e-234">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cf49e-236">Välj i listan med program hello **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-236">In hello applications list, select **Syncplicity**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. <span data-ttu-id="cf49e-238">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cf49e-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cf49e-240">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-240">Click **Add** button.</span></span> <span data-ttu-id="cf49e-241">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cf49e-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cf49e-243">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="cf49e-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cf49e-244">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cf49e-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cf49e-245">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cf49e-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cf49e-246">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cf49e-246">Testing single sign-on</span></span>

<span data-ttu-id="cf49e-247">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-247">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cf49e-248">Du bör få automatiskt inloggade tooyour Syncplicity programmet när du klickar på hello Syncplicity panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cf49e-248">When you click hello Syncplicity tile in hello Access Panel, you should get automatically signed-on tooyour Syncplicity application.</span></span>
## <a name="additional-resources"></a><span data-ttu-id="cf49e-249">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cf49e-249">Additional resources</span></span>

* [<span data-ttu-id="cf49e-250">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cf49e-250">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cf49e-251">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cf49e-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

