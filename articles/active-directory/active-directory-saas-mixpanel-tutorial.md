---
title: "Självstudier: Azure Active Directory-integrering med Mixpanel | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Mixpanel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 8da8aaefee3558c37babe3e10aeba4224ceffa16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a><span data-ttu-id="ff389-103">Självstudier: Azure Active Directory-integrering med Mixpanel</span><span class="sxs-lookup"><span data-stu-id="ff389-103">Tutorial: Azure Active Directory integration with Mixpanel</span></span>

<span data-ttu-id="ff389-104">I kursen får du lära dig hur toointegrate Mixpanel med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ff389-104">In this tutorial, you learn how toointegrate Mixpanel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ff389-105">Integrera Mixpanel med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ff389-105">Integrating Mixpanel with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ff389-106">Du kan styra i Azure AD som har åtkomst till tooMixpanel</span><span class="sxs-lookup"><span data-stu-id="ff389-106">You can control in Azure AD who has access tooMixpanel</span></span>
- <span data-ttu-id="ff389-107">Du kan aktivera din användare tooautomatically get inloggade tooMixpanel (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ff389-107">You can enable your users tooautomatically get signed-on tooMixpanel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ff389-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ff389-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ff389-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ff389-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff389-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ff389-110">Prerequisites</span></span>

<span data-ttu-id="ff389-111">tooconfigure Azure AD-integrering med Mixpanel, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ff389-111">tooconfigure Azure AD integration with Mixpanel, you need hello following items:</span></span>

- <span data-ttu-id="ff389-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ff389-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ff389-113">En Mixpanel enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ff389-113">A Mixpanel single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ff389-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ff389-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ff389-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ff389-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ff389-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ff389-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ff389-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff389-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ff389-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ff389-118">Scenario description</span></span>
<span data-ttu-id="ff389-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ff389-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ff389-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ff389-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ff389-121">Att lägga till Mixpanel från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ff389-121">Adding Mixpanel from hello gallery</span></span>
2. <span data-ttu-id="ff389-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ff389-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mixpanel-from-hello-gallery"></a><span data-ttu-id="ff389-123">Att lägga till Mixpanel från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ff389-123">Adding Mixpanel from hello gallery</span></span>
<span data-ttu-id="ff389-124">tooconfigure hello integrering av Mixpanel i Azure AD, behöver du tooadd Mixpanel hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="ff389-124">tooconfigure hello integration of Mixpanel into Azure AD, you need tooadd Mixpanel from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ff389-125">**tooadd Mixpanel från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ff389-125">**tooadd Mixpanel from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff389-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ff389-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ff389-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ff389-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ff389-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="ff389-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ff389-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff389-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ff389-133">Skriv i sökrutan hello **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="ff389-133">In hello search box, type **Mixpanel**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_search.png)

5. <span data-ttu-id="ff389-135">Markera hello resultat på panelen **Mixpanel**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="ff389-135">In hello results panel, select **Mixpanel**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ff389-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ff389-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ff389-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Mixpanel baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ff389-138">In this section, you configure and test Azure AD single sign-on with Mixpanel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ff389-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Mixpanel är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff389-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mixpanel is tooa user in Azure AD.</span></span> <span data-ttu-id="ff389-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Mixpanel toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="ff389-140">In other words, a link relationship between an Azure AD user and hello related user in Mixpanel needs toobe established.</span></span>

