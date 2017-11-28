---
title: "Självstudier: Azure Active Directory-integrering med samarbete | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och samarbete."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03760032-3d76-4b47-ab84-241f72fbd561
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: f3a88a146f2a0a70de5ef58abd46f7f26b4104f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamwork"></a><span data-ttu-id="c7d1b-103">Självstudier: Azure Active Directory-integrering med samarbete</span><span class="sxs-lookup"><span data-stu-id="c7d1b-103">Tutorial: Azure Active Directory integration with Teamwork</span></span>

<span data-ttu-id="c7d1b-104">I kursen får du lära dig hur toointegrate samarbete med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c7d1b-104">In this tutorial, you learn how toointegrate Teamwork with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c7d1b-105">Integrera samarbete med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c7d1b-105">Integrating Teamwork with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c7d1b-106">Du kan styra i Azure AD som har åtkomst till tooTeamwork</span><span class="sxs-lookup"><span data-stu-id="c7d1b-106">You can control in Azure AD who has access tooTeamwork</span></span>
- <span data-ttu-id="c7d1b-107">Du kan aktivera din användare tooautomatically get inloggade tooTeamwork (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c7d1b-107">You can enable your users tooautomatically get signed-on tooTeamwork (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c7d1b-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="c7d1b-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="c7d1b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c7d1b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7d1b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c7d1b-110">Prerequisites</span></span>

<span data-ttu-id="c7d1b-111">tooconfigure Azure AD-integrering med samarbete måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c7d1b-111">tooconfigure Azure AD integration with Teamwork, you need hello following items:</span></span>

- <span data-ttu-id="c7d1b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c7d1b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c7d1b-113">Ett samarbete enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c7d1b-113">A Teamwork single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="c7d1b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="c7d1b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c7d1b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c7d1b-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="c7d1b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c7d1b-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="c7d1b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c7d1b-118">Scenario description</span></span>
<span data-ttu-id="c7d1b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c7d1b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c7d1b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c7d1b-121">Att lägga till samarbete från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c7d1b-121">Adding Teamwork from hello gallery</span></span>
2. <span data-ttu-id="c7d1b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c7d1b-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-teamwork-from-hello-gallery"></a><span data-ttu-id="c7d1b-123">Att lägga till samarbete från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c7d1b-123">Adding Teamwork from hello gallery</span></span>
<span data-ttu-id="c7d1b-124">tooconfigure hello integrering av samarbete i Azure AD, behöver du tooadd samarbete hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-124">tooconfigure hello integration of Teamwork into Azure AD, you need tooadd Teamwork from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c7d1b-125">**tooadd samarbete från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c7d1b-125">**tooadd Teamwork from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c7d1b-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c7d1b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c7d1b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c7d1b-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c7d1b-133">Skriv i sökrutan hello **samarbete**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-133">In hello search box, type **Teamwork**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_001.png)

5. <span data-ttu-id="c7d1b-135">Markera hello resultat på panelen **samarbete**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-135">In hello results panel, select **Teamwork**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c7d1b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c7d1b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c7d1b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med samarbete baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-138">In this section, you configure and test Azure AD single sign-on with Teamwork based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c7d1b-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i samarbete är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Teamwork is tooa user in Azure AD.</span></span> <span data-ttu-id="c7d1b-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i samarbete toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-140">In other words, a link relationship between an Azure AD user and hello related user in Teamwork needs toobe established.</span></span>

<span data-ttu-id="c7d1b-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i samarbete.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Teamwork.</span></span>

<span data-ttu-id="c7d1b-142">tooconfigure och testa Azure AD enkel inloggning med samarbete, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c7d1b-142">tooconfigure and test Azure AD single sign-on with Teamwork, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c7d1b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c7d1b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c7d1b-145">**[Skapa en testanvändare samarbete](#creating-a-teamwork-test-user)**  -toohave en motsvarighet för Britta Simon i samarbete som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-145">**[Creating a Teamwork test user](#creating-a-teamwork-test-user)** - toohave a counterpart of Britta Simon in Teamwork that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="c7d1b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c7d1b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c7d1b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c7d1b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c7d1b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt program för samarbete.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Teamwork application.</span></span>

<span data-ttu-id="c7d1b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med samarbete:**</span><span class="sxs-lookup"><span data-stu-id="c7d1b-150">**tooconfigure Azure AD single sign-on with Teamwork, perform hello following steps:**</span></span>

1. <span data-ttu-id="c7d1b-151">I hello Azure Management portal på hello **samarbete** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-151">In hello Azure Management portal, on hello **Teamwork** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c7d1b-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_01.png)

3. <span data-ttu-id="c7d1b-155">På hello **samarbete domän och URL: er** avsnitt i hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.teamwork.com`</span><span class="sxs-lookup"><span data-stu-id="c7d1b-155">On hello **Teamwork Domain and URLs** section, in hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<company name>.teamwork.com`</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_02.png)

    > [!NOTE] 
    > <span data-ttu-id="c7d1b-157">Observera att detta inte är hello verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-157">Please note that this is not hello real value.</span></span> <span data-ttu-id="c7d1b-158">Du har tooupdate värdet med hello faktiska inloggning URL.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-158">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="c7d1b-159">Kontakta [samarbete supportteamet](mailto:support@teamwork.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-159">Contact [Teamwork support team](mailto:support@teamwork.com) tooget this value.</span></span> 

4. <span data-ttu-id="c7d1b-160">På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-160">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_03.png)   

5. <span data-ttu-id="c7d1b-162">På hello **skapa nya certifikat** dialogrutan Klicka hello kalendern och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-162">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="c7d1b-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-163">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="c7d1b-165">På hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-165">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_04.png)

7. <span data-ttu-id="c7d1b-167">På popup-fönster hello **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-167">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="c7d1b-169">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_05.png) 

9. <span data-ttu-id="c7d1b-171">tooget SSO konfigurerats för ditt program bör du kontakta [samarbete supportteamet](mailto:support@teamwork.com) och ge dem hello hämtas **metadata**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-171">tooget SSO configured for your application, contact [Teamwork support team](mailto:support@teamwork.com) and provide them with hello downloaded **metadata**.</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c7d1b-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c7d1b-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="c7d1b-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-173">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c7d1b-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c7d1b-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c7d1b-176">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-176">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c7d1b-178">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-178">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c7d1b-180">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-180">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c7d1b-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c7d1b-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c7d1b-184">a.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-184">a.</span></span> <span data-ttu-id="c7d1b-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c7d1b-186">b.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-186">b.</span></span> <span data-ttu-id="c7d1b-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c7d1b-188">c.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-188">c.</span></span> <span data-ttu-id="c7d1b-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c7d1b-190">d.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-190">d.</span></span> <span data-ttu-id="c7d1b-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-191">Click **Create**.</span></span> 



### <a name="creating-a-teamwork-test-user"></a><span data-ttu-id="c7d1b-192">Skapa en testanvändare samarbete</span><span class="sxs-lookup"><span data-stu-id="c7d1b-192">Creating a Teamwork test user</span></span>

<span data-ttu-id="c7d1b-193">I det här avsnittet skapar du en användare som kallas Britta Simon i samarbete.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-193">In this section, you create a user called Britta Simon in Teamwork.</span></span> <span data-ttu-id="c7d1b-194">Se tillsammans med [samarbete supportteamet](mailto:support@teamwork.com) tooadd hello användare i hello samarbete plattform.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-194">Please work with [Teamwork support team](mailto:support@teamwork.com) tooadd hello users in hello Teamwork platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c7d1b-195">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c7d1b-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c7d1b-196">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooTeamwork sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooTeamwork.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c7d1b-198">**tooassign Britta Simon tooTeamwork utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c7d1b-198">**tooassign Britta Simon tooTeamwork, perform hello following steps:**</span></span>

1. <span data-ttu-id="c7d1b-199">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-199">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c7d1b-201">Välj i listan med program hello **samarbete**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-201">In hello applications list, select **Teamwork**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_50.png) 

3. <span data-ttu-id="c7d1b-203">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c7d1b-205">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-205">Click **Add** button.</span></span> <span data-ttu-id="c7d1b-206">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c7d1b-208">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c7d1b-209">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c7d1b-210">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="c7d1b-211">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c7d1b-211">Testing single sign-on</span></span>

<span data-ttu-id="c7d1b-212">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-212">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c7d1b-213">Du bör få automatiskt inloggade tooyour samarbete programmet när du klickar på hello samarbete panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c7d1b-213">When you click hello Teamwork tile in hello Access Panel, you should get automatically signed-on tooyour Teamwork application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="c7d1b-214">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c7d1b-214">Additional resources</span></span>

* [<span data-ttu-id="c7d1b-215">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c7d1b-215">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c7d1b-216">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c7d1b-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_203.png