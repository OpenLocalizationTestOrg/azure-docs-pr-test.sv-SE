---
title: "Självstudier: Azure Active Directory-integrering med Kiteworks | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Kiteworks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7984aaf-ab1f-4a85-9646-a9523f5275d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 406417dd7f58cc3f1fa0d9e86b5cad0c1d7be750
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kiteworks"></a><span data-ttu-id="be782-103">Självstudier: Azure Active Directory-integrering med Kiteworks</span><span class="sxs-lookup"><span data-stu-id="be782-103">Tutorial: Azure Active Directory integration with Kiteworks</span></span>

<span data-ttu-id="be782-104">I kursen får du lära dig hur toointegrate Kiteworks med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="be782-104">In this tutorial, you learn how toointegrate Kiteworks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="be782-105">Integrera Kiteworks med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="be782-105">Integrating Kiteworks with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="be782-106">Du kan styra i Azure AD som har åtkomst till tooKiteworks</span><span class="sxs-lookup"><span data-stu-id="be782-106">You can control in Azure AD who has access tooKiteworks</span></span>
- <span data-ttu-id="be782-107">Du kan aktivera din användare tooautomatically get inloggade tooKiteworks (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="be782-107">You can enable your users tooautomatically get signed-on tooKiteworks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="be782-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="be782-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="be782-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="be782-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be782-110">Krav</span><span class="sxs-lookup"><span data-stu-id="be782-110">Prerequisites</span></span>

<span data-ttu-id="be782-111">tooconfigure Azure AD-integrering med Kiteworks, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="be782-111">tooconfigure Azure AD integration with Kiteworks, you need hello following items:</span></span>

- <span data-ttu-id="be782-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="be782-112">An Azure AD subscription</span></span>
- <span data-ttu-id="be782-113">En Kiteworks enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="be782-113">A Kiteworks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="be782-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="be782-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="be782-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="be782-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="be782-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="be782-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="be782-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="be782-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="be782-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="be782-118">Scenario description</span></span>
<span data-ttu-id="be782-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="be782-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="be782-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="be782-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="be782-121">Att lägga till Kiteworks från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="be782-121">Adding Kiteworks from hello gallery</span></span>
2. <span data-ttu-id="be782-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be782-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kiteworks-from-hello-gallery"></a><span data-ttu-id="be782-123">Att lägga till Kiteworks från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="be782-123">Adding Kiteworks from hello gallery</span></span>
<span data-ttu-id="be782-124">tooconfigure hello integrering av Kiteworks i Azure AD, behöver du tooadd Kiteworks hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="be782-124">tooconfigure hello integration of Kiteworks into Azure AD, you need tooadd Kiteworks from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="be782-125">**tooadd Kiteworks från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="be782-125">**tooadd Kiteworks from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="be782-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="be782-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="be782-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="be782-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="be782-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="be782-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="be782-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be782-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="be782-133">Skriv i sökrutan hello **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="be782-133">In hello search box, type **Kiteworks**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_search.png)

5. <span data-ttu-id="be782-135">Markera hello resultat på panelen **Kiteworks**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="be782-135">In hello results panel, select **Kiteworks**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="be782-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be782-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="be782-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kiteworks baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="be782-138">In this section, you configure and test Azure AD single sign-on with Kiteworks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="be782-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Kiteworks är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="be782-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kiteworks is tooa user in Azure AD.</span></span> <span data-ttu-id="be782-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Kiteworks toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="be782-140">In other words, a link relationship between an Azure AD user and hello related user in Kiteworks needs toobe established.</span></span>

<span data-ttu-id="be782-141">I Kiteworks, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="be782-141">In Kiteworks, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="be782-142">tooconfigure och testa Azure AD enkel inloggning med Kiteworks, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="be782-142">tooconfigure and test Azure AD single sign-on with Kiteworks, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="be782-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="be782-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="be782-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="be782-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="be782-145">**[Skapa en testanvändare Kiteworks](#creating-a-kiteworks-test-user)**  -toohave en motsvarighet för Britta Simon i Kiteworks som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="be782-145">**[Creating a Kiteworks test user](#creating-a-kiteworks-test-user)** - toohave a counterpart of Britta Simon in Kiteworks that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="be782-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="be782-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="be782-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="be782-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="be782-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be782-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="be782-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Kiteworks program.</span><span class="sxs-lookup"><span data-stu-id="be782-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kiteworks application.</span></span>

<span data-ttu-id="be782-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Kiteworks:**</span><span class="sxs-lookup"><span data-stu-id="be782-150">**tooconfigure Azure AD single sign-on with Kiteworks, perform hello following steps:**</span></span>

1. <span data-ttu-id="be782-151">I hello Azure-portalen på hello **Kiteworks** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="be782-151">In hello Azure portal, on hello **Kiteworks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="be782-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="be782-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_samlbase.png)

3. <span data-ttu-id="be782-155">På hello **Kiteworks domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="be782-155">On hello **Kiteworks Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_url.png)

    <span data-ttu-id="be782-157">a.</span><span class="sxs-lookup"><span data-stu-id="be782-157">a.</span></span> <span data-ttu-id="be782-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.kiteworks.com`</span><span class="sxs-lookup"><span data-stu-id="be782-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.kiteworks.com`</span></span>

    <span data-ttu-id="be782-159">b.</span><span class="sxs-lookup"><span data-stu-id="be782-159">b.</span></span> <span data-ttu-id="be782-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span><span class="sxs-lookup"><span data-stu-id="be782-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="be782-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="be782-161">These values are not real.</span></span> <span data-ttu-id="be782-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="be782-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="be782-163">Kontakta [Kiteworks klienten supportteamet](http://accellion.com/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="be782-163">Contact [Kiteworks Client support team](http://accellion.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="be782-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="be782-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_certificate.png) 

5. <span data-ttu-id="be782-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="be782-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="be782-168">På hello **Kiteworks Configuration** klickar du på **konfigurera Kiteworks** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="be782-168">On hello **Kiteworks Configuration** section, click **Configure Kiteworks** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="be782-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="be782-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_configure.png) 

7. <span data-ttu-id="be782-171">Inloggning tooyour Kiteworks företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="be782-171">Sign on tooyour Kiteworks company site as an administrator.</span></span>

8. <span data-ttu-id="be782-172">Klicka i hello verktygsfältet hello längst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="be782-172">In hello toolbar on hello top, click **Settings**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_06.png) 

9. <span data-ttu-id="be782-174">I hello **autentisering och auktorisering** klickar du på **SSO installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="be782-174">In hello **Authentication and Authorization** section, click **SSO Setup**.</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_07.png)
 
10. <span data-ttu-id="be782-176">På installationssidan för hello SSO, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="be782-176">On hello SSO Setup page, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_09.png)   

    <span data-ttu-id="be782-178">a.</span><span class="sxs-lookup"><span data-stu-id="be782-178">a.</span></span> <span data-ttu-id="be782-179">Välj **autentisera via enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="be782-179">Select **Authenticate via SSO**.</span></span>

    <span data-ttu-id="be782-180">b.</span><span class="sxs-lookup"><span data-stu-id="be782-180">b.</span></span> <span data-ttu-id="be782-181">Välj **initiera AuthnRequest**.</span><span class="sxs-lookup"><span data-stu-id="be782-181">Select **Initiate AuthnRequest**.</span></span>

    <span data-ttu-id="be782-182">c.</span><span class="sxs-lookup"><span data-stu-id="be782-182">c.</span></span> <span data-ttu-id="be782-183">I hello **IDP enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be782-183">In hello **IDP Entity ID** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="be782-184">d.</span><span class="sxs-lookup"><span data-stu-id="be782-184">d.</span></span> <span data-ttu-id="be782-185">I hello **inloggning tjänst-URL för enkel** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be782-185">In hello **Single Sign-On Service URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="be782-186">e.</span><span class="sxs-lookup"><span data-stu-id="be782-186">e.</span></span> <span data-ttu-id="be782-187">I hello **tjänst-URL för enkel logga ut** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be782-187">In hello **Single Logout Service URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="be782-188">f.</span><span class="sxs-lookup"><span data-stu-id="be782-188">f.</span></span> <span data-ttu-id="be782-189">Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll, och klistra in den i hello **RSA offentligt nyckelcertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="be782-189">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **RSA Public Key Certificate** textbox.</span></span>
 
    <span data-ttu-id="be782-190">g.</span><span class="sxs-lookup"><span data-stu-id="be782-190">g.</span></span> <span data-ttu-id="be782-191">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="be782-191">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="be782-192">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="be782-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="be782-193">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="be782-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="be782-194">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="be782-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="be782-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="be782-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="be782-196">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="be782-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="be782-198">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="be782-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="be782-199">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="be782-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="be782-201">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="be782-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="be782-203">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be782-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="be782-205">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="be782-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="be782-207">a.</span><span class="sxs-lookup"><span data-stu-id="be782-207">a.</span></span> <span data-ttu-id="be782-208">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="be782-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="be782-209">b.</span><span class="sxs-lookup"><span data-stu-id="be782-209">b.</span></span> <span data-ttu-id="be782-210">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="be782-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="be782-211">c.</span><span class="sxs-lookup"><span data-stu-id="be782-211">c.</span></span> <span data-ttu-id="be782-212">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="be782-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="be782-213">d.</span><span class="sxs-lookup"><span data-stu-id="be782-213">d.</span></span> <span data-ttu-id="be782-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="be782-214">Click **Create**.</span></span>
 
### <a name="creating-a-kiteworks-test-user"></a><span data-ttu-id="be782-215">Skapa en testanvändare Kiteworks</span><span class="sxs-lookup"><span data-stu-id="be782-215">Creating a Kiteworks test user</span></span>

<span data-ttu-id="be782-216">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Kiteworks.</span><span class="sxs-lookup"><span data-stu-id="be782-216">hello objective of this section is toocreate a user called Britta Simon in Kiteworks.</span></span>

<span data-ttu-id="be782-217">Kiteworks stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="be782-217">Kiteworks supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="be782-218">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="be782-218">There is no action item for you in this section.</span></span> <span data-ttu-id="be782-219">En ny användare skapas under ett försök tooaccess Kitewors om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="be782-219">A new user is created during an attempt tooaccess Kitewors if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="be782-220">Om du behöver toocreate en användare manuellt, måste toocontact hello [Kiteworks supportteam](http://accellion.com/support).</span><span class="sxs-lookup"><span data-stu-id="be782-220">If you need toocreate a user manually, you need toocontact hello [Kiteworks support team](http://accellion.com/support).</span></span>
 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="be782-221">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="be782-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="be782-222">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooKiteworks.</span><span class="sxs-lookup"><span data-stu-id="be782-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKiteworks.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="be782-224">**tooassign Britta Simon tooKiteworks utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="be782-224">**tooassign Britta Simon tooKiteworks, perform hello following steps:**</span></span>

1. <span data-ttu-id="be782-225">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="be782-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="be782-227">Välj i listan med program hello **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="be782-227">In hello applications list, select **Kiteworks**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_app.png) 

3. <span data-ttu-id="be782-229">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="be782-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="be782-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="be782-231">Click **Add** button.</span></span> <span data-ttu-id="be782-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be782-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="be782-234">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="be782-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="be782-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be782-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="be782-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be782-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="be782-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be782-237">Testing single sign-on</span></span>

<span data-ttu-id="be782-238">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="be782-238">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="be782-239">Du bör få automatiskt inloggade tooyour Kiteworks programmet när du klickar på hello Kiteworks panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="be782-239">When you click hello Kiteworks tile in hello Access Panel, you should get automatically signed-on tooyour Kiteworks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be782-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="be782-240">Additional resources</span></span>

* [<span data-ttu-id="be782-241">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="be782-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="be782-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="be782-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_203.png

