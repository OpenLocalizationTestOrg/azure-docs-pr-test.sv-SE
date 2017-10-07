---
title: "Självstudier: Azure Active Directory-integrering med Pantheon | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Pantheon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 5c3e54aef1f64dbab77d40a8c098172d609bfec8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a><span data-ttu-id="1da74-103">Självstudier: Azure Active Directory-integrering med Pantheon</span><span class="sxs-lookup"><span data-stu-id="1da74-103">Tutorial: Azure Active Directory integration with Pantheon</span></span>

<span data-ttu-id="1da74-104">I kursen får du lära dig hur toointegrate Pantheon med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1da74-104">In this tutorial, you learn how toointegrate Pantheon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1da74-105">Integrera Pantheon med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1da74-105">Integrating Pantheon with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1da74-106">Du kan styra i Azure AD som har åtkomst till tooPantheon</span><span class="sxs-lookup"><span data-stu-id="1da74-106">You can control in Azure AD who has access tooPantheon</span></span>
- <span data-ttu-id="1da74-107">Du kan aktivera din användare tooautomatically get inloggade tooPantheon (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1da74-107">You can enable your users tooautomatically get signed-on tooPantheon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1da74-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1da74-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1da74-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1da74-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1da74-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1da74-110">Prerequisites</span></span>

<span data-ttu-id="1da74-111">tooconfigure Azure AD-integrering med Pantheon, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="1da74-111">tooconfigure Azure AD integration with Pantheon, you need hello following items:</span></span>

- <span data-ttu-id="1da74-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1da74-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1da74-113">En Pantheon enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="1da74-113">A Pantheon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1da74-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1da74-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1da74-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1da74-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1da74-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1da74-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1da74-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1da74-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1da74-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1da74-118">Scenario description</span></span>
<span data-ttu-id="1da74-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1da74-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1da74-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1da74-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1da74-121">Att lägga till Pantheon från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1da74-121">Adding Pantheon from hello gallery</span></span>
2. <span data-ttu-id="1da74-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1da74-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pantheon-from-hello-gallery"></a><span data-ttu-id="1da74-123">Att lägga till Pantheon från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1da74-123">Adding Pantheon from hello gallery</span></span>
<span data-ttu-id="1da74-124">tooconfigure hello integrering av Pantheon i Azure AD, behöver du tooadd Pantheon hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="1da74-124">tooconfigure hello integration of Pantheon into Azure AD, you need tooadd Pantheon from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1da74-125">**tooadd Pantheon från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1da74-125">**tooadd Pantheon from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1da74-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1da74-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1da74-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1da74-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1da74-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="1da74-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="1da74-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1da74-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="1da74-133">Skriv i sökrutan hello **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="1da74-133">In hello search box, type **Pantheon**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_search.png)

5. <span data-ttu-id="1da74-135">Markera hello resultat på panelen **Pantheon**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="1da74-135">In hello results panel, select **Pantheon**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1da74-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1da74-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1da74-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Pantheon baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1da74-138">In this section, you configure and test Azure AD single sign-on with Pantheon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1da74-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Pantheon är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1da74-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pantheon is tooa user in Azure AD.</span></span> <span data-ttu-id="1da74-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Pantheon toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="1da74-140">In other words, a link relationship between an Azure AD user and hello related user in Pantheon needs toobe established.</span></span>

<span data-ttu-id="1da74-141">I Pantheon, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="1da74-141">In Pantheon, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1da74-142">tooconfigure och testa Azure AD enkel inloggning med Pantheon, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1da74-142">tooconfigure and test Azure AD single sign-on with Pantheon, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1da74-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1da74-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1da74-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1da74-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1da74-145">**[Skapa en testanvändare Pantheon](#creating-a-pantheon-test-user)**  -toohave en motsvarighet för Britta Simon i Pantheon som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1da74-145">**[Creating a Pantheon test user](#creating-a-pantheon-test-user)** - toohave a counterpart of Britta Simon in Pantheon that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1da74-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1da74-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1da74-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1da74-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1da74-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1da74-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1da74-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Pantheon program.</span><span class="sxs-lookup"><span data-stu-id="1da74-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Pantheon application.</span></span>

<span data-ttu-id="1da74-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Pantheon:**</span><span class="sxs-lookup"><span data-stu-id="1da74-150">**tooconfigure Azure AD single sign-on with Pantheon, perform hello following steps:**</span></span>

1. <span data-ttu-id="1da74-151">I hello Azure-portalen på hello **Pantheon** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1da74-151">In hello Azure portal, on hello **Pantheon** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="1da74-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1da74-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_samlbase.png)

