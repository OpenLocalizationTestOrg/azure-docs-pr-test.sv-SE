---
title: "Självstudier: Azure Active Directory-integrering med Lecorpio | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Lecorpio."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 963eb36678c589f942f63c7ab555161255324717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lecorpio"></a><span data-ttu-id="59480-103">Självstudier: Azure Active Directory-integrering med Lecorpio</span><span class="sxs-lookup"><span data-stu-id="59480-103">Tutorial: Azure Active Directory integration with Lecorpio</span></span>

<span data-ttu-id="59480-104">I kursen får du lära dig hur toointegrate Lecorpio med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="59480-104">In this tutorial, you learn how toointegrate Lecorpio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="59480-105">Integrera Lecorpio med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="59480-105">Integrating Lecorpio with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="59480-106">Du kan styra i Azure AD som har åtkomst till tooLecorpio</span><span class="sxs-lookup"><span data-stu-id="59480-106">You can control in Azure AD who has access tooLecorpio</span></span>
- <span data-ttu-id="59480-107">Du kan aktivera din användare tooautomatically get inloggade tooLecorpio (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="59480-107">You can enable your users tooautomatically get signed-on tooLecorpio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="59480-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="59480-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="59480-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="59480-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59480-110">Krav</span><span class="sxs-lookup"><span data-stu-id="59480-110">Prerequisites</span></span>

<span data-ttu-id="59480-111">tooconfigure Azure AD-integrering med Lecorpio, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="59480-111">tooconfigure Azure AD integration with Lecorpio, you need hello following items:</span></span>

- <span data-ttu-id="59480-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="59480-112">An Azure AD subscription</span></span>
- <span data-ttu-id="59480-113">En Lecorpio enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="59480-113">A Lecorpio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="59480-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="59480-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="59480-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="59480-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="59480-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="59480-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="59480-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="59480-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="59480-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="59480-118">Scenario description</span></span>
<span data-ttu-id="59480-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="59480-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="59480-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="59480-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="59480-121">Att lägga till Lecorpio från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="59480-121">Adding Lecorpio from hello gallery</span></span>
2. <span data-ttu-id="59480-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="59480-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lecorpio-from-hello-gallery"></a><span data-ttu-id="59480-123">Att lägga till Lecorpio från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="59480-123">Adding Lecorpio from hello gallery</span></span>
<span data-ttu-id="59480-124">tooconfigure hello integrering av Lecorpio i Azure AD, behöver du tooadd Lecorpio hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="59480-124">tooconfigure hello integration of Lecorpio into Azure AD, you need tooadd Lecorpio from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="59480-125">**tooadd Lecorpio från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="59480-125">**tooadd Lecorpio from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="59480-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="59480-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="59480-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="59480-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="59480-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="59480-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="59480-131">Klicka på **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="59480-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="59480-133">Skriv i sökrutan hello **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="59480-133">In hello search box, type **Lecorpio**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_search.png)

5. <span data-ttu-id="59480-135">Markera hello resultat på panelen **Lecorpio**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="59480-135">In hello results panel, select **Lecorpio**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="59480-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="59480-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="59480-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Lecorpio baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="59480-138">In this section, you configure and test Azure AD single sign-on with Lecorpio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="59480-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Lecorpio är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="59480-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lecorpio is tooa user in Azure AD.</span></span> <span data-ttu-id="59480-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Lecorpio toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="59480-140">In other words, a link relationship between an Azure AD user and hello related user in Lecorpio needs toobe established.</span></span>

<span data-ttu-id="59480-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="59480-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Lecorpio.</span></span>

<span data-ttu-id="59480-142">tooconfigure och testa Azure AD enkel inloggning med Lecorpio, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="59480-142">tooconfigure and test Azure AD single sign-on with Lecorpio, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="59480-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="59480-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="59480-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="59480-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="59480-145">**[Skapa en testanvändare Lecorpio](#creating-a-lecorpio-test-user)**  -toohave en motsvarighet för Britta Simon i Lecorpio som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="59480-145">**[Creating a Lecorpio test user](#creating-a-lecorpio-test-user)** - toohave a counterpart of Britta Simon in Lecorpio that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="59480-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="59480-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="59480-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="59480-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="59480-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="59480-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="59480-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Lecorpio program.</span><span class="sxs-lookup"><span data-stu-id="59480-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lecorpio application.</span></span>

<span data-ttu-id="59480-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Lecorpio:**</span><span class="sxs-lookup"><span data-stu-id="59480-150">**tooconfigure Azure AD single sign-on with Lecorpio, perform hello following steps:**</span></span>

1. <span data-ttu-id="59480-151">I hello Azure-portalen på hello **Lecorpio** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="59480-151">In hello Azure portal, on hello **Lecorpio** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="59480-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="59480-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_samlbase.png)

3. <span data-ttu-id="59480-155">På hello **Lecorpio domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="59480-155">On hello **Lecorpio Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_url.png)

    <span data-ttu-id="59480-157">a.</span><span class="sxs-lookup"><span data-stu-id="59480-157">a.</span></span> <span data-ttu-id="59480-158">I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="59480-158">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    <span data-ttu-id="59480-159">b.</span><span class="sxs-lookup"><span data-stu-id="59480-159">b.</span></span> <span data-ttu-id="59480-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="59480-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="59480-161">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="59480-161">These values are not hello real.</span></span> <span data-ttu-id="59480-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="59480-162">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="59480-163">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="59480-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="59480-164">Kontakta [Lecorpio klienten supportteamet](mailto:info@lecorpio.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="59480-164">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="59480-165">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="59480-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_certificate.png) 

5. <span data-ttu-id="59480-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="59480-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="59480-169">tooconfigure enkel inloggning på **Lecorpio** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Lecorpio supportteamet](mailto:info@lecorpio.com).</span><span class="sxs-lookup"><span data-stu-id="59480-169">tooconfigure single sign-on on **Lecorpio** side, you need toosend hello downloaded **Metadata XML** too[Lecorpio support team](mailto:info@lecorpio.com).</span></span>

