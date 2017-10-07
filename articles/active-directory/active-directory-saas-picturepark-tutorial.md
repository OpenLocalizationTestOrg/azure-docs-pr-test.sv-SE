---
title: "Självstudier: Azure Active Directory-integrering med Picturepark | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Picturepark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 3d826d3f73aad2f0d123f8697c6caafad7bc926a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a><span data-ttu-id="a9617-103">Självstudier: Azure Active Directory-integrering med Picturepark</span><span class="sxs-lookup"><span data-stu-id="a9617-103">Tutorial: Azure Active Directory integration with Picturepark</span></span>

<span data-ttu-id="a9617-104">I kursen får du lära dig hur toointegrate Picturepark med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a9617-104">In this tutorial, you learn how toointegrate Picturepark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a9617-105">Integrera Picturepark med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a9617-105">Integrating Picturepark with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a9617-106">Du kan styra i Azure AD som har åtkomst till tooPicturepark</span><span class="sxs-lookup"><span data-stu-id="a9617-106">You can control in Azure AD who has access tooPicturepark</span></span>
- <span data-ttu-id="a9617-107">Du kan aktivera din användare tooautomatically get inloggade tooPicturepark (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a9617-107">You can enable your users tooautomatically get signed-on tooPicturepark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a9617-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a9617-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a9617-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a9617-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9617-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a9617-110">Prerequisites</span></span>

<span data-ttu-id="a9617-111">tooconfigure Azure AD-integrering med Picturepark, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a9617-111">tooconfigure Azure AD integration with Picturepark, you need hello following items:</span></span>

- <span data-ttu-id="a9617-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a9617-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a9617-113">En Picturepark enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a9617-113">A Picturepark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a9617-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a9617-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a9617-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a9617-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a9617-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a9617-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a9617-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a9617-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a9617-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a9617-118">Scenario description</span></span>
<span data-ttu-id="a9617-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a9617-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a9617-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a9617-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a9617-121">Att lägga till Picturepark från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a9617-121">Adding Picturepark from hello gallery</span></span>
2. <span data-ttu-id="a9617-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a9617-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-picturepark-from-hello-gallery"></a><span data-ttu-id="a9617-123">Att lägga till Picturepark från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a9617-123">Adding Picturepark from hello gallery</span></span>
<span data-ttu-id="a9617-124">tooconfigure hello integrering av Picturepark i Azure AD, behöver du tooadd Picturepark hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a9617-124">tooconfigure hello integration of Picturepark into Azure AD, you need tooadd Picturepark from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a9617-125">**tooadd Picturepark från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a9617-125">**tooadd Picturepark from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a9617-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a9617-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a9617-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a9617-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a9617-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a9617-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a9617-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9617-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a9617-133">Skriv i sökrutan hello **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="a9617-133">In hello search box, type **Picturepark**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_search.png)

5. <span data-ttu-id="a9617-135">Markera hello resultat på panelen **Picturepark**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a9617-135">In hello results panel, select **Picturepark**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a9617-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a9617-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a9617-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Picturepark baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a9617-138">In this section, you configure and test Azure AD single sign-on with Picturepark based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a9617-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Picturepark är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a9617-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Picturepark is tooa user in Azure AD.</span></span> <span data-ttu-id="a9617-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Picturepark toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a9617-140">In other words, a link relationship between an Azure AD user and hello related user in Picturepark needs toobe established.</span></span>

