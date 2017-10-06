---
title: "Självstudier: Azure Active Directory-integrering med Birst flexibel Företagsanalys | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Birst flexibel Företagsanalys."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 677183b1-5348-4302-88cc-5c8ab63a3c6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: f007edcec0fb8ece215ab69f7ec7ca59ca34bddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-birst-agile-business-analytics"></a><span data-ttu-id="ddd95-103">Självstudier: Azure Active Directory-integrering med Birst flexibel Företagsanalys</span><span class="sxs-lookup"><span data-stu-id="ddd95-103">Tutorial: Azure Active Directory integration with Birst Agile Business Analytics</span></span>

<span data-ttu-id="ddd95-104">I kursen får du lära dig hur toointegrate Birst flexibel Företagsanalys med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ddd95-104">In this tutorial, you learn how toointegrate Birst Agile Business Analytics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ddd95-105">Integrera Birst flexibel Företagsanalys med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ddd95-105">Integrating Birst Agile Business Analytics with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ddd95-106">Du kan styra i Azure AD som har åtkomst tooBirst flexibel Företagsanalys</span><span class="sxs-lookup"><span data-stu-id="ddd95-106">You can control in Azure AD who has access tooBirst Agile Business Analytics</span></span>
- <span data-ttu-id="ddd95-107">Du kan aktivera din användare tooautomatically get inloggade tooBirst flexibel Företagsanalys (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ddd95-107">You can enable your users tooautomatically get signed-on tooBirst Agile Business Analytics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ddd95-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ddd95-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ddd95-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ddd95-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ddd95-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ddd95-110">Prerequisites</span></span>

<span data-ttu-id="ddd95-111">tooconfigure Azure AD-integrering med Birst flexibel Företagsanalys måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ddd95-111">tooconfigure Azure AD integration with Birst Agile Business Analytics, you need hello following items:</span></span>

- <span data-ttu-id="ddd95-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ddd95-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ddd95-113">En Birst flexibel Företagsanalys enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="ddd95-113">A Birst Agile Business Analytics single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ddd95-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ddd95-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ddd95-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ddd95-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ddd95-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ddd95-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ddd95-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ddd95-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ddd95-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ddd95-118">Scenario description</span></span>
<span data-ttu-id="ddd95-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ddd95-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ddd95-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ddd95-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ddd95-121">Att lägga till Birst flexibel Företagsanalys från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ddd95-121">Adding Birst Agile Business Analytics from hello gallery</span></span>
2. <span data-ttu-id="ddd95-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ddd95-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-birst-agile-business-analytics-from-hello-gallery"></a><span data-ttu-id="ddd95-123">Att lägga till Birst flexibel Företagsanalys från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ddd95-123">Adding Birst Agile Business Analytics from hello gallery</span></span>
<span data-ttu-id="ddd95-124">tooconfigure hello integrering av Birst flexibel Företagsanalys i Azure AD, behöver du tooadd Birst flexibel Företagsanalys hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="ddd95-124">tooconfigure hello integration of Birst Agile Business Analytics into Azure AD, you need tooadd Birst Agile Business Analytics from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ddd95-125">**tooadd Birst flexibel Företagsanalys från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ddd95-125">**tooadd Birst Agile Business Analytics from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ddd95-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ddd95-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ddd95-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ddd95-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ddd95-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddd95-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ddd95-133">Skriv i sökrutan hello **Birst flexibel Företagsanalys**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-133">In hello search box, type **Birst Agile Business Analytics**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/tutorial_birst_search.png)

5. <span data-ttu-id="ddd95-135">Markera hello resultat på panelen **Birst flexibel Företagsanalys**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="ddd95-135">In hello results panel, select **Birst Agile Business Analytics**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/tutorial_birst_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ddd95-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ddd95-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ddd95-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Birst flexibel Företagsanalys baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ddd95-138">In this section, you configure and test Azure AD single sign-on with Birst Agile Business Analytics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ddd95-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Birst flexibel Företagsanalys är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ddd95-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Birst Agile Business Analytics is tooa user in Azure AD.</span></span> <span data-ttu-id="ddd95-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Birst flexibel Företagsanalys toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="ddd95-140">In other words, a link relationship between an Azure AD user and hello related user in Birst Agile Business Analytics needs toobe established.</span></span>