<span data-ttu-id="ff389-141">I Mixpanel, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ff389-141">In Mixpanel, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ff389-142">tooconfigure och testa Azure AD enkel inloggning med Mixpanel, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ff389-142">tooconfigure and test Azure AD single sign-on with Mixpanel, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ff389-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ff389-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ff389-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff389-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ff389-145">**[Skapa en testanvändare Mixpanel](#creating-a-mixpanel-test-user)**  -toohave en motsvarighet för Britta Simon i Mixpanel som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ff389-145">**[Creating a Mixpanel test user](#creating-a-mixpanel-test-user)** - toohave a counterpart of Britta Simon in Mixpanel that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ff389-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ff389-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ff389-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ff389-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ff389-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ff389-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ff389-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Mixpanel program.</span><span class="sxs-lookup"><span data-stu-id="ff389-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mixpanel application.</span></span>

<span data-ttu-id="ff389-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Mixpanel:**</span><span class="sxs-lookup"><span data-stu-id="ff389-150">**tooconfigure Azure AD single sign-on with Mixpanel, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff389-151">I hello Azure-portalen på hello **Mixpanel** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ff389-151">In hello Azure portal, on hello **Mixpanel** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ff389-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ff389-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

3. <span data-ttu-id="ff389-155">På hello **Mixpanel domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ff389-155">On hello **Mixpanel Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_url.png)

     <span data-ttu-id="ff389-157">I hello **inloggnings-URL** textruta Skriv en URL som:`https://mixpanel.com/login/`</span><span class="sxs-lookup"><span data-stu-id="ff389-157">In hello **Sign-on URL** textbox, type a URL as: `https://mixpanel.com/login/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ff389-158">Registrera på [https://mixpanel.com/register/](https://mixpanel.com/register/) tooset in dina inloggningsuppgifter och kontakta hello [Mixpanel supportteamet](mailto:support@mixpanel.com) tooenable SSO-inställningarna för din klient.</span><span class="sxs-lookup"><span data-stu-id="ff389-158">Please register at [https://mixpanel.com/register/](https://mixpanel.com/register/) tooset up your login credentials and  contact hello [Mixpanel support team](mailto:support@mixpanel.com) tooenable SSO settings for your tenant.</span></span> <span data-ttu-id="ff389-159">Du kan också få tecken i URL-värdet om det behövs från supportteamet Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="ff389-159">You can also get your Sign On URL value if necessary from your Mixpanel support team.</span></span> 
 
4. <span data-ttu-id="ff389-160">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="ff389-160">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

5. <span data-ttu-id="ff389-162">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ff389-162">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ff389-164">På hello **Mixpanel Configuration** klickar du på **konfigurera Mixpanel** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ff389-164">On hello **Mixpanel Configuration** section, click **Configure Mixpanel** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ff389-165">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ff389-165">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_configure.png) 

7. <span data-ttu-id="ff389-167">I ett annat fönster i webbläsaren, inloggning tooyour Mixpanel programmet som administratör.</span><span class="sxs-lookup"><span data-stu-id="ff389-167">In a different browser window, sign-on tooyour Mixpanel application as an administrator.</span></span>

8. <span data-ttu-id="ff389-168">Längst ned på hello-sidan, klicka på hello lite **redskap** i hello vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="ff389-168">On bottom of hello page, click hello little **gear** icon in hello left corner.</span></span> 
   
    ![Mixpanel enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_06.png) 

9. <span data-ttu-id="ff389-170">Klicka på hello **säker programåtkomst** fliken och klicka sedan på **ändra inställningar**.</span><span class="sxs-lookup"><span data-stu-id="ff389-170">Click hello **Access security** tab, and then click **Change settings**.</span></span>
   
    ![Mixpanel inställningar](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_08.png) 

10. <span data-ttu-id="ff389-172">På hello **ändra ditt certifikat** dialogrutan sidan, klickar du på **Välj fil** tooupload din hämtat certifikat och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ff389-172">On hello **Change your certificate** dialog page, click **Choose file** tooupload your downloaded certificate, and then click **NEXT**.</span></span>
   
    ![Mixpanel inställningar](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_09.png) 

11.  <span data-ttu-id="ff389-174">Hello autentisering URL-textrutan på hello med **ändra autentiserings-URL** dialogrutan sida, klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ff389-174">In hello authentication URL textbox on hello **Change your authentication  URL** dialog page, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal, and then click **NEXT**.</span></span>
   
   ![Mixpanel inställningar](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_10.png) 

12. <span data-ttu-id="ff389-176">Klicka på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="ff389-176">Click **Done**.</span></span>

> [!TIP]
> <span data-ttu-id="ff389-177">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="ff389-177">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ff389-178">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="ff389-178">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ff389-179">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ff389-179">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ff389-180">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff389-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="ff389-181">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff389-181">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ff389-183">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ff389-183">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff389-184">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ff389-184">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ff389-186">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ff389-186">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ff389-188">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff389-188">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ff389-190">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ff389-190">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ff389-192">a.</span><span class="sxs-lookup"><span data-stu-id="ff389-192">a.</span></span> <span data-ttu-id="ff389-193">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ff389-193">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ff389-194">b.</span><span class="sxs-lookup"><span data-stu-id="ff389-194">b.</span></span> <span data-ttu-id="ff389-195">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ff389-195">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ff389-196">c.</span><span class="sxs-lookup"><span data-stu-id="ff389-196">c.</span></span> <span data-ttu-id="ff389-197">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ff389-197">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ff389-198">d.</span><span class="sxs-lookup"><span data-stu-id="ff389-198">d.</span></span> <span data-ttu-id="ff389-199">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ff389-199">Click **Create**.</span></span>
 
### <a name="creating-a-mixpanel-test-user"></a><span data-ttu-id="ff389-200">Skapa en testanvändare Mixpanel</span><span class="sxs-lookup"><span data-stu-id="ff389-200">Creating a Mixpanel test user</span></span>

<span data-ttu-id="ff389-201">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="ff389-201">hello objective of this section is toocreate a user called Britta Simon in Mixpanel.</span></span> 

1. <span data-ttu-id="ff389-202">Inloggning tooyour Mixpanel företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="ff389-202">Sign on tooyour Mixpanel company site as an administrator.</span></span>

2. <span data-ttu-id="ff389-203">Klicka på hello längst ned på sidan hello hello lite kugghjulet knappen på hello vänstra hörnet tooopen hello **inställningar** fönster.</span><span class="sxs-lookup"><span data-stu-id="ff389-203">On hello bottom of hello page, click hello little gear button on hello left corner tooopen hello **Settings** window.</span></span>

3. <span data-ttu-id="ff389-204">Klicka på hello **Team** fliken.</span><span class="sxs-lookup"><span data-stu-id="ff389-204">Click hello **Team** tab.</span></span>

4. <span data-ttu-id="ff389-205">I hello **teammedlem** textruta Skriv Brittas e-postadress i hello Azure.</span><span class="sxs-lookup"><span data-stu-id="ff389-205">In hello **team member** textbox, type Britta's email address in hello Azure.</span></span>
   
    ![Mixpanel inställningar](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_11.png) 

5. <span data-ttu-id="ff389-207">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="ff389-207">Click **Invite**.</span></span> 

> [!Note]
> <span data-ttu-id="ff389-208">hello användaren får ett e-tooset hello profil.</span><span class="sxs-lookup"><span data-stu-id="ff389-208">hello user will get an email tooset up hello profile.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ff389-209">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff389-209">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ff389-210">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMixpanel.</span><span class="sxs-lookup"><span data-stu-id="ff389-210">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMixpanel.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ff389-212">**tooassign Britta Simon tooMixpanel utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ff389-212">**tooassign Britta Simon tooMixpanel, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff389-213">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ff389-213">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ff389-215">Välj i listan med program hello **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="ff389-215">In hello applications list, select **Mixpanel**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_app.png) 

3. <span data-ttu-id="ff389-217">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ff389-217">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ff389-219">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ff389-219">Click **Add** button.</span></span> <span data-ttu-id="ff389-220">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff389-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ff389-222">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="ff389-222">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ff389-223">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff389-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ff389-224">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff389-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ff389-225">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ff389-225">Testing single sign-on</span></span>

<span data-ttu-id="ff389-226">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ff389-226">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ff389-227">Du bör få automatiskt inloggade tooyour Mixpanel programmet när du klickar på hello Mixpanel panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ff389-227">When you click hello Mixpanel tile in hello Access Panel, you should get automatically signed-on tooyour Mixpanel application.</span></span>
<span data-ttu-id="ff389-228">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ff389-228">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff389-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ff389-229">Additional resources</span></span>

* [<span data-ttu-id="ff389-230">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff389-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ff389-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ff389-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_203.png

