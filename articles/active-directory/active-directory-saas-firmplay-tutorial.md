---
title: "Självstudier: Azure Active Directory-integrering med FirmPlay - medarbetare befrämjande för rekrytering | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och FirmPlay - medarbetare befrämjande för rekrytering."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: f143e0bb8f2a42de880d77e5f033694ce3f09cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a><span data-ttu-id="e44de-103">Självstudier: Azure Active Directory-integrering med FirmPlay - medarbetare befrämjande för rekrytering</span><span class="sxs-lookup"><span data-stu-id="e44de-103">Tutorial: Azure Active Directory integration with FirmPlay - Employee Advocacy for Recruiting</span></span>

<span data-ttu-id="e44de-104">I kursen får du lära dig hur toointegrate FirmPlay - medarbetare befrämjande för rekrytering med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e44de-104">In this tutorial, you learn how toointegrate FirmPlay - Employee Advocacy for Recruiting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e44de-105">Integrera FirmPlay - medarbetare befrämjande för rekrytering med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e44de-105">Integrating FirmPlay - Employee Advocacy for Recruiting with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e44de-106">Du kan styra i Azure AD som har åtkomst till tooFirmPlay - medarbetare befrämjande för rekrytering</span><span class="sxs-lookup"><span data-stu-id="e44de-106">You can control in Azure AD who has access tooFirmPlay - Employee Advocacy for Recruiting</span></span>
- <span data-ttu-id="e44de-107">Du kan aktivera din användare tooautomatically get inloggade tooFirmPlay - medarbetare befrämjande för rekrytering (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e44de-107">You can enable your users tooautomatically get signed-on tooFirmPlay - Employee Advocacy for Recruiting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e44de-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="e44de-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="e44de-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e44de-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e44de-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e44de-110">Prerequisites</span></span>

<span data-ttu-id="e44de-111">tooconfigure Azure AD-integrering med FirmPlay - medarbetare befrämjande för rekrytering, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e44de-111">tooconfigure Azure AD integration with FirmPlay - Employee Advocacy for Recruiting, you need hello following items:</span></span>

- <span data-ttu-id="e44de-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e44de-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e44de-113">En FirmPlay - medarbetare befrämjande för rekrytering enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e44de-113">A FirmPlay - Employee Advocacy for Recruiting single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="e44de-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e44de-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="e44de-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e44de-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e44de-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e44de-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="e44de-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e44de-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="e44de-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e44de-118">Scenario description</span></span>
<span data-ttu-id="e44de-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e44de-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e44de-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e44de-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e44de-121">Lägger till FirmPlay - medarbetare befrämjande för rekrytering från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e44de-121">Adding FirmPlay - Employee Advocacy for Recruiting from hello gallery</span></span>
2. <span data-ttu-id="e44de-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e44de-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-hello-gallery"></a><span data-ttu-id="e44de-123">Lägger till FirmPlay - medarbetare befrämjande för rekrytering från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e44de-123">Adding FirmPlay - Employee Advocacy for Recruiting from hello gallery</span></span>
<span data-ttu-id="e44de-124">tooconfigure hello integrering av FirmPlay - medarbetare befrämjande för rekrytering till Azure AD måste tooadd FirmPlay - medarbetare befrämjande för rekrytering hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e44de-124">tooconfigure hello integration of FirmPlay - Employee Advocacy for Recruiting into Azure AD, you need tooadd FirmPlay - Employee Advocacy for Recruiting from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e44de-125">**tooadd FirmPlay - medarbetare befrämjande för rekrytering från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e44de-125">**tooadd FirmPlay - Employee Advocacy for Recruiting from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e44de-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e44de-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e44de-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e44de-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e44de-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e44de-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e44de-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e44de-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e44de-133">Skriv i sökrutan hello **FirmPlay - medarbetare befrämjande för rekrytering**.</span><span class="sxs-lookup"><span data-stu-id="e44de-133">In hello search box, type **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_001.png)

5. <span data-ttu-id="e44de-135">Markera hello resultat på panelen **FirmPlay - medarbetare befrämjande för rekrytering**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e44de-135">In hello results panel, select **FirmPlay - Employee Advocacy for Recruiting**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e44de-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e44de-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e44de-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FirmPlay - medarbetare befrämjande för rekrytering baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e44de-138">In this section, you configure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e44de-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i FirmPlay - medarbetare befrämjande för rekrytering är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e44de-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FirmPlay - Employee Advocacy for Recruiting is tooa user in Azure AD.</span></span> <span data-ttu-id="e44de-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i FirmPlay - medarbetare befrämjande för rekrytering toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e44de-140">In other words, a link relationship between an Azure AD user and hello related user in FirmPlay - Employee Advocacy for Recruiting needs toobe established.</span></span>

