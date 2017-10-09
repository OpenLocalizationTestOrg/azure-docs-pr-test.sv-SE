---
title: "Självstudier: Azure Active Directory-integrering med Certifikatutfärdare PPM | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Certifikatutfärdaren PPM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca9d5e71-e429-4891-8d10-3498e7210e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 571130f3be0529c986aa0d8a08e4172015cd0b40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a><span data-ttu-id="4e56f-103">Självstudier: Azure Active Directory-integrering med Certifikatutfärdare PPM</span><span class="sxs-lookup"><span data-stu-id="4e56f-103">Tutorial: Azure Active Directory integration with CA PPM</span></span>

<span data-ttu-id="4e56f-104">I kursen får du lära dig hur toointegrate CA PPM med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4e56f-104">In this tutorial, you learn how toointegrate CA PPM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4e56f-105">Integrera CA PPM med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4e56f-105">Integrating CA PPM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4e56f-106">Du kan styra i Azure AD som har åtkomst tooCA PPM</span><span class="sxs-lookup"><span data-stu-id="4e56f-106">You can control in Azure AD who has access tooCA PPM</span></span>
- <span data-ttu-id="4e56f-107">Du kan aktivera din användare tooautomatically get inloggade tooCA PPM (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4e56f-107">You can enable your users tooautomatically get signed-on tooCA PPM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4e56f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4e56f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4e56f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4e56f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e56f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4e56f-110">Prerequisites</span></span>

<span data-ttu-id="4e56f-111">tooconfigure Azure AD-integrering med Certifikatutfärdare PPM måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4e56f-111">tooconfigure Azure AD integration with CA PPM, you need hello following items:</span></span>

- <span data-ttu-id="4e56f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4e56f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4e56f-113">En CA PPM enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="4e56f-113">A CA PPM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4e56f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4e56f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4e56f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4e56f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4e56f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4e56f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4e56f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4e56f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4e56f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4e56f-118">Scenario description</span></span>
<span data-ttu-id="4e56f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4e56f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4e56f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4e56f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4e56f-121">Att lägga till CA PPM från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4e56f-121">Adding CA PPM from hello gallery</span></span>
2. <span data-ttu-id="4e56f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e56f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ca-ppm-from-hello-gallery"></a><span data-ttu-id="4e56f-123">Att lägga till CA PPM från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4e56f-123">Adding CA PPM from hello gallery</span></span>
<span data-ttu-id="4e56f-124">tooconfigure hello integrering av Certifikatutfärdaren PPM i Azure AD, behöver du tooadd CA PPM hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4e56f-124">tooconfigure hello integration of CA PPM into Azure AD, you need tooadd CA PPM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4e56f-125">**tooadd CA PPM från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e56f-125">**tooadd CA PPM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e56f-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4e56f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4e56f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4e56f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4e56f-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e56f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4e56f-133">Skriv i sökrutan hello **CA PPM**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-133">In hello search box, type **CA PPM**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_search.png)

5. <span data-ttu-id="4e56f-135">Markera hello resultat på panelen **CA PPM**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4e56f-135">In hello results panel, select **CA PPM**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4e56f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e56f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4e56f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med CA PPM baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4e56f-138">In this section, you configure and test Azure AD single sign-on with CA PPM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4e56f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Certifikatutfärdaren PPM är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4e56f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CA PPM is tooa user in Azure AD.</span></span> <span data-ttu-id="4e56f-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Certifikatutfärdaren PPM toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="4e56f-140">In other words, a link relationship between an Azure AD user and hello related user in CA PPM needs toobe established.</span></span>

<span data-ttu-id="4e56f-141">Tilldela hello värdet för hello i Certifikatutfärdaren PPM **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4e56f-141">In CA PPM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4e56f-142">tooconfigure och testa Azure AD enkel inloggning med CA PPM, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4e56f-142">tooconfigure and test Azure AD single sign-on with CA PPM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4e56f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4e56f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4e56f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e56f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4e56f-145">**[Skapa en testanvändare CA PPM](#creating-a-ca-ppm-test-user)**  -toohave en motsvarighet för Britta Simon i CA-PPM som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4e56f-145">**[Creating a CA PPM test user](#creating-a-ca-ppm-test-user)** - toohave a counterpart of Britta Simon in CA PPM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4e56f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4e56f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4e56f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4e56f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4e56f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e56f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4e56f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt CA PPM-program.</span><span class="sxs-lookup"><span data-stu-id="4e56f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CA PPM application.</span></span>

<span data-ttu-id="4e56f-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med CA PPM:**</span><span class="sxs-lookup"><span data-stu-id="4e56f-150">**tooconfigure Azure AD single sign-on with CA PPM, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e56f-151">I hello Azure-portalen på hello **CA PPM** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-151">In hello Azure portal, on hello **CA PPM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4e56f-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4e56f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_samlbase.png)

3. <span data-ttu-id="4e56f-155">På hello **URL: er och CA PPM domän** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4e56f-155">On hello **CA PPM Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_url.png)

    <span data-ttu-id="4e56f-157">a.</span><span class="sxs-lookup"><span data-stu-id="4e56f-157">a.</span></span> <span data-ttu-id="4e56f-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://ca.ondemand.saml.20.post.<companyname>`</span><span class="sxs-lookup"><span data-stu-id="4e56f-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://ca.ondemand.saml.20.post.<companyname>`</span></span>
    
    <span data-ttu-id="4e56f-159">b.</span><span class="sxs-lookup"><span data-stu-id="4e56f-159">b.</span></span> <span data-ttu-id="4e56f-160">I hello **Reply URL** textruta typ som:`https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="4e56f-160">In hello **Reply URL** textbox, type as: `https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4e56f-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4e56f-161">This value is not real.</span></span> <span data-ttu-id="4e56f-162">Uppdatera det här värdet med hello faktiska identifierare.</span><span class="sxs-lookup"><span data-stu-id="4e56f-162">Update this value with hello actual Identifier.</span></span> <span data-ttu-id="4e56f-163">Kontakta [CA PPM supportteamet](mailto:catechnicalsupport@ca.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="4e56f-163">Contact [CA PPM support team](mailto:catechnicalsupport@ca.com) tooget this value.</span></span>
 
4. <span data-ttu-id="4e56f-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="4e56f-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_certificate.png) 

5. <span data-ttu-id="4e56f-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4e56f-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4e56f-168">På hello **PPM certifikatutfärdarkonfiguration** klickar du på **konfigurera Certifikatutfärdaren PPM** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4e56f-168">On hello **CA PPM Configuration** section, click **Configure CA PPM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4e56f-169">Kopiera hello **SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4e56f-169">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_configure.png) 

7. <span data-ttu-id="4e56f-171">tooconfigure enkel inloggning på **CA PPM** sida, behöver du toosend hello hämtas **Certificate(Base64)** och **SAML enhets-ID** för[CA PPM-teamet ](mailto:catechnicalsupport@ca.com).</span><span class="sxs-lookup"><span data-stu-id="4e56f-171">tooconfigure single sign-on on **CA PPM** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Entity ID** too[CA PPM support team](mailto:catechnicalsupport@ca.com).</span></span>

> [!TIP]
> <span data-ttu-id="4e56f-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="4e56f-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4e56f-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="4e56f-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4e56f-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4e56f-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4e56f-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e56f-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="4e56f-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e56f-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4e56f-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e56f-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e56f-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4e56f-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4e56f-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4e56f-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e56f-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4e56f-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4e56f-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4e56f-187">a.</span><span class="sxs-lookup"><span data-stu-id="4e56f-187">a.</span></span> <span data-ttu-id="4e56f-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4e56f-189">b.</span><span class="sxs-lookup"><span data-stu-id="4e56f-189">b.</span></span> <span data-ttu-id="4e56f-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4e56f-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4e56f-191">c.</span><span class="sxs-lookup"><span data-stu-id="4e56f-191">c.</span></span> <span data-ttu-id="4e56f-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4e56f-193">d.</span><span class="sxs-lookup"><span data-stu-id="4e56f-193">d.</span></span> <span data-ttu-id="4e56f-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-194">Click **Create**.</span></span>
 
### <a name="creating-a-ca-ppm-test-user"></a><span data-ttu-id="4e56f-195">Skapa en CA PPM testanvändare</span><span class="sxs-lookup"><span data-stu-id="4e56f-195">Creating a CA PPM test user</span></span>

<span data-ttu-id="4e56f-196">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Certifikatutfärdaren PPM.</span><span class="sxs-lookup"><span data-stu-id="4e56f-196">In this section, you create a user called Britta Simon in CA PPM.</span></span> <span data-ttu-id="4e56f-197">Arbeta med [CA PPM supportteamet](mailto:catechnicalsupport@ca.com) tooadd hello användare i hello CA PPM-plattformen.</span><span class="sxs-lookup"><span data-stu-id="4e56f-197">Work with [CA PPM support team](mailto:catechnicalsupport@ca.com) tooadd hello users in hello CA PPM platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4e56f-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e56f-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4e56f-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCA PPM.</span><span class="sxs-lookup"><span data-stu-id="4e56f-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCA PPM.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4e56f-201">**tooassign Britta Simon tooCA PPM, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="4e56f-201">**tooassign Britta Simon tooCA PPM, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e56f-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4e56f-204">Välj i listan med program hello **CA PPM**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-204">In hello applications list, select **CA PPM**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_app.png) 

3. <span data-ttu-id="4e56f-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4e56f-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4e56f-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4e56f-208">Click **Add** button.</span></span> <span data-ttu-id="4e56f-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e56f-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4e56f-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4e56f-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4e56f-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e56f-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4e56f-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e56f-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4e56f-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e56f-214">Testing single sign-on</span></span>

<span data-ttu-id="4e56f-215">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4e56f-215">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="4e56f-216">Du bör få automatiskt inloggade tooyour CA PPM programmet när du klickar på hello CA PPM-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4e56f-216">When you click hello CA PPM tile in hello Access Panel, you should get automatically signed-on tooyour CA PPM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e56f-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4e56f-217">Additional resources</span></span>

* [<span data-ttu-id="4e56f-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4e56f-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4e56f-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4e56f-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_203.png

