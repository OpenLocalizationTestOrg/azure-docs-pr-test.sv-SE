---
title: "Självstudier: Azure Active Directory-integrering med Inkling | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Inkling."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 64c7ee45-ee8a-42f7-bf04-fd0e00833ea9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 544101f1972ec16222406b761d2b6f4987458df5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-inkling"></a><span data-ttu-id="08e83-103">Självstudier: Azure Active Directory-integrering med Inkling</span><span class="sxs-lookup"><span data-stu-id="08e83-103">Tutorial: Azure Active Directory integration with Inkling</span></span>

<span data-ttu-id="08e83-104">I kursen får du lära dig hur toointegrate Inkling med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="08e83-104">In this tutorial, you learn how toointegrate Inkling with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="08e83-105">Integrera Inkling med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="08e83-105">Integrating Inkling with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="08e83-106">Du kan styra i Azure AD som har åtkomst till tooInkling</span><span class="sxs-lookup"><span data-stu-id="08e83-106">You can control in Azure AD who has access tooInkling</span></span>
- <span data-ttu-id="08e83-107">Du kan aktivera din användare tooautomatically get inloggade tooInkling (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="08e83-107">You can enable your users tooautomatically get signed-on tooInkling (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="08e83-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="08e83-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="08e83-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="08e83-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08e83-110">Krav</span><span class="sxs-lookup"><span data-stu-id="08e83-110">Prerequisites</span></span>

<span data-ttu-id="08e83-111">tooconfigure Azure AD-integrering med Inkling, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="08e83-111">tooconfigure Azure AD integration with Inkling, you need hello following items:</span></span>

- <span data-ttu-id="08e83-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="08e83-112">An Azure AD subscription</span></span>
- <span data-ttu-id="08e83-113">En Inkling enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="08e83-113">An Inkling single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="08e83-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="08e83-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="08e83-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="08e83-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="08e83-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="08e83-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="08e83-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="08e83-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="08e83-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="08e83-118">Scenario description</span></span>
<span data-ttu-id="08e83-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="08e83-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="08e83-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="08e83-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="08e83-121">Att lägga till Inkling från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="08e83-121">Adding Inkling from hello gallery</span></span>
2. <span data-ttu-id="08e83-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08e83-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-inkling-from-hello-gallery"></a><span data-ttu-id="08e83-123">Att lägga till Inkling från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="08e83-123">Adding Inkling from hello gallery</span></span>
<span data-ttu-id="08e83-124">tooconfigure hello integrering av Inkling i Azure AD, behöver du tooadd Inkling hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="08e83-124">tooconfigure hello integration of Inkling into Azure AD, you need tooadd Inkling from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="08e83-125">**tooadd Inkling från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="08e83-125">**tooadd Inkling from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="08e83-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="08e83-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="08e83-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="08e83-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="08e83-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="08e83-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="08e83-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08e83-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="08e83-133">Skriv i sökrutan hello **Inkling**.</span><span class="sxs-lookup"><span data-stu-id="08e83-133">In hello search box, type **Inkling**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_001.png)

5. <span data-ttu-id="08e83-135">Markera hello resultat på panelen **Inkling**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="08e83-135">In hello results panel, select **Inkling**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="08e83-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08e83-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="08e83-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Inkling baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="08e83-138">In this section, you configure and test Azure AD single sign-on with Inkling based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="08e83-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Inkling är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08e83-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Inkling is tooa user in Azure AD.</span></span> <span data-ttu-id="08e83-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Inkling toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="08e83-140">In other words, a link relationship between an Azure AD user and hello related user in Inkling needs toobe established.</span></span>

<span data-ttu-id="08e83-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Inkling.</span><span class="sxs-lookup"><span data-stu-id="08e83-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Inkling.</span></span>

<span data-ttu-id="08e83-142">tooconfigure och testa Azure AD enkel inloggning med Inkling, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="08e83-142">tooconfigure and test Azure AD single sign-on with Inkling, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="08e83-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="08e83-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="08e83-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08e83-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="08e83-145">**[Skapa en testanvändare Inkling](#creating-an-inkling-test-user)**  -toohave en motsvarighet för Britta Simon i Inkling som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="08e83-145">**[Creating an Inkling test user](#creating-an-inkling-test-user)** - toohave a counterpart of Britta Simon in Inkling that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="08e83-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="08e83-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="08e83-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="08e83-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="08e83-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08e83-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="08e83-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Inkling program.</span><span class="sxs-lookup"><span data-stu-id="08e83-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Inkling application.</span></span>

<span data-ttu-id="08e83-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Inkling:**</span><span class="sxs-lookup"><span data-stu-id="08e83-150">**tooconfigure Azure AD single sign-on with Inkling, perform hello following steps:**</span></span>

1. <span data-ttu-id="08e83-151">I hello Azure Management portal på hello **Inkling** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="08e83-151">In hello Azure Management portal, on hello **Inkling** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="08e83-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="08e83-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="08e83-155">På hello **Inkling domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="08e83-155">On hello **Inkling Domain and URLs** section, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_01.png)

    <span data-ttu-id="08e83-157">a.</span><span class="sxs-lookup"><span data-stu-id="08e83-157">a.</span></span> <span data-ttu-id="08e83-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://api.inkling.com/saml/v2/metadata/<user-id>`</span><span class="sxs-lookup"><span data-stu-id="08e83-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://api.inkling.com/saml/v2/metadata/<user-id>`</span></span>

    <span data-ttu-id="08e83-159">b.</span><span class="sxs-lookup"><span data-stu-id="08e83-159">b.</span></span> <span data-ttu-id="08e83-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://api.inkling.com/saml/v2/acs/<user-id>`</span><span class="sxs-lookup"><span data-stu-id="08e83-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://api.inkling.com/saml/v2/acs/<user-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="08e83-161">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="08e83-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="08e83-162">Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="08e83-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="08e83-163">Kontakta [Inkling supportteamet](mailto:press@inkling.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="08e83-163">Contact [Inkling support team](mailto:press@inkling.com) tooget these values.</span></span>

4. <span data-ttu-id="08e83-164">På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="08e83-164">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_general_400.png)    

