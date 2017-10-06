---
title: "Självstudier: Azure Active Directory-integrering med LogicMonitor | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LogicMonitor."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: ea5cb8b574d763cb114286e3b2a5c94ab5546756
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a><span data-ttu-id="73bc4-103">Självstudier: Azure Active Directory-integrering med LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="73bc4-103">Tutorial: Azure Active Directory integration with LogicMonitor</span></span>

<span data-ttu-id="73bc4-104">I kursen får du lära dig hur toointegrate LogicMonitor med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="73bc4-104">In this tutorial, you learn how toointegrate LogicMonitor with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="73bc4-105">Integrera LogicMonitor med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="73bc4-105">Integrating LogicMonitor with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="73bc4-106">Du kan styra i Azure AD som har åtkomst till tooLogicMonitor</span><span class="sxs-lookup"><span data-stu-id="73bc4-106">You can control in Azure AD who has access tooLogicMonitor</span></span>
- <span data-ttu-id="73bc4-107">Du kan aktivera din användare tooautomatically get inloggade tooLogicMonitor (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="73bc4-107">You can enable your users tooautomatically get signed-on tooLogicMonitor (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="73bc4-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="73bc4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="73bc4-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="73bc4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73bc4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="73bc4-110">Prerequisites</span></span>

<span data-ttu-id="73bc4-111">tooconfigure Azure AD-integrering med LogicMonitor, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="73bc4-111">tooconfigure Azure AD integration with LogicMonitor, you need hello following items:</span></span>

- <span data-ttu-id="73bc4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="73bc4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="73bc4-113">En LogicMonitor enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="73bc4-113">A LogicMonitor single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="73bc4-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="73bc4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="73bc4-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="73bc4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="73bc4-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="73bc4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="73bc4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="73bc4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="73bc4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="73bc4-118">Scenario description</span></span>
<span data-ttu-id="73bc4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="73bc4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="73bc4-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="73bc4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="73bc4-121">Att lägga till LogicMonitor från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="73bc4-121">Adding LogicMonitor from hello gallery</span></span>
2. <span data-ttu-id="73bc4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73bc4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-logicmonitor-from-hello-gallery"></a><span data-ttu-id="73bc4-123">Att lägga till LogicMonitor från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="73bc4-123">Adding LogicMonitor from hello gallery</span></span>
<span data-ttu-id="73bc4-124">tooconfigure hello integrering av LogicMonitor i Azure AD, behöver du tooadd LogicMonitor hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="73bc4-124">tooconfigure hello integration of LogicMonitor into Azure AD, you need tooadd LogicMonitor from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="73bc4-125">**tooadd LogicMonitor från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="73bc4-125">**tooadd LogicMonitor from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="73bc4-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="73bc4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="73bc4-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="73bc4-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="73bc4-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73bc4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="73bc4-133">Skriv i sökrutan hello **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-133">In hello search box, type **LogicMonitor**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_search.png)

5. <span data-ttu-id="73bc4-135">Markera hello resultat på panelen **LogicMonitor**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="73bc4-135">In hello results panel, select **LogicMonitor**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="73bc4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73bc4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="73bc4-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LogicMonitor baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="73bc4-138">In this section, you configure and test Azure AD single sign-on with LogicMonitor based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="73bc4-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LogicMonitor är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="73bc4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LogicMonitor is tooa user in Azure AD.</span></span> <span data-ttu-id="73bc4-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LogicMonitor toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="73bc4-140">In other words, a link relationship between an Azure AD user and hello related user in LogicMonitor needs toobe established.</span></span>

<span data-ttu-id="73bc4-141">I LogicMonitor, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="73bc4-141">In LogicMonitor, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="73bc4-142">tooconfigure och testa Azure AD enkel inloggning med LogicMonitor, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="73bc4-142">tooconfigure and test Azure AD single sign-on with LogicMonitor, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="73bc4-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="73bc4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="73bc4-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="73bc4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="73bc4-145">**[Skapa en testanvändare LogicMonitor](#creating-a-logicmonitor-test-user)**  -toohave en motsvarighet för Britta Simon i LogicMonitor som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="73bc4-145">**[Creating a LogicMonitor test user](#creating-a-logicmonitor-test-user)** - toohave a counterpart of Britta Simon in LogicMonitor that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="73bc4-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="73bc4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="73bc4-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="73bc4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="73bc4-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73bc4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="73bc4-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt LogicMonitor program.</span><span class="sxs-lookup"><span data-stu-id="73bc4-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LogicMonitor application.</span></span>

<span data-ttu-id="73bc4-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LogicMonitor:**</span><span class="sxs-lookup"><span data-stu-id="73bc4-150">**tooconfigure Azure AD single sign-on with LogicMonitor, perform hello following steps:**</span></span>

1. <span data-ttu-id="73bc4-151">I hello Azure-portalen på hello **LogicMonitor** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-151">In hello Azure portal, on hello **LogicMonitor** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="73bc4-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="73bc4-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_samlbase.png)

3. <span data-ttu-id="73bc4-155">På hello **LogicMonitor domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="73bc4-155">On hello **LogicMonitor Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_url.png)

    <span data-ttu-id="73bc4-157">a.</span><span class="sxs-lookup"><span data-stu-id="73bc4-157">a.</span></span> <span data-ttu-id="73bc4-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="73bc4-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    <span data-ttu-id="73bc4-159">b.</span><span class="sxs-lookup"><span data-stu-id="73bc4-159">b.</span></span> <span data-ttu-id="73bc4-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="73bc4-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="73bc4-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="73bc4-161">These values are not real.</span></span> <span data-ttu-id="73bc4-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="73bc4-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="73bc4-163">Kontakta [LogicMonitor klienten supportteamet](https://www.logicmonitor.com/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="73bc4-163">Contact [LogicMonitor Client support team](https://www.logicmonitor.com/contact/) tooget these values.</span></span> 
 


4. <span data-ttu-id="73bc4-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="73bc4-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_certificate.png) 

