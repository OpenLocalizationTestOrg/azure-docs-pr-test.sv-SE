---
title: "Självstudier: Azure Active Directory-integrering med vice | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och vice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 42f65b758682ce2513b6bb38ef40a19f955c88c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a><span data-ttu-id="a4e38-103">Självstudier: Azure Active Directory-integrering med vice</span><span class="sxs-lookup"><span data-stu-id="a4e38-103">Tutorial: Azure Active Directory integration with Deputy</span></span>

<span data-ttu-id="a4e38-104">I kursen får du lära dig hur toointegrate vice med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a4e38-104">In this tutorial, you learn how toointegrate Deputy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a4e38-105">Integrera vice med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a4e38-105">Integrating Deputy with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a4e38-106">Du kan styra i Azure AD som har åtkomst till tooDeputy</span><span class="sxs-lookup"><span data-stu-id="a4e38-106">You can control in Azure AD who has access tooDeputy</span></span>
- <span data-ttu-id="a4e38-107">Du kan aktivera din användare tooautomatically get inloggade tooDeputy (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a4e38-107">You can enable your users tooautomatically get signed-on tooDeputy (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a4e38-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a4e38-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a4e38-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a4e38-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4e38-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a4e38-110">Prerequisites</span></span>

<span data-ttu-id="a4e38-111">tooconfigure Azure AD-integrering med vice måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a4e38-111">tooconfigure Azure AD integration with Deputy, you need hello following items:</span></span>

- <span data-ttu-id="a4e38-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a4e38-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a4e38-113">En vice enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a4e38-113">A Deputy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a4e38-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a4e38-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a4e38-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a4e38-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a4e38-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a4e38-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a4e38-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a4e38-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a4e38-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a4e38-118">Scenario description</span></span>
<span data-ttu-id="a4e38-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a4e38-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a4e38-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a4e38-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a4e38-121">Att lägga till vice från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a4e38-121">Adding Deputy from hello gallery</span></span>
2. <span data-ttu-id="a4e38-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a4e38-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-deputy-from-hello-gallery"></a><span data-ttu-id="a4e38-123">Att lägga till vice från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a4e38-123">Adding Deputy from hello gallery</span></span>
<span data-ttu-id="a4e38-124">tooconfigure hello integrering av vice i Azure AD, behöver du tooadd vice hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a4e38-124">tooconfigure hello integration of Deputy into Azure AD, you need tooadd Deputy from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a4e38-125">**tooadd vice från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a4e38-125">**tooadd Deputy from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4e38-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a4e38-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a4e38-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a4e38-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a4e38-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a4e38-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a4e38-133">Skriv i sökrutan hello **vice**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-133">In hello search box, type **Deputy**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_search.png)

5. <span data-ttu-id="a4e38-135">Markera hello resultat på panelen **vice**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a4e38-135">In hello results panel, select **Deputy**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a4e38-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a4e38-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a4e38-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med vice baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a4e38-138">In this section, you configure and test Azure AD single sign-on with Deputy based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a4e38-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i vice är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a4e38-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Deputy is tooa user in Azure AD.</span></span> <span data-ttu-id="a4e38-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i vice toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a4e38-140">In other words, a link relationship between an Azure AD user and hello related user in Deputy needs toobe established.</span></span>

<span data-ttu-id="a4e38-141">I vice, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a4e38-141">In Deputy, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a4e38-142">tooconfigure och testa Azure AD enkel inloggning med vice, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a4e38-142">tooconfigure and test Azure AD single sign-on with Deputy, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a4e38-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a4e38-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a4e38-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4e38-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a4e38-145">**[Skapa en testanvändare vice](#creating-a-deputy-test-user)**  -toohave en motsvarighet för Britta Simon i vice som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a4e38-145">**[Creating a Deputy test user](#creating-a-deputy-test-user)** - toohave a counterpart of Britta Simon in Deputy that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a4e38-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a4e38-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a4e38-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a4e38-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a4e38-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a4e38-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a4e38-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet vice.</span><span class="sxs-lookup"><span data-stu-id="a4e38-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Deputy application.</span></span>

<span data-ttu-id="a4e38-150">**tooconfigure Azure AD enkel inloggning med vice, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a4e38-150">**tooconfigure Azure AD single sign-on with Deputy, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4e38-151">I hello Azure-portalen på hello **vice** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-151">In hello Azure portal, on hello **Deputy** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a4e38-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a4e38-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_samlbase.png)

3. <span data-ttu-id="a4e38-155">På hello **URL: er och vice domän** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="a4e38-155">On hello **Deputy Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url1.png)

    <span data-ttu-id="a4e38-157">a.</span><span class="sxs-lookup"><span data-stu-id="a4e38-157">a.</span></span> <span data-ttu-id="a4e38-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="a4e38-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    <span data-ttu-id="a4e38-159">b.</span><span class="sxs-lookup"><span data-stu-id="a4e38-159">b.</span></span> <span data-ttu-id="a4e38-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="a4e38-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs.` |

4. <span data-ttu-id="a4e38-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="a4e38-162">Om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="a4e38-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url2.png)

    <span data-ttu-id="a4e38-164">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<your-subdomain>.<region>.deputy.com`</span><span class="sxs-lookup"><span data-stu-id="a4e38-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<your-subdomain>.<region>.deputy.com`</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="a4e38-165">Vice region suffix är valfritt eller den ska använda något av följande: Australien | na | Europa | som | la | af | en | ent Australien | ent na | ent Europa | ent-som | överordnad la | överordnad af | en överordnad</span><span class="sxs-lookup"><span data-stu-id="a4e38-165">Deputy region suffix is optional, or it should use one of these: au | na | eu |as |la |af |an |ent-au |ent-na |ent-eu |ent-as | ent-la | ent-af | ent-an</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a4e38-166">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a4e38-166">These values are not real.</span></span> <span data-ttu-id="a4e38-167">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="a4e38-167">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="a4e38-168">Kontakta [vice supportteamet](https://www.deputy.com/call-centers-customer-support-scheduling-software) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a4e38-168">Contact [Deputy support team](https://www.deputy.com/call-centers-customer-support-scheduling-software) tooget these values.</span></span> 

5. <span data-ttu-id="a4e38-169">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a4e38-169">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_certificate.png) 

6. <span data-ttu-id="a4e38-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a4e38-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="a4e38-173">På hello **vice Configuration** klickar du på **konfigurera vice** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a4e38-173">On hello **Deputy Configuration** section, click **Configure Deputy** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a4e38-174">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a4e38-174">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_configure.png) 

8. <span data-ttu-id="a4e38-176">Navigera toohello följande URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span><span class="sxs-lookup"><span data-stu-id="a4e38-176">Navigate toohello following URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span></span> <span data-ttu-id="a4e38-177">Gå för**säkerhetsinställningar** och på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-177">Go too**Security Settings** and click **Edit**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

9. <span data-ttu-id="a4e38-179">På den här **säkerhetsinställningar** utför nedanstående steg.</span><span class="sxs-lookup"><span data-stu-id="a4e38-179">On this **Security Settings** page, perform below steps.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)
    
    <span data-ttu-id="a4e38-181">a.</span><span class="sxs-lookup"><span data-stu-id="a4e38-181">a.</span></span> <span data-ttu-id="a4e38-182">Aktivera **inloggning via sociala medier**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-182">Enable **Social Login**.</span></span>
   
    <span data-ttu-id="a4e38-183">b.</span><span class="sxs-lookup"><span data-stu-id="a4e38-183">b.</span></span> <span data-ttu-id="a4e38-184">Öppna din Base64-kodat certifikat hämtas från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **OpenSSL certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="a4e38-184">Open your Base64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **OpenSSL Certificate** textbox.</span></span>
   
    <span data-ttu-id="a4e38-185">c.</span><span class="sxs-lookup"><span data-stu-id="a4e38-185">c.</span></span> <span data-ttu-id="a4e38-186">Skriv i hello SAML SSO URL: en textruta`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span><span class="sxs-lookup"><span data-stu-id="a4e38-186">In hello SAML SSO URL textbox, type `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span></span>
    
    <span data-ttu-id="a4e38-187">d.</span><span class="sxs-lookup"><span data-stu-id="a4e38-187">d.</span></span> <span data-ttu-id="a4e38-188">Ersätt i hello SAML SSO URL: en textruta `<your subdomain>` med din underdomänen.</span><span class="sxs-lookup"><span data-stu-id="a4e38-188">In hello SAML SSO URL textbox, replace `<your subdomain>` with your subdomain.</span></span>
   
    <span data-ttu-id="a4e38-189">e.</span><span class="sxs-lookup"><span data-stu-id="a4e38-189">e.</span></span> <span data-ttu-id="a4e38-190">Ersätt i hello SAML SSO URL: en textruta `<saml sso url>` med hello **SAML inloggning tjänst-URL för enkel** du har kopierat från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a4e38-190">In hello SAML SSO URL textbox, replace `<saml sso url>` with hello **SAML Single Sign-On Service URL** you have copied from hello Azure portal.</span></span>
   
    <span data-ttu-id="a4e38-191">f.</span><span class="sxs-lookup"><span data-stu-id="a4e38-191">f.</span></span> <span data-ttu-id="a4e38-192">Klicka på **Spara inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-192">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="a4e38-193">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a4e38-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a4e38-194">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a4e38-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a4e38-195">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a4e38-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a4e38-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4e38-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="a4e38-197">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4e38-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a4e38-199">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a4e38-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4e38-200">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a4e38-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a4e38-202">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a4e38-204">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a4e38-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a4e38-206">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a4e38-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a4e38-208">a.</span><span class="sxs-lookup"><span data-stu-id="a4e38-208">a.</span></span> <span data-ttu-id="a4e38-209">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a4e38-210">b.</span><span class="sxs-lookup"><span data-stu-id="a4e38-210">b.</span></span> <span data-ttu-id="a4e38-211">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a4e38-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a4e38-212">c.</span><span class="sxs-lookup"><span data-stu-id="a4e38-212">c.</span></span> <span data-ttu-id="a4e38-213">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a4e38-214">d.</span><span class="sxs-lookup"><span data-stu-id="a4e38-214">d.</span></span> <span data-ttu-id="a4e38-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-215">Click **Create**.</span></span>
 
