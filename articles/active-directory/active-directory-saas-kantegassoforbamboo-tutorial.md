---
title: "Självstudier: Azure Active Directory-integrering med Kantega SSO för bambu | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Kantega SSO för bambu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e238b574-9e9b-43b7-ab98-d2a87ff89d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 8bf637ff440e8e3948db882861bee6e73f8aa879
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bamboo"></a><span data-ttu-id="282d4-103">Självstudier: Azure Active Directory-integrering med Kantega SSO för bambu</span><span class="sxs-lookup"><span data-stu-id="282d4-103">Tutorial: Azure Active Directory integration with Kantega SSO for Bamboo</span></span>

<span data-ttu-id="282d4-104">I kursen får du lära dig hur toointegrate Kantega SSO för bambu med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="282d4-104">In this tutorial, you learn how toointegrate Kantega SSO for Bamboo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="282d4-105">Integrera Kantega SSO för bambu med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="282d4-105">Integrating Kantega SSO for Bamboo with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="282d4-106">Du kan styra i Azure AD som har åtkomst tooKantega SSO för bambu</span><span class="sxs-lookup"><span data-stu-id="282d4-106">You can control in Azure AD who has access tooKantega SSO for Bamboo</span></span>
- <span data-ttu-id="282d4-107">Du kan aktivera din användare tooautomatically get inloggade tooKantega SSO för bambu (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="282d4-107">You can enable your users tooautomatically get signed-on tooKantega SSO for Bamboo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="282d4-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="282d4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="282d4-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="282d4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="282d4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="282d4-110">Prerequisites</span></span>

<span data-ttu-id="282d4-111">tooconfigure Azure AD-integrering med Kantega SSO för bambu måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="282d4-111">tooconfigure Azure AD integration with Kantega SSO for Bamboo, you need hello following items:</span></span>

- <span data-ttu-id="282d4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="282d4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="282d4-113">En Kantega SSO för bambu enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="282d4-113">A Kantega SSO for Bamboo single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="282d4-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="282d4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="282d4-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="282d4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="282d4-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="282d4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="282d4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="282d4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="282d4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="282d4-118">Scenario description</span></span>
<span data-ttu-id="282d4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="282d4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="282d4-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="282d4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="282d4-121">Att lägga till Kantega SSO för bambu från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="282d4-121">Adding Kantega SSO for Bamboo from hello gallery</span></span>
2. <span data-ttu-id="282d4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="282d4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-bamboo-from-hello-gallery"></a><span data-ttu-id="282d4-123">Att lägga till Kantega SSO för bambu från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="282d4-123">Adding Kantega SSO for Bamboo from hello gallery</span></span>
<span data-ttu-id="282d4-124">tooconfigure hello integrering av Kantega SSO för bambu i Azure AD, behöver du tooadd Kantega SSO för bambu hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="282d4-124">tooconfigure hello integration of Kantega SSO for Bamboo into Azure AD, you need tooadd Kantega SSO for Bamboo from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="282d4-125">**tooadd Kantega SSO för bambu från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="282d4-125">**tooadd Kantega SSO for Bamboo from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="282d4-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="282d4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="282d4-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="282d4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="282d4-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="282d4-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="282d4-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="282d4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="282d4-133">Skriv i sökrutan hello **Kantega SSO för bambu**.</span><span class="sxs-lookup"><span data-stu-id="282d4-133">In hello search box, type **Kantega SSO for Bamboo**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_search.png)

5. <span data-ttu-id="282d4-135">Markera hello resultat på panelen **Kantega SSO för bambu**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="282d4-135">In hello results panel, select **Kantega SSO for Bamboo**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="282d4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="282d4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="282d4-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kantega SSO för bambu baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="282d4-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for Bamboo based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="282d4-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren Kantega SSO för bambu är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="282d4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kantega SSO for Bamboo is tooa user in Azure AD.</span></span> <span data-ttu-id="282d4-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren Kantega SSO för bambu toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="282d4-140">In other words, a link relationship between an Azure AD user and hello related user in Kantega SSO for Bamboo needs toobe established.</span></span>

