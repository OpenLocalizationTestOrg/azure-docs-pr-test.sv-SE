---
title: "Självstudier: Azure Active Directory-integrering med MaxxPoint | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och MaxxPoint."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ba026e-96fc-4ae8-b135-0169da810e99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 03b13908add8d8c62f1d1480ed2288658fce14d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-maxxpoint"></a><span data-ttu-id="5510f-103">Självstudier: Azure Active Directory-integrering med MaxxPoint</span><span class="sxs-lookup"><span data-stu-id="5510f-103">Tutorial: Azure Active Directory integration with MaxxPoint</span></span>

<span data-ttu-id="5510f-104">I kursen får du lära dig hur toointegrate MaxxPoint med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5510f-104">In this tutorial, you learn how toointegrate MaxxPoint with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5510f-105">Integrera MaxxPoint med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5510f-105">Integrating MaxxPoint with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5510f-106">Du kan styra i Azure AD som har åtkomst till tooMaxxPoint</span><span class="sxs-lookup"><span data-stu-id="5510f-106">You can control in Azure AD who has access tooMaxxPoint</span></span>
- <span data-ttu-id="5510f-107">Du kan aktivera din användare tooautomatically get inloggade tooMaxxPoint (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5510f-107">You can enable your users tooautomatically get signed-on tooMaxxPoint (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5510f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5510f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5510f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5510f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5510f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5510f-110">Prerequisites</span></span>

<span data-ttu-id="5510f-111">tooconfigure Azure AD-integrering med MaxxPoint, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5510f-111">tooconfigure Azure AD integration with MaxxPoint, you need hello following items:</span></span>

- <span data-ttu-id="5510f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5510f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5510f-113">En MaxxPoint enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="5510f-113">A MaxxPoint single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5510f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5510f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5510f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5510f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5510f-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5510f-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="5510f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5510f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5510f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5510f-118">Scenario description</span></span>
<span data-ttu-id="5510f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5510f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5510f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5510f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5510f-121">Att lägga till MaxxPoint från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5510f-121">Adding MaxxPoint from hello gallery</span></span>
2. <span data-ttu-id="5510f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5510f-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-maxxpoint-from-hello-gallery"></a><span data-ttu-id="5510f-123">Att lägga till MaxxPoint från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5510f-123">Adding MaxxPoint from hello gallery</span></span>
<span data-ttu-id="5510f-124">tooconfigure hello integrering av MaxxPoint i Azure AD, behöver du tooadd MaxxPoint hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5510f-124">tooconfigure hello integration of MaxxPoint into Azure AD, you need tooadd MaxxPoint from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5510f-125">**tooadd MaxxPoint från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5510f-125">**tooadd MaxxPoint from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5510f-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5510f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5510f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5510f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5510f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5510f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5510f-131">Klicka på **nytt program** hello längst upp i dialogrutan tooadd nytt program.</span><span class="sxs-lookup"><span data-stu-id="5510f-131">Click **New application** button on hello top of dialog tooadd new application.</span></span>

    ![Program][3]

4. <span data-ttu-id="5510f-133">Skriv i sökrutan hello **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="5510f-133">In hello search box, type **MaxxPoint**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_001.png)

5. <span data-ttu-id="5510f-135">Markera hello resultat på panelen **MaxxPoint**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5510f-135">In hello results panel, select **MaxxPoint**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5510f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5510f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5510f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med MaxxPoint baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5510f-138">In this section, you configure and test Azure AD single sign-on with MaxxPoint based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5510f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i MaxxPoint är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5510f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in MaxxPoint is tooa user in Azure AD.</span></span> <span data-ttu-id="5510f-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i MaxxPoint toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5510f-140">In other words, a link relationship between an Azure AD user and hello related user in MaxxPoint needs toobe established.</span></span>

<span data-ttu-id="5510f-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="5510f-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in MaxxPoint.</span></span>

<span data-ttu-id="5510f-142">tooconfigure och testa Azure AD enkel inloggning med MaxxPoint, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5510f-142">tooconfigure and test Azure AD single sign-on with MaxxPoint, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5510f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5510f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5510f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5510f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5510f-145">**[Skapa en testanvändare MaxxPoint](#creating-a-maxxpoint-test-user)**  -toohave en motsvarighet för Britta Simon i MaxxPoint som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="5510f-145">**[Creating a MaxxPoint test user](#creating-a-maxxpoint-test-user)** - toohave a counterpart of Britta Simon in MaxxPoint that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="5510f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5510f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5510f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5510f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5510f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5510f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5510f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt MaxxPoint program.</span><span class="sxs-lookup"><span data-stu-id="5510f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your MaxxPoint application.</span></span>

<span data-ttu-id="5510f-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med MaxxPoint:**</span><span class="sxs-lookup"><span data-stu-id="5510f-150">**tooconfigure Azure AD single sign-on with MaxxPoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="5510f-151">I hello Azure-portalen på hello **MaxxPoint** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5510f-151">In hello Azure portal, on hello **MaxxPoint** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5510f-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5510f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_300.png)