### <a name="creating-a-deputy-test-user"></a><span data-ttu-id="a4e38-216">Skapa en vice testanvändare</span><span class="sxs-lookup"><span data-stu-id="a4e38-216">Creating a Deputy test user</span></span>

<span data-ttu-id="a4e38-217">tooenable Azure AD-användare toolog i tooDeputy, måste de etableras i vice.</span><span class="sxs-lookup"><span data-stu-id="a4e38-217">tooenable Azure AD users toolog in tooDeputy, they must be provisioned into Deputy.</span></span> <span data-ttu-id="a4e38-218">Vid vice är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a4e38-218">In case of Deputy, provisioning is a manual task.</span></span>

#### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="a4e38-219">tooprovision ett användarkonto, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="a4e38-219">tooprovision a user account, perform hello following steps:</span></span>
1. <span data-ttu-id="a4e38-220">Logga in tooyour vice företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="a4e38-220">Log in tooyour Deputy company site as an administrator.</span></span>

2. <span data-ttu-id="a4e38-221">Klicka på hello övre navigeringsfönstret, **personer**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-221">On hello top navigation pane, click **People**.</span></span>
   
   <span data-ttu-id="a4e38-222">![Personer](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "personer")</span><span class="sxs-lookup"><span data-stu-id="a4e38-222">![People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "People")</span></span>

