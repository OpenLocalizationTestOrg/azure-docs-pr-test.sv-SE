---
title: "Självstudier: Azure Active Directory-integrering med Pacific tidrapporter | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Pacific tidrapporter."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e546e8ba-821a-4942-9545-c84b0670beab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 5fd57ff78a3a64c135f2de9895f4643b39e33bb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pacific-timesheet"></a><span data-ttu-id="e15ed-103">Självstudier: Azure Active Directory-integrering med Pacific tidrapporter</span><span class="sxs-lookup"><span data-stu-id="e15ed-103">Tutorial: Azure Active Directory integration with Pacific Timesheet</span></span>

<span data-ttu-id="e15ed-104">I kursen får du lära dig hur toointegrate Pacific tidrapporter med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e15ed-104">In this tutorial, you learn how toointegrate Pacific Timesheet with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e15ed-105">Integrera Pacific tidrapporter med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e15ed-105">Integrating Pacific Timesheet with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e15ed-106">Du kan styra i Azure AD som har åtkomst tooPacific tidrapporter</span><span class="sxs-lookup"><span data-stu-id="e15ed-106">You can control in Azure AD who has access tooPacific Timesheet</span></span>
- <span data-ttu-id="e15ed-107">Du kan aktivera din användare tooautomatically get inloggade tooPacific tidrapporter (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e15ed-107">You can enable your users tooautomatically get signed-on tooPacific Timesheet (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e15ed-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e15ed-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e15ed-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e15ed-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e15ed-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e15ed-110">Prerequisites</span></span>

<span data-ttu-id="e15ed-111">tooconfigure Azure AD-integrering med Pacific tidrapporter måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e15ed-111">tooconfigure Azure AD integration with Pacific Timesheet, you need hello following items:</span></span>

- <span data-ttu-id="e15ed-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e15ed-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e15ed-113">En Pacific tidrapporter enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e15ed-113">A Pacific Timesheet single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e15ed-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e15ed-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e15ed-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e15ed-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e15ed-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e15ed-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e15ed-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e15ed-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e15ed-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e15ed-118">Scenario description</span></span>
<span data-ttu-id="e15ed-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e15ed-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e15ed-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e15ed-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e15ed-121">Att lägga till Pacific tidrapporter från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e15ed-121">Adding Pacific Timesheet from hello gallery</span></span>
2. <span data-ttu-id="e15ed-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e15ed-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pacific-timesheet-from-hello-gallery"></a><span data-ttu-id="e15ed-123">Att lägga till Pacific tidrapporter från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e15ed-123">Adding Pacific Timesheet from hello gallery</span></span>
<span data-ttu-id="e15ed-124">tooconfigure hello integrering av Pacific tidrapporter i Azure AD, behöver du tooadd Pacific tidrapporter hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e15ed-124">tooconfigure hello integration of Pacific Timesheet into Azure AD, you need tooadd Pacific Timesheet from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e15ed-125">**tooadd Pacific tidrapporter från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e15ed-125">**tooadd Pacific Timesheet from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e15ed-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e15ed-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e15ed-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e15ed-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e15ed-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15ed-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e15ed-133">Skriv i sökrutan hello **Pacific tidrapporter**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-133">In hello search box, type **Pacific Timesheet**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_search.png)

5. <span data-ttu-id="e15ed-135">Markera hello resultat på panelen **Pacific tidrapporter**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e15ed-135">In hello results panel, select **Pacific Timesheet**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e15ed-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e15ed-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e15ed-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Pacific tidrapporter baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e15ed-138">In this section, you configure and test Azure AD single sign-on with Pacific Timesheet based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e15ed-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Pacific tidrapporter är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e15ed-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pacific Timesheet is tooa user in Azure AD.</span></span> <span data-ttu-id="e15ed-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Pacific tidrapporter toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e15ed-140">In other words, a link relationship between an Azure AD user and hello related user in Pacific Timesheet needs toobe established.</span></span>

<span data-ttu-id="e15ed-141">Tilldela hello värdet hello i Pacific tidrapporter **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e15ed-141">In Pacific Timesheet, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e15ed-142">tooconfigure och testa Azure AD enkel inloggning med Pacific tidrapporter, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e15ed-142">tooconfigure and test Azure AD single sign-on with Pacific Timesheet, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e15ed-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e15ed-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e15ed-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e15ed-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e15ed-145">**[Skapa en testanvändare Pacific tidrapporter](#creating-a-pacific-timesheet-test-user)**  -toohave en motsvarighet för Britta Simon i Pacific tidrapporter som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e15ed-145">**[Creating a Pacific Timesheet test user](#creating-a-pacific-timesheet-test-user)** - toohave a counterpart of Britta Simon in Pacific Timesheet that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e15ed-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e15ed-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e15ed-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e15ed-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e15ed-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e15ed-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e15ed-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Pacific tidrapporter.</span><span class="sxs-lookup"><span data-stu-id="e15ed-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Pacific Timesheet application.</span></span>

<span data-ttu-id="e15ed-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Pacific tidrapporter:**</span><span class="sxs-lookup"><span data-stu-id="e15ed-150">**tooconfigure Azure AD single sign-on with Pacific Timesheet, perform hello following steps:**</span></span>

1. <span data-ttu-id="e15ed-151">I hello Azure-portalen på hello **Pacific tidrapporter** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-151">In hello Azure portal, on hello **Pacific Timesheet** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e15ed-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e15ed-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_samlbase.png)

3. <span data-ttu-id="e15ed-155">På hello **Pacific tidrapporter domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e15ed-155">On hello **Pacific Timesheet Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_url.png)

    <span data-ttu-id="e15ed-157">a.</span><span class="sxs-lookup"><span data-stu-id="e15ed-157">a.</span></span> <span data-ttu-id="e15ed-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span><span class="sxs-lookup"><span data-stu-id="e15ed-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span></span>

    <span data-ttu-id="e15ed-159">b.</span><span class="sxs-lookup"><span data-stu-id="e15ed-159">b.</span></span> <span data-ttu-id="e15ed-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span><span class="sxs-lookup"><span data-stu-id="e15ed-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e15ed-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e15ed-161">These values are not real.</span></span> <span data-ttu-id="e15ed-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="e15ed-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="e15ed-163">Kontakta [Pacific tidrapporter supportteamet](http://www.pacifictimesheet.com/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e15ed-163">Contact [Pacific Timesheet support team](http://www.pacifictimesheet.com/support) tooget these values.</span></span>
 
4. <span data-ttu-id="e15ed-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="e15ed-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_certificate.png) 

5. <span data-ttu-id="e15ed-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e15ed-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e15ed-168">På hello **Pacific tidrapporter Configuration** klickar du på **konfigurera Pacific tidrapporter** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e15ed-168">On hello **Pacific Timesheet Configuration** section, click **Configure Pacific Timesheet** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e15ed-169">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="e15ed-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_configure.png) 

7. <span data-ttu-id="e15ed-171">tooconfigure enkel inloggning på **Pacific tidrapporter** sida, behöver du toosend hello hämtas **certifikat (Base64)**, **SAML inloggning tjänst-URL för enkel**, och  **SAML enhets-ID** för[Pacific tidrapporter supportteamet](http://www.pacifictimesheet.com/support).</span><span class="sxs-lookup"><span data-stu-id="e15ed-171">tooconfigure single sign-on on **Pacific Timesheet** side, you need toosend hello downloaded **Certificate (Base64)**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** too[Pacific Timesheet support team](http://www.pacifictimesheet.com/support).</span></span> <span data-ttu-id="e15ed-172">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="e15ed-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e15ed-173">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="e15ed-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e15ed-174">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="e15ed-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e15ed-175">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e15ed-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e15ed-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e15ed-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="e15ed-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e15ed-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e15ed-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e15ed-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e15ed-180">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e15ed-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e15ed-182">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e15ed-184">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15ed-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e15ed-186">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e15ed-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e15ed-188">a.</span><span class="sxs-lookup"><span data-stu-id="e15ed-188">a.</span></span> <span data-ttu-id="e15ed-189">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e15ed-190">b.</span><span class="sxs-lookup"><span data-stu-id="e15ed-190">b.</span></span> <span data-ttu-id="e15ed-191">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e15ed-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e15ed-192">c.</span><span class="sxs-lookup"><span data-stu-id="e15ed-192">c.</span></span> <span data-ttu-id="e15ed-193">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e15ed-194">d.</span><span class="sxs-lookup"><span data-stu-id="e15ed-194">d.</span></span> <span data-ttu-id="e15ed-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-195">Click **Create**.</span></span>
 
### <a name="creating-a-pacific-timesheet-test-user"></a><span data-ttu-id="e15ed-196">Skapa en testanvändare Pacific tidrapporter</span><span class="sxs-lookup"><span data-stu-id="e15ed-196">Creating a Pacific Timesheet test user</span></span>

<span data-ttu-id="e15ed-197">I det här avsnittet skapar du en användare som kallas Britta Simon i Pacific tidrapporter.</span><span class="sxs-lookup"><span data-stu-id="e15ed-197">In this section, you create a user called Britta Simon in Pacific Timesheet.</span></span> <span data-ttu-id="e15ed-198">Arbeta med [Pacific tidrapporter supportteamet](http://www.pacifictimesheet.com/support) toocreate en användare i hello program.</span><span class="sxs-lookup"><span data-stu-id="e15ed-198">Work with [Pacific Timesheet support team](http://www.pacifictimesheet.com/support) toocreate a user in hello application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e15ed-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e15ed-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e15ed-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPacific tidrapporter.</span><span class="sxs-lookup"><span data-stu-id="e15ed-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPacific Timesheet.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e15ed-202">**tooassign Britta Simon tooPacific tidrapporter, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="e15ed-202">**tooassign Britta Simon tooPacific Timesheet, perform hello following steps:**</span></span>

1. <span data-ttu-id="e15ed-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e15ed-205">Välj i listan med program hello **Pacific tidrapporter**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-205">In hello applications list, select **Pacific Timesheet**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_app.png) 

3. <span data-ttu-id="e15ed-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e15ed-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e15ed-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e15ed-209">Click **Add** button.</span></span> <span data-ttu-id="e15ed-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15ed-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e15ed-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e15ed-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e15ed-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15ed-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e15ed-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15ed-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e15ed-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e15ed-215">Testing single sign-on</span></span>

<span data-ttu-id="e15ed-216">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e15ed-216">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e15ed-217">Du bör få automatiskt inloggade tooyour Pacific tidrapporter programmet när du klickar på hello Pacific tidrapporter panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e15ed-217">When you click hello Pacific Timesheet tile in hello Access Panel, you should get automatically signed-on tooyour Pacific Timesheet application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e15ed-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e15ed-218">Additional resources</span></span>

* [<span data-ttu-id="e15ed-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e15ed-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e15ed-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e15ed-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_203.png

