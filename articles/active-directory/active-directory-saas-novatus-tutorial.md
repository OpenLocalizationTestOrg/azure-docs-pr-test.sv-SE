---
title: "Självstudier: Azure Active Directory-integrering med Novatus | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Novatus."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2f13779-bdb7-4408-9738-be67ed3de4e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: jeedes
ms.openlocfilehash: 7ff13f56f0f47d0c2667c9ca555801a7a06a2fa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-novatus"></a><span data-ttu-id="006ad-103">Självstudier: Azure Active Directory-integrering med Novatus</span><span class="sxs-lookup"><span data-stu-id="006ad-103">Tutorial: Azure Active Directory integration with Novatus</span></span>

<span data-ttu-id="006ad-104">I kursen får du lära dig hur toointegrate Novatus med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="006ad-104">In this tutorial, you learn how toointegrate Novatus with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="006ad-105">Integrera Novatus med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="006ad-105">Integrating Novatus with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="006ad-106">Du kan styra i Azure AD som har åtkomst till tooNovatus</span><span class="sxs-lookup"><span data-stu-id="006ad-106">You can control in Azure AD who has access tooNovatus</span></span>
- <span data-ttu-id="006ad-107">Du kan aktivera din användare tooautomatically get inloggade tooNovatus (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="006ad-107">You can enable your users tooautomatically get signed-on tooNovatus (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="006ad-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="006ad-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="006ad-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="006ad-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="006ad-110">Krav</span><span class="sxs-lookup"><span data-stu-id="006ad-110">Prerequisites</span></span>

<span data-ttu-id="006ad-111">tooconfigure Azure AD-integrering med Novatus, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="006ad-111">tooconfigure Azure AD integration with Novatus, you need hello following items:</span></span>

- <span data-ttu-id="006ad-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="006ad-112">An Azure AD subscription</span></span>
- <span data-ttu-id="006ad-113">En Novatus enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="006ad-113">A Novatus single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="006ad-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="006ad-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="006ad-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="006ad-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="006ad-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="006ad-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="006ad-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="006ad-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="006ad-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="006ad-118">Scenario description</span></span>
<span data-ttu-id="006ad-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="006ad-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="006ad-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="006ad-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="006ad-121">Att lägga till Novatus från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="006ad-121">Adding Novatus from hello gallery</span></span>
2. <span data-ttu-id="006ad-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="006ad-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-novatus-from-hello-gallery"></a><span data-ttu-id="006ad-123">Att lägga till Novatus från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="006ad-123">Adding Novatus from hello gallery</span></span>
<span data-ttu-id="006ad-124">tooconfigure hello integrering av Novatus i Azure AD, behöver du tooadd Novatus hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="006ad-124">tooconfigure hello integration of Novatus into Azure AD, you need tooadd Novatus from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="006ad-125">**tooadd Novatus från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="006ad-125">**tooadd Novatus from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="006ad-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="006ad-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="006ad-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="006ad-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="006ad-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="006ad-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="006ad-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="006ad-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="006ad-133">Skriv i sökrutan hello **Novatus**.</span><span class="sxs-lookup"><span data-stu-id="006ad-133">In hello search box, type **Novatus**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_search.png)

5. <span data-ttu-id="006ad-135">Markera hello resultat på panelen **Novatus**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="006ad-135">In hello results panel, select **Novatus**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="006ad-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="006ad-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="006ad-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Novatus baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="006ad-138">In this section, you configure and test Azure AD single sign-on with Novatus based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="006ad-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Novatus är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="006ad-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Novatus is tooa user in Azure AD.</span></span> <span data-ttu-id="006ad-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Novatus toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="006ad-140">In other words, a link relationship between an Azure AD user and hello related user in Novatus needs toobe established.</span></span>

<span data-ttu-id="006ad-141">I Novatus, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="006ad-141">In Novatus, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="006ad-142">tooconfigure och testa Azure AD enkel inloggning med Novatus, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="006ad-142">tooconfigure and test Azure AD single sign-on with Novatus, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="006ad-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="006ad-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="006ad-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="006ad-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="006ad-145">**[Skapa en testanvändare Novatus](#creating-a-novatus-test-user)**  -toohave en motsvarighet för Britta Simon i Novatus som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="006ad-145">**[Creating a Novatus test user](#creating-a-novatus-test-user)** - toohave a counterpart of Britta Simon in Novatus that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="006ad-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="006ad-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="006ad-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="006ad-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="006ad-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="006ad-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="006ad-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Novatus program.</span><span class="sxs-lookup"><span data-stu-id="006ad-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Novatus application.</span></span>

<span data-ttu-id="006ad-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Novatus:**</span><span class="sxs-lookup"><span data-stu-id="006ad-150">**tooconfigure Azure AD single sign-on with Novatus, perform hello following steps:**</span></span>

1. <span data-ttu-id="006ad-151">I hello Azure-portalen på hello **Novatus** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="006ad-151">In hello Azure portal, on hello **Novatus** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="006ad-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="006ad-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_samlbase.png)

3. <span data-ttu-id="006ad-155">På hello **Novatus domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="006ad-155">On hello **Novatus Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_url.png)

     <span data-ttu-id="006ad-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://sso.novatuscontracts.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="006ad-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://sso.novatuscontracts.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="006ad-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="006ad-158">This value is not real.</span></span> <span data-ttu-id="006ad-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="006ad-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="006ad-160">Kontakta [Novatus klienten supportteamet](mailto:jvinci@novatusinc.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="006ad-160">Contact [Novatus Client support team](mailto:jvinci@novatusinc.com) tooget this value.</span></span> 
 


