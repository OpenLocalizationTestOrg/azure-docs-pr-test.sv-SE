---
title: "Självstudier: Azure Active Directory-integrering med 123ContactForm | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och 123ContactForm."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 931255887845edd1aa7f53b9051a82a2f898e055
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a><span data-ttu-id="b6583-103">Självstudier: Azure Active Directory-integrering med 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="b6583-103">Tutorial: Azure Active Directory integration with 123ContactForm</span></span>

<span data-ttu-id="b6583-104">I kursen får du lära dig hur toointegrate 123ContactForm med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b6583-104">In this tutorial, you learn how toointegrate 123ContactForm with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b6583-105">Integrera 123ContactForm med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b6583-105">Integrating 123ContactForm with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b6583-106">Du kan styra i Azure AD som har åtkomst till too123ContactForm</span><span class="sxs-lookup"><span data-stu-id="b6583-106">You can control in Azure AD who has access too123ContactForm</span></span>
- <span data-ttu-id="b6583-107">Du kan aktivera din användare tooautomatically get inloggade too123ContactForm (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b6583-107">You can enable your users tooautomatically get signed-on too123ContactForm (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b6583-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b6583-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b6583-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b6583-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6583-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b6583-110">Prerequisites</span></span>

<span data-ttu-id="b6583-111">tooconfigure Azure AD-integrering med 123ContactForm, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b6583-111">tooconfigure Azure AD integration with 123ContactForm, you need hello following items:</span></span>

- <span data-ttu-id="b6583-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b6583-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b6583-113">En 123ContactForm enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b6583-113">A 123ContactForm single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b6583-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b6583-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b6583-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b6583-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b6583-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b6583-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b6583-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b6583-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b6583-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b6583-118">Scenario description</span></span>
<span data-ttu-id="b6583-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b6583-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b6583-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b6583-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b6583-121">Att lägga till 123ContactForm från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b6583-121">Adding 123ContactForm from hello gallery</span></span>
2. <span data-ttu-id="b6583-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b6583-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-123contactform-from-hello-gallery"></a><span data-ttu-id="b6583-123">Att lägga till 123ContactForm från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b6583-123">Adding 123ContactForm from hello gallery</span></span>
<span data-ttu-id="b6583-124">tooconfigure hello integrering av 123ContactForm i Azure AD, behöver du tooadd 123ContactForm hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b6583-124">tooconfigure hello integration of 123ContactForm into Azure AD, you need tooadd 123ContactForm from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b6583-125">**tooadd 123ContactForm från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b6583-125">**tooadd 123ContactForm from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b6583-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b6583-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b6583-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b6583-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b6583-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b6583-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b6583-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6583-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b6583-133">Skriv i sökrutan hello **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="b6583-133">In hello search box, type **123ContactForm**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_search.png)

5. <span data-ttu-id="b6583-135">Markera hello resultat på panelen **123ContactForm**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b6583-135">In hello results panel, select **123ContactForm**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b6583-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b6583-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b6583-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 123ContactForm baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b6583-138">In this section, you configure and test Azure AD single sign-on with 123ContactForm based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b6583-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i 123ContactForm är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6583-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 123ContactForm is tooa user in Azure AD.</span></span> <span data-ttu-id="b6583-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i 123ContactForm toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b6583-140">In other words, a link relationship between an Azure AD user and hello related user in 123ContactForm needs toobe established.</span></span>