3. <span data-ttu-id="1da74-155">På hello **Pantheon domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1da74-155">On hello **Pantheon Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_url.png)

    <span data-ttu-id="1da74-157">a.</span><span class="sxs-lookup"><span data-stu-id="1da74-157">a.</span></span> <span data-ttu-id="1da74-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`urn:auth0:pantheon:<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="1da74-158">In hello **Identifier** textbox, type a URL using hello following pattern: `urn:auth0:pantheon:<orgname>-SSO`</span></span>

    <span data-ttu-id="1da74-159">b.</span><span class="sxs-lookup"><span data-stu-id="1da74-159">b.</span></span> <span data-ttu-id="1da74-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="1da74-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1da74-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="1da74-161">These values are not real.</span></span> <span data-ttu-id="1da74-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="1da74-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="1da74-163">Kontakta [Pantheon supportteamet](https://pantheon.io/docs/getting-support/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="1da74-163">Contact [Pantheon support team](https://pantheon.io/docs/getting-support/) tooget these values.</span></span>

4. <span data-ttu-id="1da74-164">Pantheon program förväntar hello SAML-kontroll i specifika format, vilket kräver att du tooset hello UserIdentifier attributvärde med hello användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="1da74-164">Pantheon application expects hello SAML assertion in specific format, which requires you tooset hello UserIdentifier attribute value with hello user’s email address.</span></span> <span data-ttu-id="1da74-165">Som standard använder Azure AD hello UserPrincipalName för UserIdentifier attribut.</span><span class="sxs-lookup"><span data-stu-id="1da74-165">By default Azure AD uses hello UserPrincipalName for UserIdentifier attribute.</span></span> <span data-ttu-id="1da74-166">Men för lyckad integrering måste tooadjust det här värdet toomatch med användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="1da74-166">But for successful integration you need tooadjust this value toomatch with user’s email address.</span></span> <span data-ttu-id="1da74-167">hello integration fungerar endast när du har gjort hello korrekt mappning.</span><span class="sxs-lookup"><span data-stu-id="1da74-167">hello integration will only work after doing hello correct mapping.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_attribute.png)  


5. <span data-ttu-id="1da74-169">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="1da74-169">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_certificate.png)

6. <span data-ttu-id="1da74-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1da74-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="1da74-173">På hello **Pantheon Configuration** klickar du på **konfigurera Pantheon** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="1da74-173">On hello **Pantheon Configuration** section, click **Configure Pantheon** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1da74-174">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="1da74-174">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_configure.png) 

