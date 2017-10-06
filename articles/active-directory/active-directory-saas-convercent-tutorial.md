---
title: "Självstudier: Azure Active Directory-integrering med Convercent | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Convercent."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9c9d290-0e13-490b-b559-0be772d6a690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: e6d09d7de52779dcf05e80215df9369ebc2ed06d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-convercent"></a><span data-ttu-id="e575e-103">Självstudier: Azure Active Directory-integrering med Convercent</span><span class="sxs-lookup"><span data-stu-id="e575e-103">Tutorial: Azure Active Directory integration with Convercent</span></span>

<span data-ttu-id="e575e-104">I kursen får du lära dig hur toointegrate Convercent med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e575e-104">In this tutorial, you learn how toointegrate Convercent with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e575e-105">Integrera Convercent med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e575e-105">Integrating Convercent with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e575e-106">Du kan styra i Azure AD som har åtkomst till tooConvercent</span><span class="sxs-lookup"><span data-stu-id="e575e-106">You can control in Azure AD who has access tooConvercent</span></span>
- <span data-ttu-id="e575e-107">Du kan aktivera din användare tooautomatically get inloggade tooConvercent (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e575e-107">You can enable your users tooautomatically get signed-on tooConvercent (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e575e-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e575e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e575e-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e575e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e575e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e575e-110">Prerequisites</span></span>

<span data-ttu-id="e575e-111">tooconfigure Azure AD-integrering med Convercent, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e575e-111">tooconfigure Azure AD integration with Convercent, you need hello following items:</span></span>

- <span data-ttu-id="e575e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e575e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e575e-113">En Convercent enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e575e-113">A Convercent single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e575e-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e575e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e575e-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e575e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e575e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e575e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e575e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e575e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e575e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e575e-118">Scenario description</span></span>
<span data-ttu-id="e575e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e575e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e575e-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e575e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e575e-121">Att lägga till Convercent från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e575e-121">Adding Convercent from hello gallery</span></span>
2. <span data-ttu-id="e575e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e575e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-convercent-from-hello-gallery"></a><span data-ttu-id="e575e-123">Att lägga till Convercent från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e575e-123">Adding Convercent from hello gallery</span></span>
<span data-ttu-id="e575e-124">tooconfigure hello integrering av Convercent i Azure AD, behöver du tooadd Convercent hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e575e-124">tooconfigure hello integration of Convercent into Azure AD, you need tooadd Convercent from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e575e-125">**tooadd Convercent från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e575e-125">**tooadd Convercent from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e575e-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e575e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e575e-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e575e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e575e-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e575e-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e575e-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e575e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e575e-133">Skriv i sökrutan hello **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="e575e-133">In hello search box, type **Convercent**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_search.png)

5. <span data-ttu-id="e575e-135">Markera hello resultat på panelen **Convercent**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e575e-135">In hello results panel, select **Convercent**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e575e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e575e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e575e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Convercent baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e575e-138">In this section, you configure and test Azure AD single sign-on with Convercent based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e575e-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Convercent är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e575e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Convercent is tooa user in Azure AD.</span></span> <span data-ttu-id="e575e-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Convercent toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e575e-140">In other words, a link relationship between an Azure AD user and hello related user in Convercent needs toobe established.</span></span>

<span data-ttu-id="e575e-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Convercent.</span><span class="sxs-lookup"><span data-stu-id="e575e-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Convercent.</span></span>

