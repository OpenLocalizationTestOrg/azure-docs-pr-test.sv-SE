---
title: "Självstudier: Azure Active Directory-integrering med Lucidchart | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Lucidchart."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 774e5f423097650a3cae8e8ca13b2c65b8470736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a><span data-ttu-id="27633-103">Självstudier: Azure Active Directory-integrering med Lucidchart</span><span class="sxs-lookup"><span data-stu-id="27633-103">Tutorial: Azure Active Directory integration with Lucidchart</span></span>

<span data-ttu-id="27633-104">I kursen får du lära dig hur toointegrate Lucidchart med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="27633-104">In this tutorial, you learn how toointegrate Lucidchart with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="27633-105">Integrera Lucidchart med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="27633-105">Integrating Lucidchart with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="27633-106">Du kan styra i Azure AD som har åtkomst till tooLucidchart</span><span class="sxs-lookup"><span data-stu-id="27633-106">You can control in Azure AD who has access tooLucidchart</span></span>
- <span data-ttu-id="27633-107">Du kan aktivera din användare tooautomatically get inloggade tooLucidchart (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="27633-107">You can enable your users tooautomatically get signed-on tooLucidchart (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="27633-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="27633-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="27633-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="27633-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27633-110">Krav</span><span class="sxs-lookup"><span data-stu-id="27633-110">Prerequisites</span></span>

<span data-ttu-id="27633-111">tooconfigure Azure AD-integrering med Lucidchart, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="27633-111">tooconfigure Azure AD integration with Lucidchart, you need hello following items:</span></span>

- <span data-ttu-id="27633-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="27633-112">An Azure AD subscription</span></span>
- <span data-ttu-id="27633-113">En Lucidchart enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="27633-113">A Lucidchart single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="27633-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="27633-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="27633-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="27633-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="27633-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="27633-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="27633-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="27633-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="27633-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="27633-118">Scenario description</span></span>
<span data-ttu-id="27633-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="27633-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="27633-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="27633-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="27633-121">Att lägga till Lucidchart från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="27633-121">Adding Lucidchart from hello gallery</span></span>
2. <span data-ttu-id="27633-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27633-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lucidchart-from-hello-gallery"></a><span data-ttu-id="27633-123">Att lägga till Lucidchart från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="27633-123">Adding Lucidchart from hello gallery</span></span>
<span data-ttu-id="27633-124">tooconfigure hello integrering av Lucidchart i Azure AD, behöver du tooadd Lucidchart hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="27633-124">tooconfigure hello integration of Lucidchart into Azure AD, you need tooadd Lucidchart from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="27633-125">**tooadd Lucidchart från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="27633-125">**tooadd Lucidchart from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="27633-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="27633-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="27633-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="27633-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="27633-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="27633-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="27633-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27633-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="27633-133">Skriv i sökrutan hello **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="27633-133">In hello search box, type **Lucidchart**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_search.png)

5. <span data-ttu-id="27633-135">Markera hello resultat på panelen **Lucidchart**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="27633-135">In hello results panel, select **Lucidchart**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="27633-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27633-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="27633-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Lucidchart baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="27633-138">In this section, you configure and test Azure AD single sign-on with Lucidchart based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="27633-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Lucidchart är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="27633-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lucidchart is tooa user in Azure AD.</span></span> <span data-ttu-id="27633-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Lucidchart toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="27633-140">In other words, a link relationship between an Azure AD user and hello related user in Lucidchart needs toobe established.</span></span>

<span data-ttu-id="27633-141">I Lucidchart, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="27633-141">In Lucidchart, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="27633-142">tooconfigure och testa Azure AD enkel inloggning med Lucidchart, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="27633-142">tooconfigure and test Azure AD single sign-on with Lucidchart, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="27633-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="27633-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="27633-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27633-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="27633-145">**[Skapa en testanvändare Lucidchart](#creating-a-lucidchart-test-user)**  -toohave en motsvarighet för Britta Simon i Lucidchart som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="27633-145">**[Creating a Lucidchart test user](#creating-a-lucidchart-test-user)** - toohave a counterpart of Britta Simon in Lucidchart that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="27633-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="27633-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="27633-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="27633-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="27633-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27633-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="27633-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Lucidchart program.</span><span class="sxs-lookup"><span data-stu-id="27633-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lucidchart application.</span></span>

<span data-ttu-id="27633-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Lucidchart:**</span><span class="sxs-lookup"><span data-stu-id="27633-150">**tooconfigure Azure AD single sign-on with Lucidchart, perform hello following steps:**</span></span>

1. <span data-ttu-id="27633-151">I hello Azure-portalen på hello **Lucidchart** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="27633-151">In hello Azure portal, on hello **Lucidchart** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="27633-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="27633-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_samlbase.png)

3. <span data-ttu-id="27633-155">På hello **Lucidchart domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="27633-155">On hello **Lucidchart Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_url.png)

    <span data-ttu-id="27633-157">I hello **inloggnings-URL** textruta Skriv en URL som:`https://chart2.office.lucidchart.com/saml/sso/azure`</span><span class="sxs-lookup"><span data-stu-id="27633-157">In hello **Sign-on URL** textbox, type a URL as: `https://chart2.office.lucidchart.com/saml/sso/azure`</span></span>

