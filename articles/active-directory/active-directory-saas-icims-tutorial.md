---
title: "Självstudier: Azure Active Directory-integrering med ICIMS | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ICIMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 72dbd649-e4b1-4d72-ad76-636d84922596
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 3fa970f008e64e84b0a1280f691f0181851b757c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-icims"></a><span data-ttu-id="49126-103">Självstudier: Azure Active Directory-integrering med ICIMS</span><span class="sxs-lookup"><span data-stu-id="49126-103">Tutorial: Azure Active Directory integration with ICIMS</span></span>

<span data-ttu-id="49126-104">I kursen får du lära dig hur toointegrate ICIMS med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="49126-104">In this tutorial, you learn how toointegrate ICIMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="49126-105">Integrera ICIMS med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="49126-105">Integrating ICIMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="49126-106">Du kan styra i Azure AD som har åtkomst till tooICIMS</span><span class="sxs-lookup"><span data-stu-id="49126-106">You can control in Azure AD who has access tooICIMS</span></span>
- <span data-ttu-id="49126-107">Du kan aktivera din användare tooautomatically get inloggade tooICIMS (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="49126-107">You can enable your users tooautomatically get signed-on tooICIMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="49126-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="49126-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="49126-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="49126-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49126-110">Krav</span><span class="sxs-lookup"><span data-stu-id="49126-110">Prerequisites</span></span>

<span data-ttu-id="49126-111">tooconfigure Azure AD-integrering med ICIMS, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="49126-111">tooconfigure Azure AD integration with ICIMS, you need hello following items:</span></span>

- <span data-ttu-id="49126-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="49126-112">An Azure AD subscription</span></span>
- <span data-ttu-id="49126-113">En ICIMS enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="49126-113">An ICIMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="49126-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="49126-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="49126-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="49126-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="49126-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="49126-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="49126-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="49126-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="49126-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="49126-118">Scenario description</span></span>
<span data-ttu-id="49126-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="49126-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="49126-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="49126-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="49126-121">Att lägga till ICIMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="49126-121">Adding ICIMS from hello gallery</span></span>
2. <span data-ttu-id="49126-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="49126-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-icims-from-hello-gallery"></a><span data-ttu-id="49126-123">Att lägga till ICIMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="49126-123">Adding ICIMS from hello gallery</span></span>
<span data-ttu-id="49126-124">tooconfigure hello integrering av ICIMS i Azure AD, behöver du tooadd ICIMS hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="49126-124">tooconfigure hello integration of ICIMS into Azure AD, you need tooadd ICIMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="49126-125">**tooadd ICIMS från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="49126-125">**tooadd ICIMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="49126-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="49126-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="49126-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="49126-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="49126-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="49126-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="49126-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="49126-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="49126-133">Skriv i sökrutan hello **ICIMS**väljer **ICIMS** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="49126-133">In hello search box, type **ICIMS**, select **ICIMS** from result panel then click **Add** button tooadd hello application.</span></span>

    ![ICIMS i hello resultatlistan](./media/active-directory-saas-icims-tutorial/tutorial_icims_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="49126-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="49126-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="49126-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ICIMS baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="49126-136">In this section, you configure and test Azure AD single sign-on with ICIMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="49126-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ICIMS är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="49126-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ICIMS is tooa user in Azure AD.</span></span> <span data-ttu-id="49126-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ICIMS toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="49126-138">In other words, a link relationship between an Azure AD user and hello related user in ICIMS needs toobe established.</span></span>

<span data-ttu-id="49126-139">I ICIMS, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="49126-139">In ICIMS, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="49126-140">tooconfigure och testa Azure AD enkel inloggning med ICIMS, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="49126-140">tooconfigure and test Azure AD single sign-on with ICIMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="49126-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="49126-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="49126-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="49126-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="49126-143">**[Skapa en testanvändare ICIMS](#create-an-icims-test-user)**  -toohave en motsvarighet för Britta Simon i ICIMS som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="49126-143">**[Create an ICIMS test user](#create-an-icims-test-user)** - toohave a counterpart of Britta Simon in ICIMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="49126-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="49126-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="49126-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="49126-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="49126-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="49126-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="49126-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt ICIMS program.</span><span class="sxs-lookup"><span data-stu-id="49126-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ICIMS application.</span></span>

<span data-ttu-id="49126-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ICIMS:**</span><span class="sxs-lookup"><span data-stu-id="49126-148">**tooconfigure Azure AD single sign-on with ICIMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="49126-149">I hello Azure-portalen på hello **ICIMS** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="49126-149">In hello Azure portal, on hello **ICIMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="49126-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="49126-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-icims-tutorial/tutorial_icims_samlbase.png)

3. <span data-ttu-id="49126-153">På hello **ICIMS domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="49126-153">On hello **ICIMS Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och ICIMS domän med enkel inloggning information](./media/active-directory-saas-icims-tutorial/tutorial_icims_url.png)

    <span data-ttu-id="49126-155">a.</span><span class="sxs-lookup"><span data-stu-id="49126-155">a.</span></span> <span data-ttu-id="49126-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant name>.icims.com`</span><span class="sxs-lookup"><span data-stu-id="49126-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant name>.icims.com`</span></span>

    <span data-ttu-id="49126-157">b.</span><span class="sxs-lookup"><span data-stu-id="49126-157">b.</span></span> <span data-ttu-id="49126-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant name>.icims.com`</span><span class="sxs-lookup"><span data-stu-id="49126-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant name>.icims.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="49126-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="49126-159">These values are not real.</span></span> <span data-ttu-id="49126-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="49126-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="49126-161">Kontakta [ICIMS klienten supportteamet](https://www.icims.com/contact-us) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="49126-161">Contact [ICIMS Client support team](https://www.icims.com/contact-us) tooget these values.</span></span> 
 
4. <span data-ttu-id="49126-162">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="49126-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-icims-tutorial/tutorial_icims_certificate.png) 

5. <span data-ttu-id="49126-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="49126-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-icims-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="49126-166">På hello **ICIMS Configuration** klickar du på **konfigurera ICIMS** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="49126-166">On hello **ICIMS Configuration** section, click **Configure ICIMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="49126-167">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="49126-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![ICIMS konfiguration](./media/active-directory-saas-icims-tutorial/tutorial_icims_configure.png) 

7. <span data-ttu-id="49126-169">tooconfigure enkel inloggning på **ICIMS** sida, behöver du toosend hello hämtas **XML-Metadata för**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för [ICIMS supportteam](https://www.icims.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="49126-169">tooconfigure single sign-on on **ICIMS** side, you need toosend hello downloaded **Metadata XML**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[ICIMS support team](https://www.icims.com/contact-us).</span></span> <span data-ttu-id="49126-170">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="49126-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="49126-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="49126-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="49126-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="49126-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="49126-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="49126-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="49126-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="49126-174">Create an Azure AD test user</span></span>
<span data-ttu-id="49126-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="49126-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="49126-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="49126-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="49126-178">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="49126-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-icims-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="49126-180">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="49126-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-icims-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="49126-182">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="49126-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-icims-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="49126-184">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="49126-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-icims-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="49126-186">a.</span><span class="sxs-lookup"><span data-stu-id="49126-186">a.</span></span> <span data-ttu-id="49126-187">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="49126-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="49126-188">b.</span><span class="sxs-lookup"><span data-stu-id="49126-188">b.</span></span> <span data-ttu-id="49126-189">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="49126-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="49126-190">c.</span><span class="sxs-lookup"><span data-stu-id="49126-190">c.</span></span> <span data-ttu-id="49126-191">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="49126-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="49126-192">d.</span><span class="sxs-lookup"><span data-stu-id="49126-192">d.</span></span> <span data-ttu-id="49126-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="49126-193">Click **Create**.</span></span>
 
### <a name="create-an-icims-test-user"></a><span data-ttu-id="49126-194">Skapa en testanvändare ICIMS</span><span class="sxs-lookup"><span data-stu-id="49126-194">Create an ICIMS test user</span></span>

<span data-ttu-id="49126-195">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i ICIMS.</span><span class="sxs-lookup"><span data-stu-id="49126-195">hello objective of this section is toocreate a user called Britta Simon in ICIMS.</span></span> <span data-ttu-id="49126-196">Arbeta med [ICIMS supportteam](https://www.icims.com/contact-us) tooadd hello användare i hello ICIMS konto.</span><span class="sxs-lookup"><span data-stu-id="49126-196">Work with [ICIMS support team](https://www.icims.com/contact-us) tooadd hello users in hello ICIMS account.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="49126-197">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="49126-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="49126-198">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooICIMS.</span><span class="sxs-lookup"><span data-stu-id="49126-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooICIMS.</span></span>

![Tilldela hello användarroll][200]

<span data-ttu-id="49126-200">**tooassign Britta Simon tooICIMS utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="49126-200">**tooassign Britta Simon tooICIMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="49126-201">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="49126-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="49126-203">Välj i listan med program hello **ICIMS**.</span><span class="sxs-lookup"><span data-stu-id="49126-203">In hello applications list, select **ICIMS**.</span></span>

    ![Hej ICIMS länken i listan med program hello](./media/active-directory-saas-icims-tutorial/tutorial_icims_app.png) 

3. <span data-ttu-id="49126-205">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="49126-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="49126-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="49126-207">Click **Add** button.</span></span> <span data-ttu-id="49126-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="49126-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="49126-210">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="49126-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="49126-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="49126-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="49126-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="49126-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="49126-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="49126-213">Test single sign-on</span></span>

<span data-ttu-id="49126-214">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="49126-214">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="49126-215">Du bör få automatiskt inloggade tooyour ICIMS programmet när du klickar på hello ICIMS panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="49126-215">When you click hello ICIMS tile in hello Access Panel, you should get automatically signed-on tooyour ICIMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49126-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="49126-216">Additional resources</span></span>

* [<span data-ttu-id="49126-217">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="49126-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="49126-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="49126-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icims-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icims-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icims-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icims-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-icims-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icims-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icims-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-icims-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-icims-tutorial/tutorial_general_203.png