5. <span data-ttu-id="73bc4-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="73bc4-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="73bc4-168">Logga in tooyour **LogicMonitor** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="73bc4-168">Log in tooyour **LogicMonitor** company site as an administrator.</span></span>

7. <span data-ttu-id="73bc4-169">Hello-menyn överst hello **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-169">In hello menu on hello top, click **Settings**.</span></span>
   
   <span data-ttu-id="73bc4-170">![Inställningar för](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="73bc4-170">![Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "Settings")</span></span>

8. <span data-ttu-id="73bc4-171">I hello navigering bat hello vänster, klickar du på **enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="73bc4-171">In hello navigation bat on hello left side, click **Single Sign On**</span></span>
   
   <span data-ttu-id="73bc4-172">![Enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="73bc4-172">![Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "Single Sign-On")</span></span>

9. <span data-ttu-id="73bc4-173">I hello **inställningar för enkel inloggning (SSO)** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="73bc4-173">In hello **Single Sign-on (SSO) settings** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="73bc4-174">![Enkel inloggning inställningar](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="73bc4-174">![Single Sign-On Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="73bc4-175">a.</span><span class="sxs-lookup"><span data-stu-id="73bc4-175">a.</span></span> <span data-ttu-id="73bc4-176">Välj **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-176">Select **Enable Single Sign-on**.</span></span>

   <span data-ttu-id="73bc4-177">b.</span><span class="sxs-lookup"><span data-stu-id="73bc4-177">b.</span></span> <span data-ttu-id="73bc4-178">Som **standard rolltilldelning**väljer **readonly**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-178">As **Default Role Assignment**, select **readonly**.</span></span>
   
   <span data-ttu-id="73bc4-179">c.</span><span class="sxs-lookup"><span data-stu-id="73bc4-179">c.</span></span> <span data-ttu-id="73bc4-180">Öppna hello hämtas metadata-filen i anteckningar och klistra in innehållet i filen som hello i hello **identitet providern Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="73bc4-180">Open hello downloaded metadata file in notepad, and then paste content of hello file into hello **Identity Provider Metadata** textbox.</span></span>
   
   <span data-ttu-id="73bc4-181">d.</span><span class="sxs-lookup"><span data-stu-id="73bc4-181">d.</span></span> <span data-ttu-id="73bc4-182">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="73bc4-183">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="73bc4-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="73bc4-184">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="73bc4-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="73bc4-185">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="73bc4-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="73bc4-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="73bc4-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="73bc4-187">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="73bc4-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="73bc4-189">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="73bc4-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="73bc4-190">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="73bc4-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="73bc4-192">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="73bc4-194">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73bc4-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="73bc4-196">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="73bc4-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="73bc4-198">a.</span><span class="sxs-lookup"><span data-stu-id="73bc4-198">a.</span></span> <span data-ttu-id="73bc4-199">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="73bc4-200">b.</span><span class="sxs-lookup"><span data-stu-id="73bc4-200">b.</span></span> <span data-ttu-id="73bc4-201">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="73bc4-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="73bc4-202">c.</span><span class="sxs-lookup"><span data-stu-id="73bc4-202">c.</span></span> <span data-ttu-id="73bc4-203">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="73bc4-204">d.</span><span class="sxs-lookup"><span data-stu-id="73bc4-204">d.</span></span> <span data-ttu-id="73bc4-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-205">Click **Create**.</span></span>
 
### <a name="creating-a-logicmonitor-test-user"></a><span data-ttu-id="73bc4-206">Skapa en testanvändare LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="73bc4-206">Creating a LogicMonitor test user</span></span>

<span data-ttu-id="73bc4-207">AAD användare toobe kan toosign i, måste de vara etablerade toohello LogicMonitor program med hjälp av deras användarnamn för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="73bc4-207">For AAD users toobe able toosign in, they must be provisioned toohello LogicMonitor application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="73bc4-208">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="73bc4-208">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="73bc4-209">Logga in tooyour LogicMonitor företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="73bc4-209">Log in tooyour LogicMonitor company site as an administrator.</span></span>

2. <span data-ttu-id="73bc4-210">Hello-menyn överst hello **inställningar**, och klicka sedan på **roller och användare**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-210">In hello menu on hello top, click **Settings**, and then click **Roles and Users**.</span></span>
   
   <span data-ttu-id="73bc4-211">![Roller och användare](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "roller och användare")</span><span class="sxs-lookup"><span data-stu-id="73bc4-211">![Roles and Users](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "Roles and Users")</span></span>

3. <span data-ttu-id="73bc4-212">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-212">Click **Add**.</span></span>

4. <span data-ttu-id="73bc4-213">I hello **Lägg till ett konto** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="73bc4-213">In hello **Add an account** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="73bc4-214">![Lägg till ett konto](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "Lägg till ett konto")</span><span class="sxs-lookup"><span data-stu-id="73bc4-214">![Add an account](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "Add an account")</span></span>
   
   <span data-ttu-id="73bc4-215">a.</span><span class="sxs-lookup"><span data-stu-id="73bc4-215">a.</span></span> <span data-ttu-id="73bc4-216">Typen hello **användarnamn**, **e-post**, **lösenord**, och **ange lösenord** värdena för hello Azure Active Directory-användare som du vill tooprovision i hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="73bc4-216">Type hello **Username**, **Email**, **Password**, and **Retype password** values of hello Azure Active Directory user you want tooprovision into hello related textboxes.</span></span>
   
   <span data-ttu-id="73bc4-217">b.</span><span class="sxs-lookup"><span data-stu-id="73bc4-217">b.</span></span> <span data-ttu-id="73bc4-218">Välj **roller**, **Visa behörigheter**, och hello **Status**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-218">Select **Roles**, **View Permissions**, and hello **Status**.</span></span>
   
   <span data-ttu-id="73bc4-219">c.</span><span class="sxs-lookup"><span data-stu-id="73bc4-219">c.</span></span> <span data-ttu-id="73bc4-220">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-220">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="73bc4-221">Du kan använda något annat LogicMonitor användarens konto skapas verktyg eller API: er som tillhandahålls av LogicMonitor tooprovision Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="73bc4-221">You can use any other LogicMonitor user account creation tools or APIs provided by LogicMonitor tooprovision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="73bc4-222">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="73bc4-222">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="73bc4-223">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLogicMonitor.</span><span class="sxs-lookup"><span data-stu-id="73bc4-223">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLogicMonitor.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="73bc4-225">**tooassign Britta Simon tooLogicMonitor utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="73bc4-225">**tooassign Britta Simon tooLogicMonitor, perform hello following steps:**</span></span>

1. <span data-ttu-id="73bc4-226">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-226">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="73bc4-228">Välj i listan med program hello **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-228">In hello applications list, select **LogicMonitor**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_app.png) 

3. <span data-ttu-id="73bc4-230">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="73bc4-230">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="73bc4-232">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="73bc4-232">Click **Add** button.</span></span> <span data-ttu-id="73bc4-233">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73bc4-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="73bc4-235">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="73bc4-235">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="73bc4-236">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73bc4-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="73bc4-237">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73bc4-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="73bc4-238">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73bc4-238">Testing single sign-on</span></span>

<span data-ttu-id="73bc4-239">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="73bc4-239">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="73bc4-240">Du bör få automatiskt inloggade tooyour LogicMonitor programmet när du klickar på hello LogicMonitor panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="73bc4-240">When you click hello LogicMonitor tile in hello Access Panel, you should get automatically signed-on tooyour LogicMonitor application.</span></span>
<span data-ttu-id="73bc4-241">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="73bc4-241">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="73bc4-242">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="73bc4-242">Additional resources</span></span>

* [<span data-ttu-id="73bc4-243">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="73bc4-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="73bc4-244">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="73bc4-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_203.png