8. <span data-ttu-id="1da74-176">tooconfigure enkel inloggning på **Pantheon** sida, behöver du toosend hello hämtas **certifikat** och **SAML enkel inloggning Tjänstwebbadress** för[Pantheon supportteam](https://pantheon.io/docs/getting-support/).</span><span class="sxs-lookup"><span data-stu-id="1da74-176">tooconfigure single sign-on on **Pantheon** side, you need toosend hello downloaded **Certificate** and **SAML Single Sign-On Service URL** too[Pantheon support team](https://pantheon.io/docs/getting-support/).</span></span>

     > [!Note]
     > <span data-ttu-id="1da74-177">Du måste också tooprovide hello e-domäner information och datum och tid när du vill att tooenable anslutningen.</span><span class="sxs-lookup"><span data-stu-id="1da74-177">You also need tooprovide hello Email Domain(s) information and Date Time when you want tooenable this connection.</span></span> <span data-ttu-id="1da74-178">Du hittar mer information om den från [här](https://pantheon.io/docs/sso-organizations/)</span><span class="sxs-lookup"><span data-stu-id="1da74-178">You can find more details about it from [here](https://pantheon.io/docs/sso-organizations/)</span></span>

> [!TIP]
> <span data-ttu-id="1da74-179">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="1da74-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1da74-180">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="1da74-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1da74-181">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1da74-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1da74-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1da74-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="1da74-183">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1da74-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="1da74-185">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1da74-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1da74-186">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1da74-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1da74-188">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1da74-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1da74-190">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1da74-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1da74-192">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1da74-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pantheon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1da74-194">a.</span><span class="sxs-lookup"><span data-stu-id="1da74-194">a.</span></span> <span data-ttu-id="1da74-195">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1da74-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1da74-196">b.</span><span class="sxs-lookup"><span data-stu-id="1da74-196">b.</span></span> <span data-ttu-id="1da74-197">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1da74-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1da74-198">c.</span><span class="sxs-lookup"><span data-stu-id="1da74-198">c.</span></span> <span data-ttu-id="1da74-199">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1da74-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1da74-200">d.</span><span class="sxs-lookup"><span data-stu-id="1da74-200">d.</span></span> <span data-ttu-id="1da74-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1da74-201">Click **Create**.</span></span>
 
### <a name="creating-a-pantheon-test-user"></a><span data-ttu-id="1da74-202">Skapa en testanvändare Pantheon</span><span class="sxs-lookup"><span data-stu-id="1da74-202">Creating a Pantheon test user</span></span>

<span data-ttu-id="1da74-203">I det här avsnittet skapar du en användare som kallas Britta Simon i Pantheon.</span><span class="sxs-lookup"><span data-stu-id="1da74-203">In this section, you create a user called Britta Simon in Pantheon.</span></span> <span data-ttu-id="1da74-204">Följ hello under steg tooadd hello användare i Pantheon.</span><span class="sxs-lookup"><span data-stu-id="1da74-204">Please follow hello below steps tooadd hello user in Pantheon.</span></span> 

>[!NOTE] 
><span data-ttu-id="1da74-205">För enkel inloggning måste toowork användaren toobe skapas första i Pantheon.</span><span class="sxs-lookup"><span data-stu-id="1da74-205">For SSO toowork user needs toobe created first in Pantheon.</span></span>

1. <span data-ttu-id="1da74-206">Inloggningen tooPantheon med autentiseringsuppgifter som administratör.</span><span class="sxs-lookup"><span data-stu-id="1da74-206">Login tooPantheon with admin credentials.</span></span>

2. <span data-ttu-id="1da74-207">Navigera för**organisation** instrumentpanelens sida.</span><span class="sxs-lookup"><span data-stu-id="1da74-207">Navigate too**Organization** dashboard page.</span></span>
 
3. <span data-ttu-id="1da74-208">Klicka på **personer**.</span><span class="sxs-lookup"><span data-stu-id="1da74-208">Click **People**.</span></span>

4. <span data-ttu-id="1da74-209">Klicka på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="1da74-209">Click **Add user**.</span></span>

5. <span data-ttu-id="1da74-210">Ange hello användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="1da74-210">Enter hello user's email address.</span></span>

6. <span data-ttu-id="1da74-211">Välj hello användarroll.</span><span class="sxs-lookup"><span data-stu-id="1da74-211">Choose hello user's role.</span></span>

7. <span data-ttu-id="1da74-212">Klicka på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="1da74-212">Click **Add user**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1da74-213">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="1da74-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1da74-214">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPantheon.</span><span class="sxs-lookup"><span data-stu-id="1da74-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPantheon.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="1da74-216">**tooassign Britta Simon tooPantheon utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1da74-216">**tooassign Britta Simon tooPantheon, perform hello following steps:**</span></span>

1. <span data-ttu-id="1da74-217">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1da74-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1da74-219">Välj i listan med program hello **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="1da74-219">In hello applications list, select **Pantheon**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_app.png) 

3. <span data-ttu-id="1da74-221">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1da74-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="1da74-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1da74-223">Click **Add** button.</span></span> <span data-ttu-id="1da74-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1da74-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="1da74-226">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="1da74-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1da74-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1da74-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1da74-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1da74-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1da74-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1da74-229">Testing single sign-on</span></span>

<span data-ttu-id="1da74-230">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1da74-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1da74-231">Du bör få automatiskt inloggade tooyour Pantheon programmet när du klickar på hello Pantheon panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1da74-231">When you click hello Pantheon tile in hello Access Panel, you should get automatically signed-on tooyour Pantheon application.</span></span>
<span data-ttu-id="1da74-232">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1da74-232">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1da74-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1da74-233">Additional resources</span></span>

* [<span data-ttu-id="1da74-234">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1da74-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1da74-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1da74-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_203.png