5. <span data-ttu-id="08e83-166">På hello **skapa nya certifikat** dialogrutan Klicka hello kalendern och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="08e83-166">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="08e83-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="08e83-167">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="08e83-169">På hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="08e83-169">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_02.png)

7. <span data-ttu-id="08e83-171">På popup-fönster hello **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="08e83-171">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="08e83-173">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="08e83-173">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_03.png) 

9. <span data-ttu-id="08e83-175">tooget SSO konfigurerats för ditt program bör du kontakta [Inkling supportteamet](mailto:press@inkling.com) och ge dem med hämtade **metadata**.</span><span class="sxs-lookup"><span data-stu-id="08e83-175">tooget SSO configured for your application, contact [Inkling support team](mailto:press@inkling.com) and provide them with downloaded **metadata**.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="08e83-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="08e83-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="08e83-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08e83-177">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="08e83-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="08e83-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="08e83-180">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="08e83-180">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="08e83-182">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="08e83-182">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="08e83-184">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08e83-184">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="08e83-186">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="08e83-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="08e83-188">a.</span><span class="sxs-lookup"><span data-stu-id="08e83-188">a.</span></span> <span data-ttu-id="08e83-189">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="08e83-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="08e83-190">b.</span><span class="sxs-lookup"><span data-stu-id="08e83-190">b.</span></span> <span data-ttu-id="08e83-191">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="08e83-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="08e83-192">c.</span><span class="sxs-lookup"><span data-stu-id="08e83-192">c.</span></span> <span data-ttu-id="08e83-193">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="08e83-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="08e83-194">d.</span><span class="sxs-lookup"><span data-stu-id="08e83-194">d.</span></span> <span data-ttu-id="08e83-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="08e83-195">Click **Create**.</span></span> 



### <a name="creating-an-inkling-test-user"></a><span data-ttu-id="08e83-196">Skapa en testanvändare Inkling</span><span class="sxs-lookup"><span data-stu-id="08e83-196">Creating an Inkling test user</span></span>

<span data-ttu-id="08e83-197">I det här avsnittet skapar du en användare som kallas Britta Simon i Inkling.</span><span class="sxs-lookup"><span data-stu-id="08e83-197">In this section, you create a user called Britta Simon in Inkling.</span></span> <span data-ttu-id="08e83-198">Se tillsammans med [Inkling supportteamet](mailto:press@inkling.com) tooadd hello användare i hello Inkling plattform.</span><span class="sxs-lookup"><span data-stu-id="08e83-198">Please work with [Inkling support team](mailto:press@inkling.com) tooadd hello users in hello Inkling platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="08e83-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="08e83-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="08e83-200">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooInkling sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="08e83-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooInkling.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="08e83-202">**tooassign Britta Simon tooInkling utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="08e83-202">**tooassign Britta Simon tooInkling, perform hello following steps:**</span></span>

1. <span data-ttu-id="08e83-203">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="08e83-203">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="08e83-205">Välj i listan med program hello **Inkling**.</span><span class="sxs-lookup"><span data-stu-id="08e83-205">In hello applications list, select **Inkling**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_50.png) 

3. <span data-ttu-id="08e83-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="08e83-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="08e83-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="08e83-209">Click **Add** button.</span></span> <span data-ttu-id="08e83-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08e83-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="08e83-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="08e83-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="08e83-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08e83-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="08e83-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08e83-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="08e83-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08e83-215">Testing single sign-on</span></span>

<span data-ttu-id="08e83-216">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="08e83-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="08e83-217">Du bör få automatiskt inloggade tooyour Inkling programmet när du klickar på hello Inkling panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="08e83-217">When you click hello Inkling tile in hello Access Panel, you should get automatically signed-on tooyour Inkling application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="08e83-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="08e83-218">Additional resources</span></span>

* [<span data-ttu-id="08e83-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="08e83-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="08e83-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="08e83-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_203.png