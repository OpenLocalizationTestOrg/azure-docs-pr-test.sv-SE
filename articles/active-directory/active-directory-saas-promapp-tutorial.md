---
title: "Självstudier: Azure Active Directory-integrering med Promapp | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Promapp."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: 02de7679b0c86d7aa8cacb41762f900dbf2ff231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a><span data-ttu-id="00b03-103">Självstudier: Azure Active Directory-integrering med Promapp</span><span class="sxs-lookup"><span data-stu-id="00b03-103">Tutorial: Azure Active Directory integration with Promapp</span></span>

<span data-ttu-id="00b03-104">I kursen får du lära dig hur toointegrate Promapp med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="00b03-104">In this tutorial, you learn how toointegrate Promapp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="00b03-105">Integrera Promapp med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="00b03-105">Integrating Promapp with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="00b03-106">Du kan styra i Azure AD som har åtkomst till tooPromapp</span><span class="sxs-lookup"><span data-stu-id="00b03-106">You can control in Azure AD who has access tooPromapp</span></span>
- <span data-ttu-id="00b03-107">Du kan aktivera din användare tooautomatically get inloggade tooPromapp (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="00b03-107">You can enable your users tooautomatically get signed-on tooPromapp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="00b03-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="00b03-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="00b03-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="00b03-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00b03-110">Krav</span><span class="sxs-lookup"><span data-stu-id="00b03-110">Prerequisites</span></span>

<span data-ttu-id="00b03-111">tooconfigure Azure AD-integrering med Promapp, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="00b03-111">tooconfigure Azure AD integration with Promapp, you need hello following items:</span></span>

- <span data-ttu-id="00b03-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="00b03-112">An Azure AD subscription</span></span>
- <span data-ttu-id="00b03-113">En Promapp enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="00b03-113">A Promapp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="00b03-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="00b03-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="00b03-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="00b03-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="00b03-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="00b03-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="00b03-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="00b03-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="00b03-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="00b03-118">Scenario description</span></span>
<span data-ttu-id="00b03-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="00b03-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="00b03-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="00b03-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="00b03-121">Att lägga till Promapp från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="00b03-121">Adding Promapp from hello gallery</span></span>
2. <span data-ttu-id="00b03-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00b03-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-promapp-from-hello-gallery"></a><span data-ttu-id="00b03-123">Att lägga till Promapp från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="00b03-123">Adding Promapp from hello gallery</span></span>
<span data-ttu-id="00b03-124">tooconfigure hello integrering av Promapp i Azure AD, behöver du tooadd Promapp hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="00b03-124">tooconfigure hello integration of Promapp into Azure AD, you need tooadd Promapp from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="00b03-125">**tooadd Promapp från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="00b03-125">**tooadd Promapp from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="00b03-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="00b03-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="00b03-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="00b03-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="00b03-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="00b03-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="00b03-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b03-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="00b03-133">Skriv i sökrutan hello **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="00b03-133">In hello search box, type **Promapp**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_search.png)

5. <span data-ttu-id="00b03-135">Markera hello resultat på panelen **Promapp**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="00b03-135">In hello results panel, select **Promapp**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="00b03-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00b03-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="00b03-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Promapp baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="00b03-138">In this section, you configure and test Azure AD single sign-on with Promapp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="00b03-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Promapp är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="00b03-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Promapp is tooa user in Azure AD.</span></span> <span data-ttu-id="00b03-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Promapp toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="00b03-140">In other words, a link relationship between an Azure AD user and hello related user in Promapp needs toobe established.</span></span>

<span data-ttu-id="00b03-141">I Promapp, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="00b03-141">In Promapp, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="00b03-142">tooconfigure och testa Azure AD enkel inloggning med Promapp, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="00b03-142">tooconfigure and test Azure AD single sign-on with Promapp, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="00b03-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="00b03-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="00b03-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="00b03-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="00b03-145">**[Skapa en testanvändare Promapp](#creating-a-promapp-test-user)**  -toohave en motsvarighet för Britta Simon i Promapp som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="00b03-145">**[Creating a Promapp test user](#creating-a-promapp-test-user)** - toohave a counterpart of Britta Simon in Promapp that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="00b03-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="00b03-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="00b03-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="00b03-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="00b03-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00b03-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="00b03-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Promapp program.</span><span class="sxs-lookup"><span data-stu-id="00b03-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Promapp application.</span></span>

<span data-ttu-id="00b03-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Promapp:**</span><span class="sxs-lookup"><span data-stu-id="00b03-150">**tooconfigure Azure AD single sign-on with Promapp, perform hello following steps:**</span></span>

1. <span data-ttu-id="00b03-151">I hello Azure-portalen på hello **Promapp** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="00b03-151">In hello Azure portal, on hello **Promapp** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="00b03-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="00b03-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_samlbase.png)

3. <span data-ttu-id="00b03-155">På hello **Promapp domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="00b03-155">On hello **Promapp Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_url.png)

    <span data-ttu-id="00b03-157">a.</span><span class="sxs-lookup"><span data-stu-id="00b03-157">a.</span></span> <span data-ttu-id="00b03-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span><span class="sxs-lookup"><span data-stu-id="00b03-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span></span>

    <span data-ttu-id="00b03-159">b.</span><span class="sxs-lookup"><span data-stu-id="00b03-159">b.</span></span> <span data-ttu-id="00b03-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://DOMAINNAME.promapp.com/TENANTNAME`</span><span class="sxs-lookup"><span data-stu-id="00b03-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="00b03-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="00b03-161">These values are not real.</span></span> <span data-ttu-id="00b03-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="00b03-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="00b03-163">Kontakta [Promapp klienten supportteamet](https://www.promapp.com/about-us/contact-us/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="00b03-163">Contact [Promapp Client support team](https://www.promapp.com/about-us/contact-us/) tooget these values.</span></span>