<span data-ttu-id="b6583-141">I 123ContactForm, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b6583-141">In 123ContactForm, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b6583-142">tooconfigure och testa Azure AD enkel inloggning med 123ContactForm, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b6583-142">tooconfigure and test Azure AD single sign-on with 123ContactForm, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b6583-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b6583-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b6583-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b6583-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b6583-145">**[Skapa en testanvändare 123ContactForm](#creating-a-123contactform-test-user)**  -toohave en motsvarighet för Britta Simon i 123ContactForm som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b6583-145">**[Creating a 123ContactForm test user](#creating-a-123contactform-test-user)** - toohave a counterpart of Britta Simon in 123ContactForm that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b6583-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b6583-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b6583-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b6583-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b6583-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b6583-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b6583-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt 123ContactForm program.</span><span class="sxs-lookup"><span data-stu-id="b6583-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 123ContactForm application.</span></span>

<span data-ttu-id="b6583-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med 123ContactForm:**</span><span class="sxs-lookup"><span data-stu-id="b6583-150">**tooconfigure Azure AD single sign-on with 123ContactForm, perform hello following steps:**</span></span>

1. <span data-ttu-id="b6583-151">I hello Azure-portalen på hello **123ContactForm** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b6583-151">In hello Azure portal, on hello **123ContactForm** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b6583-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b6583-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. <span data-ttu-id="b6583-155">På hello **123ContactForm domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b6583-155">On hello **123ContactForm Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/url1.png)

    <span data-ttu-id="b6583-157">a.</span><span class="sxs-lookup"><span data-stu-id="b6583-157">a.</span></span> <span data-ttu-id="b6583-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span><span class="sxs-lookup"><span data-stu-id="b6583-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span></span>

    <span data-ttu-id="b6583-159">b.</span><span class="sxs-lookup"><span data-stu-id="b6583-159">b.</span></span> <span data-ttu-id="b6583-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span><span class="sxs-lookup"><span data-stu-id="b6583-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span></span>

4. <span data-ttu-id="b6583-161">Om du inte vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b6583-161">If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/url2.png)

    <span data-ttu-id="b6583-163">a.</span><span class="sxs-lookup"><span data-stu-id="b6583-163">a.</span></span> <span data-ttu-id="b6583-164">Klicka på hello **visa avancerade inställningar för URL: en** alternativet</span><span class="sxs-lookup"><span data-stu-id="b6583-164">Click hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="b6583-165">b.</span><span class="sxs-lookup"><span data-stu-id="b6583-165">b.</span></span> <span data-ttu-id="b6583-166">I hello **logga URL** textruta Skriv en URL som:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span><span class="sxs-lookup"><span data-stu-id="b6583-166">In hello **Sign On URL** textbox, type a URL as: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b6583-167">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b6583-167">These values are not real.</span></span> <span data-ttu-id="b6583-168">Du behöver tooupdate dessa värde från faktiska URL: er och identifierare som beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="b6583-168">You'll need tooupdate these value from actual URLs and Identifier which is explained later in hello tutorial.</span></span>
    
