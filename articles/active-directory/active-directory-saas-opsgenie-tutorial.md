---
title: "Självstudier: Azure Active Directory-integrering med OpsGenie | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och OpsGenie."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 50d31c234fb9716d05d681b9bc4164740a3a662b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a><span data-ttu-id="53075-103">Självstudier: Azure Active Directory-integrering med OpsGenie</span><span class="sxs-lookup"><span data-stu-id="53075-103">Tutorial: Azure Active Directory integration with OpsGenie</span></span>

<span data-ttu-id="53075-104">I kursen får du lära dig hur toointegrate OpsGenie med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="53075-104">In this tutorial, you learn how toointegrate OpsGenie with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="53075-105">Integrera OpsGenie med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="53075-105">Integrating OpsGenie with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="53075-106">Du kan styra i Azure AD som har åtkomst till tooOpsGenie</span><span class="sxs-lookup"><span data-stu-id="53075-106">You can control in Azure AD who has access tooOpsGenie</span></span>
- <span data-ttu-id="53075-107">Du kan aktivera din användare tooautomatically get inloggade tooOpsGenie (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="53075-107">You can enable your users tooautomatically get signed-on tooOpsGenie (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="53075-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="53075-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="53075-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="53075-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53075-110">Krav</span><span class="sxs-lookup"><span data-stu-id="53075-110">Prerequisites</span></span>

<span data-ttu-id="53075-111">tooconfigure Azure AD-integrering med OpsGenie, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="53075-111">tooconfigure Azure AD integration with OpsGenie, you need hello following items:</span></span>

- <span data-ttu-id="53075-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="53075-112">An Azure AD subscription</span></span>
- <span data-ttu-id="53075-113">En OpsGenie enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="53075-113">A OpsGenie single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="53075-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="53075-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="53075-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="53075-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="53075-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="53075-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="53075-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="53075-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="53075-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="53075-118">Scenario description</span></span>
<span data-ttu-id="53075-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="53075-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="53075-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="53075-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="53075-121">Att lägga till OpsGenie från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="53075-121">Adding OpsGenie from hello gallery</span></span>
2. <span data-ttu-id="53075-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="53075-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-opsgenie-from-hello-gallery"></a><span data-ttu-id="53075-123">Att lägga till OpsGenie från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="53075-123">Adding OpsGenie from hello gallery</span></span>
<span data-ttu-id="53075-124">tooconfigure hello integrering av OpsGenie i Azure AD, behöver du tooadd OpsGenie hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="53075-124">tooconfigure hello integration of OpsGenie into Azure AD, you need tooadd OpsGenie from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="53075-125">**tooadd OpsGenie från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="53075-125">**tooadd OpsGenie from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="53075-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="53075-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="53075-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="53075-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="53075-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="53075-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="53075-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="53075-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="53075-133">Skriv i sökrutan hello **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="53075-133">In hello search box, type **OpsGenie**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_search.png)

5. <span data-ttu-id="53075-135">Markera hello resultat på panelen **OpsGenie**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="53075-135">In hello results panel, select **OpsGenie**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="53075-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="53075-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="53075-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med OpsGenie baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="53075-138">In this section, you configure and test Azure AD single sign-on with OpsGenie based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="53075-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i OpsGenie är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="53075-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in OpsGenie is tooa user in Azure AD.</span></span> <span data-ttu-id="53075-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i OpsGenie toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="53075-140">In other words, a link relationship between an Azure AD user and hello related user in OpsGenie needs toobe established.</span></span>

<span data-ttu-id="53075-141">I OpsGenie, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="53075-141">In OpsGenie, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="53075-142">tooconfigure och testa Azure AD enkel inloggning med OpsGenie, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="53075-142">tooconfigure and test Azure AD single sign-on with OpsGenie, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="53075-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="53075-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="53075-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="53075-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="53075-145">**[Skapa en testanvändare OpsGenie](#creating-a-opsgenie-test-user)**  -toohave en motsvarighet för Britta Simon i OpsGenie som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="53075-145">**[Creating a OpsGenie test user](#creating-a-opsgenie-test-user)** - toohave a counterpart of Britta Simon in OpsGenie that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="53075-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="53075-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="53075-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="53075-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="53075-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="53075-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="53075-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt OpsGenie program.</span><span class="sxs-lookup"><span data-stu-id="53075-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your OpsGenie application.</span></span>

<span data-ttu-id="53075-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med OpsGenie:**</span><span class="sxs-lookup"><span data-stu-id="53075-150">**tooconfigure Azure AD single sign-on with OpsGenie, perform hello following steps:**</span></span>

1. <span data-ttu-id="53075-151">I hello Azure-portalen på hello **OpsGenie** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="53075-151">In hello Azure portal, on hello **OpsGenie** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="53075-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="53075-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

3. <span data-ttu-id="53075-155">På hello **OpsGenie domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="53075-155">On hello **OpsGenie Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_url.png)

    <span data-ttu-id="53075-157">I hello **inloggnings-URL** textruta typen hello URL:`https://app.opsgenie.com/auth/login`</span><span class="sxs-lookup"><span data-stu-id="53075-157">In hello **Sign-on URL** textbox, type hello URL: `https://app.opsgenie.com/auth/login`</span></span>

4. <span data-ttu-id="53075-158">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="53075-158">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_certificate.png) 