<span data-ttu-id="a9617-141">I Picturepark, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a9617-141">In Picturepark, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a9617-142">tooconfigure och testa Azure AD enkel inloggning med Picturepark, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a9617-142">tooconfigure and test Azure AD single sign-on with Picturepark, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a9617-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a9617-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a9617-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a9617-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a9617-145">**[Skapa en testanvändare Picturepark](#creating-a-picturepark-test-user)**  -toohave en motsvarighet för Britta Simon i Picturepark som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a9617-145">**[Creating a Picturepark test user](#creating-a-picturepark-test-user)** - toohave a counterpart of Britta Simon in Picturepark that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a9617-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a9617-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a9617-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a9617-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a9617-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a9617-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a9617-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Picturepark program.</span><span class="sxs-lookup"><span data-stu-id="a9617-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Picturepark application.</span></span>

<span data-ttu-id="a9617-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Picturepark:**</span><span class="sxs-lookup"><span data-stu-id="a9617-150">**tooconfigure Azure AD single sign-on with Picturepark, perform hello following steps:**</span></span>

1. <span data-ttu-id="a9617-151">I hello Azure-portalen på hello **Picturepark** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a9617-151">In hello Azure portal, on hello **Picturepark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a9617-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a9617-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. <span data-ttu-id="a9617-155">På hello **Picturepark domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a9617-155">On hello **Picturepark Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_url.png)

    <span data-ttu-id="a9617-157">a.</span><span class="sxs-lookup"><span data-stu-id="a9617-157">a.</span></span> <span data-ttu-id="a9617-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.picturepark.com`</span><span class="sxs-lookup"><span data-stu-id="a9617-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.picturepark.com`</span></span>

    <span data-ttu-id="a9617-159">b.</span><span class="sxs-lookup"><span data-stu-id="a9617-159">b.</span></span> <span data-ttu-id="a9617-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="a9617-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span> 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > <span data-ttu-id="a9617-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a9617-161">These values are not real.</span></span> <span data-ttu-id="a9617-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="a9617-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a9617-163">Kontakta [Picturepark klienten supportteamet](https://picturepark.com/about/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a9617-163">Contact [Picturepark Client support team](https://picturepark.com/about/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="a9617-164">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a9617-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. <span data-ttu-id="a9617-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a9617-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a9617-168">På hello **Picturepark Configuration** klickar du på **konfigurera Picturepark** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a9617-168">On hello **Picturepark Configuration** section, click **Configure Picturepark** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a9617-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a9617-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_configure.png) 

7. <span data-ttu-id="a9617-171">Logga in på webbplatsen Picturepark företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="a9617-171">In a different web browser window, log into your Picturepark company site as an administrator.</span></span>

8. <span data-ttu-id="a9617-172">Klicka i hello verktygsfältet hello längst upp **Administrationsverktyg**, och klicka sedan på **hanteringskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="a9617-172">In hello toolbar on hello top, click **Administrative tools**, and then click **Management Console**.</span></span>
   
    <span data-ttu-id="a9617-173">![Konsolen för hantering av](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Management Console")</span><span class="sxs-lookup"><span data-stu-id="a9617-173">![Management Console](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Management Console")</span></span>

9. <span data-ttu-id="a9617-174">Klicka på **autentisering**, och klicka sedan på **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="a9617-174">Click **Authentication**, and then click **Identity providers**.</span></span>
   
    <span data-ttu-id="a9617-175">![Autentisering](./media/active-directory-saas-picturepark-tutorial/ic795063.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="a9617-175">![Authentication](./media/active-directory-saas-picturepark-tutorial/ic795063.png "Authentication")</span></span>

10. <span data-ttu-id="a9617-176">I hello **identitet providerkonfigurationen** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a9617-176">In hello **Identity provider configuration** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="a9617-177">![Identitet providerkonfigurationen](./media/active-directory-saas-picturepark-tutorial/ic795064.png "identitet providerkonfigurationen")</span><span class="sxs-lookup"><span data-stu-id="a9617-177">![Identity provider configuration](./media/active-directory-saas-picturepark-tutorial/ic795064.png "Identity provider configuration")</span></span>
   
    <span data-ttu-id="a9617-178">a.</span><span class="sxs-lookup"><span data-stu-id="a9617-178">a.</span></span> <span data-ttu-id="a9617-179">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a9617-179">Click **Add**.</span></span>
  
    <span data-ttu-id="a9617-180">b.</span><span class="sxs-lookup"><span data-stu-id="a9617-180">b.</span></span> <span data-ttu-id="a9617-181">Ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a9617-181">Type a name for your configuration.</span></span>
   
    <span data-ttu-id="a9617-182">c.</span><span class="sxs-lookup"><span data-stu-id="a9617-182">c.</span></span> <span data-ttu-id="a9617-183">Välj **anges som standard**.</span><span class="sxs-lookup"><span data-stu-id="a9617-183">Select **Set as default**.</span></span>
   
    <span data-ttu-id="a9617-184">d.</span><span class="sxs-lookup"><span data-stu-id="a9617-184">d.</span></span> <span data-ttu-id="a9617-185">I **Issuer URI** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a9617-185">In **Issuer URI** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="a9617-186">e.</span><span class="sxs-lookup"><span data-stu-id="a9617-186">e.</span></span> <span data-ttu-id="a9617-187">I **betrodda utfärdare USB utskrifts** textruta klistra in hello värdet för **tumavtrycket** som du har kopierat från **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a9617-187">In **Trusted Issuer Thumb Print** textbox, paste hello value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span> 

11. <span data-ttu-id="a9617-188">Klicka på **JoinDefaultUsersGroup**.</span><span class="sxs-lookup"><span data-stu-id="a9617-188">Click **JoinDefaultUsersGroup**.</span></span>

12. <span data-ttu-id="a9617-189">tooset hello **Emailaddress** attribut i hello **anspråk** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="a9617-189">tooset hello **Emailaddress** attribute in hello **Claim** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` and click **Save**.</span></span>

      <span data-ttu-id="a9617-190">![Konfigurationen](./media/active-directory-saas-picturepark-tutorial/ic795065.png "konfiguration")</span><span class="sxs-lookup"><span data-stu-id="a9617-190">![Configuration](./media/active-directory-saas-picturepark-tutorial/ic795065.png "Configuration")</span></span>

