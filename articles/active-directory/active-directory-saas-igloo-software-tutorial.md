---
title: "Självstudier: Azure Active Directory-integrering med Igloo programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Igloo programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 406405d4faa6e56f1005a61e69a29ef2ef2eb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a><span data-ttu-id="81f89-103">Självstudier: Azure Active Directory-integrering med Igloo programvara</span><span class="sxs-lookup"><span data-stu-id="81f89-103">Tutorial: Azure Active Directory integration with Igloo Software</span></span>

<span data-ttu-id="81f89-104">I kursen får du lära dig hur toointegrate Igloo programvara med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="81f89-104">In this tutorial, you learn how toointegrate Igloo Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="81f89-105">Integrera Igloo programvara med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="81f89-105">Integrating Igloo Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="81f89-106">Du kan styra i Azure AD som har åtkomst tooIgloo programvara</span><span class="sxs-lookup"><span data-stu-id="81f89-106">You can control in Azure AD who has access tooIgloo Software</span></span>
- <span data-ttu-id="81f89-107">Du kan aktivera din användare tooautomatically get inloggade tooIgloo programvara (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="81f89-107">You can enable your users tooautomatically get signed-on tooIgloo Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="81f89-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="81f89-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="81f89-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="81f89-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81f89-110">Krav</span><span class="sxs-lookup"><span data-stu-id="81f89-110">Prerequisites</span></span>

<span data-ttu-id="81f89-111">tooconfigure Azure AD-integrering med Igloo programvara, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="81f89-111">tooconfigure Azure AD integration with Igloo Software, you need hello following items:</span></span>

- <span data-ttu-id="81f89-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="81f89-112">An Azure AD subscription</span></span>
- <span data-ttu-id="81f89-113">En Igloo programvara enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="81f89-113">An Igloo Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="81f89-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="81f89-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="81f89-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="81f89-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="81f89-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="81f89-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="81f89-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="81f89-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="81f89-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="81f89-118">Scenario description</span></span>
<span data-ttu-id="81f89-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="81f89-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="81f89-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="81f89-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="81f89-121">Lägga till Igloo programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="81f89-121">Adding Igloo Software from hello gallery</span></span>
2. <span data-ttu-id="81f89-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="81f89-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-igloo-software-from-hello-gallery"></a><span data-ttu-id="81f89-123">Lägga till Igloo programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="81f89-123">Adding Igloo Software from hello gallery</span></span>
<span data-ttu-id="81f89-124">tooconfigure hello integrering av Igloo programvara i Azure AD, behöver du tooadd Igloo programvara från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="81f89-124">tooconfigure hello integration of Igloo Software into Azure AD, you need tooadd Igloo Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="81f89-125">**tooadd Igloo programvara från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="81f89-125">**tooadd Igloo Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="81f89-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="81f89-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="81f89-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="81f89-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="81f89-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="81f89-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="81f89-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81f89-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="81f89-133">Skriv i sökrutan hello **Igloo programvara**.</span><span class="sxs-lookup"><span data-stu-id="81f89-133">In hello search box, type **Igloo Software**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. <span data-ttu-id="81f89-135">Markera hello resultat på panelen **Igloo programvara**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="81f89-135">In hello results panel, select **Igloo Software**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="81f89-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="81f89-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="81f89-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Igloo programvara baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="81f89-138">In this section, you configure and test Azure AD single sign-on with Igloo Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="81f89-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Igloo programvara är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="81f89-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Igloo Software is tooa user in Azure AD.</span></span> <span data-ttu-id="81f89-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Igloo programvara toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="81f89-140">In other words, a link relationship between an Azure AD user and hello related user in Igloo Software needs toobe established.</span></span>

<span data-ttu-id="81f89-141">I Igloo programvara, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="81f89-141">In Igloo Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="81f89-142">tooconfigure och testa Azure AD enkel inloggning med Igloo programvara, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="81f89-142">tooconfigure and test Azure AD single sign-on with Igloo Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="81f89-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="81f89-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="81f89-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="81f89-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="81f89-145">**[Skapa en testanvändare Igloo programvara](#creating-an-igloo-software-test-user)**  -toohave en motsvarighet för Britta Simon Igloo programvara som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="81f89-145">**[Creating an Igloo Software test user](#creating-an-igloo-software-test-user)** - toohave a counterpart of Britta Simon in Igloo Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="81f89-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="81f89-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="81f89-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="81f89-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="81f89-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="81f89-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="81f89-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Igloo programmet.</span><span class="sxs-lookup"><span data-stu-id="81f89-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Igloo Software application.</span></span>

<span data-ttu-id="81f89-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Igloo programvara:**</span><span class="sxs-lookup"><span data-stu-id="81f89-150">**tooconfigure Azure AD single sign-on with Igloo Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="81f89-151">I hello Azure-portalen på hello **Igloo programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="81f89-151">In hello Azure portal, on hello **Igloo Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="81f89-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="81f89-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. <span data-ttu-id="81f89-155">På hello **Igloo programvara domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="81f89-155">On hello **Igloo Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    <span data-ttu-id="81f89-157">a.</span><span class="sxs-lookup"><span data-stu-id="81f89-157">a.</span></span> <span data-ttu-id="81f89-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.igloocommmunities.com`</span><span class="sxs-lookup"><span data-stu-id="81f89-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.igloocommmunities.com`</span></span>

    <span data-ttu-id="81f89-159">b.</span><span class="sxs-lookup"><span data-stu-id="81f89-159">b.</span></span> <span data-ttu-id="81f89-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="81f89-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    <span data-ttu-id="81f89-161">c.</span><span class="sxs-lookup"><span data-stu-id="81f89-161">c.</span></span> <span data-ttu-id="81f89-162">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="81f89-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="81f89-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="81f89-163">These values are not real.</span></span> <span data-ttu-id="81f89-164">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="81f89-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="81f89-165">Kontakta [Igloo Programvaruklienten supportteamet](https://www.igloosoftware.com/services/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="81f89-165">Contact [Igloo Software Client support team](https://www.igloosoftware.com/services/support) tooget these values.</span></span> 

4. <span data-ttu-id="81f89-166">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="81f89-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. <span data-ttu-id="81f89-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="81f89-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="81f89-170">På hello **Igloo programvarukonfiguration** klickar du på **konfigurera Igloo programvara** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="81f89-170">On hello **Igloo Software Configuration** section, click **Configure Igloo Software** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="81f89-171">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="81f89-171">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. <span data-ttu-id="81f89-173">Logga in tooyour Igloo programvara företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="81f89-173">In a different web browser window, log in tooyour Igloo Software company site as an administrator.</span></span>

8. <span data-ttu-id="81f89-174">Gå toohello **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="81f89-174">Go toohello **Control Panel**.</span></span>
   
     <span data-ttu-id="81f89-175">![Kontrollpanelen](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Kontrollpanelen")</span><span class="sxs-lookup"><span data-stu-id="81f89-175">![Control Panel](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Control Panel")</span></span>

9. <span data-ttu-id="81f89-176">I hello **medlemskap** klickar du på **logga inställningarna**.</span><span class="sxs-lookup"><span data-stu-id="81f89-176">In hello **Membership** tab, click **Sign In Settings**.</span></span>
   
    <span data-ttu-id="81f89-177">![Logga in inställningar](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "logga in inställningarna")</span><span class="sxs-lookup"><span data-stu-id="81f89-177">![Sign in Settings](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "Sign in Settings")</span></span>

10. <span data-ttu-id="81f89-178">I hello SAML-konfigurationsavsnittet, klickar du på **konfigurera SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="81f89-178">In hello SAML Configuration section, click **Configure SAML Authentication**.</span></span>
   
    <span data-ttu-id="81f89-179">![SAML-konfiguration](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="81f89-179">![SAML Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML Configuration")</span></span>
   
11. <span data-ttu-id="81f89-180">I hello **Allmän konfiguration** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="81f89-180">In hello **General Configuration** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="81f89-181">![Allmän konfiguration](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "Allmän konfiguration")</span><span class="sxs-lookup"><span data-stu-id="81f89-181">![General Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "General Configuration")</span></span>

    <span data-ttu-id="81f89-182">a.</span><span class="sxs-lookup"><span data-stu-id="81f89-182">a.</span></span> <span data-ttu-id="81f89-183">I hello **anslutningsnamn** textruta skriver du ett eget namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="81f89-183">In hello **Connection Name** textbox, type a custom name for your configuration.</span></span>
   
    <span data-ttu-id="81f89-184">b.</span><span class="sxs-lookup"><span data-stu-id="81f89-184">b.</span></span> <span data-ttu-id="81f89-185">I hello **IdP inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="81f89-185">In hello **IdP Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="81f89-186">c.</span><span class="sxs-lookup"><span data-stu-id="81f89-186">c.</span></span> <span data-ttu-id="81f89-187">I hello **IdP logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="81f89-187">In hello **IdP Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="81f89-188">d.</span><span class="sxs-lookup"><span data-stu-id="81f89-188">d.</span></span> <span data-ttu-id="81f89-189">Välj **logga ut svar och typ av begäran HTTP** som **efter**.</span><span class="sxs-lookup"><span data-stu-id="81f89-189">Select **Logout Response and Request HTTP Type** as **POST**.</span></span>
   
    <span data-ttu-id="81f89-190">e.</span><span class="sxs-lookup"><span data-stu-id="81f89-190">e.</span></span> <span data-ttu-id="81f89-191">Öppna din **Base64-** kodat certifikat i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **certifikat med offentlig** textruta.</span><span class="sxs-lookup"><span data-stu-id="81f89-191">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **Public Certificate** textbox.</span></span>
    
12. <span data-ttu-id="81f89-192">I hello **svar och Autentiseringskonfiguration**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="81f89-192">In hello **Response and Authentication Configuration**, perform hello following steps:</span></span>
    
    <span data-ttu-id="81f89-193">![Svar och Autentiseringskonfiguration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "svar och Autentiseringskonfiguration")</span><span class="sxs-lookup"><span data-stu-id="81f89-193">![Response and Authentication Configuration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Response and Authentication Configuration")</span></span>
  
      <span data-ttu-id="81f89-194">a.</span><span class="sxs-lookup"><span data-stu-id="81f89-194">a.</span></span> <span data-ttu-id="81f89-195">Som **identitetsleverantör**väljer **Microsoft ADFS**.</span><span class="sxs-lookup"><span data-stu-id="81f89-195">As **Identity Provider**, select **Microsoft ADFS**.</span></span>
      
      <span data-ttu-id="81f89-196">b.</span><span class="sxs-lookup"><span data-stu-id="81f89-196">b.</span></span> <span data-ttu-id="81f89-197">Som **Ämnesidentifierarens typ**väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="81f89-197">As **Identifier Type**, select **Email Address**.</span></span> 

      <span data-ttu-id="81f89-198">c.</span><span class="sxs-lookup"><span data-stu-id="81f89-198">c.</span></span> <span data-ttu-id="81f89-199">I hello **e-attributet** textruta typen **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="81f89-199">In hello **Email Attribute** textbox, type **emailaddress**.</span></span>

      <span data-ttu-id="81f89-200">d.</span><span class="sxs-lookup"><span data-stu-id="81f89-200">d.</span></span> <span data-ttu-id="81f89-201">I hello **förnamn attributet** textruta typen **givenname**.</span><span class="sxs-lookup"><span data-stu-id="81f89-201">In hello **First Name Attribute** textbox, type **givenname**.</span></span>

      <span data-ttu-id="81f89-202">e.</span><span class="sxs-lookup"><span data-stu-id="81f89-202">e.</span></span> <span data-ttu-id="81f89-203">I hello **senaste namnattributet** textruta typen **efternamn**.</span><span class="sxs-lookup"><span data-stu-id="81f89-203">In hello **Last Name Attribute** textbox, type **surname**.</span></span>

13. <span data-ttu-id="81f89-204">Utför hello följande steg toocomplete hello konfiguration:</span><span class="sxs-lookup"><span data-stu-id="81f89-204">Perform hello following steps toocomplete hello configuration:</span></span>
    
    <span data-ttu-id="81f89-205">![Skapa användare på inloggning](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "skapa användare på Logga in")</span><span class="sxs-lookup"><span data-stu-id="81f89-205">![User creation on Sign in](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "User creation on Sign in")</span></span> 

     <span data-ttu-id="81f89-206">a.</span><span class="sxs-lookup"><span data-stu-id="81f89-206">a.</span></span> <span data-ttu-id="81f89-207">Som **skapa användare på inloggning**väljer **skapa en ny användare på din plats när de loggar in**.</span><span class="sxs-lookup"><span data-stu-id="81f89-207">As **User creation on Sign in**, select **Create a new user in your site when they sign in**.</span></span>

     <span data-ttu-id="81f89-208">b.</span><span class="sxs-lookup"><span data-stu-id="81f89-208">b.</span></span> <span data-ttu-id="81f89-209">Som **inloggning inställningar**väljer **Använd SAML-knappen ”Logga in” skärmen**.</span><span class="sxs-lookup"><span data-stu-id="81f89-209">As **Sign in Settings**, select **Use SAML button on “Sign in” screen**.</span></span>

     <span data-ttu-id="81f89-210">c.</span><span class="sxs-lookup"><span data-stu-id="81f89-210">c.</span></span> <span data-ttu-id="81f89-211">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="81f89-211">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="81f89-212">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="81f89-212">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="81f89-213">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="81f89-213">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="81f89-214">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="81f89-214">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="81f89-215">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="81f89-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="81f89-216">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="81f89-216">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="81f89-218">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="81f89-218">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="81f89-219">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="81f89-219">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="81f89-221">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="81f89-221">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="81f89-223">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81f89-223">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="81f89-225">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="81f89-225">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="81f89-227">a.</span><span class="sxs-lookup"><span data-stu-id="81f89-227">a.</span></span> <span data-ttu-id="81f89-228">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="81f89-228">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="81f89-229">b.</span><span class="sxs-lookup"><span data-stu-id="81f89-229">b.</span></span> <span data-ttu-id="81f89-230">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="81f89-230">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="81f89-231">c.</span><span class="sxs-lookup"><span data-stu-id="81f89-231">c.</span></span> <span data-ttu-id="81f89-232">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="81f89-232">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="81f89-233">d.</span><span class="sxs-lookup"><span data-stu-id="81f89-233">d.</span></span> <span data-ttu-id="81f89-234">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="81f89-234">Click **Create**.</span></span>
 
### <a name="creating-an-igloo-software-test-user"></a><span data-ttu-id="81f89-235">Skapa en testanvändare Igloo programvara</span><span class="sxs-lookup"><span data-stu-id="81f89-235">Creating an Igloo Software test user</span></span>

<span data-ttu-id="81f89-236">Det finns inga uppgiften för du tooconfigure användaretablering tooIgloo programvara.</span><span class="sxs-lookup"><span data-stu-id="81f89-236">There is no action item for you tooconfigure user provisioning tooIgloo Software.</span></span>  

<span data-ttu-id="81f89-237">När en tilldelad användare försöker toolog i tooIgloo programvara med hjälp av hello åtkomstpanelen, Igloo programvara kontrollerar om hello användaren finns.</span><span class="sxs-lookup"><span data-stu-id="81f89-237">When an assigned user tries toolog in tooIgloo Software using hello access panel, Igloo Software checks whether hello user exists.</span></span>  <span data-ttu-id="81f89-238">Om det finns inget användarkonto ännu, skapas den automatiskt med Igloo program.</span><span class="sxs-lookup"><span data-stu-id="81f89-238">If there is no user account available yet, it is automatically created by Igloo Software.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="81f89-239">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="81f89-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="81f89-240">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIgloo programvara.</span><span class="sxs-lookup"><span data-stu-id="81f89-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIgloo Software.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="81f89-242">**tooassign Britta Simon tooIgloo programvara, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="81f89-242">**tooassign Britta Simon tooIgloo Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="81f89-243">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="81f89-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="81f89-245">Välj i listan med program hello **Igloo programvara**.</span><span class="sxs-lookup"><span data-stu-id="81f89-245">In hello applications list, select **Igloo Software**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. <span data-ttu-id="81f89-247">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="81f89-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="81f89-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="81f89-249">Click **Add** button.</span></span> <span data-ttu-id="81f89-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81f89-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="81f89-252">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="81f89-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="81f89-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81f89-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="81f89-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81f89-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="81f89-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="81f89-255">Testing single sign-on</span></span>

<span data-ttu-id="81f89-256">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="81f89-256">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="81f89-257">När du klickar på hello Igloo programvara panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Igloo program.</span><span class="sxs-lookup"><span data-stu-id="81f89-257">When you click hello Igloo Software tile in hello Access Panel, you should get automatically signed-on tooyour Igloo Software application.</span></span>
<span data-ttu-id="81f89-258">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="81f89-258">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="81f89-259">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="81f89-259">Additional resources</span></span>

* [<span data-ttu-id="81f89-260">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="81f89-260">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="81f89-261">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="81f89-261">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

