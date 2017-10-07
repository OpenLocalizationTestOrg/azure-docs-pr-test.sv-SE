---
title: "Självstudier: Azure Active Directory-integrering med HappyFox | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och HappyFox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 1190652d7d1144c7eddea339cb3f9175912407fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a><span data-ttu-id="e4cf9-103">Självstudier: Azure Active Directory-integrering med HappyFox</span><span class="sxs-lookup"><span data-stu-id="e4cf9-103">Tutorial: Azure Active Directory integration with HappyFox</span></span>

<span data-ttu-id="e4cf9-104">I kursen får du lära dig hur toointegrate HappyFox med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e4cf9-104">In this tutorial, you learn how toointegrate HappyFox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e4cf9-105">Integrera HappyFox med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e4cf9-105">Integrating HappyFox with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e4cf9-106">Du kan styra i Azure AD som har åtkomst till tooHappyFox</span><span class="sxs-lookup"><span data-stu-id="e4cf9-106">You can control in Azure AD who has access tooHappyFox</span></span>
- <span data-ttu-id="e4cf9-107">Du kan aktivera din användare tooautomatically get inloggade tooHappyFox (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e4cf9-107">You can enable your users tooautomatically get signed-on tooHappyFox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e4cf9-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e4cf9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e4cf9-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e4cf9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4cf9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e4cf9-110">Prerequisites</span></span>

<span data-ttu-id="e4cf9-111">tooconfigure Azure AD-integrering med HappyFox, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e4cf9-111">tooconfigure Azure AD integration with HappyFox, you need hello following items:</span></span>

- <span data-ttu-id="e4cf9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e4cf9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e4cf9-113">En HappyFox enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e4cf9-113">A HappyFox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e4cf9-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e4cf9-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e4cf9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e4cf9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e4cf9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e4cf9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e4cf9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e4cf9-118">Scenario description</span></span>
<span data-ttu-id="e4cf9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e4cf9-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e4cf9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e4cf9-121">Att lägga till HappyFox från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e4cf9-121">Adding HappyFox from hello gallery</span></span>
2. <span data-ttu-id="e4cf9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e4cf9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-happyfox-from-hello-gallery"></a><span data-ttu-id="e4cf9-123">Att lägga till HappyFox från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e4cf9-123">Adding HappyFox from hello gallery</span></span>
<span data-ttu-id="e4cf9-124">tooconfigure hello integrering av HappyFox i Azure AD, behöver du tooadd HappyFox hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-124">tooconfigure hello integration of HappyFox into Azure AD, you need tooadd HappyFox from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e4cf9-125">**tooadd HappyFox från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e4cf9-125">**tooadd HappyFox from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4cf9-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e4cf9-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e4cf9-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e4cf9-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e4cf9-133">Skriv i sökrutan hello **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-133">In hello search box, type **HappyFox**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_search.png)

5. <span data-ttu-id="e4cf9-135">Markera hello resultat på panelen **HappyFox**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-135">In hello results panel, select **HappyFox**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e4cf9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e4cf9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e4cf9-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med HappyFox baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-138">In this section, you configure and test Azure AD single sign-on with HappyFox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e4cf9-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i HappyFox är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in HappyFox is tooa user in Azure AD.</span></span> <span data-ttu-id="e4cf9-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i HappyFox toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-140">In other words, a link relationship between an Azure AD user and hello related user in HappyFox needs toobe established.</span></span>

<span data-ttu-id="e4cf9-141">I HappyFox, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-141">In HappyFox, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e4cf9-142">tooconfigure och testa Azure AD enkel inloggning med HappyFox, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e4cf9-142">tooconfigure and test Azure AD single sign-on with HappyFox, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e4cf9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e4cf9-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e4cf9-145">**[Skapa en testanvändare HappyFox](#creating-a-happyfox-test-user)**  -toohave en motsvarighet för Britta Simon i HappyFox som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-145">**[Creating a HappyFox test user](#creating-a-happyfox-test-user)** - toohave a counterpart of Britta Simon in HappyFox that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e4cf9-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e4cf9-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e4cf9-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e4cf9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e4cf9-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt HappyFox program.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HappyFox application.</span></span>

<span data-ttu-id="e4cf9-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med HappyFox:**</span><span class="sxs-lookup"><span data-stu-id="e4cf9-150">**tooconfigure Azure AD single sign-on with HappyFox, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4cf9-151">I hello Azure-portalen på hello **HappyFox** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-151">In hello Azure portal, on hello **HappyFox** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e4cf9-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_samlbase.png)

3. <span data-ttu-id="e4cf9-155">På hello **HappyFox domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e4cf9-155">On hello **HappyFox Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_url.png)

    <span data-ttu-id="e4cf9-157">a.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-157">a.</span></span> <span data-ttu-id="e4cf9-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.happyfox.com/`</span><span class="sxs-lookup"><span data-stu-id="e4cf9-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.happyfox.com/`</span></span>

    <span data-ttu-id="e4cf9-159">b.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-159">b.</span></span> <span data-ttu-id="e4cf9-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.happyfox.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="e4cf9-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.happyfox.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e4cf9-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-161">These values are not real.</span></span> <span data-ttu-id="e4cf9-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e4cf9-163">Kontakta [HappyFox klienten supportteamet](https://support.happyfox.com/home) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-163">Contact [HappyFox Client support team](https://support.happyfox.com/home) tooget these values.</span></span> 
 
4. <span data-ttu-id="e4cf9-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_certificate.png) 

5. <span data-ttu-id="e4cf9-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e4cf9-168">På hello **HappyFox Configuration** klickar du på **konfigurera HappyFox** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-168">On hello **HappyFox Configuration** section, click **Configure HappyFox** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e4cf9-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnittet**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_configure.png) 

7. <span data-ttu-id="e4cf9-171">Logga in tooyour HappyFox personal portal och gå för**hantera**, klicka på **integreringar** fliken.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-171">Sign on tooyour HappyFox staff portal and navigate too**Manage**, Click on **Integrations** tab.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/header.png) 

8. <span data-ttu-id="e4cf9-173">I hello integreringar fliken klickar du på **konfigurera** under **SAML Integration** tooopen hello enkel inloggning på inställningar.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-173">In hello Integrations tab, Click **Configure** under **SAML Integration** tooopen hello Single Sign On Settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/configure.png) 

9. <span data-ttu-id="e4cf9-175">Klistra in hello i SAML konfigurationsavsnittet **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen i **mål-URL för SSO** textruta.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-175">Inside SAML configuration section, paste hello **SAML Single Sign-On Service URL** that you have copied from Azure portal into **SSO Target URL** textbox.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/targeturl.png)

10. <span data-ttu-id="e4cf9-177">Öppna hello certifikat hämtas från Azure-portalen i anteckningar och klistra in innehållet i **IdP signatur** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-177">Open hello certificate downloaded from Azure portal in notepad and paste its content in **IdP Signature** section.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/cert.png)

11. <span data-ttu-id="e4cf9-179">Klicka på **Spara inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-179">Click **Save Settings** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/savesettings.png)

> [!TIP]
> <span data-ttu-id="e4cf9-181">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="e4cf9-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e4cf9-182">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e4cf9-183">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e4cf9-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e4cf9-184">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4cf9-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="e4cf9-185">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e4cf9-187">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e4cf9-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4cf9-188">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e4cf9-190">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e4cf9-192">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e4cf9-194">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e4cf9-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e4cf9-196">a.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-196">a.</span></span> <span data-ttu-id="e4cf9-197">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e4cf9-198">b.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-198">b.</span></span> <span data-ttu-id="e4cf9-199">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e4cf9-200">c.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-200">c.</span></span> <span data-ttu-id="e4cf9-201">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e4cf9-202">d.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-202">d.</span></span> <span data-ttu-id="e4cf9-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-203">Click **Create**.</span></span>
 
### <a name="creating-a-happyfox-test-user"></a><span data-ttu-id="e4cf9-204">Skapa en testanvändare HappyFox</span><span class="sxs-lookup"><span data-stu-id="e4cf9-204">Creating a HappyFox test user</span></span>

<span data-ttu-id="e4cf9-205">Programmet stöder bara i tid användaretablering och authentication-användare skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-205">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span>
        
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e4cf9-206">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4cf9-206">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e4cf9-207">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHappyFox.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-207">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHappyFox.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e4cf9-209">**tooassign Britta Simon tooHappyFox utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e4cf9-209">**tooassign Britta Simon tooHappyFox, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4cf9-210">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-210">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e4cf9-212">Välj i listan med program hello **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-212">In hello applications list, select **HappyFox**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_app.png) 

3. <span data-ttu-id="e4cf9-214">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-214">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e4cf9-216">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-216">Click **Add** button.</span></span> <span data-ttu-id="e4cf9-217">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e4cf9-219">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-219">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e4cf9-220">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e4cf9-221">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e4cf9-222">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e4cf9-222">Testing single sign-on</span></span>

<span data-ttu-id="e4cf9-223">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-223">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

1. <span data-ttu-id="e4cf9-224">När du klickar på hello HappyFox panelen i hello åtkomstpanelen bör du hämta inloggningssidan för HappyFox program.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-224">When you click hello HappyFox tile in hello Access Panel, you should get login page of HappyFox application.</span></span> <span data-ttu-id="e4cf9-225">Du bör se hello **'SAML-** knappen hello-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-225">You should see hello **‘SAML’** button on hello sign-in page.</span></span>

    ![plugin-programmet](./media/active-directory-saas-happyfox-tutorial/saml.png) 

2. <span data-ttu-id="e4cf9-227">Klicka på hello **'SAML-** knappen toolog i tooHappyFox med hjälp av Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="e4cf9-227">Click hello **‘SAML’** button toolog in tooHappyFox using your Azure AD account.</span></span>

<span data-ttu-id="e4cf9-228">Läs mer om hello åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e4cf9-228">For more information about hello Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e4cf9-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e4cf9-229">Additional resources</span></span>

* [<span data-ttu-id="e4cf9-230">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e4cf9-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e4cf9-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e4cf9-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_203.png

