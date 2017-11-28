---
title: "Självstudier: Azure Active Directory-integrering med Menlo säkerhet | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Menlo säkerhet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: 193d12eedf31f4f08e1d141936d6e918c36a2109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a><span data-ttu-id="fd8d3-103">Självstudier: Azure Active Directory-integrering med Menlo säkerhet</span><span class="sxs-lookup"><span data-stu-id="fd8d3-103">Tutorial: Azure Active Directory integration with Menlo Security</span></span>

<span data-ttu-id="fd8d3-104">I kursen får du lära dig hur toointegrate Menlo säkerhet med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="fd8d3-104">In this tutorial, you learn how toointegrate Menlo Security with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fd8d3-105">Integrera Menlo säkerhet med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-105">Integrating Menlo Security with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fd8d3-106">Du kan styra i Azure AD som har åtkomst tooMenlo säkerhet</span><span class="sxs-lookup"><span data-stu-id="fd8d3-106">You can control in Azure AD who has access tooMenlo Security</span></span>
- <span data-ttu-id="fd8d3-107">Du kan aktivera din användare tooautomatically get inloggade tooMenlo säkerhet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="fd8d3-107">You can enable your users tooautomatically get signed-on tooMenlo Security (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fd8d3-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fd8d3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="fd8d3-109">Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="fd8d3-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fd8d3-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd8d3-111">Krav</span><span class="sxs-lookup"><span data-stu-id="fd8d3-111">Prerequisites</span></span>

<span data-ttu-id="fd8d3-112">tooconfigure Azure AD-integrering med Menlo säkerhet, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-112">tooconfigure Azure AD integration with Menlo Security, you need hello following items:</span></span>

- <span data-ttu-id="fd8d3-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fd8d3-113">An Azure AD subscription</span></span>
- <span data-ttu-id="fd8d3-114">En Menlo säkerhet enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="fd8d3-114">A Menlo Security single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fd8d3-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fd8d3-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fd8d3-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fd8d3-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fd8d3-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fd8d3-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="fd8d3-119">Scenario description</span></span>
<span data-ttu-id="fd8d3-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fd8d3-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fd8d3-122">Lägga till Menlo säkerhet från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fd8d3-122">Adding Menlo Security from hello gallery</span></span>
2. <span data-ttu-id="fd8d3-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd8d3-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-menlo-security-from-hello-gallery"></a><span data-ttu-id="fd8d3-124">Lägga till Menlo säkerhet från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fd8d3-124">Adding Menlo Security from hello gallery</span></span>
<span data-ttu-id="fd8d3-125">tooconfigure hello integrering av Menlo säkerhet i Azure AD, behöver du tooadd Menlo säkerhet hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-125">tooconfigure hello integration of Menlo Security into Azure AD, you need tooadd Menlo Security from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fd8d3-126">**tooadd Menlo säkerhet från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fd8d3-126">**tooadd Menlo Security from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fd8d3-127">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fd8d3-129">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fd8d3-130">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-130">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="fd8d3-132">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="fd8d3-134">Skriv i sökrutan hello **Menlo säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-134">In hello search box, type **Menlo Security**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_search.png)

5. <span data-ttu-id="fd8d3-136">Markera hello resultat på panelen **Menlo säkerhet**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-136">In hello results panel, select **Menlo Security**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fd8d3-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd8d3-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fd8d3-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Menlo säkerhet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-139">In this section, you configure and test Azure AD single sign-on with Menlo Security based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="fd8d3-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Menlo säkerhet är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Menlo Security is tooa user in Azure AD.</span></span> <span data-ttu-id="fd8d3-141">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Menlo säkerhet toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-141">In other words, a link relationship between an Azure AD user and hello related user in Menlo Security needs toobe established.</span></span>

<span data-ttu-id="fd8d3-142">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Menlo säkerhet.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Menlo Security.</span></span>

