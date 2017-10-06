---
title: "Självstudier: Azure Active Directory-integrering med Nexonia | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Nexonia."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a93b771a-9bc3-444a-bdc0-457f8bb7e780
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/1/2017
ms.author: jeedes
ms.openlocfilehash: 3778804084a7989414260bae5654cf76e9ccca6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nexonia"></a><span data-ttu-id="6a48d-103">Självstudier: Azure Active Directory-integrering med Nexonia</span><span class="sxs-lookup"><span data-stu-id="6a48d-103">Tutorial: Azure Active Directory integration with Nexonia</span></span>

<span data-ttu-id="6a48d-104">I kursen får du lära dig hur toointegrate Nexonia med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6a48d-104">In this tutorial, you learn how toointegrate Nexonia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6a48d-105">Integrera Nexonia med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6a48d-105">Integrating Nexonia with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6a48d-106">Du kan styra i Azure AD som har åtkomst till tooNexonia</span><span class="sxs-lookup"><span data-stu-id="6a48d-106">You can control in Azure AD who has access tooNexonia</span></span>
- <span data-ttu-id="6a48d-107">Du kan aktivera din användare tooautomatically get inloggade tooNexonia (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6a48d-107">You can enable your users tooautomatically get signed-on tooNexonia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6a48d-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6a48d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6a48d-109">Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a48d-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="6a48d-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6a48d-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a48d-111">Krav</span><span class="sxs-lookup"><span data-stu-id="6a48d-111">Prerequisites</span></span>

<span data-ttu-id="6a48d-112">tooconfigure Azure AD-integrering med Nexonia, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6a48d-112">tooconfigure Azure AD integration with Nexonia, you need hello following items:</span></span>

- <span data-ttu-id="6a48d-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6a48d-113">An Azure AD subscription</span></span>
- <span data-ttu-id="6a48d-114">En Nexonia enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="6a48d-114">A Nexonia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6a48d-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6a48d-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6a48d-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6a48d-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6a48d-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6a48d-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6a48d-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6a48d-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6a48d-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6a48d-119">Scenario description</span></span>
<span data-ttu-id="6a48d-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6a48d-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6a48d-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6a48d-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6a48d-122">Att lägga till Nexonia från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6a48d-122">Adding Nexonia from hello gallery</span></span>
2. <span data-ttu-id="6a48d-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6a48d-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-nexonia-from-hello-gallery"></a><span data-ttu-id="6a48d-124">Att lägga till Nexonia från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6a48d-124">Adding Nexonia from hello gallery</span></span>
<span data-ttu-id="6a48d-125">tooconfigure hello integrering av Nexonia i Azure AD, behöver du tooadd Nexonia hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6a48d-125">tooconfigure hello integration of Nexonia into Azure AD, you need tooadd Nexonia from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6a48d-126">**tooadd Nexonia från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6a48d-126">**tooadd Nexonia from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a48d-127">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6a48d-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6a48d-129">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6a48d-130">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-130">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6a48d-132">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a48d-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6a48d-134">Skriv i sökrutan hello **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-134">In hello search box, type **Nexonia**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_search.png)

5. <span data-ttu-id="6a48d-136">Markera hello resultat på panelen **Nexonia**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6a48d-136">In hello results panel, select **Nexonia**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6a48d-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6a48d-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6a48d-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Nexonia baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6a48d-139">In this section, you configure and test Azure AD single sign-on with Nexonia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6a48d-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Nexonia är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a48d-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Nexonia is tooa user in Azure AD.</span></span> <span data-ttu-id="6a48d-141">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Nexonia toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6a48d-141">In other words, a link relationship between an Azure AD user and hello related user in Nexonia needs toobe established.</span></span>

<span data-ttu-id="6a48d-142">I Nexonia, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="6a48d-142">In Nexonia, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6a48d-143">tooconfigure och testa Azure AD enkel inloggning med Nexonia, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6a48d-143">tooconfigure and test Azure AD single sign-on with Nexonia, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6a48d-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6a48d-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6a48d-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6a48d-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6a48d-146">**[Skapa en testanvändare Nexonia](#creating-a-nexonia-test-user)**  -toohave en motsvarighet för Britta Simon i Nexonia som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6a48d-146">**[Creating a Nexonia test user](#creating-a-nexonia-test-user)** - toohave a counterpart of Britta Simon in Nexonia that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6a48d-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6a48d-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6a48d-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6a48d-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6a48d-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6a48d-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6a48d-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Nexonia program.</span><span class="sxs-lookup"><span data-stu-id="6a48d-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Nexonia application.</span></span>

>[!Note]
><span data-ttu-id="6a48d-151">Om du har problem i hello-integrering och sedan refererar detta [länken](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) för guide för felsökning.</span><span class="sxs-lookup"><span data-stu-id="6a48d-151">If you have issues in hello integration then refer this [link](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) for troubleshooting guide.</span></span> <span data-ttu-id="6a48d-152">Om du fortfarande inte har hittat hello lösning, öka du hello supportbegäran från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6a48d-152">If you still have not found hello solution, then raise hello support request from Azure portal.</span></span>

<span data-ttu-id="6a48d-153">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Nexonia:**</span><span class="sxs-lookup"><span data-stu-id="6a48d-153">**tooconfigure Azure AD single sign-on with Nexonia, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a48d-154">I hello Azure-portalen på hello **Nexonia** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-154">In hello Azure portal, on hello **Nexonia** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6a48d-156">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6a48d-156">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_samlbase.png)

