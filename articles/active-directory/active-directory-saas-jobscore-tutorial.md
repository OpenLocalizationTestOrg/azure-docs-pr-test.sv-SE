---
title: "Självstudier: Azure Active Directory-integrering med JobScore | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och JobScore."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f51b32-e55c-4c66-96e8-50a2f9c2194a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 6693a5fd96bfd7fbcd7197983b5f04d061970bdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscore"></a><span data-ttu-id="7ee84-103">Självstudier: Azure Active Directory-integrering med JobScore</span><span class="sxs-lookup"><span data-stu-id="7ee84-103">Tutorial: Azure Active Directory integration with JobScore</span></span>

<span data-ttu-id="7ee84-104">I kursen får du lära dig hur toointegrate JobScore med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7ee84-104">In this tutorial, you learn how toointegrate JobScore with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7ee84-105">Integrera JobScore med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7ee84-105">Integrating JobScore with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7ee84-106">Du kan styra i Azure AD som har åtkomst till tooJobScore</span><span class="sxs-lookup"><span data-stu-id="7ee84-106">You can control in Azure AD who has access tooJobScore</span></span>
- <span data-ttu-id="7ee84-107">Du kan aktivera din användare tooautomatically get inloggade tooJobScore (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7ee84-107">You can enable your users tooautomatically get signed-on tooJobScore (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7ee84-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7ee84-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7ee84-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7ee84-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ee84-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7ee84-110">Prerequisites</span></span>

<span data-ttu-id="7ee84-111">tooconfigure Azure AD-integrering med JobScore, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7ee84-111">tooconfigure Azure AD integration with JobScore, you need hello following items:</span></span>

- <span data-ttu-id="7ee84-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7ee84-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7ee84-113">En JobScore enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7ee84-113">A JobScore single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7ee84-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7ee84-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7ee84-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7ee84-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7ee84-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7ee84-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7ee84-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7ee84-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7ee84-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7ee84-118">Scenario description</span></span>
<span data-ttu-id="7ee84-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7ee84-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7ee84-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7ee84-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7ee84-121">Att lägga till JobScore från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7ee84-121">Adding JobScore from hello gallery</span></span>
2. <span data-ttu-id="7ee84-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ee84-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jobscore-from-hello-gallery"></a><span data-ttu-id="7ee84-123">Att lägga till JobScore från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7ee84-123">Adding JobScore from hello gallery</span></span>
<span data-ttu-id="7ee84-124">tooconfigure hello integrering av JobScore i Azure AD, behöver du tooadd JobScore hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="7ee84-124">tooconfigure hello integration of JobScore into Azure AD, you need tooadd JobScore from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7ee84-125">**tooadd JobScore från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7ee84-125">**tooadd JobScore from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ee84-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7ee84-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7ee84-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7ee84-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7ee84-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ee84-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7ee84-133">Skriv i sökrutan hello **JobScore**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-133">In hello search box, type **JobScore**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_search.png)

5. <span data-ttu-id="7ee84-135">Markera hello resultat på panelen **JobScore**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="7ee84-135">In hello results panel, select **JobScore**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7ee84-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ee84-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7ee84-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med JobScore baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7ee84-138">In this section, you configure and test Azure AD single sign-on with JobScore based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7ee84-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i JobScore är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ee84-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in JobScore is tooa user in Azure AD.</span></span> <span data-ttu-id="7ee84-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i JobScore toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="7ee84-140">In other words, a link relationship between an Azure AD user and hello related user in JobScore needs toobe established.</span></span>