3. <span data-ttu-id="5510f-155">På hello **MaxxPoint domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge**, utan måste tooperform några steg.</span><span class="sxs-lookup"><span data-stu-id="5510f-155">On hello **MaxxPoint Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, no need tooperform any steps.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_02.png)
    
4. <span data-ttu-id="5510f-157">På hello **MaxxPoint domän och URL: er** om du vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5510f-157">On hello **MaxxPoint Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_03.png)

    <span data-ttu-id="5510f-159">a.</span><span class="sxs-lookup"><span data-stu-id="5510f-159">a.</span></span> <span data-ttu-id="5510f-160">Klicka på **visa avancerade inställningar för URL: en** alternativet</span><span class="sxs-lookup"><span data-stu-id="5510f-160">Click **Show advanced URL settings** option</span></span>

    <span data-ttu-id="5510f-161">b.</span><span class="sxs-lookup"><span data-stu-id="5510f-161">b.</span></span> <span data-ttu-id="5510f-162">I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span><span class="sxs-lookup"><span data-stu-id="5510f-162">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5510f-163">Observera att detta inte är hello verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="5510f-163">Please note that this is not hello real value.</span></span> <span data-ttu-id="5510f-164">Du har tooupdate värdet med hello faktiska inloggning URL.</span><span class="sxs-lookup"><span data-stu-id="5510f-164">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="5510f-165">Anropa MaxxPoint team i **888-728-0950** tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="5510f-165">Call MaxxPoint team on **888-728-0950** tooget this value.</span></span>

5. <span data-ttu-id="5510f-166">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="5510f-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_06.png) 

6. <span data-ttu-id="5510f-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5510f-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="5510f-170">tooget SSO konfigurerats för ditt program, ringer MaxxPoint supportgrupp på **888-728-0950** och de kommer hjälpa dig ytterligare på hur tooprovide dem hello hämtas **XML-Metadata för** fil.</span><span class="sxs-lookup"><span data-stu-id="5510f-170">tooget SSO configured for your application, call MaxxPoint support team on **888-728-0950** and they'll assist you further on how tooprovide them hello downloaded **Metadata XML** file.</span></span> 

> [!TIP]
> <span data-ttu-id="5510f-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="5510f-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5510f-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klickar du på **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="5510f-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5510f-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5510f-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5510f-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5510f-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="5510f-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5510f-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5510f-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5510f-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5510f-178">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5510f-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5510f-180">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="5510f-180">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5510f-182">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5510f-182">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5510f-184">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5510f-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5510f-186">a.</span><span class="sxs-lookup"><span data-stu-id="5510f-186">a.</span></span> <span data-ttu-id="5510f-187">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5510f-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5510f-188">b.</span><span class="sxs-lookup"><span data-stu-id="5510f-188">b.</span></span> <span data-ttu-id="5510f-189">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5510f-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5510f-190">c.</span><span class="sxs-lookup"><span data-stu-id="5510f-190">c.</span></span> <span data-ttu-id="5510f-191">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5510f-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5510f-192">d.</span><span class="sxs-lookup"><span data-stu-id="5510f-192">d.</span></span> <span data-ttu-id="5510f-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5510f-193">Click **Create**.</span></span> 

### <a name="creating-a-maxxpoint-test-user"></a><span data-ttu-id="5510f-194">Skapa en testanvändare MaxxPoint</span><span class="sxs-lookup"><span data-stu-id="5510f-194">Creating a MaxxPoint test user</span></span>

<span data-ttu-id="5510f-195">I det här avsnittet skapar du en användare som kallas Britta Simon i MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="5510f-195">In this section, you create a user called Britta Simon in MaxxPoint.</span></span> <span data-ttu-id="5510f-196">Kontakta supportteamet för MaxxPoint på **888-728-0950** tooadd hello användare i hello MaxxPoint program.</span><span class="sxs-lookup"><span data-stu-id="5510f-196">Please call MaxxPoint support team on **888-728-0950** tooadd hello users in hello MaxxPoint application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5510f-197">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5510f-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5510f-198">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooMaxxPoint sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5510f-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooMaxxPoint.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5510f-200">**tooassign Britta Simon tooMaxxPoint utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5510f-200">**tooassign Britta Simon tooMaxxPoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="5510f-201">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5510f-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5510f-203">Välj i listan med program hello **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="5510f-203">In hello applications list, select **MaxxPoint**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_50.png) 

3. <span data-ttu-id="5510f-205">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5510f-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5510f-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5510f-207">Click **Add** button.</span></span> <span data-ttu-id="5510f-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5510f-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5510f-210">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5510f-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5510f-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5510f-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5510f-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5510f-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5510f-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5510f-213">Testing single sign-on</span></span>

<span data-ttu-id="5510f-214">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5510f-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5510f-215">Du bör få automatiskt inloggade tooyour MaxxPoint programmet när du klickar på hello MaxxPoint panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5510f-215">When you click hello MaxxPoint tile in hello Access Panel, you should get automatically signed-on tooyour MaxxPoint application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="5510f-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5510f-216">Additional resources</span></span>

* [<span data-ttu-id="5510f-217">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5510f-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5510f-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5510f-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_203.png