3. <span data-ttu-id="a4e38-223">Klicka på hello **lägga till personer** och klicka sedan på **lägga till en enda person**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-223">Click hello **Add People** button and click **Add a single person**.</span></span>
   
   <span data-ttu-id="a4e38-224">![Lägg till personer](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "lägga till personer")</span><span class="sxs-lookup"><span data-stu-id="a4e38-224">![Add People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Add People")</span></span>

4. <span data-ttu-id="a4e38-225">Utför följande steg hello och klickar på **Spara & bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-225">Perform hello following steps and click **Save & Invite**.</span></span>
   
   <span data-ttu-id="a4e38-226">![Ny användare](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="a4e38-226">![New User](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "New User")</span></span>

   <span data-ttu-id="a4e38-227">a.</span><span class="sxs-lookup"><span data-stu-id="a4e38-227">a.</span></span> <span data-ttu-id="a4e38-228">I hello **namn** textruta, ange namnet på hello användaren som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-228">In hello **Name** textbox, type name of hello user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="a4e38-229">b.</span><span class="sxs-lookup"><span data-stu-id="a4e38-229">b.</span></span> <span data-ttu-id="a4e38-230">I hello **e-post** textruta typen hello e-postadressen för en Azure AD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="a4e38-230">In hello **Email** textbox, type hello email address of an Azure AD account you want tooprovision.</span></span>
   
   <span data-ttu-id="a4e38-231">c.</span><span class="sxs-lookup"><span data-stu-id="a4e38-231">c.</span></span> <span data-ttu-id="a4e38-232">I hello **fungerar på** textruta typnamn hello företag.</span><span class="sxs-lookup"><span data-stu-id="a4e38-232">In hello **Work at** textbox, type hello business name.</span></span>
   
   <span data-ttu-id="a4e38-233">d.</span><span class="sxs-lookup"><span data-stu-id="a4e38-233">d.</span></span> <span data-ttu-id="a4e38-234">Klicka på **Spara & bjuda in** knappen.</span><span class="sxs-lookup"><span data-stu-id="a4e38-234">Click **Save & Invite** button.</span></span>

5. <span data-ttu-id="a4e38-235">hello AAD användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="a4e38-235">hello AAD account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span> <span data-ttu-id="a4e38-236">Du kan använda något annat vice användarens konto skapas verktyg eller API: er som tillhandahålls av vice tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="a4e38-236">You can use any other Deputy user account creation tools or APIs provided by Deputy tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a4e38-237">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4e38-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a4e38-238">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDeputy.</span><span class="sxs-lookup"><span data-stu-id="a4e38-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDeputy.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a4e38-240">**tooassign Britta Simon tooDeputy utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a4e38-240">**tooassign Britta Simon tooDeputy, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4e38-241">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a4e38-243">Välj i listan med program hello **vice**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-243">In hello applications list, select **Deputy**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_app.png) 

3. <span data-ttu-id="a4e38-245">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a4e38-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a4e38-247">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a4e38-247">Click **Add** button.</span></span> <span data-ttu-id="a4e38-248">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a4e38-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a4e38-250">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a4e38-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a4e38-251">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a4e38-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a4e38-252">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a4e38-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a4e38-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a4e38-253">Testing single sign-on</span></span>

<span data-ttu-id="a4e38-254">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a4e38-254">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="a4e38-255">Du bör få automatiskt inloggade tooyour vice programmet när du klickar på hello vice panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a4e38-255">When you click hello Deputy tile in hello Access Panel, you should get automatically signed-on tooyour Deputy application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a4e38-256">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a4e38-256">Additional resources</span></span>

* [<span data-ttu-id="a4e38-257">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a4e38-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a4e38-258">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a4e38-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png

