---
title: "Självstudier: Azure Active Directory-integrering med Samanage | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Samanage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c8edc29f113b8088438618a731e97c0f4f155b9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a><span data-ttu-id="02c8f-103">Självstudier: Azure Active Directory-integrering med Samanage</span><span class="sxs-lookup"><span data-stu-id="02c8f-103">Tutorial: Azure Active Directory integration with Samanage</span></span>

<span data-ttu-id="02c8f-104">I kursen får du lära dig hur toointegrate Samanage med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="02c8f-104">In this tutorial, you learn how toointegrate Samanage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="02c8f-105">Integrera Samanage med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="02c8f-105">Integrating Samanage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="02c8f-106">Du kan styra i Azure AD som har åtkomst till tooSamanage</span><span class="sxs-lookup"><span data-stu-id="02c8f-106">You can control in Azure AD who has access tooSamanage</span></span>
- <span data-ttu-id="02c8f-107">Du kan aktivera din användare tooautomatically get inloggade tooSamanage (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="02c8f-107">You can enable your users tooautomatically get signed-on tooSamanage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="02c8f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="02c8f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="02c8f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="02c8f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="02c8f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="02c8f-110">Prerequisites</span></span>

<span data-ttu-id="02c8f-111">tooconfigure Azure AD-integrering med Samanage, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="02c8f-111">tooconfigure Azure AD integration with Samanage, you need hello following items:</span></span>

- <span data-ttu-id="02c8f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="02c8f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="02c8f-113">En Samanage enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="02c8f-113">A Samanage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="02c8f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="02c8f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="02c8f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="02c8f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="02c8f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="02c8f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="02c8f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="02c8f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="02c8f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="02c8f-118">Scenario description</span></span>
<span data-ttu-id="02c8f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="02c8f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="02c8f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="02c8f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="02c8f-121">Att lägga till Samanage från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="02c8f-121">Adding Samanage from hello gallery</span></span>
2. <span data-ttu-id="02c8f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="02c8f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-samanage-from-hello-gallery"></a><span data-ttu-id="02c8f-123">Att lägga till Samanage från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="02c8f-123">Adding Samanage from hello gallery</span></span>
<span data-ttu-id="02c8f-124">tooconfigure hello integrering av Samanage i Azure AD, behöver du tooadd Samanage hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="02c8f-124">tooconfigure hello integration of Samanage into Azure AD, you need tooadd Samanage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="02c8f-125">**tooadd Samanage från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="02c8f-125">**tooadd Samanage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="02c8f-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="02c8f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="02c8f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="02c8f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="02c8f-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="02c8f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="02c8f-133">Skriv i sökrutan hello **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-133">In hello search box, type **Samanage**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_search.png)

5. <span data-ttu-id="02c8f-135">Markera hello resultat på panelen **Samanage**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="02c8f-135">In hello results panel, select **Samanage**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="02c8f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="02c8f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="02c8f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Samanage baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="02c8f-138">In this section, you configure and test Azure AD single sign-on with Samanage based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="02c8f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Samanage är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="02c8f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Samanage is tooa user in Azure AD.</span></span> <span data-ttu-id="02c8f-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Samanage toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="02c8f-140">In other words, a link relationship between an Azure AD user and hello related user in Samanage needs toobe established.</span></span>

<span data-ttu-id="02c8f-141">I Samanage, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-141">In Samanage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="02c8f-142">tooconfigure och testa Azure AD enkel inloggning med Samanage, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="02c8f-142">tooconfigure and test Azure AD single sign-on with Samanage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="02c8f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="02c8f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="02c8f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="02c8f-145">**[Skapa en testanvändare Samanage](#creating-a-samanage-test-user)**  -toohave en motsvarighet för Britta Simon i Samanage som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="02c8f-145">**[Creating a Samanage test user](#creating-a-samanage-test-user)** - toohave a counterpart of Britta Simon in Samanage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="02c8f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="02c8f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="02c8f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="02c8f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="02c8f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="02c8f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="02c8f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Samanage program.</span><span class="sxs-lookup"><span data-stu-id="02c8f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Samanage application.</span></span>

<span data-ttu-id="02c8f-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Samanage:**</span><span class="sxs-lookup"><span data-stu-id="02c8f-150">**tooconfigure Azure AD single sign-on with Samanage, perform hello following steps:**</span></span>

1. <span data-ttu-id="02c8f-151">I hello Azure-portalen på hello **Samanage** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-151">In hello Azure portal, on hello **Samanage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="02c8f-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="02c8f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_samlbase.png)

3. <span data-ttu-id="02c8f-155">På hello **Samanage domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="02c8f-155">On hello **Samanage Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_url.png)

    <span data-ttu-id="02c8f-157">a.</span><span class="sxs-lookup"><span data-stu-id="02c8f-157">a.</span></span> <span data-ttu-id="02c8f-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<Company Name>.samanage.com/saml_login/<Company Name>`</span><span class="sxs-lookup"><span data-stu-id="02c8f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<Company Name>.samanage.com/saml_login/<Company Name>`</span></span>

    <span data-ttu-id="02c8f-159">b.</span><span class="sxs-lookup"><span data-stu-id="02c8f-159">b.</span></span> <span data-ttu-id="02c8f-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<Company Name>.samanage.com`</span><span class="sxs-lookup"><span data-stu-id="02c8f-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<Company Name>.samanage.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="02c8f-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="02c8f-161">These values are not real.</span></span> <span data-ttu-id="02c8f-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare som beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-162">Update these values with hello actual Sign-on URL and Identifier, which is explained later in hello tutorial.</span></span> <span data-ttu-id="02c8f-163">För mer information kontaktar [Samanage klienten supportteamet](https://www.samanage.com/support).</span><span class="sxs-lookup"><span data-stu-id="02c8f-163">For more details contact [Samanage Client support team](https://www.samanage.com/support).</span></span>    
 
4. <span data-ttu-id="02c8f-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="02c8f-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_certificate.png) 

5. <span data-ttu-id="02c8f-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="02c8f-168">På hello **Samanage Configuration** klickar du på **konfigurera Samanage** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="02c8f-168">On hello **Samanage Configuration** section, click **Configure Samanage** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="02c8f-169">Kopiera hello **Sign-Out URL och SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="02c8f-169">Copy hello **Sign-Out URL, and SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_configure.png) 

7. <span data-ttu-id="02c8f-171">Logga in på webbplatsen Samanage företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="02c8f-171">In a different web browser window, log into your Samanage company site as an administrator.</span></span>

8. <span data-ttu-id="02c8f-172">Klicka på **instrumentpanelen** och välj **installationsprogrammet** i vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="02c8f-172">Click **Dashboard** and select **Setup** in left navigation pane.</span></span>
   
    <span data-ttu-id="02c8f-173">![Instrumentpanelen](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "instrumentpanelen")</span><span class="sxs-lookup"><span data-stu-id="02c8f-173">![Dashboard](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Dashboard")</span></span>

9. <span data-ttu-id="02c8f-174">Klicka på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-174">Click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="02c8f-175">![Enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="02c8f-175">![Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "Single Sign-On")</span></span>

10. <span data-ttu-id="02c8f-176">Navigera för**inloggningen med SAML** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="02c8f-176">Navigate too**Login using SAML** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="02c8f-177">![Logga in med SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "inloggningen med SAML")</span><span class="sxs-lookup"><span data-stu-id="02c8f-177">![Login using SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "Login using SAML")</span></span>
 
    <span data-ttu-id="02c8f-178">a.</span><span class="sxs-lookup"><span data-stu-id="02c8f-178">a.</span></span> <span data-ttu-id="02c8f-179">Klicka på **aktivera enkel inloggning med SAML**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-179">Click **Enable Single Sign-On with SAML**.</span></span>  
 
    <span data-ttu-id="02c8f-180">b.</span><span class="sxs-lookup"><span data-stu-id="02c8f-180">b.</span></span> <span data-ttu-id="02c8f-181">I hello **identitet providern URL** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-181">In hello **Identity Provider URL** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>    
 
    <span data-ttu-id="02c8f-182">c.</span><span class="sxs-lookup"><span data-stu-id="02c8f-182">c.</span></span> <span data-ttu-id="02c8f-183">Bekräfta hello **inloggnings-URL** matchar hello **logga URL** av **Samanage domän och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-183">Confirm hello **Login URL** matches hello **Sign On URL** of **Samanage Domain and URLs** section in Azure portal.</span></span>
 
    <span data-ttu-id="02c8f-184">d.</span><span class="sxs-lookup"><span data-stu-id="02c8f-184">d.</span></span> <span data-ttu-id="02c8f-185">I hello **logga ut URL** textruta ange hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-185">In hello **Logout URL** textbox, enter hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="02c8f-186">e.</span><span class="sxs-lookup"><span data-stu-id="02c8f-186">e.</span></span> <span data-ttu-id="02c8f-187">I hello **SAML utfärdaren** textruta typen hello app-id URI som angetts i din identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="02c8f-187">In hello **SAML Issuer** textbox, type hello app id URI set in your identity provider.</span></span>
 
    <span data-ttu-id="02c8f-188">f.</span><span class="sxs-lookup"><span data-stu-id="02c8f-188">f.</span></span> <span data-ttu-id="02c8f-189">Öppna din Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **klistra in din identitetsleverantör x.509-certifikatet nedan** textruta.</span><span class="sxs-lookup"><span data-stu-id="02c8f-189">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **Paste your Identity Provider x.509 Certificate below** textbox.</span></span>
 
    <span data-ttu-id="02c8f-190">g.</span><span class="sxs-lookup"><span data-stu-id="02c8f-190">g.</span></span> <span data-ttu-id="02c8f-191">Klicka på **skapa användare om de inte finns i Samanage**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-191">Click **Create users if they do not exist in Samanage**.</span></span>
 
    <span data-ttu-id="02c8f-192">h.</span><span class="sxs-lookup"><span data-stu-id="02c8f-192">h.</span></span> <span data-ttu-id="02c8f-193">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-193">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="02c8f-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="02c8f-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="02c8f-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="02c8f-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="02c8f-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="02c8f-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="02c8f-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="02c8f-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="02c8f-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="02c8f-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="02c8f-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="02c8f-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="02c8f-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="02c8f-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="02c8f-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="02c8f-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="02c8f-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="02c8f-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="02c8f-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="02c8f-209">a.</span><span class="sxs-lookup"><span data-stu-id="02c8f-209">a.</span></span> <span data-ttu-id="02c8f-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="02c8f-211">b.</span><span class="sxs-lookup"><span data-stu-id="02c8f-211">b.</span></span> <span data-ttu-id="02c8f-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="02c8f-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="02c8f-213">c.</span><span class="sxs-lookup"><span data-stu-id="02c8f-213">c.</span></span> <span data-ttu-id="02c8f-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="02c8f-215">d.</span><span class="sxs-lookup"><span data-stu-id="02c8f-215">d.</span></span> <span data-ttu-id="02c8f-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-216">Click **Create**.</span></span>
 
### <a name="creating-a-samanage-test-user"></a><span data-ttu-id="02c8f-217">Skapa en testanvändare Samanage</span><span class="sxs-lookup"><span data-stu-id="02c8f-217">Creating a Samanage test user</span></span>

<span data-ttu-id="02c8f-218">tooenable Azure AD-användare toolog i tooSamanage, måste de etableras i Samanage.</span><span class="sxs-lookup"><span data-stu-id="02c8f-218">tooenable Azure AD users toolog in tooSamanage, they must be provisioned into Samanage.</span></span>  
<span data-ttu-id="02c8f-219">Hello gäller Samanage är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="02c8f-219">In hello case of Samanage, provisioning is a manual task.</span></span>

<span data-ttu-id="02c8f-220">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="02c8f-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="02c8f-221">Logga in på webbplatsen Samanage företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="02c8f-221">Log into your Samanage company site as an administrator.</span></span>

2. <span data-ttu-id="02c8f-222">Klicka på **instrumentpanelen** och välj **installationsprogrammet** i navigeringen till vänster Panorera.</span><span class="sxs-lookup"><span data-stu-id="02c8f-222">Click **Dashboard** and select **Setup** in left navigation pan.</span></span>
   
    <span data-ttu-id="02c8f-223">![Installationsprogrammet](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="02c8f-223">![Setup](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Setup")</span></span>

3. <span data-ttu-id="02c8f-224">Klicka på hello **användare** fliken</span><span class="sxs-lookup"><span data-stu-id="02c8f-224">Click hello **Users** tab</span></span>
   
    <span data-ttu-id="02c8f-225">![Användare](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "användare")</span><span class="sxs-lookup"><span data-stu-id="02c8f-225">![Users](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "Users")</span></span>

4. <span data-ttu-id="02c8f-226">Klicka på **ny användare**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-226">Click **New User**.</span></span>
   
    <span data-ttu-id="02c8f-227">![Ny användare](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="02c8f-227">![New User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "New User")</span></span>

5. <span data-ttu-id="02c8f-228">Typen hello **namn** och hello **e-postadress** för ett Azure Active Directory-konto du vill tooprovision och på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-228">Type hello **Name** and hello **Email Address** of an Azure Active Directory account you want tooprovision and click **Create user**.</span></span>
   
    <span data-ttu-id="02c8f-229">![Skapa användare](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="02c8f-229">![Create User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "Create User")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="02c8f-230">hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="02c8f-230">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span> <span data-ttu-id="02c8f-231">Du kan använda något annat Samanage användarens konto skapas verktyg eller API: er som tillhandahålls av Samanage tooprovision Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="02c8f-231">You can use any other Samanage user account creation tools or APIs provided by Samanage tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="02c8f-232">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="02c8f-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="02c8f-233">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSamanage.</span><span class="sxs-lookup"><span data-stu-id="02c8f-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSamanage.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="02c8f-235">**tooassign Britta Simon tooSamanage utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="02c8f-235">**tooassign Britta Simon tooSamanage, perform hello following steps:**</span></span>

1. <span data-ttu-id="02c8f-236">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="02c8f-238">Välj i listan med program hello **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-238">In hello applications list, select **Samanage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_app.png) 

3. <span data-ttu-id="02c8f-240">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="02c8f-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="02c8f-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-242">Click **Add** button.</span></span> <span data-ttu-id="02c8f-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="02c8f-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="02c8f-245">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="02c8f-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="02c8f-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="02c8f-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="02c8f-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="02c8f-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="02c8f-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="02c8f-248">Testing single sign-on</span></span>

<span data-ttu-id="02c8f-249">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-249">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="02c8f-250">Du bör få automatiskt inloggade tooyour Samanage programmet när du klickar på hello Samanage panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="02c8f-250">When you click hello Samanage tile in hello Access Panel, you should get automatically signed-on tooyour Samanage application.</span></span>
<span data-ttu-id="02c8f-251">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="02c8f-251">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="02c8f-252">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="02c8f-252">Additional resources</span></span>

* [<span data-ttu-id="02c8f-253">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="02c8f-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="02c8f-254">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="02c8f-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_203.png