5. <span data-ttu-id="b6583-169">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="b6583-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. <span data-ttu-id="b6583-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b6583-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="b6583-173">tooconfigure enkel inloggning på **123ContactForm** sida, gå för[https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b6583-173">tooconfigure single sign-on on **123ContactForm** side, go too[https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) and perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/submit.png) 

    <span data-ttu-id="b6583-175">a.</span><span class="sxs-lookup"><span data-stu-id="b6583-175">a.</span></span> <span data-ttu-id="b6583-176">I hello **e-post** textruta hello e-post för hello användaren engångsfaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="b6583-176">In hello **Email** textbox, type hello email of hello user i.e</span></span> <span data-ttu-id="b6583-177">**BrittaSimon@Contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="b6583-177">**BrittaSimon@Contoso.com**.</span></span>

    <span data-ttu-id="b6583-178">b.</span><span class="sxs-lookup"><span data-stu-id="b6583-178">b.</span></span> <span data-ttu-id="b6583-179">Klicka på **överför** och bläddra hello Metadata XML-fil som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b6583-179">Click **Upload** and browse hello Metadata XML file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="b6583-180">c.</span><span class="sxs-lookup"><span data-stu-id="b6583-180">c.</span></span> <span data-ttu-id="b6583-181">Klicka på **skicka formuläret**.</span><span class="sxs-lookup"><span data-stu-id="b6583-181">Click **SUBMIT FORM**.</span></span>

8. <span data-ttu-id="b6583-182">På hello **konfigurera Appinställningar i Microsoft Azure AD enkel inloggning -** utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b6583-182">On hello **Microsoft Azure AD - Single sign-on - Configure App Settings** perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/url3.png)

    <span data-ttu-id="b6583-184">a.</span><span class="sxs-lookup"><span data-stu-id="b6583-184">a.</span></span> <span data-ttu-id="b6583-185">Om du inte vill tooconfigure hello programmet i **IDP initierade läge**, kopiera hello **IDENTIFIERARE** värde för din instans och klistra in den i **identifierare** TextBox-kontroll i **123ContactForm domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b6583-185">If you wish tooconfigure hello application in **IDP initiated mode**, copy hello **IDENTIFIER** value for your instance and paste it in **Identifier** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="b6583-186">b.</span><span class="sxs-lookup"><span data-stu-id="b6583-186">b.</span></span> <span data-ttu-id="b6583-187">Om du inte vill tooconfigure hello programmet i **IDP initierade läge**, kopiera hello **REPLY URL** värde för din instans och klistra in den i **Reply URL** TextBox-kontroll i **123ContactForm domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b6583-187">If you wish tooconfigure hello application in **IDP initiated mode**, copy hello **REPLY URL** value for your instance and paste it in **Reply URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="b6583-188">c.</span><span class="sxs-lookup"><span data-stu-id="b6583-188">c.</span></span> <span data-ttu-id="b6583-189">Om du inte vill tooconfigure hello programmet i **SP initierade läge**, kopiera hello **SIGN ON URL** värde för din instans och klistra in den i **logga URL** TextBox-kontroll i **123ContactForm domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b6583-189">If you wish tooconfigure hello application in **SP initiated mode**, copy hello **SIGN ON URL** value for your instance and paste it in **Sign On URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="b6583-190">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="b6583-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b6583-191">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b6583-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b6583-192">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b6583-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b6583-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b6583-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="b6583-194">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b6583-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b6583-196">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b6583-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b6583-197">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b6583-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b6583-199">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b6583-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b6583-201">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6583-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b6583-203">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b6583-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b6583-205">a.</span><span class="sxs-lookup"><span data-stu-id="b6583-205">a.</span></span> <span data-ttu-id="b6583-206">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b6583-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b6583-207">b.</span><span class="sxs-lookup"><span data-stu-id="b6583-207">b.</span></span> <span data-ttu-id="b6583-208">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b6583-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b6583-209">c.</span><span class="sxs-lookup"><span data-stu-id="b6583-209">c.</span></span> <span data-ttu-id="b6583-210">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b6583-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b6583-211">d.</span><span class="sxs-lookup"><span data-stu-id="b6583-211">d.</span></span> <span data-ttu-id="b6583-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b6583-212">Click **Create**.</span></span>
 
### <a name="creating-a-123contactform-test-user"></a><span data-ttu-id="b6583-213">Skapa en testanvändare 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="b6583-213">Creating a 123ContactForm test user</span></span>

<span data-ttu-id="b6583-214">Programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b6583-214">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b6583-215">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b6583-215">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b6583-216">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst too123ContactForm.</span><span class="sxs-lookup"><span data-stu-id="b6583-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too123ContactForm.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b6583-218">**tooassign Britta Simon too123ContactForm utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b6583-218">**tooassign Britta Simon too123ContactForm, perform hello following steps:**</span></span>

1. <span data-ttu-id="b6583-219">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b6583-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b6583-221">Välj i listan med program hello **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="b6583-221">In hello applications list, select **123ContactForm**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_app.png) 

3. <span data-ttu-id="b6583-223">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b6583-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b6583-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b6583-225">Click **Add** button.</span></span> <span data-ttu-id="b6583-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6583-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b6583-228">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b6583-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b6583-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6583-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b6583-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6583-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b6583-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b6583-231">Testing single sign-on</span></span>

<span data-ttu-id="b6583-232">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b6583-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b6583-233">Du bör få automatiskt inloggade tooyour 123ContactForm programmet när du klickar på hello 123ContactForm panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b6583-233">When you click hello 123ContactForm tile in hello Access Panel, you should get automatically signed-on tooyour 123ContactForm application.</span></span>
<span data-ttu-id="b6583-234">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b6583-234">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6583-235">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b6583-235">Additional resources</span></span>

* [<span data-ttu-id="b6583-236">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b6583-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b6583-237">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b6583-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_203.png

