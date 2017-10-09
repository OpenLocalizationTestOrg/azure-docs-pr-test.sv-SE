---
title: "Självstudier: Azure Active Directory-integrering med Asana | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Asana."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: 9ac35dedc809b2b3dfb461d92a5afb066ccdedde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a><span data-ttu-id="ef0ab-103">Självstudier: Azure Active Directory-integrering med Asana</span><span class="sxs-lookup"><span data-stu-id="ef0ab-103">Tutorial: Azure Active Directory integration with Asana</span></span>

<span data-ttu-id="ef0ab-104">I kursen får du lära dig hur toointegrate Asana med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ef0ab-104">In this tutorial, you learn how toointegrate Asana with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ef0ab-105">Integrera Asana med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ef0ab-105">Integrating Asana with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ef0ab-106">Du kan styra i Azure AD som har åtkomst till tooAsana</span><span class="sxs-lookup"><span data-stu-id="ef0ab-106">You can control in Azure AD who has access tooAsana</span></span>
- <span data-ttu-id="ef0ab-107">Du kan aktivera din användare tooautomatically get inloggade tooAsana (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ef0ab-107">You can enable your users tooautomatically get signed-on tooAsana (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ef0ab-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ef0ab-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ef0ab-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ef0ab-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef0ab-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ef0ab-110">Prerequisites</span></span>

<span data-ttu-id="ef0ab-111">tooconfigure Azure AD-integrering med Asana, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ef0ab-111">tooconfigure Azure AD integration with Asana, you need hello following items:</span></span>

- <span data-ttu-id="ef0ab-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ef0ab-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ef0ab-113">En Asana enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="ef0ab-113">An Asana single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ef0ab-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ef0ab-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ef0ab-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ef0ab-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ef0ab-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ef0ab-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ef0ab-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ef0ab-118">Scenario description</span></span>
<span data-ttu-id="ef0ab-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ef0ab-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ef0ab-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ef0ab-121">Att lägga till Asana från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ef0ab-121">Adding Asana from hello gallery</span></span>
2. <span data-ttu-id="ef0ab-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ef0ab-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asana-from-hello-gallery"></a><span data-ttu-id="ef0ab-123">Att lägga till Asana från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ef0ab-123">Adding Asana from hello gallery</span></span>
<span data-ttu-id="ef0ab-124">tooconfigure hello integrering av Asana i Azure AD, behöver du tooadd Asana hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-124">tooconfigure hello integration of Asana into Azure AD, you need tooadd Asana from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ef0ab-125">**tooadd Asana från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ef0ab-125">**tooadd Asana from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ef0ab-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="ef0ab-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ef0ab-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="ef0ab-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="ef0ab-133">Skriv i sökrutan hello **Asana**väljer **Asana** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-133">In hello search box, type **Asana**, select **Asana** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="ef0ab-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ef0ab-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="ef0ab-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Asana baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-136">In this section, you configure and test Azure AD single sign-on with Asana based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ef0ab-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Asana är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Asana is tooa user in Azure AD.</span></span> <span data-ttu-id="ef0ab-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Asana toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-138">In other words, a link relationship between an Azure AD user and hello related user in Asana needs toobe established.</span></span>

<span data-ttu-id="ef0ab-139">I Asana, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-139">In Asana, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ef0ab-140">tooconfigure och testa Azure AD enkel inloggning med Asana, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ef0ab-140">tooconfigure and test Azure AD single sign-on with Asana, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ef0ab-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ef0ab-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ef0ab-143">**[Skapa en testanvändare Asana](#create-an-asana-test-user)**  -toohave en motsvarighet för Britta Simon i Asana som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-143">**[Create an Asana test user](#create-an-asana-test-user)** - toohave a counterpart of Britta Simon in Asana that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ef0ab-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ef0ab-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="ef0ab-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ef0ab-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="ef0ab-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Asana program.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Asana application.</span></span>

<span data-ttu-id="ef0ab-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Asana:**</span><span class="sxs-lookup"><span data-stu-id="ef0ab-148">**tooconfigure Azure AD single sign-on with Asana, perform hello following steps:**</span></span>

1. <span data-ttu-id="ef0ab-149">I hello Azure-portalen på hello **Asana** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-149">In hello Azure portal, on hello **Asana** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ef0ab-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. <span data-ttu-id="ef0ab-153">På hello **Asana domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ef0ab-153">On hello **Asana Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Asana domän med enkel inloggning information](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    <span data-ttu-id="ef0ab-155">a.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-155">a.</span></span> <span data-ttu-id="ef0ab-156">I hello **inloggnings-URL** textruta typen URL:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="ef0ab-156">In hello **Sign-on URL** textbox, type URL: `https://app.asana.com/`</span></span>

    <span data-ttu-id="ef0ab-157">b.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-157">b.</span></span> <span data-ttu-id="ef0ab-158">I hello **identifierare** textruta TYPVÄRDE:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="ef0ab-158">In hello **Identifier** textbox, type value: `https://app.asana.com/`</span></span>
 
4. <span data-ttu-id="ef0ab-159">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. <span data-ttu-id="ef0ab-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ef0ab-163">På hello **Asana Configuration** klickar du på **konfigurera Asana** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-163">On hello **Asana Configuration** section, click **Configure Asana** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ef0ab-164">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ef0ab-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Asana konfiguration](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. <span data-ttu-id="ef0ab-166">I ett annat fönster i webbläsaren, inloggning tooyour Asana program.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-166">In a different browser window, sign-on tooyour Asana application.</span></span> <span data-ttu-id="ef0ab-167">tooconfigure SSO i Asana hello arbetsytan inställningar genom att klicka på hello arbetsytans namn på hello övre högra hörnet av hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-167">tooconfigure SSO in Asana, access hello workspace settings by clicking hello workspace name on hello top right corner of hello screen.</span></span> <span data-ttu-id="ef0ab-168">Klicka på  **\<arbetsytans namn\> inställningar**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-168">Then, click on **\<your workspace name\> Settings**.</span></span> 
   
    ![Asana sso-inställningar](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. <span data-ttu-id="ef0ab-170">På hello **organisationsinställningar** -fönstret klickar du på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-170">On hello **Organization settings** window, click **Administration**.</span></span> <span data-ttu-id="ef0ab-171">Klicka på **medlemmar måste logga in via SAML** tooenable hello SSO konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-171">Then, click **Members must log in via SAML** tooenable hello SSO configuration.</span></span> <span data-ttu-id="ef0ab-172">hello utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ef0ab-172">hello perform hello following steps:</span></span>
   
    ![Konfigurera inställningar för enkel inloggning organisation](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     <span data-ttu-id="ef0ab-174">a.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-174">a.</span></span> <span data-ttu-id="ef0ab-175">I hello **inloggning Sidadress** textruta klistra in hello **SAML inloggning tjänst-URL för enkel**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-175">In hello **Sign-in page URL** textbox, paste hello **SAML Single Sign-On Service URL**.</span></span>

     <span data-ttu-id="ef0ab-176">b.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-176">b.</span></span> <span data-ttu-id="ef0ab-177">Högerklicka på hello certifikat hämtas från Azure-portalen och öppna hello certifikatfil med anteckningar eller din önskade textredigerare.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-177">Right click hello certificate downloaded from Azure portal, then open hello certificate file using Notepad or your preferred text editor.</span></span> <span data-ttu-id="ef0ab-178">Kopiera hello innehåll mellan hello börjar och hello slutet certifikat och klistra in den i hello **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-178">Copy hello content between hello begin and hello end certificate title and paste it in hello **X.509 Certificate** textbox.</span></span>

9. <span data-ttu-id="ef0ab-179">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-179">Click **Save**.</span></span> <span data-ttu-id="ef0ab-180">Gå för[Asana guide för att konfigurera enkel inloggning](https://asana.com/guide/help/premium/authentication#gl-saml) om du behöver ytterligare hjälp.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-180">Go too[Asana guide for setting up SSO](https://asana.com/guide/help/premium/authentication#gl-saml) if you need further assistance.</span></span>

> [!TIP]
> <span data-ttu-id="ef0ab-181">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="ef0ab-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ef0ab-182">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ef0ab-183">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ef0ab-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ef0ab-184">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ef0ab-184">Create an Azure AD test user</span></span>

<span data-ttu-id="ef0ab-185">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="ef0ab-187">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ef0ab-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ef0ab-188">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ef0ab-190">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ef0ab-192">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ef0ab-194">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ef0ab-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ef0ab-196">a.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-196">a.</span></span> <span data-ttu-id="ef0ab-197">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ef0ab-198">b.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-198">b.</span></span> <span data-ttu-id="ef0ab-199">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ef0ab-200">c.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-200">c.</span></span> <span data-ttu-id="ef0ab-201">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ef0ab-202">d.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-202">d.</span></span> <span data-ttu-id="ef0ab-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-203">Click **Create**.</span></span>
 
### <a name="create-an-asana-test-user"></a><span data-ttu-id="ef0ab-204">Skapa en testanvändare Asana</span><span class="sxs-lookup"><span data-stu-id="ef0ab-204">Create an Asana test user</span></span>

<span data-ttu-id="ef0ab-205">I det här avsnittet skapar du en användare som kallas Britta Simon i Asana.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-205">In this section, you create a user called Britta Simon in Asana.</span></span>

1. <span data-ttu-id="ef0ab-206">På **Asana**, gå toohello **team** avsnitt på hello vänstra panelen.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-206">On **Asana**, go toohello **Teams** section on hello left panel.</span></span> <span data-ttu-id="ef0ab-207">Klicka på hello plus logga knappen.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-207">Click hello plus sign button.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. <span data-ttu-id="ef0ab-209">Skriv e-post hello britta.simon@contoso.com i hello textruta och välj sedan **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-209">Type hello email britta.simon@contoso.com in hello text box and then select **Invite**.</span></span>

3. <span data-ttu-id="ef0ab-210">Klicka på **skicka inbjudan**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-210">Click **Send Invite**.</span></span> <span data-ttu-id="ef0ab-211">hello nya användaren får ett e-postmeddelande till sin e-postkonto.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-211">hello new user will receive an email into her email account.</span></span> <span data-ttu-id="ef0ab-212">Hon behöver toocreate och validera hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-212">She will need toocreate and validate hello account.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="ef0ab-213">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="ef0ab-213">Assign hello Azure AD test user</span></span>

<span data-ttu-id="ef0ab-214">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAsana.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAsana.</span></span>

![Tilldela hello användarroll][200]

<span data-ttu-id="ef0ab-216">**tooassign Britta Simon tooAsana utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ef0ab-216">**tooassign Britta Simon tooAsana, perform hello following steps:**</span></span>

1. <span data-ttu-id="ef0ab-217">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ef0ab-219">Välj i listan med program hello **Asana**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-219">In hello applications list, select **Asana**.</span></span>

    ![Hej Asana länken i listan med program hello](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. <span data-ttu-id="ef0ab-221">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="ef0ab-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-223">Click **Add** button.</span></span> <span data-ttu-id="ef0ab-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="ef0ab-226">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ef0ab-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ef0ab-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="ef0ab-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ef0ab-229">Test single sign-on</span></span>

<span data-ttu-id="ef0ab-230">hello syftet med det här avsnittet är tootest din Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-230">hello objective of this section is tootest your Azure AD single sign-on.</span></span>

<span data-ttu-id="ef0ab-231">Gå tooAsana inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-231">Go tooAsana login page.</span></span> <span data-ttu-id="ef0ab-232">Infoga hello e-postadress i hello e-postadress textruta britta.simon@contoso.com. Lämna tomt hello lösenordsrutan och klicka sedan på **loggar In**.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-232">In hello Email address textbox, insert hello email address britta.simon@contoso.com. Leave hello password textbox in blank and then click **Log In**.</span></span> <span data-ttu-id="ef0ab-233">Du kommer att omdirigerade tooAzure AD-inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-233">You will be redirected tooAzure AD login page.</span></span> <span data-ttu-id="ef0ab-234">Slutför Azure AD-autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-234">Complete your Azure AD credentials.</span></span> <span data-ttu-id="ef0ab-235">Nu kan är du inloggad Asana.</span><span class="sxs-lookup"><span data-stu-id="ef0ab-235">Now, you are logged in on Asana.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef0ab-236">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ef0ab-236">Additional resources</span></span>

* [<span data-ttu-id="ef0ab-237">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ef0ab-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ef0ab-238">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ef0ab-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
