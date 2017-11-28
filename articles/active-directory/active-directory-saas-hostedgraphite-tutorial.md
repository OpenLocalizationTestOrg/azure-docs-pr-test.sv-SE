---
title: "Självstudier: Azure Active Directory-integrering med värd grafit | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och värdbaserade grafit."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: d8914f6417ba8fbdef1a48e1b36635200ba130d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a><span data-ttu-id="7b603-103">Självstudier: Azure Active Directory-integrering med värd grafit</span><span class="sxs-lookup"><span data-stu-id="7b603-103">Tutorial: Azure Active Directory integration with Hosted Graphite</span></span>

<span data-ttu-id="7b603-104">I kursen får du lära dig hur toointegrate Hosted grafit med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7b603-104">In this tutorial, you learn how toointegrate Hosted Graphite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7b603-105">Integrera finns grafit med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7b603-105">Integrating Hosted Graphite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7b603-106">Du kan styra i Azure AD som har åtkomst tooHosted grafit</span><span class="sxs-lookup"><span data-stu-id="7b603-106">You can control in Azure AD who has access tooHosted Graphite</span></span>
- <span data-ttu-id="7b603-107">Du kan aktivera din användare tooautomatically get inloggade tooHosted grafit (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7b603-107">You can enable your users tooautomatically get signed-on tooHosted Graphite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7b603-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7b603-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7b603-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7b603-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b603-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7b603-110">Prerequisites</span></span>

<span data-ttu-id="7b603-111">tooconfigure Azure AD-integrering med värd grafit måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7b603-111">tooconfigure Azure AD integration with Hosted Graphite, you need hello following items:</span></span>

- <span data-ttu-id="7b603-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7b603-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7b603-113">En värd grafit enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7b603-113">A Hosted Graphite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7b603-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7b603-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7b603-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7b603-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7b603-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7b603-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7b603-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7b603-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7b603-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7b603-118">Scenario description</span></span>
<span data-ttu-id="7b603-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7b603-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7b603-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7b603-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7b603-121">Att lägga till värd grafit från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7b603-121">Adding Hosted Graphite from hello gallery</span></span>
2. <span data-ttu-id="7b603-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7b603-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hosted-graphite-from-hello-gallery"></a><span data-ttu-id="7b603-123">Att lägga till värd grafit från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7b603-123">Adding Hosted Graphite from hello gallery</span></span>
<span data-ttu-id="7b603-124">tooconfigure hello integrering av värdbaserade grafit i Azure AD, behöver du tooadd Hosted grafit hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="7b603-124">tooconfigure hello integration of Hosted Graphite into Azure AD, you need tooadd Hosted Graphite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7b603-125">**tooadd Hosted grafit från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7b603-125">**tooadd Hosted Graphite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b603-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7b603-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7b603-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7b603-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7b603-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="7b603-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7b603-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b603-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7b603-133">Skriv i sökrutan hello **finns grafit**.</span><span class="sxs-lookup"><span data-stu-id="7b603-133">In hello search box, type **Hosted Graphite**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

5. <span data-ttu-id="7b603-135">Markera hello resultat på panelen **finns grafit**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="7b603-135">In hello results panel, select **Hosted Graphite**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7b603-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7b603-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7b603-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med värd grafit baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7b603-138">In this section, you configure and test Azure AD single sign-on with Hosted Graphite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7b603-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i värd grafit är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7b603-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Hosted Graphite is tooa user in Azure AD.</span></span> <span data-ttu-id="7b603-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i finns grafit toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="7b603-140">In other words, a link relationship between an Azure AD user and hello related user in Hosted Graphite needs toobe established.</span></span>

<span data-ttu-id="7b603-141">I värdbaserade grafit tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7b603-141">In Hosted Graphite, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7b603-142">tooconfigure och testa Azure AD enkel inloggning med värd grafit, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7b603-142">tooconfigure and test Azure AD single sign-on with Hosted Graphite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7b603-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7b603-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7b603-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7b603-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7b603-145">**[Skapa en värdbaserad grafit testanvändare](#creating-a-hosted-graphite-test-user)**  -toohave en motsvarighet för Britta Simon i värd grafit som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7b603-145">**[Creating a Hosted Graphite test user](#creating-a-hosted-graphite-test-user)** - toohave a counterpart of Britta Simon in Hosted Graphite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7b603-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7b603-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7b603-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7b603-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7b603-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7b603-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7b603-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program finns grafit.</span><span class="sxs-lookup"><span data-stu-id="7b603-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Hosted Graphite application.</span></span>

<span data-ttu-id="7b603-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med värd grafit:**</span><span class="sxs-lookup"><span data-stu-id="7b603-150">**tooconfigure Azure AD single sign-on with Hosted Graphite, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b603-151">I hello Azure-portalen på hello **finns grafit** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7b603-151">In hello Azure portal, on hello **Hosted Graphite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7b603-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7b603-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

3. <span data-ttu-id="7b603-155">På hello **finns grafit domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7b603-155">On hello **Hosted Graphite Domain and URLs** section, if you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    <span data-ttu-id="7b603-157">a.</span><span class="sxs-lookup"><span data-stu-id="7b603-157">a.</span></span> <span data-ttu-id="7b603-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.hostedgraphite.com/metadata/<user id>`</span><span class="sxs-lookup"><span data-stu-id="7b603-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.hostedgraphite.com/metadata/<user id>`</span></span>

    <span data-ttu-id="7b603-159">b.</span><span class="sxs-lookup"><span data-stu-id="7b603-159">b.</span></span> <span data-ttu-id="7b603-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.hostedgraphite.com/complete/saml/<user id>`</span><span class="sxs-lookup"><span data-stu-id="7b603-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.hostedgraphite.com/complete/saml/<user id>`</span></span>

4. <span data-ttu-id="7b603-161">På hello **finns grafit domän och URL: er** om du vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7b603-161">On hello **Hosted Graphite Domain and URLs** section, if you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    <span data-ttu-id="7b603-163">a.</span><span class="sxs-lookup"><span data-stu-id="7b603-163">a.</span></span> <span data-ttu-id="7b603-164">Klicka på hello **visa avancerade inställningar för URL: en** alternativet</span><span class="sxs-lookup"><span data-stu-id="7b603-164">Click on hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="7b603-165">b.</span><span class="sxs-lookup"><span data-stu-id="7b603-165">b.</span></span> <span data-ttu-id="7b603-166">I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.hostedgraphite.com/login/saml/<user id>/`</span><span class="sxs-lookup"><span data-stu-id="7b603-166">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://www.hostedgraphite.com/login/saml/<user id>/`</span></span>   

    > [!NOTE] 
    > <span data-ttu-id="7b603-167">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="7b603-167">Please note that these are not hello real values.</span></span> <span data-ttu-id="7b603-168">Du har tooupdate dessa värden med hello faktiska identifierare, Reply URL och logga URL.</span><span class="sxs-lookup"><span data-stu-id="7b603-168">You have tooupdate these values with hello actual Identifier, Reply URL and Sign On URL.</span></span> <span data-ttu-id="7b603-169">tooget dessa värden, går du tooAccess -> SAML-installationen på dina program på serversidan eller kontakta [finns grafit supportteamet](mailto:help@hostedgraphite.com).</span><span class="sxs-lookup"><span data-stu-id="7b603-169">tooget these values, you can go tooAccess->SAML setup on your Application side or Contact [Hosted Graphite support team](mailto:help@hostedgraphite.com).</span></span>
    >
 
5. <span data-ttu-id="7b603-170">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="7b603-170">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

6. <span data-ttu-id="7b603-172">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7b603-172">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="7b603-174">På hello **finns grafit Configuration** klickar du på **Konfigurera värdbaserade grafit** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7b603-174">On hello **Hosted Graphite Configuration** section, click **Configure Hosted Graphite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7b603-175">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7b603-175">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

8. <span data-ttu-id="7b603-177">Inloggning tooyour finns grafit innehavaren som administratör.</span><span class="sxs-lookup"><span data-stu-id="7b603-177">Sign-on tooyour Hosted Graphite tenant as an administrator.</span></span>

9. <span data-ttu-id="7b603-178">Gå toohello **SAML installationssidan** i sidopanelen hello (**åtkomst SAML Inställningar ->**).</span><span class="sxs-lookup"><span data-stu-id="7b603-178">Go toohello **SAML Setup page** in hello sidebar (**Access -> SAML Setup**).</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

10. <span data-ttu-id="7b603-180">Bekräfta dessa URL: er matchar konfigurationen på hello **finns grafit domän och URL: er** avsnitt i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7b603-180">Confirm these URls match your configuration done on hello **Hosted Graphite Domain and URLs** section of hello Azure portal.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

11. <span data-ttu-id="7b603-182">I **entiteten eller utfärdaren ID** och **inloggnings-URL för enkel inloggning** textrutor, klistra in hello värdet för **SAML enhets-ID** och **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7b603-182">In  **Entity or Issuer ID** and **SSO Login URL** textboxes, paste hello value of **SAML Entity ID** and **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

12. <span data-ttu-id="7b603-184">Välj ”**skrivskyddad**” som **standard användarrollen**.</span><span class="sxs-lookup"><span data-stu-id="7b603-184">Select "**Read-only**" as **Default User Role**.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

13. <span data-ttu-id="7b603-186">Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="7b603-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

14. <span data-ttu-id="7b603-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7b603-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="7b603-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="7b603-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7b603-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="7b603-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7b603-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7b603-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7b603-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7b603-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="7b603-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7b603-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7b603-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7b603-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b603-196">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7b603-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7b603-198">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7b603-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7b603-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b603-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7b603-202">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7b603-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7b603-204">a.</span><span class="sxs-lookup"><span data-stu-id="7b603-204">a.</span></span> <span data-ttu-id="7b603-205">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7b603-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7b603-206">b.</span><span class="sxs-lookup"><span data-stu-id="7b603-206">b.</span></span> <span data-ttu-id="7b603-207">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7b603-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7b603-208">c.</span><span class="sxs-lookup"><span data-stu-id="7b603-208">c.</span></span> <span data-ttu-id="7b603-209">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7b603-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7b603-210">d.</span><span class="sxs-lookup"><span data-stu-id="7b603-210">d.</span></span> <span data-ttu-id="7b603-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7b603-211">Click **Create**.</span></span>
 
### <a name="creating-a-hosted-graphite-test-user"></a><span data-ttu-id="7b603-212">Skapa en värdbaserad grafit testanvändare</span><span class="sxs-lookup"><span data-stu-id="7b603-212">Creating a Hosted Graphite test user</span></span>

<span data-ttu-id="7b603-213">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i värd grafit.</span><span class="sxs-lookup"><span data-stu-id="7b603-213">hello objective of this section is toocreate a user called Britta Simon in Hosted Graphite.</span></span> <span data-ttu-id="7b603-214">Värdbaserade grafit stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="7b603-214">Hosted Graphite supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="7b603-215">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="7b603-215">There is no action item for you in this section.</span></span> <span data-ttu-id="7b603-216">En ny användare skapas under ett försök tooaccess Hosted grafit om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="7b603-216">A new user will be created during an attempt tooaccess Hosted Graphite if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="7b603-217">Om du behöver toocreate en användare manuellt, måste toocontact hello finns grafit supportteamet via < mailto:help@hostedgraphite.com >.</span><span class="sxs-lookup"><span data-stu-id="7b603-217">If you need toocreate a user manually, you need toocontact hello Hosted Graphite support team via <mailto:help@hostedgraphite.com>.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7b603-218">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7b603-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7b603-219">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHosted grafit.</span><span class="sxs-lookup"><span data-stu-id="7b603-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHosted Graphite.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7b603-221">**tooassign Britta Simon tooHosted grafit, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="7b603-221">**tooassign Britta Simon tooHosted Graphite, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b603-222">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7b603-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7b603-224">Välj i listan med program hello **finns grafit**.</span><span class="sxs-lookup"><span data-stu-id="7b603-224">In hello applications list, select **Hosted Graphite**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

3. <span data-ttu-id="7b603-226">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7b603-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7b603-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7b603-228">Click **Add** button.</span></span> <span data-ttu-id="7b603-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b603-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7b603-231">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="7b603-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7b603-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b603-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7b603-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b603-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7b603-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7b603-234">Testing single sign-on</span></span>

<span data-ttu-id="7b603-235">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7b603-235">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="7b603-236">Du bör få automatiskt inloggade tooyour finns grafit programmet när du klickar på hello finns grafit panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7b603-236">When you click hello Hosted Graphite tile in hello Access Panel, you should get automatically signed-on tooyour Hosted Graphite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b603-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7b603-237">Additional resources</span></span>

* [<span data-ttu-id="7b603-238">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7b603-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7b603-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7b603-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png