> [!TIP]
> <span data-ttu-id="a9617-191">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a9617-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a9617-192">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a9617-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a9617-193">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a9617-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a9617-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a9617-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="a9617-195">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a9617-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a9617-197">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a9617-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a9617-198">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a9617-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a9617-200">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a9617-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a9617-202">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9617-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a9617-204">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a9617-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a9617-206">a.</span><span class="sxs-lookup"><span data-stu-id="a9617-206">a.</span></span> <span data-ttu-id="a9617-207">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a9617-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a9617-208">b.</span><span class="sxs-lookup"><span data-stu-id="a9617-208">b.</span></span> <span data-ttu-id="a9617-209">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a9617-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a9617-210">c.</span><span class="sxs-lookup"><span data-stu-id="a9617-210">c.</span></span> <span data-ttu-id="a9617-211">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a9617-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a9617-212">d.</span><span class="sxs-lookup"><span data-stu-id="a9617-212">d.</span></span> <span data-ttu-id="a9617-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a9617-213">Click **Create**.</span></span>
 
### <a name="creating-a-picturepark-test-user"></a><span data-ttu-id="a9617-214">Skapa en testanvändare Picturepark</span><span class="sxs-lookup"><span data-stu-id="a9617-214">Creating a Picturepark test user</span></span>

<span data-ttu-id="a9617-215">I ordning tooenable Azure AD-användare toolog i Picturepark, måste de etableras i Picturepark.</span><span class="sxs-lookup"><span data-stu-id="a9617-215">In order tooenable Azure AD users toolog into Picturepark, they must be provisioned into Picturepark.</span></span> <span data-ttu-id="a9617-216">Hello gäller Picturepark är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a9617-216">In hello case of Picturepark, provisioning is a manual task.</span></span>

<span data-ttu-id="a9617-217">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="a9617-217">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="a9617-218">Logga in tooyour **Picturepark** klient.</span><span class="sxs-lookup"><span data-stu-id="a9617-218">Log in tooyour **Picturepark** tenant.</span></span>

2. <span data-ttu-id="a9617-219">Klicka i hello verktygsfältet hello längst upp **Administrationsverktyg**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="a9617-219">In hello toolbar on hello top, click **Administrative tools**, and then click **Users**.</span></span>
   
    <span data-ttu-id="a9617-220">![Användare](./media/active-directory-saas-picturepark-tutorial/ic795067.png "användare")</span><span class="sxs-lookup"><span data-stu-id="a9617-220">![Users](./media/active-directory-saas-picturepark-tutorial/ic795067.png "Users")</span></span>

3. <span data-ttu-id="a9617-221">I hello **översikt över användare** klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="a9617-221">In hello **Users overview** tab, click **New**.</span></span>
   
    <span data-ttu-id="a9617-222">![Användarhantering](./media/active-directory-saas-picturepark-tutorial/ic795068.png "användarhantering")</span><span class="sxs-lookup"><span data-stu-id="a9617-222">![User management](./media/active-directory-saas-picturepark-tutorial/ic795068.png "User management")</span></span>