<span data-ttu-id="282d4-141">Kantega SSO för bambu, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="282d4-141">In Kantega SSO for Bamboo, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="282d4-142">tooconfigure och testa Azure AD enkel inloggning med Kantega SSO för bambu, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="282d4-142">tooconfigure and test Azure AD single sign-on with Kantega SSO for Bamboo, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="282d4-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="282d4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="282d4-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="282d4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="282d4-145">**[Skapa en Kantega SSO för bambu testanvändare](#creating-a-kantega-sso-for-bamboo-test-user)**  -toohave en motsvarighet för Britta Simon Kantega SSO för bambu som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="282d4-145">**[Creating a Kantega SSO for Bamboo test user](#creating-a-kantega-sso-for-bamboo-test-user)** - toohave a counterpart of Britta Simon in Kantega SSO for Bamboo that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="282d4-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="282d4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="282d4-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="282d4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="282d4-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="282d4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="282d4-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Kantega SSO för bambu program.</span><span class="sxs-lookup"><span data-stu-id="282d4-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kantega SSO for Bamboo application.</span></span>

<span data-ttu-id="282d4-150">**tooconfigure Azure AD enkel inloggning med Kantega SSO för bambu, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="282d4-150">**tooconfigure Azure AD single sign-on with Kantega SSO for Bamboo, perform hello following steps:**</span></span>

1. <span data-ttu-id="282d4-151">I hello Azure-portalen på hello **Kantega SSO för bambu** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="282d4-151">In hello Azure portal, on hello **Kantega SSO for Bamboo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="282d4-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="282d4-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_samlbase.png)

3. <span data-ttu-id="282d4-155">I **IDP** initierade läge på hello **Kantega SSO bambu domän och URL: er** avsnittet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="282d4-155">In **IDP** initiated mode, on hello **Kantega SSO for Bamboo Domain and URLs** section perform hello following step :</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_url1.png)
    
    <span data-ttu-id="282d4-157">a.</span><span class="sxs-lookup"><span data-stu-id="282d4-157">a.</span></span> <span data-ttu-id="282d4-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="282d4-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="282d4-159">b.</span><span class="sxs-lookup"><span data-stu-id="282d4-159">b.</span></span> <span data-ttu-id="282d4-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="282d4-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="282d4-161">I **SP** initierade läge, kontrollera **visa avancerade inställningar för URL: en** och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="282d4-161">In **SP** initiated mode, check **Show advanced URL settings** and  perform hello following step :</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_url2.png)
    
    <span data-ttu-id="282d4-163">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="282d4-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="282d4-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="282d4-164">These values are not real.</span></span> <span data-ttu-id="282d4-165">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="282d4-165">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="282d4-166">Dessa värden tas emot under hello konfigurationen av bambu plugin-programmet som beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="282d4-166">These values are recieved during hello configuration of Bamboo plugin which is explained later in hello tutorial.</span></span>

5. <span data-ttu-id="282d4-167">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="282d4-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_certificate.png) 

6. <span data-ttu-id="282d4-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="282d4-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="282d4-171">Logga in tooyour bambu på lokal server som administratör i ett annat webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="282d4-171">In a different web browser window, log in tooyour Bamboo  on premise server as an administrator.</span></span>

8. <span data-ttu-id="282d4-172">Hovra över kugge och klicka på hello **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="282d4-172">Hover on cog and click hello **Add-ons**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon1.png)

9. <span data-ttu-id="282d4-174">Klicka under tillägg fliken avsnitt **söka efter nya tillägg**.</span><span class="sxs-lookup"><span data-stu-id="282d4-174">Under Add-ons tab section, click **Find new add-ons**.</span></span> <span data-ttu-id="282d4-175">Sök **Kantega SSO för bambu (SAML & Kerberos)** och på **installera** knappen tooinstall hello ny SAML-plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="282d4-175">Search **Kantega SSO for Bamboo (SAML & Kerberos)** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon2.png)

10. <span data-ttu-id="282d4-177">hello plugin installationen startar.</span><span class="sxs-lookup"><span data-stu-id="282d4-177">hello plugin installation will start.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon21.png)