<span data-ttu-id="e44de-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i FirmPlay - medarbetare befrämjande för rekrytering.</span><span class="sxs-lookup"><span data-stu-id="e44de-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in FirmPlay - Employee Advocacy for Recruiting.</span></span>

<span data-ttu-id="e44de-142">tooconfigure och testa Azure AD enkel inloggning med FirmPlay - medarbetare befrämjande för rekrytering, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e44de-142">tooconfigure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e44de-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e44de-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e44de-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e44de-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e44de-145">**[Skapa en FirmPlay - medarbetare befrämjande för rekrytering testanvändare](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  -toohave en motsvarighet för Britta Simon i FirmPlay: medarbetare befrämjande för rekrytering som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="e44de-145">**[Creating a FirmPlay - Employee Advocacy for Recruiting test user](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)** - toohave a counterpart of Britta Simon in FirmPlay: Employee Advocacy for Recruiting that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="e44de-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e44de-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e44de-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e44de-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e44de-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e44de-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e44de-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i din FirmPlay - medarbetare befrämjande för rekrytering program.</span><span class="sxs-lookup"><span data-stu-id="e44de-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your FirmPlay - Employee Advocacy for Recruiting application.</span></span>

<span data-ttu-id="e44de-150">**tooconfigure Azure AD enkel inloggning med FirmPlay - medarbetare befrämjande för rekrytering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="e44de-150">**tooconfigure Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, perform hello following steps:**</span></span>

1. <span data-ttu-id="e44de-151">I hello Azure Management portal på hello **FirmPlay - medarbetare befrämjande för rekrytering** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e44de-151">In hello Azure Management portal, on hello **FirmPlay - Employee Advocacy for Recruiting** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e44de-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e44de-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_01.png)

3. <span data-ttu-id="e44de-155">På hello **FirmPlay - medarbetare befrämjande rekrytering domän och URL: er** avsnitt i hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<your-subdomain>.firmplay.com/`</span><span class="sxs-lookup"><span data-stu-id="e44de-155">On hello **FirmPlay - Employee Advocacy for Recruiting Domain and URLs** section, in hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<your-subdomain>.firmplay.com/`</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > <span data-ttu-id="e44de-157">Observera att detta inte är hello verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="e44de-157">Please note that this is not hello real value.</span></span> <span data-ttu-id="e44de-158">Du har tooupdate värdet med hello faktiska inloggning URL.</span><span class="sxs-lookup"><span data-stu-id="e44de-158">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="e44de-159">Kontakta [FirmPlay - medarbetare befrämjande för rekrytering supportteamet](mailto:engineering@firmplay.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="e44de-159">Contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) tooget this value.</span></span> 

4. <span data-ttu-id="e44de-160">På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="e44de-160">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_03.png)   

5. <span data-ttu-id="e44de-162">På hello **skapa nya certifikat** dialogrutan Klicka hello kalendern och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="e44de-162">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="e44de-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e44de-163">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="e44de-165">På hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e44de-165">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_04.png)

7. <span data-ttu-id="e44de-167">På popup-fönster hello **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e44de-167">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="e44de-169">På hello **SAML-signeringscertifikat** klickar du på **certifikat (base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="e44de-169">On hello **SAML Signing Certificate** section, click **Certificate (base64)** and then save hello certificate file on your computer.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_05.png) 

9. <span data-ttu-id="e44de-171">På hello **FirmPlay - medarbetare befrämjande för rekrytering konfigurationen** klickar du på **konfigurera FirmPlay - medarbetare befrämjande för rekrytering** tooopen **konfigurera inloggning**dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e44de-171">On hello **FirmPlay - Employee Advocacy for Recruiting Configuration** section, click **Configure FirmPlay - Employee Advocacy for Recruiting** tooopen **Configure sign-on** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_07.png)