5. <span data-ttu-id="53075-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="53075-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="53075-162">På hello **OpsGenie Configuration** klickar du på **konfigurera OpsGenie** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="53075-162">On hello **OpsGenie Configuration** section, click **Configure OpsGenie** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="53075-163">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="53075-163">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_configure.png) 

7. <span data-ttu-id="53075-165">Öppna en annan instans för webbläsaren och logga sedan in tooOpsGenie som administratör.</span><span class="sxs-lookup"><span data-stu-id="53075-165">Open another browser instance, and then log-in tooOpsGenie as an administrator.</span></span>

8. <span data-ttu-id="53075-166">Klicka på **inställningar**, och klicka sedan på hello **enkel inloggning** fliken.</span><span class="sxs-lookup"><span data-stu-id="53075-166">Click **Settings**, and then click hello **Single Sign On** tab.</span></span>
   
    ![OpsGenie enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png)

9. <span data-ttu-id="53075-168">Välj tooenable SSO **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="53075-168">tooenable SSO, select **Enabled**.</span></span>
   
    ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 

10. <span data-ttu-id="53075-170">I hello **Provider** klickar du på hello **Azure Active Directory** fliken.</span><span class="sxs-lookup"><span data-stu-id="53075-170">In hello **Provider** section, click hello **Azure Active Directory** tab.</span></span>
   
    ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 

11. <span data-ttu-id="53075-172">Utför följande hello hello Azure Active Directory dialogrutan på sidan:</span><span class="sxs-lookup"><span data-stu-id="53075-172">On hello Azure Active Directory dialog page, perform hello following steps:</span></span>
   
    ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    <span data-ttu-id="53075-174">a.</span><span class="sxs-lookup"><span data-stu-id="53075-174">a.</span></span> <span data-ttu-id="53075-175">Klistra in **enkel inloggning på Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **SAML 2.0 Endpoint** textruta.</span><span class="sxs-lookup"><span data-stu-id="53075-175">Paste **Single Sign On Service URL**, which you have copied from hello Azure portal into hello **SAML 2.0 Endpoint** textbox.</span></span>
    
    <span data-ttu-id="53075-176">b.</span><span class="sxs-lookup"><span data-stu-id="53075-176">b.</span></span> <span data-ttu-id="53075-177">Öppna din hämtade Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den i hello **X.500 certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="53075-177">Open your downloaded base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it into hello **X.500 Certificate** textbox.</span></span>
    
    <span data-ttu-id="53075-178">c.</span><span class="sxs-lookup"><span data-stu-id="53075-178">c.</span></span> <span data-ttu-id="53075-179">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="53075-179">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="53075-180">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="53075-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="53075-181">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="53075-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="53075-182">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="53075-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="53075-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="53075-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="53075-184">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="53075-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="53075-186">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="53075-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="53075-187">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="53075-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="53075-189">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="53075-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="53075-191">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="53075-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="53075-193">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="53075-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="53075-195">a.</span><span class="sxs-lookup"><span data-stu-id="53075-195">a.</span></span> <span data-ttu-id="53075-196">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="53075-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="53075-197">b.</span><span class="sxs-lookup"><span data-stu-id="53075-197">b.</span></span> <span data-ttu-id="53075-198">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="53075-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="53075-199">c.</span><span class="sxs-lookup"><span data-stu-id="53075-199">c.</span></span> <span data-ttu-id="53075-200">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="53075-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="53075-201">d.</span><span class="sxs-lookup"><span data-stu-id="53075-201">d.</span></span> <span data-ttu-id="53075-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="53075-202">Click **Create**.</span></span>
 