3. <span data-ttu-id="6a48d-158">På hello **Nexonia domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6a48d-158">On hello **Nexonia Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_url.png)

    <span data-ttu-id="6a48d-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span><span class="sxs-lookup"><span data-stu-id="6a48d-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6a48d-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="6a48d-161">This value is not real.</span></span> <span data-ttu-id="6a48d-162">Uppdatera det här värdet med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="6a48d-162">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="6a48d-163">Kontakta [Nexonia supportteamet](https://nexonia.zendesk.com/hc/requests/new) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="6a48d-163">Contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) tooget this value.</span></span> 


4. <span data-ttu-id="6a48d-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="6a48d-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_certificate.png) 

5. <span data-ttu-id="6a48d-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6a48d-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6a48d-168">På hello **Nexonia Configuration** klickar du på **konfigurera Nexonia** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6a48d-168">On hello **Nexonia Configuration** section, click **Configure Nexonia** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6a48d-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6a48d-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_configure.png) 

7. <span data-ttu-id="6a48d-171">tooget SSO konfigurerats för ditt program bör du kontakta [Nexonia supportteamet](https://nexonia.zendesk.com/hc/requests/new) och ge dem hello följande:</span><span class="sxs-lookup"><span data-stu-id="6a48d-171">tooget SSO configured for your application, contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) and provide them with hello following:</span></span>

    <span data-ttu-id="6a48d-172">• hello hämtas **certifikat**</span><span class="sxs-lookup"><span data-stu-id="6a48d-172">• hello downloaded **certificate**</span></span>

    <span data-ttu-id="6a48d-173">• hello **SAML enhets-ID**</span><span class="sxs-lookup"><span data-stu-id="6a48d-173">• hello **SAML Entity ID**</span></span>

    <span data-ttu-id="6a48d-174">• hello **SAML enkel inloggning tjänst-URL**</span><span class="sxs-lookup"><span data-stu-id="6a48d-174">• hello **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="6a48d-175">• hello **Sign-Out URL**</span><span class="sxs-lookup"><span data-stu-id="6a48d-175">• hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="6a48d-176">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6a48d-176">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6a48d-177">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6a48d-177">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6a48d-178">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6a48d-178">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6a48d-179">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a48d-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="6a48d-180">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6a48d-180">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6a48d-182">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6a48d-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a48d-183">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6a48d-183">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6a48d-185">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-185">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6a48d-187">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a48d-187">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6a48d-189">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6a48d-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6a48d-191">a.</span><span class="sxs-lookup"><span data-stu-id="6a48d-191">a.</span></span> <span data-ttu-id="6a48d-192">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6a48d-193">b.</span><span class="sxs-lookup"><span data-stu-id="6a48d-193">b.</span></span> <span data-ttu-id="6a48d-194">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6a48d-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6a48d-195">c.</span><span class="sxs-lookup"><span data-stu-id="6a48d-195">c.</span></span> <span data-ttu-id="6a48d-196">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6a48d-197">d.</span><span class="sxs-lookup"><span data-stu-id="6a48d-197">d.</span></span> <span data-ttu-id="6a48d-198">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-198">Click **Create**.</span></span>
 
### <a name="creating-a-nexonia-test-user"></a><span data-ttu-id="6a48d-199">Skapa en testanvändare Nexonia</span><span class="sxs-lookup"><span data-stu-id="6a48d-199">Creating a Nexonia test user</span></span>

<span data-ttu-id="6a48d-200">I det här avsnittet skapar du en användare som kallas Britta Simon i Nexonia.</span><span class="sxs-lookup"><span data-stu-id="6a48d-200">In this section, you create a user called Britta Simon in Nexonia.</span></span> <span data-ttu-id="6a48d-201">Arbeta med [Nexonia supportteamet](https://nexonia.zendesk.com/hc/requests/new) tooadd hello användare i hello Nexonia plattform.</span><span class="sxs-lookup"><span data-stu-id="6a48d-201">Work with [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) tooadd hello users in hello Nexonia platform.</span></span> <span data-ttu-id="6a48d-202">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6a48d-202">Users must be created and activated before you use single sign-on.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6a48d-203">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a48d-203">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6a48d-204">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNexonia.</span><span class="sxs-lookup"><span data-stu-id="6a48d-204">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNexonia.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6a48d-206">**tooassign Britta Simon tooNexonia utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6a48d-206">**tooassign Britta Simon tooNexonia, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a48d-207">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-207">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6a48d-209">Välj i listan med program hello **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-209">In hello applications list, select **Nexonia**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_app.png) 

3. <span data-ttu-id="6a48d-211">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6a48d-211">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6a48d-213">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6a48d-213">Click **Add** button.</span></span> <span data-ttu-id="6a48d-214">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a48d-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6a48d-216">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6a48d-216">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6a48d-217">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a48d-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6a48d-218">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a48d-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6a48d-219">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6a48d-219">Testing single sign-on</span></span>

<span data-ttu-id="6a48d-220">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6a48d-220">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6a48d-221">Du bör få automatiskt inloggade tooyour Nexonia programmet när du klickar på hello Nexonia panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6a48d-221">When you click hello Nexonia tile in hello Access Panel, you should get automatically signed-on tooyour Nexonia application.</span></span>
<span data-ttu-id="6a48d-222">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="6a48d-222">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a48d-223">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6a48d-223">Additional resources</span></span>

* [<span data-ttu-id="6a48d-224">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6a48d-224">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6a48d-225">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6a48d-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_203.png