4. <span data-ttu-id="006ad-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="006ad-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_certificate.png) 

5. <span data-ttu-id="006ad-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="006ad-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="006ad-165">På hello **Novatus Configuration** klickar du på **konfigurera Novatus** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="006ad-165">On hello **Novatus Configuration** section, click **Configure Novatus** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="006ad-166">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="006ad-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_configure.png) 

7. <span data-ttu-id="006ad-168">tooget SSO konfigurerats för ditt program bör du kontakta din [Novatus supportteam](mailto:jvinci@novatusinc.com).</span><span class="sxs-lookup"><span data-stu-id="006ad-168">tooget SSO configured for your application, contact your [Novatus support team](mailto:jvinci@novatusinc.com).</span></span> <span data-ttu-id="006ad-169">Koppla hello **hämtat certifikat** filen tooyour e-post och dela hello **metadata URL: er** (**Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**) med Novatus team tooset in enkel inloggning på sidan.</span><span class="sxs-lookup"><span data-stu-id="006ad-169">Attach hello **downloaded certificate** file tooyour mail and share hello **metadata urls** (**Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL**) with Novatus team tooset up SSO on their side.</span></span>

> [!TIP]
> <span data-ttu-id="006ad-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="006ad-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="006ad-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="006ad-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="006ad-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="006ad-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="006ad-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="006ad-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="006ad-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="006ad-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="006ad-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="006ad-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="006ad-177">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="006ad-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="006ad-179">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="006ad-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="006ad-181">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="006ad-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="006ad-183">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="006ad-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-novatus-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="006ad-185">a.</span><span class="sxs-lookup"><span data-stu-id="006ad-185">a.</span></span> <span data-ttu-id="006ad-186">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="006ad-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="006ad-187">b.</span><span class="sxs-lookup"><span data-stu-id="006ad-187">b.</span></span> <span data-ttu-id="006ad-188">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="006ad-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="006ad-189">c.</span><span class="sxs-lookup"><span data-stu-id="006ad-189">c.</span></span> <span data-ttu-id="006ad-190">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="006ad-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="006ad-191">d.</span><span class="sxs-lookup"><span data-stu-id="006ad-191">d.</span></span> <span data-ttu-id="006ad-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="006ad-192">Click **Create**.</span></span>
 
### <a name="creating-a-novatus-test-user"></a><span data-ttu-id="006ad-193">Skapa en testanvändare Novatus</span><span class="sxs-lookup"><span data-stu-id="006ad-193">Creating a Novatus test user</span></span>

<span data-ttu-id="006ad-194">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Novatus.</span><span class="sxs-lookup"><span data-stu-id="006ad-194">hello objective of this section is toocreate a user called Britta Simon in Novatus.</span></span> <span data-ttu-id="006ad-195">Novatus stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="006ad-195">Novatus supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="006ad-196">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="006ad-196">There is no action item for you in this section.</span></span> <span data-ttu-id="006ad-197">En ny användare skapas under ett försök tooaccess Novatus om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="006ad-197">A new user will be created during an attempt tooaccess Novatus if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="006ad-198">Om du behöver toocreate en användare manuellt, måste toocontact hello [Novatus supportteam](mailto:jvinci@novatusinc.com).</span><span class="sxs-lookup"><span data-stu-id="006ad-198">If you need toocreate an user manually, you need toocontact hello [Novatus support team](mailto:jvinci@novatusinc.com).</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="006ad-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="006ad-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="006ad-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNovatus.</span><span class="sxs-lookup"><span data-stu-id="006ad-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNovatus.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="006ad-202">**tooassign Britta Simon tooNovatus utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="006ad-202">**tooassign Britta Simon tooNovatus, perform hello following steps:**</span></span>

1. <span data-ttu-id="006ad-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="006ad-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="006ad-205">Välj i listan med program hello **Novatus**.</span><span class="sxs-lookup"><span data-stu-id="006ad-205">In hello applications list, select **Novatus**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_app.png) 

3. <span data-ttu-id="006ad-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="006ad-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="006ad-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="006ad-209">Click **Add** button.</span></span> <span data-ttu-id="006ad-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="006ad-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="006ad-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="006ad-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="006ad-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="006ad-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="006ad-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="006ad-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="006ad-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="006ad-215">Testing single sign-on</span></span>

<span data-ttu-id="006ad-216">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="006ad-216">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="006ad-217">Du bör få automatiskt inloggade tooyour Novatus programmet när du klickar på hello Novatus panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="006ad-217">When you click hello Novatus tile in hello Access Panel, you should get automatically signed-on tooyour Novatus application.</span></span> <span data-ttu-id="006ad-218">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="006ad-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="006ad-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="006ad-219">Additional resources</span></span>

* [<span data-ttu-id="006ad-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="006ad-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="006ad-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="006ad-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_203.png