<span data-ttu-id="7ee84-141">I JobScore, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7ee84-141">In JobScore, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7ee84-142">tooconfigure och testa Azure AD enkel inloggning med JobScore, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7ee84-142">tooconfigure and test Azure AD single sign-on with JobScore, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7ee84-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7ee84-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7ee84-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7ee84-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7ee84-145">**[Skapa en testanvändare JobScore](#creating-a-jobscore-test-user)**  -toohave en motsvarighet för Britta Simon i JobScore som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7ee84-145">**[Creating a JobScore test user](#creating-a-jobscore-test-user)** - toohave a counterpart of Britta Simon in JobScore that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7ee84-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ee84-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7ee84-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7ee84-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7ee84-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ee84-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7ee84-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt JobScore program.</span><span class="sxs-lookup"><span data-stu-id="7ee84-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your JobScore application.</span></span>

<span data-ttu-id="7ee84-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med JobScore:**</span><span class="sxs-lookup"><span data-stu-id="7ee84-150">**tooconfigure Azure AD single sign-on with JobScore, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ee84-151">I hello Azure-portalen på hello **JobScore** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-151">In hello Azure portal, on hello **JobScore** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7ee84-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ee84-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_samlbase.png)

3. <span data-ttu-id="7ee84-155">På hello **JobScore domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ee84-155">On hello **JobScore Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_url.png)

    <span data-ttu-id="7ee84-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://hire.jobscore.com/auth/adfs/<company name>`</span><span class="sxs-lookup"><span data-stu-id="7ee84-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://hire.jobscore.com/auth/adfs/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7ee84-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7ee84-158">This value is not real.</span></span> <span data-ttu-id="7ee84-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="7ee84-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="7ee84-160">Kontakta [JobScore klienten supportteamet](mailto:support@jobscore.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="7ee84-160">Contact [JobScore Client support team](mailto:support@jobscore.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="7ee84-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="7ee84-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_certificate.png) 

5. <span data-ttu-id="7ee84-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ee84-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7ee84-165">tooconfigure enkel inloggning på **JobScore** sida, behöver du toosend hello hämtas **XML-Metadata för** för[JobScore supportteamet](mailto:support@jobscore.com).</span><span class="sxs-lookup"><span data-stu-id="7ee84-165">tooconfigure single sign-on on **JobScore** side, you need toosend hello downloaded **Metadata XML** too[JobScore support team](mailto:support@jobscore.com).</span></span> 

> [!TIP]
> <span data-ttu-id="7ee84-166">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="7ee84-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7ee84-167">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="7ee84-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7ee84-168">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7ee84-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7ee84-169">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ee84-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="7ee84-170">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7ee84-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7ee84-172">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7ee84-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ee84-173">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7ee84-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7ee84-175">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-175">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7ee84-177">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ee84-177">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7ee84-179">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ee84-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7ee84-181">a.</span><span class="sxs-lookup"><span data-stu-id="7ee84-181">a.</span></span> <span data-ttu-id="7ee84-182">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-182">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7ee84-183">b.</span><span class="sxs-lookup"><span data-stu-id="7ee84-183">b.</span></span> <span data-ttu-id="7ee84-184">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7ee84-184">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7ee84-185">c.</span><span class="sxs-lookup"><span data-stu-id="7ee84-185">c.</span></span> <span data-ttu-id="7ee84-186">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7ee84-187">d.</span><span class="sxs-lookup"><span data-stu-id="7ee84-187">d.</span></span> <span data-ttu-id="7ee84-188">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-188">Click **Create**.</span></span>
 
### <a name="creating-a-jobscore-test-user"></a><span data-ttu-id="7ee84-189">Skapa en testanvändare JobScore</span><span class="sxs-lookup"><span data-stu-id="7ee84-189">Creating a JobScore test user</span></span>

<span data-ttu-id="7ee84-190">I det här avsnittet skapar du en användare som kallas Britta Simon i JobScore.</span><span class="sxs-lookup"><span data-stu-id="7ee84-190">In this section, you create a user called Britta Simon in JobScore.</span></span> <span data-ttu-id="7ee84-191">Arbeta med [JobScore supportteamet](mailto:support@jobscore.com) tooadd hello användare i hello JobScore plattform.</span><span class="sxs-lookup"><span data-stu-id="7ee84-191">Work with [JobScore support team](mailto:support@jobscore.com) tooadd hello users in hello JobScore platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7ee84-192">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ee84-192">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7ee84-193">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooJobScore.</span><span class="sxs-lookup"><span data-stu-id="7ee84-193">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJobScore.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7ee84-195">**tooassign Britta Simon tooJobScore utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7ee84-195">**tooassign Britta Simon tooJobScore, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ee84-196">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-196">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7ee84-198">Välj i listan med program hello **JobScore**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-198">In hello applications list, select **JobScore**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_app.png) 

3. <span data-ttu-id="7ee84-200">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7ee84-200">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7ee84-202">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ee84-202">Click **Add** button.</span></span> <span data-ttu-id="7ee84-203">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ee84-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7ee84-205">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="7ee84-205">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7ee84-206">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ee84-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7ee84-207">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ee84-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7ee84-208">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ee84-208">Testing single sign-on</span></span>

<span data-ttu-id="7ee84-209">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7ee84-209">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7ee84-210">Du bör få automatiskt inloggade tooyour JobScore programmet när du klickar på hello JobScore panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7ee84-210">When you click hello JobScore tile in hello Access Panel, you should get automatically signed-on tooyour JobScore application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ee84-211">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7ee84-211">Additional resources</span></span>

* [<span data-ttu-id="7ee84-212">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ee84-212">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7ee84-213">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7ee84-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_203.png

