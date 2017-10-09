---
title: "Självstudier: Azure Active Directory-integrering med 8 x 8 virtuella Office | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och 8 x 8 virtuella Office."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: df5c5de77285cd3912b68cc3b1e3eee274aa951c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a><span data-ttu-id="9dbc0-103">Självstudier: Azure Active Directory-integrering med 8 x 8 virtuella Office</span><span class="sxs-lookup"><span data-stu-id="9dbc0-103">Tutorial: Azure Active Directory integration with 8x8 Virtual Office</span></span>

<span data-ttu-id="9dbc0-104">I kursen får du lära dig hur toointegrate 8 x 8 virtuella Office med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9dbc0-104">In this tutorial, you learn how toointegrate 8x8 Virtual Office with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9dbc0-105">Integrera 8 x 8 virtuella Office med Azure AD kan du med hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9dbc0-105">Integrating 8x8 Virtual Office with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9dbc0-106">Du kan styra i Azure AD som har åtkomst too8x8 virtuella Office</span><span class="sxs-lookup"><span data-stu-id="9dbc0-106">You can control in Azure AD who has access too8x8 Virtual Office</span></span>
- <span data-ttu-id="9dbc0-107">Du kan aktivera användarna tooautomatically hämta inloggade too8x8 virtuella Office (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9dbc0-107">You can enable your users tooautomatically get signed-on too8x8 Virtual Office (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9dbc0-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9dbc0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9dbc0-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9dbc0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9dbc0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9dbc0-110">Prerequisites</span></span>

<span data-ttu-id="9dbc0-111">tooconfigure Azure AD-integrering med 8 x 8 virtuella Office måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9dbc0-111">tooconfigure Azure AD integration with 8x8 Virtual Office, you need hello following items:</span></span>

- <span data-ttu-id="9dbc0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9dbc0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9dbc0-113">En 8 x 8 virtuella Office enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="9dbc0-113">A 8x8 Virtual Office single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9dbc0-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9dbc0-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9dbc0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9dbc0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9dbc0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9dbc0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9dbc0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9dbc0-118">Scenario description</span></span>
<span data-ttu-id="9dbc0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9dbc0-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9dbc0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9dbc0-121">Lägger till 8 x 8 virtuella Office från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9dbc0-121">Adding 8x8 Virtual Office from hello gallery</span></span>
2. <span data-ttu-id="9dbc0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9dbc0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-8x8-virtual-office-from-hello-gallery"></a><span data-ttu-id="9dbc0-123">Lägger till 8 x 8 virtuella Office från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9dbc0-123">Adding 8x8 Virtual Office from hello gallery</span></span>
<span data-ttu-id="9dbc0-124">tooconfigure hello integrering av 8 x 8 virtuella Office i Azure AD, behöver du tooadd 8 x 8 virtuella Office hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-124">tooconfigure hello integration of 8x8 Virtual Office into Azure AD, you need tooadd 8x8 Virtual Office from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9dbc0-125">**tooadd 8 x 8 virtuella Office från galleriet hello utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9dbc0-125">**tooadd 8x8 Virtual Office from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9dbc0-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9dbc0-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9dbc0-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9dbc0-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9dbc0-133">Skriv i sökrutan hello **8 x 8 virtuella Office**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-133">In hello search box, type **8x8 Virtual Office**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. <span data-ttu-id="9dbc0-135">Markera hello resultat på panelen **8 x 8 virtuella Office**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-135">In hello results panel, select **8x8 Virtual Office**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9dbc0-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9dbc0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9dbc0-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 8 x 8 virtuella Office baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-138">In this section, you configure and test Azure AD single sign-on with 8x8 Virtual Office based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9dbc0-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i 8 x 8 virtuella Office är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 8x8 Virtual Office is tooa user in Azure AD.</span></span> <span data-ttu-id="9dbc0-140">Med andra ord en länk mellan en Azure AD-användare och hello relaterade användare i 8 x 8 virtuella Office måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-140">In other words, a link relationship between an Azure AD user and hello related user in 8x8 Virtual Office needs toobe established.</span></span>

<span data-ttu-id="9dbc0-141">8 x 8 virtuella kontoret, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-141">In 8x8 Virtual Office, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9dbc0-142">tooconfigure och testa Azure AD enkel inloggning med 8 x 8 virtuella kontor, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9dbc0-142">tooconfigure and test Azure AD single sign-on with 8x8 Virtual Office, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9dbc0-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9dbc0-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9dbc0-145">**[Skapa en testanvändare för 8 x 8 virtuella Office](#creating-a-8x8-virtual-office-test-user)**  -toohave en motsvarighet för Britta Simon i 8 x 8 virtuella Office som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-145">**[Creating a 8x8 Virtual Office test user](#creating-a-8x8-virtual-office-test-user)** - toohave a counterpart of Britta Simon in 8x8 Virtual Office that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9dbc0-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9dbc0-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9dbc0-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9dbc0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9dbc0-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt 8 x 8 virtuella Office-program.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 8x8 Virtual Office application.</span></span>

<span data-ttu-id="9dbc0-150">**tooconfigure Azure AD enkel inloggning med 8 x 8 virtuella Office utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9dbc0-150">**tooconfigure Azure AD single sign-on with 8x8 Virtual Office, perform hello following steps:**</span></span>

1. <span data-ttu-id="9dbc0-151">I hello Azure-portalen på hello **8 x 8 virtuella Office** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-151">In hello Azure portal, on hello **8x8 Virtual Office** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9dbc0-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. <span data-ttu-id="9dbc0-155">På hello **8 x 8 virtuella Office domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9dbc0-155">On hello **8x8 Virtual Office Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    <span data-ttu-id="9dbc0-157">a.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-157">a.</span></span> <span data-ttu-id="9dbc0-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="9dbc0-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>

    <span data-ttu-id="9dbc0-159">| `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |</span><span class="sxs-lookup"><span data-stu-id="9dbc0-159">| `https://sso.8x8.com/<companyname>` |
 | `https://www.8x8.com/<companyname>` |
 | `https://sso.8x8pilot.com/<companyname>` |</span></span>

    <span data-ttu-id="9dbc0-160">b.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-160">b.</span></span> <span data-ttu-id="9dbc0-161">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="9dbc0-161">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>

    <span data-ttu-id="9dbc0-162">| `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|</span><span class="sxs-lookup"><span data-stu-id="9dbc0-162">| `https://<subdomain>.8x8.com/saml2` |
 | `https://<subdomain>.8x8pilot.com/saml2`|</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9dbc0-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-163">These values are not real.</span></span> <span data-ttu-id="9dbc0-164">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-164">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="9dbc0-165">Kontakta [8 x 8 virtuella Office supportteamet](https://www.8x8.com/about-us/contact-us) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-165">Contact [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us) tooget these values.</span></span>
 


4. <span data-ttu-id="9dbc0-166">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Raw)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-166">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. <span data-ttu-id="9dbc0-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9dbc0-170">På hello **8 x 8 virtuella Office Configuration** klickar du på **konfigurera 8 x 8 virtuella Office** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-170">On hello **8x8 Virtual Office Configuration** section, click **Configure 8x8 Virtual Office** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9dbc0-171">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="9dbc0-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. <span data-ttu-id="9dbc0-173">Inloggning tooyour 8 x 8 virtuella Office innehavaren som administratör.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-173">Sign-on tooyour 8x8 Virtual Office tenant as an administrator.</span></span>

8. <span data-ttu-id="9dbc0-174">Välj **virtuella Office konto hanterare** på panelen för programmet.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-174">Select **Virtual Office Account Mgr** on Application Panel.</span></span>
   
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. <span data-ttu-id="9dbc0-176">Välj **företag** toomanage-kontot och klicka på **logga In** knappen.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-176">Select **Business** account toomanage and click **Sign In** button.</span></span>
   
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. <span data-ttu-id="9dbc0-178">Klicka på **konton** fliken i hello menylistan.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-178">Click **Accounts** tab in hello menu list.</span></span>
   
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. <span data-ttu-id="9dbc0-180">Klicka på **enkel inloggning** i hello listan över konton.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-180">Click **Single Sign On** in hello list of Accounts.</span></span>
   
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. <span data-ttu-id="9dbc0-182">Välj **enkel inloggning** under autentiseringsmetod och klicka på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-182">Select **Single Sign On** under Authentication method and click **SAML**.</span></span>
    
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. <span data-ttu-id="9dbc0-184">Kopiera **SAML SSO URL**, **vaken ut tjänst-URL för enkel** och **utfärdar-URL** från Azure AD för**tecken i URL: en**, **logga ut URL**  och **utfärdar-URL** i 8 x 8 virtuella Office.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-184">Copy **SAML SSO URL**, **Single Sing Out Service URL** and **Issuer URL** from Azure AD too**Sign In URL**, **Sign Out URL** and **Issuer URL** in 8x8 Virtual Office.</span></span> 
    
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. <span data-ttu-id="9dbc0-186">Klicka på **webbläsare** tooupload hello certifikat som du hämtade från Azure AD, och klicka sedan på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-186">Click **Browser** button tooupload hello certificate which you downloaded from Azure AD, and click hello **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="9dbc0-187">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9dbc0-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9dbc0-188">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9dbc0-189">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9dbc0-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9dbc0-190">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9dbc0-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="9dbc0-191">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9dbc0-193">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9dbc0-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9dbc0-194">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9dbc0-196">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9dbc0-198">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9dbc0-200">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9dbc0-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9dbc0-202">a.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-202">a.</span></span> <span data-ttu-id="9dbc0-203">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9dbc0-204">b.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-204">b.</span></span> <span data-ttu-id="9dbc0-205">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9dbc0-206">c.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-206">c.</span></span> <span data-ttu-id="9dbc0-207">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9dbc0-208">d.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-208">d.</span></span> <span data-ttu-id="9dbc0-209">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-209">Click **Create**.</span></span>
 
### <a name="creating-a-8x8-virtual-office-test-user"></a><span data-ttu-id="9dbc0-210">Skapa en 8 x 8 virtuella Office testanvändare</span><span class="sxs-lookup"><span data-stu-id="9dbc0-210">Creating a 8x8 Virtual Office test user</span></span>

<span data-ttu-id="9dbc0-211">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i 8 x 8 virtuella Office.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-211">hello objective of this section is toocreate a user called Britta Simon in 8x8 Virtual Office.</span></span> <span data-ttu-id="9dbc0-212">8 x 8 virtuella Office stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-212">8x8 Virtual Office supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="9dbc0-213">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-213">There is no action item for you in this section.</span></span> <span data-ttu-id="9dbc0-214">En ny användare skapas under ett försök tooaccess 8 x 8 virtuella Office om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-214">A new user is created during an attempt tooaccess 8x8 Virtual Office if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="9dbc0-215">Om du behöver toocreate en användare manuellt, måste toocontact hello [8 x 8 virtuella Office supportteamet](https://www.8x8.com/about-us/contact-us).</span><span class="sxs-lookup"><span data-stu-id="9dbc0-215">If you need toocreate a user manually, you need toocontact hello [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9dbc0-216">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9dbc0-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9dbc0-217">I det här avsnittet kan du aktivera Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst till too8x8 virtuella Office.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too8x8 Virtual Office.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9dbc0-219">**tooassign Britta Simon too8x8 virtuella Office, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9dbc0-219">**tooassign Britta Simon too8x8 Virtual Office, perform hello following steps:**</span></span>

1. <span data-ttu-id="9dbc0-220">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9dbc0-222">Välj i listan med program hello **8 x 8 virtuella Office**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-222">In hello applications list, select **8x8 Virtual Office**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. <span data-ttu-id="9dbc0-224">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9dbc0-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-226">Click **Add** button.</span></span> <span data-ttu-id="9dbc0-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9dbc0-229">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9dbc0-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9dbc0-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9dbc0-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9dbc0-232">Testing single sign-on</span></span>

<span data-ttu-id="9dbc0-233">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-233">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9dbc0-234">När du klickar på hello 8 x 8 virtuella Office-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour 8 x 8 virtuella Office-program.</span><span class="sxs-lookup"><span data-stu-id="9dbc0-234">When you click hello 8x8 Virtual Office tile in hello Access Panel, you should get automatically signed-on tooyour 8x8 Virtual Office application.</span></span>
<span data-ttu-id="9dbc0-235">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="9dbc0-235">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9dbc0-236">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9dbc0-236">Additional resources</span></span>

* [<span data-ttu-id="9dbc0-237">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9dbc0-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9dbc0-238">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9dbc0-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png