4. <span data-ttu-id="a9617-223">På hello **skapa användare** dialogrutan hello utför följande steg på en giltig Azure Active Directory-användare som du vill tooprovision:</span><span class="sxs-lookup"><span data-stu-id="a9617-223">On hello **Create User** dialog, perform hello following steps of a valid Azure Active Directory User you want tooprovision:</span></span>
   
    <span data-ttu-id="a9617-224">![Skapa användare](./media/active-directory-saas-picturepark-tutorial/ic795069.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="a9617-224">![Create User](./media/active-directory-saas-picturepark-tutorial/ic795069.png "Create User")</span></span>
   
    <span data-ttu-id="a9617-225">a.</span><span class="sxs-lookup"><span data-stu-id="a9617-225">a.</span></span> <span data-ttu-id="a9617-226">I hello **e-postadress** textruta typen hello **e-postadress** för hello användare  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="a9617-226">In hello **Email Address** textbox, type hello **email address** of hello user **BrittaSimon@contoso.com**.</span></span>  
   
    <span data-ttu-id="a9617-227">b.</span><span class="sxs-lookup"><span data-stu-id="a9617-227">b.</span></span> <span data-ttu-id="a9617-228">I hello **lösenord** och **Bekräfta lösenord** textrutor, typen hello **lösenord** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a9617-228">In hello **Password** and **Confirm Password** textboxes, type hello **password** of BrittaSimon.</span></span> 
   
    <span data-ttu-id="a9617-229">c.</span><span class="sxs-lookup"><span data-stu-id="a9617-229">c.</span></span> <span data-ttu-id="a9617-230">I hello **Förnamn** textruta typen hello **Förnamn** för hello användare **Britta**.</span><span class="sxs-lookup"><span data-stu-id="a9617-230">In hello **First Name** textbox, type hello **First Name** of hello user **Britta**.</span></span> 
   
    <span data-ttu-id="a9617-231">d.</span><span class="sxs-lookup"><span data-stu-id="a9617-231">d.</span></span> <span data-ttu-id="a9617-232">I hello **efternamn** textruta typen hello **efternamn** för hello användare **Simon**.</span><span class="sxs-lookup"><span data-stu-id="a9617-232">In hello **Last Name** textbox, type hello **Last Name** of hello user **Simon**.</span></span>
   
    <span data-ttu-id="a9617-233">e.</span><span class="sxs-lookup"><span data-stu-id="a9617-233">e.</span></span> <span data-ttu-id="a9617-234">I hello **företagets** textruta typen hello **företagsnamn** för hello användare.</span><span class="sxs-lookup"><span data-stu-id="a9617-234">In hello **Company** textbox, type hello **Company name** of hello user.</span></span> 
   
    <span data-ttu-id="a9617-235">f.</span><span class="sxs-lookup"><span data-stu-id="a9617-235">f.</span></span> <span data-ttu-id="a9617-236">I hello **land** textruta väljer hello **land** för hello användare.</span><span class="sxs-lookup"><span data-stu-id="a9617-236">In hello **Country** textbox, select hello **Country** of hello user.</span></span>
  
    <span data-ttu-id="a9617-237">g.</span><span class="sxs-lookup"><span data-stu-id="a9617-237">g.</span></span> <span data-ttu-id="a9617-238">I hello **ZIP** textruta typen hello **postnummer** på hello ort.</span><span class="sxs-lookup"><span data-stu-id="a9617-238">In hello **ZIP** textbox, type hello **ZIP code** of hello city.</span></span>
   
    <span data-ttu-id="a9617-239">h.</span><span class="sxs-lookup"><span data-stu-id="a9617-239">h.</span></span> <span data-ttu-id="a9617-240">I hello **Stad** textruta typen hello **Ortnamn** för hello användare.</span><span class="sxs-lookup"><span data-stu-id="a9617-240">In hello **City** textbox, type hello **City name** of hello user.</span></span>

    <span data-ttu-id="a9617-241">Jag.</span><span class="sxs-lookup"><span data-stu-id="a9617-241">i.</span></span> <span data-ttu-id="a9617-242">Välj en **språk**.</span><span class="sxs-lookup"><span data-stu-id="a9617-242">Select a **Language**.</span></span>
   
    <span data-ttu-id="a9617-243">j.</span><span class="sxs-lookup"><span data-stu-id="a9617-243">j.</span></span> <span data-ttu-id="a9617-244">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a9617-244">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="a9617-245">Du kan använda något annat Picturepark användarens konto skapas verktyg eller API: er som tillhandahålls av Picturepark tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a9617-245">You can use any other Picturepark user account creation tools or APIs provided by Picturepark tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a9617-246">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a9617-246">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a9617-247">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPicturepark.</span><span class="sxs-lookup"><span data-stu-id="a9617-247">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPicturepark.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a9617-249">**tooassign Britta Simon tooPicturepark utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a9617-249">**tooassign Britta Simon tooPicturepark, perform hello following steps:**</span></span>

1. <span data-ttu-id="a9617-250">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a9617-250">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a9617-252">Välj i listan med program hello **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="a9617-252">In hello applications list, select **Picturepark**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_app.png) 

3. <span data-ttu-id="a9617-254">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a9617-254">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a9617-256">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a9617-256">Click **Add** button.</span></span> <span data-ttu-id="a9617-257">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9617-257">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a9617-259">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a9617-259">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a9617-260">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9617-260">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a9617-261">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9617-261">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a9617-262">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a9617-262">Testing single sign-on</span></span>

<span data-ttu-id="a9617-263">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a9617-263">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a9617-264">Du bör få automatiskt inloggade tooyour Picturepark programmet när du klickar på hello Picturepark panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a9617-264">When you click hello Picturepark tile in hello Access Panel, you should get automatically signed-on tooyour Picturepark application.</span></span> <span data-ttu-id="a9617-265">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a9617-265">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9617-266">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a9617-266">Additional resources</span></span>

* [<span data-ttu-id="a9617-267">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a9617-267">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a9617-268">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a9617-268">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_203.png