<span data-ttu-id="fd8d3-143">tooconfigure och testa Azure AD enkel inloggning med Menlo säkerhet, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-143">tooconfigure and test Azure AD single sign-on with Menlo Security, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fd8d3-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fd8d3-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fd8d3-146">**[Skapa en testanvändare Menlo säkerhet](#creating-a-menlo-security-test-user)**  -toohave en motsvarighet för Britta Simon i Menlo säkerhet som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-146">**[Creating a Menlo Security test user](#creating-a-menlo-security-test-user)** - toohave a counterpart of Britta Simon in Menlo Security that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fd8d3-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fd8d3-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fd8d3-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd8d3-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fd8d3-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Menlo säkerhetsprogram.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Menlo Security application.</span></span>

<span data-ttu-id="fd8d3-151">**Utför följande hello tooconfigure Azure AD enkel inloggning med Menlo säkerhet:**</span><span class="sxs-lookup"><span data-stu-id="fd8d3-151">**tooconfigure Azure AD single sign-on with Menlo Security, perform hello following steps:**</span></span>

1. <span data-ttu-id="fd8d3-152">I hello Azure-portalen på hello **Menlo säkerhet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-152">In hello Azure portal, on hello **Menlo Security** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="fd8d3-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

3. <span data-ttu-id="fd8d3-156">På hello **Menlo säkerhetsdomän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-156">On hello **Menlo Security Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    <span data-ttu-id="fd8d3-158">a.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-158">a.</span></span> <span data-ttu-id="fd8d3-159">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.menlosecurity.com/account/login`</span><span class="sxs-lookup"><span data-stu-id="fd8d3-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.menlosecurity.com/account/login`</span></span>

    <span data-ttu-id="fd8d3-160">b.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-160">b.</span></span> <span data-ttu-id="fd8d3-161">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="fd8d3-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fd8d3-162">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-162">These values are not hello real.</span></span> <span data-ttu-id="fd8d3-163">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-163">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="fd8d3-164">Kontakta [Menlo Säkerhetsklient supportteamet](https://www.menlosecurity.com/menlo-contact) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-164">Contact [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="fd8d3-165">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

5. <span data-ttu-id="fd8d3-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fd8d3-169">På hello **Menlo säkerhetskonfiguration** klickar du på **konfigurera säkerheten för Menlo** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-169">On hello **Menlo Security Configuration** section, click **Configure Menlo Security** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="fd8d3-170">Kopiera hello **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="fd8d3-170">Copy hello **SAML Entity ID**, and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

7. <span data-ttu-id="fd8d3-172">tooconfigure enkel inloggning på **Menlo säkerhet** sida, inloggning toohello **Menlo säkerhet** webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-172">tooconfigure single sign-on on **Menlo Security** side, login toohello **Menlo Security** website as an administrator.</span></span>

8. <span data-ttu-id="fd8d3-173">Under **inställningar** gå för**autentisering** och utföra följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-173">Under **Settings** go too**Authentication** and perform following actions:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/menlo_user_setup.png)

    <span data-ttu-id="fd8d3-175">a.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-175">a.</span></span> <span data-ttu-id="fd8d3-176">Markera kryssrutan hello **aktivera användarautentisering med SAML**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-176">Tick hello checkbox **Enable user authentication using SAML**.</span></span>

    <span data-ttu-id="fd8d3-177">b.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-177">b.</span></span> <span data-ttu-id="fd8d3-178">Välj **Tillåt extern åtkomst** för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-178">Select **Allow External Access** too**Yes**.</span></span>

    <span data-ttu-id="fd8d3-179">c.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-179">c.</span></span> <span data-ttu-id="fd8d3-180">Under **SAML-providern**väljer **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-180">Under **SAML Provider**, select **Azure Active Directory**.</span></span>

    <span data-ttu-id="fd8d3-181">d.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-181">d.</span></span> <span data-ttu-id="fd8d3-182">**SAML 2.0 Endpoint** : klistra in hello **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-182">**SAML 2.0 Endpoint** : Paste hello **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="fd8d3-183">e.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-183">e.</span></span> <span data-ttu-id="fd8d3-184">**Service-Identifier (utfärdaren)** : klistra in hello **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-184">**Service Identifier (Issuer)** : Paste hello **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="fd8d3-185">f.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-185">f.</span></span> <span data-ttu-id="fd8d3-186">**X.509-certifikat** : öppna hello **certifikat (Base64)** hämtas från hello Azure-portalen i anteckningar och klistra in den i den här rutan.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-186">**X.509 Certificate** : Open hello **Certificate (Base64)** downloaded from hello Azure Portal in notepad and paste it in this box.</span></span>

    <span data-ttu-id="fd8d3-187">g.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-187">g.</span></span> <span data-ttu-id="fd8d3-188">Klicka på **spara** toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-188">Click **Save** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="fd8d3-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="fd8d3-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fd8d3-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fd8d3-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fd8d3-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fd8d3-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd8d3-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="fd8d3-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="fd8d3-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fd8d3-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fd8d3-196">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fd8d3-198">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fd8d3-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fd8d3-202">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fd8d3-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fd8d3-204">a.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-204">a.</span></span> <span data-ttu-id="fd8d3-205">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fd8d3-206">b.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-206">b.</span></span> <span data-ttu-id="fd8d3-207">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fd8d3-208">c.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-208">c.</span></span> <span data-ttu-id="fd8d3-209">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="fd8d3-210">d.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-210">d.</span></span> <span data-ttu-id="fd8d3-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-211">Click **Create**.</span></span>
 
### <a name="creating-a-menlo-security-test-user"></a><span data-ttu-id="fd8d3-212">Skapa en testanvändare Menlo säkerhet</span><span class="sxs-lookup"><span data-stu-id="fd8d3-212">Creating a Menlo Security test user</span></span>
 
<span data-ttu-id="fd8d3-213">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Menlo säkerhet.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-213">In this section, you create a user called Britta Simon in Menlo Security.</span></span> <span data-ttu-id="fd8d3-214">Arbeta med [Menlo Säkerhetsklient supportteamet](https://www.menlosecurity.com/menlo-contact) tooadd hello användare i hello Menlo säkerhet plattform.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-214">Work with [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) tooadd hello users in hello Menlo Security platform.</span></span> <span data-ttu-id="fd8d3-215">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-215">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="fd8d3-216">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd8d3-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="fd8d3-217">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMenlo säkerhet.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMenlo Security.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="fd8d3-219">**tooassign Britta Simon tooMenlo säkerhet, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fd8d3-219">**tooassign Britta Simon tooMenlo Security, perform hello following steps:**</span></span>

1. <span data-ttu-id="fd8d3-220">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="fd8d3-222">Välj i listan med program hello **Menlo säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-222">In hello applications list, select **Menlo Security**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

3. <span data-ttu-id="fd8d3-224">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="fd8d3-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-226">Click **Add** button.</span></span> <span data-ttu-id="fd8d3-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="fd8d3-229">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fd8d3-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fd8d3-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fd8d3-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fd8d3-232">Testing single sign-on</span></span>

<span data-ttu-id="fd8d3-233">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-233">In this section, you test your Azure AD single sign-on configuration.</span></span>

<span data-ttu-id="fd8d3-234">Öppna ett webbläsarfönster i ett ”InPrivate” eller ”Incognito” läge-tootrigger en ny autentisering.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-234">Open a browser window in an "InPrivate" or "Incognito" mode tootrigger a new authentication.</span></span>  <span data-ttu-id="fd8d3-235">I Internet Explorer, använder du Ctrl + Skift + P.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-235">In Internet Explorer, use Ctrl+Shift+P.</span></span>  <span data-ttu-id="fd8d3-236">I Chrome, använder du Ctrl + Shift + N.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-236">In Chrome, use Ctrl+Shift+N.</span></span>  <span data-ttu-id="fd8d3-237">Hello privat webbläsarfönster bläddrar du tooa skyddade resurser och utföra en Azure AD-inloggning.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-237">In hello private browsing window, browse tooa protected resource and perform an Azure AD login.</span></span>  <span data-ttu-id="fd8d3-238">Du kommer att vidtas toohello begärda webbplatsen i en session isolering efter genomförd inloggning.</span><span class="sxs-lookup"><span data-stu-id="fd8d3-238">Upon successful login, you will be taken toohello requested site in an isolation session.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fd8d3-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="fd8d3-239">Additional resources</span></span>

* [<span data-ttu-id="fd8d3-240">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fd8d3-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fd8d3-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fd8d3-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_203.png