10. <span data-ttu-id="e44de-174">tooget SSO konfigurerats för ditt program bör du kontakta [FirmPlay - medarbetare befrämjande för rekrytering supportteamet](mailto:engineering@firmplay.com) och ge dem hello följande:</span><span class="sxs-lookup"><span data-stu-id="e44de-174">tooget SSO configured for your application, contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) and provide them with hello following:</span></span> 

    <span data-ttu-id="e44de-175">• hello hämtas **certifikatfilen**</span><span class="sxs-lookup"><span data-stu-id="e44de-175">•  hello downloaded **Certificate file**</span></span>

    <span data-ttu-id="e44de-176">• hello **SAML enkel inloggning tjänst-URL**</span><span class="sxs-lookup"><span data-stu-id="e44de-176">•  hello **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="e44de-177">• hello **SAML enhets-ID**</span><span class="sxs-lookup"><span data-stu-id="e44de-177">•  hello **SAML Entity ID**</span></span>

    <span data-ttu-id="e44de-178">• hello **Sign-Out URL**</span><span class="sxs-lookup"><span data-stu-id="e44de-178">•  hello **Sign-Out URL**</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e44de-179">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e44de-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="e44de-180">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e44de-180">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e44de-182">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e44de-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e44de-183">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e44de-183">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e44de-185">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="e44de-185">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e44de-187">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e44de-187">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e44de-189">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e44de-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e44de-191">a.</span><span class="sxs-lookup"><span data-stu-id="e44de-191">a.</span></span> <span data-ttu-id="e44de-192">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e44de-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e44de-193">b.</span><span class="sxs-lookup"><span data-stu-id="e44de-193">b.</span></span> <span data-ttu-id="e44de-194">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e44de-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e44de-195">c.</span><span class="sxs-lookup"><span data-stu-id="e44de-195">c.</span></span> <span data-ttu-id="e44de-196">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e44de-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e44de-197">d.</span><span class="sxs-lookup"><span data-stu-id="e44de-197">d.</span></span> <span data-ttu-id="e44de-198">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e44de-198">Click **Create**.</span></span> 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a><span data-ttu-id="e44de-199">Skapa en FirmPlay - medarbetare befrämjande för rekrytering testanvändare</span><span class="sxs-lookup"><span data-stu-id="e44de-199">Creating a FirmPlay - Employee Advocacy for Recruiting test user</span></span>

<span data-ttu-id="e44de-200">I det här avsnittet skapar du en användare som kallas Britta Simon i FirmPlay - medarbetare befrämjande för rekrytering.</span><span class="sxs-lookup"><span data-stu-id="e44de-200">In this section, you create a user called Britta Simon in FirmPlay - Employee Advocacy for Recruiting.</span></span> <span data-ttu-id="e44de-201">Se tillsammans med [FirmPlay - medarbetare befrämjande för rekrytering supportteamet](mailto:engineering@firmplay.com) tooadd hello användare i hello FirmPlay - medarbetare befrämjande för rekrytering plattform.</span><span class="sxs-lookup"><span data-stu-id="e44de-201">Please work with [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) tooadd hello users in hello FirmPlay - Employee Advocacy for Recruiting platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e44de-202">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e44de-202">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e44de-203">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooFirmPlay - medarbetare befrämjande för rekrytering.</span><span class="sxs-lookup"><span data-stu-id="e44de-203">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooFirmPlay - Employee Advocacy for Recruiting.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e44de-205">**tooassign Britta Simon tooFirmPlay - medarbetare befrämjande för rekrytering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="e44de-205">**tooassign Britta Simon tooFirmPlay - Employee Advocacy for Recruiting, perform hello following steps:**</span></span>

1. <span data-ttu-id="e44de-206">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e44de-206">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e44de-208">Välj i listan med program hello **FirmPlay - medarbetare befrämjande för rekrytering**.</span><span class="sxs-lookup"><span data-stu-id="e44de-208">In hello applications list, select **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_50.png) 

3. <span data-ttu-id="e44de-210">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e44de-210">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e44de-212">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e44de-212">Click **Add** button.</span></span> <span data-ttu-id="e44de-213">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e44de-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e44de-215">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e44de-215">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e44de-216">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e44de-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e44de-217">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e44de-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="e44de-218">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e44de-218">Testing single sign-on</span></span>

<span data-ttu-id="e44de-219">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e44de-219">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e44de-220">När du klickar på hello FirmPlay - medarbetare befrämjande för rekrytering panelen i hello åtkomstpanelen, bör du hämta automatiskt inloggade tooyour FirmPlay - medarbetare befrämjande för rekrytering program.</span><span class="sxs-lookup"><span data-stu-id="e44de-220">When you click hello FirmPlay - Employee Advocacy for Recruiting tile in hello Access Panel, you should get automatically signed-on tooyour FirmPlay - Employee Advocacy for Recruiting application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="e44de-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e44de-221">Additional resources</span></span>

* [<span data-ttu-id="e44de-222">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e44de-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e44de-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e44de-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_203.png