4. <span data-ttu-id="00b03-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="00b03-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_certificate.png) 

5. <span data-ttu-id="00b03-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="00b03-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-promapp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="00b03-168">På hello **Promapp Configuration** klickar du på **konfigurera Promapp** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="00b03-168">On hello **Promapp Configuration** section, click **Configure Promapp** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="00b03-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="00b03-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_configure.png) 

7. <span data-ttu-id="00b03-171">Inloggning tooyour Promapp företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="00b03-171">Sign-on tooyour Promapp company site as administrator.</span></span> 

8. <span data-ttu-id="00b03-172">Hello-menyn överst hello **Admin**.</span><span class="sxs-lookup"><span data-stu-id="00b03-172">In hello menu on hello top, click **Admin**.</span></span> 
   
    ![Azure AD-Single Sign-On][12]

9. <span data-ttu-id="00b03-174">Klicka på **Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="00b03-174">Click **Configure**.</span></span> 
   
    ![Azure AD-Single Sign-On][13]

10. <span data-ttu-id="00b03-176">På hello **säkerhet** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="00b03-176">On hello **Security** dialog, perform hello following steps:</span></span>
   
    ![Azure AD-Single Sign-On][14]
    
    <span data-ttu-id="00b03-178">a.</span><span class="sxs-lookup"><span data-stu-id="00b03-178">a.</span></span> <span data-ttu-id="00b03-179">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **SSO inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="00b03-179">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **SSO-Login URL** textbox.</span></span>
    
    <span data-ttu-id="00b03-180">b.</span><span class="sxs-lookup"><span data-stu-id="00b03-180">b.</span></span> <span data-ttu-id="00b03-181">Som **SSO - läget för enkel inloggning**väljer **valfritt**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="00b03-181">As **SSO - Single Sign-on Mode**, select **Optional**, and then click **Save**.</span></span>

    <span data-ttu-id="00b03-182">c.</span><span class="sxs-lookup"><span data-stu-id="00b03-182">c.</span></span> <span data-ttu-id="00b03-183">Öppna hello hämtat certifikat i anteckningar, kopiera hello certifikatinnehåll utan hello första raden (---BEGIN CERTIFICATE---) och hello sista raden (---slut certifikat---), klistrar du in den i hello **SSO x.509-certifikat** textruta och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="00b03-183">Open hello downloaded certificate in notepad, copy hello certificate content without hello first line (-----BEGIN CERTIFICATE-----) and hello last line (-----END CERTIFICATE-----), paste it into hello **SSO-x.509 Certificate** textbox, and then click **Save**.</span></span>
        
> [!TIP]
> <span data-ttu-id="00b03-184">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="00b03-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="00b03-185">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="00b03-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="00b03-186">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="00b03-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="00b03-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="00b03-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="00b03-188">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="00b03-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="00b03-190">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="00b03-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="00b03-191">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="00b03-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="00b03-193">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="00b03-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="00b03-195">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b03-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="00b03-197">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="00b03-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="00b03-199">a.</span><span class="sxs-lookup"><span data-stu-id="00b03-199">a.</span></span> <span data-ttu-id="00b03-200">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="00b03-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="00b03-201">b.</span><span class="sxs-lookup"><span data-stu-id="00b03-201">b.</span></span> <span data-ttu-id="00b03-202">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="00b03-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="00b03-203">c.</span><span class="sxs-lookup"><span data-stu-id="00b03-203">c.</span></span> <span data-ttu-id="00b03-204">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="00b03-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="00b03-205">d.</span><span class="sxs-lookup"><span data-stu-id="00b03-205">d.</span></span> <span data-ttu-id="00b03-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="00b03-206">Click **Create**.</span></span>
 
### <a name="creating-a-promapp-test-user"></a><span data-ttu-id="00b03-207">Skapa en testanvändare Promapp</span><span class="sxs-lookup"><span data-stu-id="00b03-207">Creating a Promapp test user</span></span>

<span data-ttu-id="00b03-208">Hej Promapp programmet stöder Just-in-Time-etablering.</span><span class="sxs-lookup"><span data-stu-id="00b03-208">hello Promapp application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="00b03-209">Det innebär ett konto skapas automatiskt om det behövs under ett försök tooaccess hello-program med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="00b03-209">This means, a user account is automatically created if necessary during an attempt tooaccess hello application using hello Access Panel.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="00b03-210">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="00b03-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="00b03-211">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPromapp.</span><span class="sxs-lookup"><span data-stu-id="00b03-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPromapp.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="00b03-213">**tooassign Britta Simon tooPromapp utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="00b03-213">**tooassign Britta Simon tooPromapp, perform hello following steps:**</span></span>

1. <span data-ttu-id="00b03-214">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="00b03-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="00b03-216">Välj i listan med program hello **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="00b03-216">In hello applications list, select **Promapp**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_app.png) 

3. <span data-ttu-id="00b03-218">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="00b03-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="00b03-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="00b03-220">Click **Add** button.</span></span> <span data-ttu-id="00b03-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b03-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="00b03-223">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="00b03-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="00b03-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b03-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="00b03-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00b03-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="00b03-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00b03-226">Testing single sign-on</span></span>

<span data-ttu-id="00b03-227">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="00b03-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="00b03-228">Du bör få automatiskt inloggade tooyour Promapp programmet när du klickar på hello Promapp panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="00b03-228">When you click hello Promapp tile in hello Access Panel, you should get automatically signed-on tooyour Promapp application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00b03-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="00b03-229">Additional resources</span></span>

* [<span data-ttu-id="00b03-230">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="00b03-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="00b03-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="00b03-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_07.png

[100]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_203.png