<span data-ttu-id="ddd95-141">Tilldela hello värdet för hello i Birst flexibel Företagsanalys **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ddd95-141">In Birst Agile Business Analytics, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ddd95-142">tooconfigure och testa Azure AD enkel inloggning med Birst flexibel Företagsanalys, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ddd95-142">tooconfigure and test Azure AD single sign-on with Birst Agile Business Analytics, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ddd95-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ddd95-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ddd95-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ddd95-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ddd95-145">**[Skapa en testanvändare Birst flexibel Företagsanalys](#creating-a-birst-agile-business-analytics-test-user)**  -toohave en motsvarighet för Britta Simon i Birst flexibel Företagsanalys som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ddd95-145">**[Creating a Birst Agile Business Analytics test user](#creating-a-birst-agile-business-analytics-test-user)** - toohave a counterpart of Britta Simon in Birst Agile Business Analytics that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ddd95-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ddd95-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ddd95-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ddd95-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ddd95-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ddd95-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ddd95-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Birst flexibel Företagsanalys.</span><span class="sxs-lookup"><span data-stu-id="ddd95-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Birst Agile Business Analytics application.</span></span>

<span data-ttu-id="ddd95-150">**tooconfigure Azure AD enkel inloggning med Birst flexibel Företagsanalys utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ddd95-150">**tooconfigure Azure AD single sign-on with Birst Agile Business Analytics, perform hello following steps:**</span></span>

1. <span data-ttu-id="ddd95-151">I hello Azure-portalen på hello **Birst flexibel Företagsanalys** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-151">In hello Azure portal, on hello **Birst Agile Business Analytics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ddd95-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ddd95-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_samlbase.png)

3. <span data-ttu-id="ddd95-155">På hello **Birst flexibel Business Analytics domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ddd95-155">On hello **Birst Agile Business Analytics Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_url.png)

     <span data-ttu-id="ddd95-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="ddd95-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

     <span data-ttu-id="ddd95-158">hello URL är beroende av hello datacenter att ditt konto Birst finns:</span><span class="sxs-lookup"><span data-stu-id="ddd95-158">hello URL depends on hello datacenter that your Birst account is located:</span></span> 

     * <span data-ttu-id="ddd95-159">För USA datacenter använder du följande hello mönster:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="ddd95-159">For US datacenter use following hello pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span> 

     * <span data-ttu-id="ddd95-160">För Europa använder datacenter hello följande mönster:`https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="ddd95-160">For Europe datacenter use hello following pattern: `https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ddd95-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ddd95-161">This value is not real.</span></span> <span data-ttu-id="ddd95-162">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="ddd95-162">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="ddd95-163">Kontakta [Birst flexibel Business Analytics Client supportteamet](mailto:info@birst.com) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="ddd95-163">Contact [Birst Agile Business Analytics Client support team](mailto:info@birst.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="ddd95-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="ddd95-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_certificate.png) 

5. <span data-ttu-id="ddd95-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ddd95-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ddd95-168">På hello **Birst flexibel Business Analytics Configuration** klickar du på **konfigurera Birst flexibel Företagsanalys** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ddd95-168">On hello **Birst Agile Business Analytics Configuration** section, click **Configure Birst Agile Business Analytics** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ddd95-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ddd95-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_configure.png) 

7. <span data-ttu-id="ddd95-171">tooconfigure enkel inloggning på **Birst flexibel Företagsanalys** sida, behöver du toosend hello hämtas **certifikat (Base64)**, **Sign-Out URL SAML enhets-ID och SAML enkel inloggning Tjänst-URL** för[Birst flexibel Företagsanalys supportteam](mailto:info@birst.com).</span><span class="sxs-lookup"><span data-stu-id="ddd95-171">tooconfigure single sign-on on **Birst Agile Business Analytics** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Birst Agile Business Analytics support team](mailto:info@birst.com).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="ddd95-172">Nämnt tooBirst team som den här integreringen behöver SHA256-algoritmen (SHA1 stöds inte) så att de kan konfigurera hello SSO på hello lämplig server som **app2101** osv.</span><span class="sxs-lookup"><span data-stu-id="ddd95-172">Mention tooBirst team that this integration needs SHA256 Algorithm (SHA1 will not be supported) so that they can set hello SSO on hello appropriate server like **app2101** etc.</span></span>
  