11. <span data-ttu-id="282d4-179">När hello installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="282d4-179">Once hello installation is complete.</span></span> <span data-ttu-id="282d4-180">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="282d4-180">Click **Close**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon33.png)

12. <span data-ttu-id="282d4-182">Klicka på **Hantera**.</span><span class="sxs-lookup"><span data-stu-id="282d4-182">Click **Manage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon34.png)
    
13. <span data-ttu-id="282d4-184">Klicka på **konfigurera** tooconfigure hello nytt plugin-program.</span><span class="sxs-lookup"><span data-stu-id="282d4-184">Click **Configure** tooconfigure hello new plugin.</span></span>  

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon3.png)

14. <span data-ttu-id="282d4-186">I hello **SAML** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="282d4-186">In hello **SAML** section.</span></span> <span data-ttu-id="282d4-187">Välj **Azure Active Directory (AD Azure)** från hello **Lägg till identitetsleverantör** listrutan.</span><span class="sxs-lookup"><span data-stu-id="282d4-187">Select **Azure Active Directory (Azure AD)** from hello **Add identity provider** dropdown.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon4.png)

15. <span data-ttu-id="282d4-189">Välj prenumerationsnivån som **grundläggande**.</span><span class="sxs-lookup"><span data-stu-id="282d4-189">Select subscription level as **Basic**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon5.png)

16. <span data-ttu-id="282d4-191">På hello **appegenskaper** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="282d4-191">On hello **App properties** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon6.png)

    <span data-ttu-id="282d4-193">a.</span><span class="sxs-lookup"><span data-stu-id="282d4-193">a.</span></span> <span data-ttu-id="282d4-194">Kopiera hello **App-ID URI** värde och använda det som **identifierare, Reply URL och inloggnings-URL** på hello **Kantega SSO bambu domän och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="282d4-194">Copy hello **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on hello **Kantega SSO for Bamboo Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="282d4-195">b.</span><span class="sxs-lookup"><span data-stu-id="282d4-195">b.</span></span> <span data-ttu-id="282d4-196">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="282d4-196">Click **Next**.</span></span>

17. <span data-ttu-id="282d4-197">På hello **Metadata importera** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="282d4-197">On hello **Metadata import** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon7.png)

    <span data-ttu-id="282d4-199">a.</span><span class="sxs-lookup"><span data-stu-id="282d4-199">a.</span></span> <span data-ttu-id="282d4-200">Välj **metadatafil på datorn**, och överföra metadata-fil som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="282d4-200">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="282d4-201">b.</span><span class="sxs-lookup"><span data-stu-id="282d4-201">b.</span></span> <span data-ttu-id="282d4-202">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="282d4-202">Click **Next**.</span></span>

18. <span data-ttu-id="282d4-203">På hello **och enkel inloggning** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="282d4-203">On hello **Name and SSO location** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon8.png)

    <span data-ttu-id="282d4-205">a.</span><span class="sxs-lookup"><span data-stu-id="282d4-205">a.</span></span> <span data-ttu-id="282d4-206">Lägg till namnet på hello identitetsleverantör i **identitet providernamn** textruta (t.ex Azure AD).</span><span class="sxs-lookup"><span data-stu-id="282d4-206">Add Name of hello Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="282d4-207">b.</span><span class="sxs-lookup"><span data-stu-id="282d4-207">b.</span></span> <span data-ttu-id="282d4-208">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="282d4-208">Click **Next**.</span></span>

19. <span data-ttu-id="282d4-209">Kontrollera hello signeringscertifikat och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="282d4-209">Verify hello Signing certificate and click **Next**.</span></span>    

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon9.png)

