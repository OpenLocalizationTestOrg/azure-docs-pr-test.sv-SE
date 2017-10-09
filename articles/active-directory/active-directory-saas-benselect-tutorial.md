---
title: "Självstudier: Azure Active Directory-integrering med BenSelect | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och BenSelect."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffa17478-3ea1-4356-a289-545b5b9a4494
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: c3705da337bf8f6e76de58cd21c5b047c8f5e12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benselect"></a><span data-ttu-id="86dcc-103">Självstudier: Azure Active Directory-integrering med BenSelect</span><span class="sxs-lookup"><span data-stu-id="86dcc-103">Tutorial: Azure Active Directory integration with BenSelect</span></span>

<span data-ttu-id="86dcc-104">I kursen får du lära dig hur toointegrate BenSelect med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="86dcc-104">In this tutorial, you learn how toointegrate BenSelect with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="86dcc-105">Integrera BenSelect med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="86dcc-105">Integrating BenSelect with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="86dcc-106">Du kan styra i Azure AD som har åtkomst till tooBenSelect</span><span class="sxs-lookup"><span data-stu-id="86dcc-106">You can control in Azure AD who has access tooBenSelect</span></span>
- <span data-ttu-id="86dcc-107">Du kan aktivera din användare tooautomatically get inloggade tooBenSelect (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="86dcc-107">You can enable your users tooautomatically get signed-on tooBenSelect (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="86dcc-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="86dcc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="86dcc-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="86dcc-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86dcc-110">Krav</span><span class="sxs-lookup"><span data-stu-id="86dcc-110">Prerequisites</span></span>

<span data-ttu-id="86dcc-111">tooconfigure Azure AD-integrering med BenSelect, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="86dcc-111">tooconfigure Azure AD integration with BenSelect, you need hello following items:</span></span>

- <span data-ttu-id="86dcc-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="86dcc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="86dcc-113">En BenSelect enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="86dcc-113">A BenSelect single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="86dcc-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="86dcc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="86dcc-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="86dcc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="86dcc-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="86dcc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="86dcc-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="86dcc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="86dcc-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="86dcc-118">Scenario description</span></span>
<span data-ttu-id="86dcc-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="86dcc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="86dcc-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="86dcc-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="86dcc-121">Att lägga till BenSelect från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="86dcc-121">Adding BenSelect from hello gallery</span></span>
2. <span data-ttu-id="86dcc-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86dcc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benselect-from-hello-gallery"></a><span data-ttu-id="86dcc-123">Att lägga till BenSelect från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="86dcc-123">Adding BenSelect from hello gallery</span></span>
<span data-ttu-id="86dcc-124">tooconfigure hello integrering av BenSelect i Azure AD, behöver du tooadd BenSelect hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="86dcc-124">tooconfigure hello integration of BenSelect into Azure AD, you need tooadd BenSelect from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="86dcc-125">**tooadd BenSelect från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="86dcc-125">**tooadd BenSelect from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="86dcc-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="86dcc-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="86dcc-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="86dcc-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="86dcc-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dcc-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="86dcc-133">Skriv i sökrutan hello **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-133">In hello search box, type **BenSelect**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_search.png)

5. <span data-ttu-id="86dcc-135">Markera hello resultat på panelen **BenSelect**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="86dcc-135">In hello results panel, select **BenSelect**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="86dcc-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86dcc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="86dcc-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BenSelect baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="86dcc-138">In this section, you configure and test Azure AD single sign-on with BenSelect based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="86dcc-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i BenSelect är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86dcc-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BenSelect is tooa user in Azure AD.</span></span> <span data-ttu-id="86dcc-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i BenSelect toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="86dcc-140">In other words, a link relationship between an Azure AD user and hello related user in BenSelect needs toobe established.</span></span>

<span data-ttu-id="86dcc-141">I BenSelect, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="86dcc-141">In BenSelect, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="86dcc-142">tooconfigure och testa Azure AD enkel inloggning med BenSelect, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="86dcc-142">tooconfigure and test Azure AD single sign-on with BenSelect, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="86dcc-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="86dcc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="86dcc-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="86dcc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="86dcc-145">**[Skapa en testanvändare BenSelect](#creating-a-benselect-test-user)**  -toohave en motsvarighet för Britta Simon i BenSelect som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="86dcc-145">**[Creating a BenSelect test user](#creating-a-benselect-test-user)** - toohave a counterpart of Britta Simon in BenSelect that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="86dcc-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="86dcc-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="86dcc-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="86dcc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="86dcc-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86dcc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="86dcc-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt BenSelect program.</span><span class="sxs-lookup"><span data-stu-id="86dcc-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BenSelect application.</span></span>

<span data-ttu-id="86dcc-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med BenSelect:**</span><span class="sxs-lookup"><span data-stu-id="86dcc-150">**tooconfigure Azure AD single sign-on with BenSelect, perform hello following steps:**</span></span>

1. <span data-ttu-id="86dcc-151">I hello Azure-portalen på hello **BenSelect** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-151">In hello Azure portal, on hello **BenSelect** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="86dcc-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="86dcc-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_samlbase.png)

3. <span data-ttu-id="86dcc-155">På hello **BenSelect domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="86dcc-155">On hello **BenSelect Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_url.png)

    <span data-ttu-id="86dcc-157">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span><span class="sxs-lookup"><span data-stu-id="86dcc-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="86dcc-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="86dcc-158">This value is not real.</span></span> <span data-ttu-id="86dcc-159">Uppdatera det här värdet med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="86dcc-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="86dcc-160">Kontakta [BenSelect supportteamet](mailto:support@selerix.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="86dcc-160">Contact [BenSelect support team](mailto:support@selerix.com) tooget this value.</span></span>
 
4. <span data-ttu-id="86dcc-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="86dcc-161">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_certificate.png) 

5. <span data-ttu-id="86dcc-163">BenSelect program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="86dcc-163">BenSelect application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="86dcc-164">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="86dcc-164">Configure hello following claims for this application.</span></span> <span data-ttu-id="86dcc-165">Du kan hantera hello värden för attributen från hello **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="86dcc-165">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="86dcc-166">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="86dcc-166">hello following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_06.png)