> [!TIP]
> <span data-ttu-id="59480-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="59480-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="59480-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="59480-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="59480-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="59480-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="59480-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="59480-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="59480-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="59480-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="59480-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="59480-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="59480-177">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="59480-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="59480-179">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="59480-179">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="59480-181">Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="59480-181">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="59480-183">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="59480-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="59480-185">a.</span><span class="sxs-lookup"><span data-stu-id="59480-185">a.</span></span> <span data-ttu-id="59480-186">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="59480-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="59480-187">b.</span><span class="sxs-lookup"><span data-stu-id="59480-187">b.</span></span> <span data-ttu-id="59480-188">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="59480-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="59480-189">c.</span><span class="sxs-lookup"><span data-stu-id="59480-189">c.</span></span> <span data-ttu-id="59480-190">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="59480-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="59480-191">d.</span><span class="sxs-lookup"><span data-stu-id="59480-191">d.</span></span> <span data-ttu-id="59480-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="59480-192">Click **Create**.</span></span>
 
### <a name="creating-a-lecorpio-test-user"></a><span data-ttu-id="59480-193">Skapa en testanvändare Lecorpio</span><span class="sxs-lookup"><span data-stu-id="59480-193">Creating a Lecorpio test user</span></span>

<span data-ttu-id="59480-194">I det här avsnittet skapar du en användare som kallas Britta Simon i Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="59480-194">In this section, you create a user called Britta Simon in Lecorpio.</span></span> 

<span data-ttu-id="59480-195">Kontakta [Lecorpio klienten supportteamet](mailto:info@lecorpio.com) tooadd hello användare i hello Lecorpio program.</span><span class="sxs-lookup"><span data-stu-id="59480-195">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) tooadd hello users in hello Lecorpio application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="59480-196">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="59480-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="59480-197">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLecorpio.</span><span class="sxs-lookup"><span data-stu-id="59480-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLecorpio.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="59480-199">**tooassign Britta Simon tooLecorpio utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="59480-199">**tooassign Britta Simon tooLecorpio, perform hello following steps:**</span></span>

1. <span data-ttu-id="59480-200">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="59480-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="59480-202">Välj i listan med program hello **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="59480-202">In hello applications list, select **Lecorpio**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_app.png) 

3. <span data-ttu-id="59480-204">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="59480-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="59480-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="59480-206">Click **Add** button.</span></span> <span data-ttu-id="59480-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="59480-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="59480-209">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="59480-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="59480-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="59480-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="59480-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="59480-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="59480-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="59480-212">Testing single sign-on</span></span>

<span data-ttu-id="59480-213">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="59480-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="59480-214">Du bör få automatiskt inloggade tooyour Lecorpio programmet när du klickar på hello Lecorpio panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="59480-214">When you click hello Lecorpio tile in hello Access Panel, you should get automatically signed-on tooyour Lecorpio application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59480-215">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="59480-215">Additional resources</span></span>

* [<span data-ttu-id="59480-216">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="59480-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="59480-217">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="59480-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_203.png