> [!TIP]
> <span data-ttu-id="ddd95-173">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="ddd95-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ddd95-174">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="ddd95-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ddd95-175">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ddd95-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ddd95-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ddd95-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="ddd95-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ddd95-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ddd95-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ddd95-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ddd95-180">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ddd95-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ddd95-182">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ddd95-184">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddd95-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ddd95-186">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ddd95-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ddd95-188">a.</span><span class="sxs-lookup"><span data-stu-id="ddd95-188">a.</span></span> <span data-ttu-id="ddd95-189">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ddd95-190">b.</span><span class="sxs-lookup"><span data-stu-id="ddd95-190">b.</span></span> <span data-ttu-id="ddd95-191">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ddd95-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ddd95-192">c.</span><span class="sxs-lookup"><span data-stu-id="ddd95-192">c.</span></span> <span data-ttu-id="ddd95-193">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ddd95-194">d.</span><span class="sxs-lookup"><span data-stu-id="ddd95-194">d.</span></span> <span data-ttu-id="ddd95-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-195">Click **Create**.</span></span>
 
### <a name="creating-a-birst-agile-business-analytics-test-user"></a><span data-ttu-id="ddd95-196">Skapa en testanvändare Birst flexibel Företagsanalys</span><span class="sxs-lookup"><span data-stu-id="ddd95-196">Creating a Birst Agile Business Analytics test user</span></span>

<span data-ttu-id="ddd95-197">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Birst flexibel Företagsanalys.</span><span class="sxs-lookup"><span data-stu-id="ddd95-197">hello objective of this section is toocreate a user called Britta Simon in Birst Agile Business Analytics.</span></span> <span data-ttu-id="ddd95-198">Arbeta med [Birst flexibel Företagsanalys supportteam](mailto:info@birst.com) tooadd hello användare i hello Birst konto.</span><span class="sxs-lookup"><span data-stu-id="ddd95-198">Work with [Birst Agile Business Analytics support team](mailto:info@birst.com) tooadd hello users in hello Birst account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ddd95-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="ddd95-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ddd95-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBirst flexibel Företagsanalys.</span><span class="sxs-lookup"><span data-stu-id="ddd95-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBirst Agile Business Analytics.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ddd95-202">**tooassign Britta Simon tooBirst flexibel Företagsanalys utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ddd95-202">**tooassign Britta Simon tooBirst Agile Business Analytics, perform hello following steps:**</span></span>

1. <span data-ttu-id="ddd95-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ddd95-205">Välj i listan med program hello **Birst flexibel Företagsanalys**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-205">In hello applications list, select **Birst Agile Business Analytics**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_app.png) 

3. <span data-ttu-id="ddd95-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ddd95-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ddd95-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ddd95-209">Click **Add** button.</span></span> <span data-ttu-id="ddd95-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddd95-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ddd95-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="ddd95-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ddd95-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddd95-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ddd95-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddd95-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ddd95-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ddd95-215">Testing single sign-on</span></span>

<span data-ttu-id="ddd95-216">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ddd95-216">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="ddd95-217">Du bör få automatiskt inloggade tooyour Birst flexibel Företagsanalys programmet när du klickar på hello Birst flexibel Företagsanalys panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ddd95-217">When you click hello Birst Agile Business Analytics tile in hello Access Panel, you should get automatically signed-on tooyour Birst Agile Business Analytics application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ddd95-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ddd95-218">Additional resources</span></span>

* [<span data-ttu-id="ddd95-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ddd95-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ddd95-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ddd95-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-birst-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-birst-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-birst-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-birst-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-birst-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-birst-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-birst-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-birst-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-birst-tutorial/tutorial_general_203.png