4. <span data-ttu-id="27633-158">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="27633-158">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_certificate.png) 

5. <span data-ttu-id="27633-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="27633-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="27633-162">Logga in på webbplatsen Lucidchart företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="27633-162">In a different web browser window, log into your Lucidchart company site as an administrator.</span></span>

7. <span data-ttu-id="27633-163">Hello-menyn överst hello **Team**.</span><span class="sxs-lookup"><span data-stu-id="27633-163">In hello menu on hello top, click **Team**.</span></span>
   
    <span data-ttu-id="27633-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span><span class="sxs-lookup"><span data-stu-id="27633-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span></span>

8. <span data-ttu-id="27633-165">Klicka på **program \> hantera SAML**.</span><span class="sxs-lookup"><span data-stu-id="27633-165">Click **Applications \> Manage SAML**.</span></span>
   
    <span data-ttu-id="27633-166">![Hantera SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "hantera SAML")</span><span class="sxs-lookup"><span data-stu-id="27633-166">![Manage SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "Manage SAML")</span></span>

9. <span data-ttu-id="27633-167">På hello **SAML autentiseringsinställningar** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="27633-167">On hello **SAML Authentication Settings** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="27633-168">a.</span><span class="sxs-lookup"><span data-stu-id="27633-168">a.</span></span> <span data-ttu-id="27633-169">Välj **aktivera SAML-autentisering**, och klicka sedan på **valfritt**.</span><span class="sxs-lookup"><span data-stu-id="27633-169">Select **Enable SAML Authentication**, and then click **Optional**.</span></span>

    <span data-ttu-id="27633-170">![Inställningar för SAML-autentisering](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "inställningar för SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="27633-170">![SAML Authentication Settings](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "SAML Authentication Settings")</span></span>
 
    <span data-ttu-id="27633-171">b.</span><span class="sxs-lookup"><span data-stu-id="27633-171">b.</span></span> <span data-ttu-id="27633-172">I hello **domän** textruta Skriv din domän och klicka sedan på **ändra certifikatet**.</span><span class="sxs-lookup"><span data-stu-id="27633-172">In hello **Domain** textbox, type your domain, and then click **Change Certificate**.</span></span>

    <span data-ttu-id="27633-173">![Ändra certifikat](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "ändra certifikat")</span><span class="sxs-lookup"><span data-stu-id="27633-173">![Change Certificate](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "Change Certificate")</span></span>
 
    <span data-ttu-id="27633-174">c.</span><span class="sxs-lookup"><span data-stu-id="27633-174">c.</span></span> <span data-ttu-id="27633-175">Öppna din hämtade metadatafil, kopiera hello innehåll och klistra in den i hello **överföra Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="27633-175">Open your downloaded metadata file, copy hello content, and then paste it into hello **Upload Metadata** textbox.</span></span>

    <span data-ttu-id="27633-176">![Överföra Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "överföra Metadata")</span><span class="sxs-lookup"><span data-stu-id="27633-176">![Upload Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "Upload Metadata")</span></span>
 
    <span data-ttu-id="27633-177">d.</span><span class="sxs-lookup"><span data-stu-id="27633-177">d.</span></span> <span data-ttu-id="27633-178">Välj **automatiskt lägga till nya användare toohello team**, och klicka sedan på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="27633-178">Select **Automatically Add new users toohello team**, and then click **Save changes**.</span></span>

    <span data-ttu-id="27633-179">![Spara ändringarna](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "spara ändringar")</span><span class="sxs-lookup"><span data-stu-id="27633-179">![Save Changes](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "Save Changes")</span></span>

> [!TIP]
> <span data-ttu-id="27633-180">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="27633-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="27633-181">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="27633-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="27633-182">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="27633-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="27633-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="27633-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="27633-184">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27633-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="27633-186">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="27633-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="27633-187">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="27633-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="27633-189">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="27633-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="27633-191">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27633-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="27633-193">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="27633-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="27633-195">a.</span><span class="sxs-lookup"><span data-stu-id="27633-195">a.</span></span> <span data-ttu-id="27633-196">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="27633-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="27633-197">b.</span><span class="sxs-lookup"><span data-stu-id="27633-197">b.</span></span> <span data-ttu-id="27633-198">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="27633-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="27633-199">c.</span><span class="sxs-lookup"><span data-stu-id="27633-199">c.</span></span> <span data-ttu-id="27633-200">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="27633-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="27633-201">d.</span><span class="sxs-lookup"><span data-stu-id="27633-201">d.</span></span> <span data-ttu-id="27633-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="27633-202">Click **Create**.</span></span>
 
### <a name="creating-a-lucidchart-test-user"></a><span data-ttu-id="27633-203">Skapa en testanvändare Lucidchart</span><span class="sxs-lookup"><span data-stu-id="27633-203">Creating a Lucidchart test user</span></span>

<span data-ttu-id="27633-204">Det finns inga uppgiften för du tooconfigure användaretablering tooLucidchart.</span><span class="sxs-lookup"><span data-stu-id="27633-204">There is no action item for you tooconfigure user provisioning tooLucidchart.</span></span>  <span data-ttu-id="27633-205">När en tilldelad användare försöker toolog till Lucidchart med hello åtkomstpanelen, kontrollerar Lucidchart om hello användaren finns.</span><span class="sxs-lookup"><span data-stu-id="27633-205">When an assigned user tries toolog into Lucidchart using hello access panel, Lucidchart checks whether hello user exists.</span></span>  

<span data-ttu-id="27633-206">Om det finns inget användarkonto ännu, skapas den automatiskt av Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="27633-206">If there is no user account available yet, it is automatically created by Lucidchart.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="27633-207">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="27633-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="27633-208">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLucidchart.</span><span class="sxs-lookup"><span data-stu-id="27633-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLucidchart.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="27633-210">**tooassign Britta Simon tooLucidchart utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="27633-210">**tooassign Britta Simon tooLucidchart, perform hello following steps:**</span></span>

1. <span data-ttu-id="27633-211">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="27633-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="27633-213">Välj i listan med program hello **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="27633-213">In hello applications list, select **Lucidchart**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_app.png) 

3. <span data-ttu-id="27633-215">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="27633-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="27633-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="27633-217">Click **Add** button.</span></span> <span data-ttu-id="27633-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27633-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="27633-220">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="27633-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="27633-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27633-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="27633-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27633-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="27633-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27633-223">Testing single sign-on</span></span>

<span data-ttu-id="27633-224">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="27633-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="27633-225">Du bör få automatiskt inloggade tooyour Lucidchart programmet när du klickar på hello Lucidchart panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="27633-225">When you click hello Lucidchart tile in hello Access Panel, you should get automatically signed-on tooyour Lucidchart application.</span></span>
<span data-ttu-id="27633-226">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="27633-226">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27633-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="27633-227">Additional resources</span></span>

* [<span data-ttu-id="27633-228">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="27633-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="27633-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="27633-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_203.png

