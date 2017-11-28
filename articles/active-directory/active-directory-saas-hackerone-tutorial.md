---
title: "Självstudier: Azure Active Directory-integrering med Hackerone | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Hackerone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: c9dc033e26e79a7233dcfb3899c62684d4a19652
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a><span data-ttu-id="cd1bc-103">Självstudier: Azure Active Directory-integrering med HackerOne</span><span class="sxs-lookup"><span data-stu-id="cd1bc-103">Tutorial: Azure Active Directory integration with HackerOne</span></span>

<span data-ttu-id="cd1bc-104">I kursen får du lära dig hur toointegrate HackerOne med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cd1bc-104">In this tutorial, you learn how toointegrate HackerOne with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cd1bc-105">Integrera HackerOne med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cd1bc-105">Integrating HackerOne with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cd1bc-106">Du kan styra i Azure AD som har åtkomst till tooHackerOne</span><span class="sxs-lookup"><span data-stu-id="cd1bc-106">You can control in Azure AD who has access tooHackerOne</span></span>
- <span data-ttu-id="cd1bc-107">Du kan aktivera din användare tooautomatically get inloggade tooHackerOne (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cd1bc-107">You can enable your users tooautomatically get signed-on tooHackerOne (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cd1bc-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cd1bc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cd1bc-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cd1bc-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd1bc-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cd1bc-110">Prerequisites</span></span>

<span data-ttu-id="cd1bc-111">tooconfigure Azure AD-integrering med HackerOne, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="cd1bc-111">tooconfigure Azure AD integration with HackerOne, you need hello following items:</span></span>

- <span data-ttu-id="cd1bc-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cd1bc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cd1bc-113">En HackerOne enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="cd1bc-113">A HackerOne single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cd1bc-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cd1bc-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cd1bc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cd1bc-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cd1bc-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd1bc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cd1bc-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cd1bc-118">Scenario description</span></span>
<span data-ttu-id="cd1bc-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cd1bc-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cd1bc-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cd1bc-121">Att lägga till HackerOne från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="cd1bc-121">Adding HackerOne from hello gallery</span></span>
2. <span data-ttu-id="cd1bc-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd1bc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hackerone-from-hello-gallery"></a><span data-ttu-id="cd1bc-123">Att lägga till HackerOne från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="cd1bc-123">Adding HackerOne from hello gallery</span></span>
<span data-ttu-id="cd1bc-124">tooconfigure hello integrering av HackerOne i Azure AD, behöver du tooadd HackerOne hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-124">tooconfigure hello integration of HackerOne into Azure AD, you need tooadd HackerOne from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cd1bc-125">**tooadd HackerOne från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cd1bc-125">**tooadd HackerOne from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd1bc-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cd1bc-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cd1bc-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cd1bc-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cd1bc-133">Skriv i sökrutan hello **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-133">In hello search box, type **HackerOne**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_search.png)

5. <span data-ttu-id="cd1bc-135">Markera hello resultat på panelen **HackerOne**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-135">In hello results panel, select **HackerOne**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cd1bc-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd1bc-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="cd1bc-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med HackerOne baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-138">In this section, you configure and test Azure AD single sign-on with HackerOne based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cd1bc-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i HackerOne är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in HackerOne is tooa user in Azure AD.</span></span> <span data-ttu-id="cd1bc-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i HackerOne toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-140">In other words, a link relationship between an Azure AD user and hello related user in HackerOne needs toobe established.</span></span>

<span data-ttu-id="cd1bc-141">I HackerOne, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-141">In HackerOne, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cd1bc-142">tooconfigure och testa Azure AD enkel inloggning med HackerOne, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cd1bc-142">tooconfigure and test Azure AD single sign-on with HackerOne, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cd1bc-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cd1bc-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cd1bc-145">**[Skapa en testanvändare HackerOne](#creating-a-hackerone-test-user)**  -toohave en motsvarighet för Britta Simon i HackerOne som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-145">**[Creating a HackerOne test user](#creating-a-hackerone-test-user)** - toohave a counterpart of Britta Simon in HackerOne that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cd1bc-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cd1bc-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cd1bc-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd1bc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cd1bc-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt HackerOne program.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HackerOne application.</span></span>

<span data-ttu-id="cd1bc-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med HackerOne:**</span><span class="sxs-lookup"><span data-stu-id="cd1bc-150">**tooconfigure Azure AD single sign-on with HackerOne, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd1bc-151">I hello Azure-portalen på hello **HackerOne** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-151">In hello Azure portal, on hello **HackerOne** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cd1bc-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_samlbase.png)

3. <span data-ttu-id="cd1bc-155">På hello **HackerOne enkel inloggnings-URL och identifierare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cd1bc-155">On hello **HackerOne Single sign-on URL and Identifier** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_url.png)

    <span data-ttu-id="cd1bc-157">a.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-157">a.</span></span> <span data-ttu-id="cd1bc-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://hackerone.com/<company name>/authentication`</span><span class="sxs-lookup"><span data-stu-id="cd1bc-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://hackerone.com/<company name>/authentication`</span></span>

    <span data-ttu-id="cd1bc-159">b.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-159">b.</span></span> <span data-ttu-id="cd1bc-160">I hello **identifierare** textruta Skriv en URL som:`https://hackerone.com/users/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="cd1bc-160">In hello **Identifier** textbox, type a URL as:  `https://hackerone.com/users/saml/metadata`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="cd1bc-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-161">This value is not real.</span></span> <span data-ttu-id="cd1bc-162">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-162">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="cd1bc-163">Kontakta [HackerOne supportteamet](mailto:support@hackerone.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-163">Contact [HackerOne support team](mailto:support@hackerone.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="cd1bc-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_certificate.png) 

5. <span data-ttu-id="cd1bc-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cd1bc-168">På hello **HackerOne Configuration** klickar du på **konfigurera HackerOne** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-168">On hello **HackerOne Configuration** section, click **Configure HackerOne** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cd1bc-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="cd1bc-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_configure.png) 

7. <span data-ttu-id="cd1bc-171">Inloggning tooyour HackerOne-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-171">Sign On tooyour HackerOne tenant as an administrator.</span></span>

8. <span data-ttu-id="cd1bc-172">Hello hello överst klickar du på menyn hello ”**inställningar**”.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-172">In hello menu on hello top, click hello "**Settings**."</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

9. <span data-ttu-id="cd1bc-174">Navigera för ”**autentisering**” och klicka på ”**Lägg till SAML-inställningar**”.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-174">Navigate too"**Authentication**" and click "**Add SAML settings**."</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 

10. <span data-ttu-id="cd1bc-176">På hello **SAML inställningar** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cd1bc-176">On hello **SAML Settings** dialog, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    <span data-ttu-id="cd1bc-178">a.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-178">a.</span></span> <span data-ttu-id="cd1bc-179">I hello **e-postdomän** textruta skriver en registrerad domän.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-179">In hello **Email Domain** textbox, type a registered domain.</span></span>

    <span data-ttu-id="cd1bc-180">b.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-180">b.</span></span> <span data-ttu-id="cd1bc-181">I **enkel inloggning på URL: en** textrutor, klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-181">In  **Single Sign On URL** textboxes, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="cd1bc-182">c.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-182">c.</span></span> <span data-ttu-id="cd1bc-183">Öppna din **certifikatfilen** i anteckningar som hämtas från Azure-portalen, kopiera hello innehållet i den till Urklipp och klistra in den toohello **X509 certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-183">Open your **Certificate file** in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X509 Certificate**  textbox.</span></span>
    
    <span data-ttu-id="cd1bc-184">d.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-184">d.</span></span> <span data-ttu-id="cd1bc-185">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-185">Click **Save**.</span></span>

11. <span data-ttu-id="cd1bc-186">Utför följande hello på hello autentiseringsinställningar dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="cd1bc-186">On hello Authentication Settings dialog, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    <span data-ttu-id="cd1bc-188">a.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-188">a.</span></span> <span data-ttu-id="cd1bc-189">Klicka på **Kör test**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-189">Click **Run test**.</span></span>

    <span data-ttu-id="cd1bc-190">b.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-190">b.</span></span> <span data-ttu-id="cd1bc-191">Om hello värdet för hello **Status** fältet är lika med **senast teststatus: skapa**, kontakta din [HackerOne supportteamet](mailto:support@hackerone.com) toorequest en granskning av din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-191">If hello value of hello **Status** field equals **Last test status: created**, contact your [HackerOne support team](mailto:support@hackerone.com) toorequest a review of your configuration.</span></span>

> [!TIP]
> <span data-ttu-id="cd1bc-192">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="cd1bc-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cd1bc-193">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cd1bc-194">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cd1bc-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cd1bc-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd1bc-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="cd1bc-196">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cd1bc-198">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cd1bc-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd1bc-199">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cd1bc-201">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cd1bc-203">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cd1bc-205">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cd1bc-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cd1bc-207">a.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-207">a.</span></span> <span data-ttu-id="cd1bc-208">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cd1bc-209">b.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-209">b.</span></span> <span data-ttu-id="cd1bc-210">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cd1bc-211">c.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-211">c.</span></span> <span data-ttu-id="cd1bc-212">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cd1bc-213">d.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-213">d.</span></span> <span data-ttu-id="cd1bc-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-214">Click **Create**.</span></span>
 
### <a name="creating-a-hackerone-test-user"></a><span data-ttu-id="cd1bc-215">Skapa en testanvändare HackerOne</span><span class="sxs-lookup"><span data-stu-id="cd1bc-215">Creating a HackerOne test user</span></span>

<span data-ttu-id="cd1bc-216">Därefter skapar du en användare som kallas Britta Simon i HackerOne.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-216">Next, you create a user called Britta Simon in HackerOne.</span></span> <span data-ttu-id="cd1bc-217">HackerOne stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-217">HackerOne supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="cd1bc-218">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-218">There is no action item for you in this section.</span></span> <span data-ttu-id="cd1bc-219">När du använder HackerOne skapas en ny användare om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-219">When you access HackerOne, a new user is created if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="cd1bc-220">Om du behöver toocreate en användare manuellt, måste toocontact hello certifiera supportteamet.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-220">If you need toocreate a user manually, you need toocontact hello Certify support team.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cd1bc-221">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd1bc-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cd1bc-222">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHackerOne.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHackerOne.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cd1bc-224">**tooassign Britta Simon tooHackerOne utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cd1bc-224">**tooassign Britta Simon tooHackerOne, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd1bc-225">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cd1bc-227">Välj i listan med program hello **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-227">In hello applications list, select **HackerOne**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_app.png) 

3. <span data-ttu-id="cd1bc-229">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cd1bc-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-231">Click **Add** button.</span></span> <span data-ttu-id="cd1bc-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cd1bc-234">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cd1bc-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cd1bc-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cd1bc-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd1bc-237">Testing single sign-on</span></span>

<span data-ttu-id="cd1bc-238">Slutligen kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-238">Finally, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="cd1bc-239">Du bör få automatiskt inloggade tooyour HackerOne programmet när du klickar på hello HackerOne panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cd1bc-239">When you click hello HackerOne tile in hello Access Panel, you should get automatically signed-on tooyour HackerOne application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cd1bc-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cd1bc-240">Additional resources</span></span>

* [<span data-ttu-id="cd1bc-241">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cd1bc-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cd1bc-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cd1bc-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png