20. <span data-ttu-id="282d4-211">På hello **bambu användarkonton** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="282d4-211">On hello **Bamboo user accounts** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon10.png)

    <span data-ttu-id="282d4-213">a.</span><span class="sxs-lookup"><span data-stu-id="282d4-213">a.</span></span> <span data-ttu-id="282d4-214">Välj **skapa användare i Bambus interna katalogen om det behövs** och ange hello rätt namn i hello gruppen för användare (kan vara flera Nej.</span><span class="sxs-lookup"><span data-stu-id="282d4-214">Select **Create users in Bamboo's internal Directory if needed** and enter hello appropriate name of hello group for users (can be multiple no.</span></span> <span data-ttu-id="282d4-215">av (grupper avgränsade med kommatecken).</span><span class="sxs-lookup"><span data-stu-id="282d4-215">of groups separated by comma).</span></span>

    <span data-ttu-id="282d4-216">b.</span><span class="sxs-lookup"><span data-stu-id="282d4-216">b.</span></span> <span data-ttu-id="282d4-217">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="282d4-217">Click **Next**.</span></span>

21. <span data-ttu-id="282d4-218">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="282d4-218">Click **Finish**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon11.png)

22. <span data-ttu-id="282d4-220">På hello **kända domäner för Azure AD** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="282d4-220">On hello **Known domains for Azure AD** section, perform following steps:</span></span>   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/addon12.png)

    <span data-ttu-id="282d4-222">a.</span><span class="sxs-lookup"><span data-stu-id="282d4-222">a.</span></span> <span data-ttu-id="282d4-223">Välj **kända domäner** hello vänstra panelen av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="282d4-223">Select **Known domains** from hello left panel of hello page.</span></span>

    <span data-ttu-id="282d4-224">b.</span><span class="sxs-lookup"><span data-stu-id="282d4-224">b.</span></span> <span data-ttu-id="282d4-225">Ange domännamnet i hello **kända domäner** textruta.</span><span class="sxs-lookup"><span data-stu-id="282d4-225">Enter domain name in hello **Known domains** textbox.</span></span>

    <span data-ttu-id="282d4-226">c.</span><span class="sxs-lookup"><span data-stu-id="282d4-226">c.</span></span> <span data-ttu-id="282d4-227">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="282d4-227">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="282d4-228">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="282d4-228">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="282d4-229">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="282d4-229">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="282d4-230">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="282d4-230">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="282d4-231">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="282d4-231">Creating an Azure AD test user</span></span>
<span data-ttu-id="282d4-232">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="282d4-232">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="282d4-234">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="282d4-234">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="282d4-235">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="282d4-235">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbamboo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="282d4-237">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="282d4-237">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbamboo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="282d4-239">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="282d4-239">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbamboo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="282d4-241">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="282d4-241">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforbamboo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="282d4-243">a.</span><span class="sxs-lookup"><span data-stu-id="282d4-243">a.</span></span> <span data-ttu-id="282d4-244">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="282d4-244">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="282d4-245">b.</span><span class="sxs-lookup"><span data-stu-id="282d4-245">b.</span></span> <span data-ttu-id="282d4-246">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="282d4-246">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="282d4-247">c.</span><span class="sxs-lookup"><span data-stu-id="282d4-247">c.</span></span> <span data-ttu-id="282d4-248">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="282d4-248">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="282d4-249">d.</span><span class="sxs-lookup"><span data-stu-id="282d4-249">d.</span></span> <span data-ttu-id="282d4-250">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="282d4-250">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-bamboo-test-user"></a><span data-ttu-id="282d4-251">Skapa en Kantega SSO för bambu testanvändare</span><span class="sxs-lookup"><span data-stu-id="282d4-251">Creating a Kantega SSO for Bamboo test user</span></span>

<span data-ttu-id="282d4-252">tooenable Azure AD-användare toolog i tooBamboo, måste de etableras i bambu.</span><span class="sxs-lookup"><span data-stu-id="282d4-252">tooenable Azure AD users toolog in tooBamboo, they must be provisioned into Bamboo.</span></span> <span data-ttu-id="282d4-253">Kantega SSO för bambu är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="282d4-253">In Kantega SSO for Bamboo, provisioning is a manual task.</span></span>

<span data-ttu-id="282d4-254">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="282d4-254">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="282d4-255">Logga in tooyour bambu på lokal server som administratör.</span><span class="sxs-lookup"><span data-stu-id="282d4-255">Log in tooyour Bamboo on premise server as an administrator.</span></span>

2. <span data-ttu-id="282d4-256">Hovra över kugge och klicka på hello **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="282d4-256">Hover on cog and click hello **User management**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforbamboo-tutorial/user1.png) 