### <a name="creating-a-opsgenie-test-user"></a><span data-ttu-id="53075-203">Skapa en testanvändare OpsGenie</span><span class="sxs-lookup"><span data-stu-id="53075-203">Creating a OpsGenie test user</span></span>

<span data-ttu-id="53075-204">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="53075-204">hello objective of this section is toocreate a user called Britta Simon in OpsGenie.</span></span> 

1. <span data-ttu-id="53075-205">Logga in på din OpsGenie klient som en administratör i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="53075-205">In a web browser window, log into your OpsGenie tenant as an administrator.</span></span>

2. <span data-ttu-id="53075-206">Navigera tooUsers listan genom att klicka på **användaren** i den vänstra panelen.</span><span class="sxs-lookup"><span data-stu-id="53075-206">Navigate tooUsers list by clicking **User** in left panel.</span></span>
   
   ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. <span data-ttu-id="53075-208">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="53075-208">Click **Add User**.</span></span>

4. <span data-ttu-id="53075-209">På hello **Lägg till användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="53075-209">On hello **Add User** dialog, perform hello following steps:</span></span>
   
   ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   <span data-ttu-id="53075-211">a.</span><span class="sxs-lookup"><span data-stu-id="53075-211">a.</span></span> <span data-ttu-id="53075-212">I hello **e-post** textruta typen hello e-postadressen för BrittaSimon åtgärdas i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="53075-212">In hello **Email** textbox, type hello email address of BrittaSimon addressed in Azure Active Directory.</span></span>
   
   <span data-ttu-id="53075-213">b.</span><span class="sxs-lookup"><span data-stu-id="53075-213">b.</span></span> <span data-ttu-id="53075-214">I hello **fullständiga namn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="53075-214">In hello **Full Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="53075-215">c.</span><span class="sxs-lookup"><span data-stu-id="53075-215">c.</span></span> <span data-ttu-id="53075-216">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="53075-216">Click **Save**.</span></span> 

>[!NOTE]
><span data-ttu-id="53075-217">Britta får ett e-postmeddelande med instruktioner för att konfigurera sin profil.</span><span class="sxs-lookup"><span data-stu-id="53075-217">Britta gets an email with instructions for setting up her profile.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="53075-218">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="53075-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="53075-219">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooOpsGenie.</span><span class="sxs-lookup"><span data-stu-id="53075-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOpsGenie.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="53075-221">**tooassign Britta Simon tooOpsGenie utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="53075-221">**tooassign Britta Simon tooOpsGenie, perform hello following steps:**</span></span>

1. <span data-ttu-id="53075-222">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="53075-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="53075-224">Välj i listan med program hello **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="53075-224">In hello applications list, select **OpsGenie**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_app.png) 

3. <span data-ttu-id="53075-226">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="53075-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="53075-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="53075-228">Click **Add** button.</span></span> <span data-ttu-id="53075-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="53075-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="53075-231">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="53075-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="53075-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="53075-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="53075-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="53075-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="53075-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="53075-234">Testing single sign-on</span></span>

<span data-ttu-id="53075-235">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="53075-235">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="53075-236">Du bör få automatiskt inloggade tooyour OpsGenie programmet när du klickar på hello OpsGenie panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="53075-236">When you click hello OpsGenie tile in hello Access Panel, you should get automatically signed-on tooyour OpsGenie application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53075-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="53075-237">Additional resources</span></span>

* [<span data-ttu-id="53075-238">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="53075-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="53075-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="53075-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png