<span data-ttu-id="e575e-142">tooconfigure och testa Azure AD enkel inloggning med Convercent, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e575e-142">tooconfigure and test Azure AD single sign-on with Convercent, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e575e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e575e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e575e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e575e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e575e-145">**[Skapa en testanvändare Convercent](#creating-a-convercent-test-user)**  -toohave en motsvarighet för Britta Simon i Convercent som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e575e-145">**[Creating a Convercent test user](#creating-a-convercent-test-user)** - toohave a counterpart of Britta Simon in Convercent that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e575e-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e575e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e575e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e575e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e575e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e575e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e575e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Convercent program.</span><span class="sxs-lookup"><span data-stu-id="e575e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Convercent application.</span></span>

<span data-ttu-id="e575e-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Convercent:**</span><span class="sxs-lookup"><span data-stu-id="e575e-150">**tooconfigure Azure AD single sign-on with Convercent, perform hello following steps:**</span></span>

1. <span data-ttu-id="e575e-151">I hello Azure-portalen på hello **Convercent** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e575e-151">In hello Azure portal, on hello **Convercent** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e575e-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e575e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_samlbase.png)

3. <span data-ttu-id="e575e-155">På hello **Convercent domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e575e-155">On hello **Convercent Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url.png)

    <span data-ttu-id="e575e-157">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="e575e-157">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.convercent.com/`</span></span>
 
4. <span data-ttu-id="e575e-158">Om du inte vill tooconfigure hello programmet i **SP initierade läge**, på hello **Convercent domän och URL: er** avsnittet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e575e-158">If you wish tooconfigure hello application in **SP initiated mode**, on hello **Convercent Domain and URLs** section perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url1.png)

     <span data-ttu-id="e575e-160">a.</span><span class="sxs-lookup"><span data-stu-id="e575e-160">a.</span></span> <span data-ttu-id="e575e-161">Klicka på **”visa avancerade inställningar för URL”.**</span><span class="sxs-lookup"><span data-stu-id="e575e-161">Click **"Show advanced URL settings."**</span></span> 

     <span data-ttu-id="e575e-162">b.</span><span class="sxs-lookup"><span data-stu-id="e575e-162">b.</span></span> <span data-ttu-id="e575e-163">I hello **logga URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="e575e-163">In hello **Sign On URL** textbox, type hello value using hello following pattern: `https://<instancename>.convercent.com/`</span></span>

     <span data-ttu-id="e575e-164">c.</span><span class="sxs-lookup"><span data-stu-id="e575e-164">c.</span></span> <span data-ttu-id="e575e-165">I hello **Relay tillstånd** textruta hello TYPVÄRDE med hello följande mönster:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="e575e-165">In hello **Relay State** textbox, type hello value using hello following pattern: `https://<instancename>.convercent.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e575e-166">Dessa värden är inte hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="e575e-166">These values are not hello real values.</span></span> <span data-ttu-id="e575e-167">Uppdatera dessa värden med hello faktiska identifierare, logga URL och Relay tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e575e-167">Update these values with hello actual Identifier, Sign On URL and Relay State.</span></span> <span data-ttu-id="e575e-168">Kontakta [Convercent klienten supportteamet](http://support.convercent.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e575e-168">Contact [Convercent Client support team](http://support.convercent.com) tooget these values.</span></span>

5. <span data-ttu-id="e575e-169">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e575e-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_certificate.png) 

6. <span data-ttu-id="e575e-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e575e-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="e575e-173">tooget SSO konfigurerats för ditt program bör du kontakta [Convercent supportteamet](mailto:support@convercent.com) och ge dem hello hämtas **XML-Metadata för**.</span><span class="sxs-lookup"><span data-stu-id="e575e-173">tooget SSO configured for your application, contact [Convercent support team](mailto:support@convercent.com) and provide them with hello downloaded **Metadata XML**.</span></span>

> [!TIP]
> <span data-ttu-id="e575e-174">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="e575e-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e575e-175">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="e575e-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e575e-176">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e575e-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e575e-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e575e-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="e575e-178">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e575e-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e575e-180">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e575e-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e575e-181">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e575e-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e575e-183">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e575e-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e575e-185">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e575e-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e575e-187">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e575e-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e575e-189">a.</span><span class="sxs-lookup"><span data-stu-id="e575e-189">a.</span></span> <span data-ttu-id="e575e-190">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e575e-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e575e-191">b.</span><span class="sxs-lookup"><span data-stu-id="e575e-191">b.</span></span> <span data-ttu-id="e575e-192">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e575e-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e575e-193">c.</span><span class="sxs-lookup"><span data-stu-id="e575e-193">c.</span></span> <span data-ttu-id="e575e-194">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e575e-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e575e-195">d.</span><span class="sxs-lookup"><span data-stu-id="e575e-195">d.</span></span> <span data-ttu-id="e575e-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e575e-196">Click **Create**.</span></span>
 
### <a name="creating-a-convercent-test-user"></a><span data-ttu-id="e575e-197">Skapa en testanvändare Convercent</span><span class="sxs-lookup"><span data-stu-id="e575e-197">Creating a Convercent test user</span></span>

<span data-ttu-id="e575e-198">Arbeta med [Convercent supportteamet](mailto:support@convercent.com) tooadd hello användare i hello Convercent plattform.</span><span class="sxs-lookup"><span data-stu-id="e575e-198">Work with [Convercent support team](mailto:support@convercent.com) tooadd hello users in hello Convercent platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e575e-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e575e-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e575e-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooConvercent.</span><span class="sxs-lookup"><span data-stu-id="e575e-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooConvercent.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e575e-202">**tooassign Britta Simon tooConvercent utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e575e-202">**tooassign Britta Simon tooConvercent, perform hello following steps:**</span></span>

1. <span data-ttu-id="e575e-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e575e-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e575e-205">Välj i listan med program hello **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="e575e-205">In hello applications list, select **Convercent**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_app.png) 

3. <span data-ttu-id="e575e-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e575e-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e575e-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e575e-209">Click **Add** button.</span></span> <span data-ttu-id="e575e-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e575e-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e575e-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e575e-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e575e-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e575e-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e575e-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e575e-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e575e-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e575e-215">Testing single sign-on</span></span>

<span data-ttu-id="e575e-216">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e575e-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e575e-217">Du bör få automatiskt inloggade tooyour Convercent programmet när du klickar på hello Convercent panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e575e-217">When you click hello Convercent tile in hello Access Panel, you should get automatically signed-on tooyour Convercent application.</span></span>
<span data-ttu-id="e575e-218">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e575e-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e575e-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e575e-219">Additional resources</span></span>

* [<span data-ttu-id="e575e-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e575e-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e575e-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e575e-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_203.png