3. <span data-ttu-id="282d4-258">Klicka på **användare**.</span><span class="sxs-lookup"><span data-stu-id="282d4-258">Click **Users**.</span></span> <span data-ttu-id="282d4-259">Under hello **Lägg till användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="282d4-259">Under hello **Add user** section, Perform follwing steps:</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforbamboo-tutorial/user2.png) 

    <span data-ttu-id="282d4-261">a.</span><span class="sxs-lookup"><span data-stu-id="282d4-261">a.</span></span> <span data-ttu-id="282d4-262">I hello **användarnamn** textruta hello e-post för användare som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="282d4-262">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="282d4-263">b.</span><span class="sxs-lookup"><span data-stu-id="282d4-263">b.</span></span> <span data-ttu-id="282d4-264">I hello **lösenord** textruta typen hello lösenord för användare.</span><span class="sxs-lookup"><span data-stu-id="282d4-264">In hello **Password** textbox, type hello password of user.</span></span>

    <span data-ttu-id="282d4-265">c.</span><span class="sxs-lookup"><span data-stu-id="282d4-265">c.</span></span> <span data-ttu-id="282d4-266">I hello **Bekräfta lösenord** textruta hello ange lösenordet för användaren.</span><span class="sxs-lookup"><span data-stu-id="282d4-266">In hello **Confirm Password** textbox, reenter hello password of user.</span></span>
    
    <span data-ttu-id="282d4-267">d.</span><span class="sxs-lookup"><span data-stu-id="282d4-267">d.</span></span> <span data-ttu-id="282d4-268">I hello **fullständiga namn** textruta typen hello användarens som Britta Simon fullständiga namn.</span><span class="sxs-lookup"><span data-stu-id="282d4-268">In hello **Full Name** textbox, type full name of hello user like Britta Simon.</span></span>
    
    <span data-ttu-id="282d4-269">e.</span><span class="sxs-lookup"><span data-stu-id="282d4-269">e.</span></span> <span data-ttu-id="282d4-270">I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="282d4-270">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="282d4-271">f.</span><span class="sxs-lookup"><span data-stu-id="282d4-271">f.</span></span> <span data-ttu-id="282d4-272">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="282d4-272">Click **Save**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="282d4-273">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="282d4-273">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="282d4-274">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooKantega SSO för bambu.</span><span class="sxs-lookup"><span data-stu-id="282d4-274">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKantega SSO for Bamboo.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="282d4-276">**tooassign Britta Simon tooKantega SSO för bambu, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="282d4-276">**tooassign Britta Simon tooKantega SSO for Bamboo, perform hello following steps:**</span></span>

1. <span data-ttu-id="282d4-277">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="282d4-277">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="282d4-279">Välj i listan med program hello **Kantega SSO för bambu**.</span><span class="sxs-lookup"><span data-stu-id="282d4-279">In hello applications list, select **Kantega SSO for Bamboo**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_app.png) 

3. <span data-ttu-id="282d4-281">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="282d4-281">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="282d4-283">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="282d4-283">Click **Add** button.</span></span> <span data-ttu-id="282d4-284">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="282d4-284">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="282d4-286">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="282d4-286">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="282d4-287">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="282d4-287">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="282d4-288">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="282d4-288">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="282d4-289">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="282d4-289">Testing single sign-on</span></span>

<span data-ttu-id="282d4-290">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="282d4-290">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="282d4-291">När du klickar på hello Kantega SSO för bambu panelen i hello åtkomstpanelen bör du hämta automatiskt inloggade tooyour Kantega SSO för bambu program.</span><span class="sxs-lookup"><span data-stu-id="282d4-291">When you click hello Kantega SSO for Bamboo tile in hello Access Panel, you should get automatically signed-on tooyour Kantega SSO for Bamboo application.</span></span>
<span data-ttu-id="282d4-292">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="282d4-292">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="282d4-293">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="282d4-293">Additional resources</span></span>

* [<span data-ttu-id="282d4-294">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="282d4-294">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="282d4-295">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="282d4-295">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforbamboo-tutorial/tutorial_general_203.png