6. <span data-ttu-id="86dcc-168">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="86dcc-168">In hello **User Attributes** section on hello **Single sign-on** dialog:</span></span>

    <span data-ttu-id="86dcc-169">a.</span><span class="sxs-lookup"><span data-stu-id="86dcc-169">a.</span></span> <span data-ttu-id="86dcc-170">I hello **användar-ID** listrutan, Välj **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-170">In hello **User Identifier** dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="86dcc-171">b.</span><span class="sxs-lookup"><span data-stu-id="86dcc-171">b.</span></span> <span data-ttu-id="86dcc-172">I hello **e** listrutan, Välj **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-172">In hello **Mail** dropdown list, select **user.userprincipalname**.</span></span>

7. <span data-ttu-id="86dcc-173">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="86dcc-173">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="86dcc-175">På hello **BenSelect Configuration** klickar du på **konfigurera BenSelect** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="86dcc-175">On hello **BenSelect Configuration** section, click **Configure BenSelect** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="86dcc-176">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="86dcc-176">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_configure.png) 

9. <span data-ttu-id="86dcc-178">tooconfigure enkel inloggning på **BenSelect** sida, behöver du toosend hello hämtas **Certificate(Raw)** och **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**för[BenSelect supportteamet](mailto:support@selerix.com).</span><span class="sxs-lookup"><span data-stu-id="86dcc-178">tooconfigure single sign-on on **BenSelect** side, you need toosend hello downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[BenSelect support team](mailto:support@selerix.com).</span></span>

   >[!NOTE]
   ><span data-ttu-id="86dcc-179">Du behöver toomention som den här integrationen kräver hello SHA256-algoritmen (SHA1 inte stöds) tooset hello SSO på hello lämplig server som app2101 osv.</span><span class="sxs-lookup"><span data-stu-id="86dcc-179">You need toomention that this integration requires hello SHA256 algorithm (SHA1 is not supported) tooset hello SSO on hello appropriate server like app2101 etc.</span></span> 
   
> [!TIP]
> <span data-ttu-id="86dcc-180">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="86dcc-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="86dcc-181">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="86dcc-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="86dcc-182">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="86dcc-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="86dcc-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="86dcc-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="86dcc-184">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="86dcc-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="86dcc-186">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="86dcc-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="86dcc-187">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="86dcc-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="86dcc-189">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="86dcc-191">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dcc-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="86dcc-193">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="86dcc-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="86dcc-195">a.</span><span class="sxs-lookup"><span data-stu-id="86dcc-195">a.</span></span> <span data-ttu-id="86dcc-196">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="86dcc-197">b.</span><span class="sxs-lookup"><span data-stu-id="86dcc-197">b.</span></span> <span data-ttu-id="86dcc-198">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="86dcc-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="86dcc-199">c.</span><span class="sxs-lookup"><span data-stu-id="86dcc-199">c.</span></span> <span data-ttu-id="86dcc-200">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="86dcc-201">d.</span><span class="sxs-lookup"><span data-stu-id="86dcc-201">d.</span></span> <span data-ttu-id="86dcc-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-202">Click **Create**.</span></span>
 
### <a name="creating-a-benselect-test-user"></a><span data-ttu-id="86dcc-203">Skapa en testanvändare BenSelect</span><span class="sxs-lookup"><span data-stu-id="86dcc-203">Creating a BenSelect test user</span></span>

<span data-ttu-id="86dcc-204">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i BenSelect.</span><span class="sxs-lookup"><span data-stu-id="86dcc-204">hello objective of this section is toocreate a user called Britta Simon in BenSelect.</span></span> <span data-ttu-id="86dcc-205">Arbeta med [BenSelect supportteamet](mailto:support@selerix.com) tooadd hello användare i hello BenSelect konto.</span><span class="sxs-lookup"><span data-stu-id="86dcc-205">Work with [BenSelect support team](mailto:support@selerix.com) tooadd hello users in hello BenSelect account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="86dcc-206">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="86dcc-206">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="86dcc-207">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBenSelect.</span><span class="sxs-lookup"><span data-stu-id="86dcc-207">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBenSelect.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="86dcc-209">**tooassign Britta Simon tooBenSelect utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="86dcc-209">**tooassign Britta Simon tooBenSelect, perform hello following steps:**</span></span>

1. <span data-ttu-id="86dcc-210">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-210">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="86dcc-212">Välj i listan med program hello **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-212">In hello applications list, select **BenSelect**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_app.png) 

3. <span data-ttu-id="86dcc-214">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="86dcc-214">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="86dcc-216">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="86dcc-216">Click **Add** button.</span></span> <span data-ttu-id="86dcc-217">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dcc-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="86dcc-219">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="86dcc-219">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="86dcc-220">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dcc-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="86dcc-221">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dcc-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="86dcc-222">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86dcc-222">Testing single sign-on</span></span>

<span data-ttu-id="86dcc-223">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="86dcc-223">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="86dcc-224">Du bör få automatiskt inloggade tooyour BenSelect programmet när du klickar på hello BenSelect panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="86dcc-224">When you click hello BenSelect tile in hello Access Panel, you should get automatically signed-on tooyour BenSelect application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86dcc-225">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="86dcc-225">Additional resources</span></span>

* [<span data-ttu-id="86dcc-226">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="86dcc-226">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="86dcc-227">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="86dcc-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_203.png

