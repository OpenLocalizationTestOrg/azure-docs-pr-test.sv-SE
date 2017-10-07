---
title: "Självstudier: Azure Active Directory-integrering med Autotask arbetsplats | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Autotask arbetsplats."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 7f820f24e8e9493fa2e1c075f2ef61d7eaa84f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a><span data-ttu-id="764ed-103">Självstudier: Azure Active Directory-integrering med Autotask arbetsplats</span><span class="sxs-lookup"><span data-stu-id="764ed-103">Tutorial: Azure Active Directory integration with Autotask Workplace</span></span>

<span data-ttu-id="764ed-104">I kursen får du lära dig hur toointegrate Autotask arbetsplats med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="764ed-104">In this tutorial, you learn how toointegrate Autotask Workplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="764ed-105">Integrera Autotask arbetsplats med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="764ed-105">Integrating Autotask Workplace with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="764ed-106">Du kan styra i Azure AD som har åtkomst tooAutotask arbetsplats</span><span class="sxs-lookup"><span data-stu-id="764ed-106">You can control in Azure AD who has access tooAutotask Workplace</span></span>
- <span data-ttu-id="764ed-107">Du kan aktivera din användare tooautomatically get inloggade tooAutotask arbetsplats (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="764ed-107">You can enable your users tooautomatically get signed-on tooAutotask Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="764ed-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="764ed-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="764ed-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="764ed-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="764ed-110">Krav</span><span class="sxs-lookup"><span data-stu-id="764ed-110">Prerequisites</span></span>

<span data-ttu-id="764ed-111">tooconfigure Azure AD-integrering med Autotask arbetsplats, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="764ed-111">tooconfigure Azure AD integration with Autotask Workplace, you need hello following items:</span></span>

- <span data-ttu-id="764ed-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="764ed-112">An Azure AD subscription</span></span>
- <span data-ttu-id="764ed-113">En Autotask arbetsplatsen enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="764ed-113">An Autotask Workplace single-sign on enabled subscription</span></span>
- <span data-ttu-id="764ed-114">Du måste vara administratör eller super administratör i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="764ed-114">You must be an administrator or super administrator in Workplace.</span></span>
- <span data-ttu-id="764ed-115">Du måste ha ett administratörskontot i hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="764ed-115">You must have an administrator account in hello Azure AD.</span></span>
- <span data-ttu-id="764ed-116">hello-användare som använder den här funktionen måste ha konton inom arbetsplats och hello Azure AD och deras e-postadresser för både måste matcha.</span><span class="sxs-lookup"><span data-stu-id="764ed-116">hello users that will utilize this feature must have accounts within Workplace and hello Azure AD, and their email addresses for both must match.</span></span>

> [!NOTE]
> <span data-ttu-id="764ed-117">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="764ed-117">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="764ed-118">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="764ed-118">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="764ed-119">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="764ed-119">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="764ed-120">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="764ed-120">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="764ed-121">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="764ed-121">Scenario description</span></span>
<span data-ttu-id="764ed-122">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="764ed-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="764ed-123">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="764ed-123">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="764ed-124">Att lägga till Autotask arbetsplats från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="764ed-124">Adding Autotask Workplace from hello gallery</span></span>
2. <span data-ttu-id="764ed-125">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="764ed-125">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-autotask-workplace-from-hello-gallery"></a><span data-ttu-id="764ed-126">Att lägga till Autotask arbetsplats från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="764ed-126">Adding Autotask Workplace from hello gallery</span></span>
<span data-ttu-id="764ed-127">tooconfigure hello integrering av Autotask arbetsplats i Azure AD, behöver du tooadd Autotask arbetsplats hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="764ed-127">tooconfigure hello integration of Autotask Workplace into Azure AD, you need tooadd Autotask Workplace from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="764ed-128">**tooadd Autotask arbetsplats från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="764ed-128">**tooadd Autotask Workplace from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="764ed-129">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="764ed-129">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="764ed-131">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="764ed-131">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="764ed-132">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="764ed-132">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="764ed-134">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="764ed-134">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="764ed-136">Skriv i sökrutan hello **Autotask arbetsplats**väljer **Autotask arbetsplats** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="764ed-136">In hello search box, type **Autotask Workplace**, select  **Autotask Workplace**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Autotask arbetsplats i hello resulterar lista](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="764ed-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="764ed-138">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="764ed-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Autotask arbetsplats baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="764ed-139">In this section, you configure and test Azure AD single sign-on with Autotask Workplace based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="764ed-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren Autotask arbetsplatsen är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="764ed-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Autotask Workplace is tooa user in Azure AD.</span></span> <span data-ttu-id="764ed-141">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Autotask arbetsplats toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="764ed-141">In other words, a link relationship between an Azure AD user and hello related user in Autotask Workplace needs toobe established.</span></span>

<span data-ttu-id="764ed-142">Tilldela hello värdet hello Autotask arbetsplatsen **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="764ed-142">In Autotask Workplace, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="764ed-143">tooconfigure och testa Azure AD enkel inloggning med Autotask arbetsplats, måste toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="764ed-143">tooconfigure and test Azure AD single sign-on with Autotask Workplace, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="764ed-144">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="764ed-144">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="764ed-145">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="764ed-145">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="764ed-146">**[Skapa en testanvändare Autotask arbetsplats](#create-an-autotask-workplace-test-user)**  -toohave en motsvarighet för Britta Simon Autotask arbetsplatsen som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="764ed-146">**[Create an Autotask Workplace test user](#create-an-autotask-workplace-test-user)** - toohave a counterpart of Britta Simon in Autotask Workplace that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="764ed-147">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="764ed-147">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="764ed-148">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="764ed-148">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="764ed-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="764ed-149">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="764ed-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Autotask arbetsplats.</span><span class="sxs-lookup"><span data-stu-id="764ed-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Autotask Workplace application.</span></span>

<span data-ttu-id="764ed-151">**Utför följande hello tooconfigure Azure AD enkel inloggning med Autotask arbetsplats:**</span><span class="sxs-lookup"><span data-stu-id="764ed-151">**tooconfigure Azure AD single sign-on with Autotask Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="764ed-152">I hello Azure-portalen på hello **Autotask arbetsplats** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="764ed-152">In hello Azure portal, on hello **Autotask Workplace** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="764ed-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="764ed-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

3. <span data-ttu-id="764ed-156">Om du inte vill tooconfigure hello programmet i **IDP** initierade läge, utför följande steg på hello hello **Autotask arbetsplats domän och URL: er** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="764ed-156">If you wish tooconfigure hello application in **IDP** initiated mode, perform hello following steps on hello **Autotask Workplace Domain and URLs** section:</span></span>

    ![Autotask arbetsplats domän URL: er och enkel inloggning information för IDP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    <span data-ttu-id="764ed-158">a.</span><span class="sxs-lookup"><span data-stu-id="764ed-158">a.</span></span> <span data-ttu-id="764ed-159">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="764ed-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span></span>

    <span data-ttu-id="764ed-160">b.</span><span class="sxs-lookup"><span data-stu-id="764ed-160">b.</span></span> <span data-ttu-id="764ed-161">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="764ed-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span></span>

4. <span data-ttu-id="764ed-162">Om du inte vill tooconfigure hello programmet i **SP** initierade läge, kontrollera **visa avancerade inställningar för URL: en** och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="764ed-162">If you wish tooconfigure hello application in **SP** initiated mode, check **Show advanced URL settings** and perform hello following steps:</span></span>

    ![Autotask arbetsplats domän URL: er och enkel inloggning information för SP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    <span data-ttu-id="764ed-164">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.awp.autotask.net/loginsso`</span><span class="sxs-lookup"><span data-stu-id="764ed-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.awp.autotask.net/loginsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="764ed-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="764ed-165">These values are not real.</span></span> <span data-ttu-id="764ed-166">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="764ed-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="764ed-167">Kontakta [Autotask arbetsplats klienten supportteamet](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="764ed-167">Contact [Autotask Workplace Client support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget these values.</span></span> 

5. <span data-ttu-id="764ed-168">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="764ed-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

6. <span data-ttu-id="764ed-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="764ed-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="764ed-172">I en annan webbläsarfönster hello logga in tooWorkplace Online med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="764ed-172">In a different web browser window, Log in tooWorkplace Online using hello administrator credentials.</span></span>

    >[!Note]
    ><span data-ttu-id="764ed-173">När du konfigurerar hello IdP, måste en underdomän toobe anges.</span><span class="sxs-lookup"><span data-stu-id="764ed-173">When configuring hello IdP, a subdomain will need toobe specified.</span></span> <span data-ttu-id="764ed-174">tooconfirm hello rätt underdomän, inloggning tooWorkplace Online.</span><span class="sxs-lookup"><span data-stu-id="764ed-174">tooconfirm hello correct subdomain, login tooWorkplace Online.</span></span> <span data-ttu-id="764ed-175">När du loggade in gör du Observera toohello underdomän i hello-URL.</span><span class="sxs-lookup"><span data-stu-id="764ed-175">Once logged in, make note toohello subdomain in hello URL.</span></span>
    ><span data-ttu-id="764ed-176">hello underdomän ingår hello mellan hello ”https://” och ”.awp.autotask.net/” och bör vara oss eu, ca eller au.</span><span class="sxs-lookup"><span data-stu-id="764ed-176">hello subdomain is hello part between hello “https://“ and “.awp.autotask.net/“ and should be us, eu, ca, or au.</span></span>

8. <span data-ttu-id="764ed-177">Gå för**Configuration** > **enkel inloggning** och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="764ed-177">Go too**Configuration** > **Single Sign-On** and perform hello following steps:</span></span>

    ![Autotask enkel inloggning konfiguration](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    <span data-ttu-id="764ed-179">a.</span><span class="sxs-lookup"><span data-stu-id="764ed-179">a.</span></span> <span data-ttu-id="764ed-180">Välj hello **XML-metadatafil** alternativ och sedan ladda upp hello **XML-Metadata för** hämtas från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="764ed-180">Select hello **XML Metadata File** option, and then upload hello **Metadata XML** downloaded from Azure portal.</span></span>

    <span data-ttu-id="764ed-181">b.</span><span class="sxs-lookup"><span data-stu-id="764ed-181">b.</span></span> <span data-ttu-id="764ed-182">Klicka på **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="764ed-182">Click **Enable SSO**.</span></span>
    
    ![Godkänn Autotask Single Sign-on konfiguration](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    <span data-ttu-id="764ed-184">c.</span><span class="sxs-lookup"><span data-stu-id="764ed-184">c.</span></span> <span data-ttu-id="764ed-185">Välj hello **jag bekräfta informationen är korrekt och den här IdP tillförlitliga** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="764ed-185">Select hello **I confirm this information is correct and I trust this IdP** check box.</span></span>

    <span data-ttu-id="764ed-186">d.</span><span class="sxs-lookup"><span data-stu-id="764ed-186">d.</span></span> <span data-ttu-id="764ed-187">Klicka på **godkänna**.</span><span class="sxs-lookup"><span data-stu-id="764ed-187">Click **Approve**.</span></span>
     
>[!Note]
><span data-ttu-id="764ed-188">Om du behöver hjälp med att konfigurera Autotask arbetsplats finns [den här sidan](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget hjälp med ditt arbetsplatskonto.</span><span class="sxs-lookup"><span data-stu-id="764ed-188">If you require assistance with configuring Autotask Workplace, please see [this page](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget assistance with your Workplace account.</span></span>

> [!TIP]
> <span data-ttu-id="764ed-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="764ed-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="764ed-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="764ed-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="764ed-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="764ed-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="764ed-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="764ed-192">Create an Azure AD test user</span></span>

<span data-ttu-id="764ed-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="764ed-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="764ed-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="764ed-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="764ed-196">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="764ed-196">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="764ed-198">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="764ed-198">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="764ed-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="764ed-200">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="764ed-202">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="764ed-202">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="764ed-204">a.</span><span class="sxs-lookup"><span data-stu-id="764ed-204">a.</span></span> <span data-ttu-id="764ed-205">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="764ed-205">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="764ed-206">b.</span><span class="sxs-lookup"><span data-stu-id="764ed-206">b.</span></span> <span data-ttu-id="764ed-207">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="764ed-207">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="764ed-208">c.</span><span class="sxs-lookup"><span data-stu-id="764ed-208">c.</span></span> <span data-ttu-id="764ed-209">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="764ed-209">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="764ed-210">d.</span><span class="sxs-lookup"><span data-stu-id="764ed-210">d.</span></span> <span data-ttu-id="764ed-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="764ed-211">Click **Create**.</span></span>

### <a name="create-an-autotask-workplace-test-user"></a><span data-ttu-id="764ed-212">Skapa en testanvändare Autotask arbetsplats</span><span class="sxs-lookup"><span data-stu-id="764ed-212">Create an Autotask Workplace test user</span></span>

<span data-ttu-id="764ed-213">I det här avsnittet skapar du en användare som kallas Britta Simon i Autotask.</span><span class="sxs-lookup"><span data-stu-id="764ed-213">In this section, you create a user called Britta Simon in Autotask.</span></span> <span data-ttu-id="764ed-214">Se tillsammans med [Autotask arbetsplats supportteamet](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooadd hello användare i hello Autotask arbetsplats plattform.</span><span class="sxs-lookup"><span data-stu-id="764ed-214">Please work with [Autotask Workplace support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooadd hello users in hello Autotask Workplace platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="764ed-215">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="764ed-215">Assign hello Azure AD test user</span></span>

<span data-ttu-id="764ed-216">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAutotask arbetsplats.</span><span class="sxs-lookup"><span data-stu-id="764ed-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAutotask Workplace.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="764ed-218">**tooassign Britta Simon tooAutotask arbetsplats utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="764ed-218">**tooassign Britta Simon tooAutotask Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="764ed-219">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="764ed-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="764ed-221">Välj i listan med program hello **Autotask arbetsplats**.</span><span class="sxs-lookup"><span data-stu-id="764ed-221">In hello applications list, select **Autotask Workplace**.</span></span>

    ![Hej Autotask arbetsplats länken i listan med program hello](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

3. <span data-ttu-id="764ed-223">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="764ed-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="764ed-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="764ed-225">Click **Add** button.</span></span> <span data-ttu-id="764ed-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="764ed-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="764ed-228">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="764ed-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="764ed-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="764ed-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="764ed-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="764ed-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="764ed-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="764ed-231">Test single sign-on</span></span>

<span data-ttu-id="764ed-232">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="764ed-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="764ed-233">Du bör få automatiskt inloggade tooyour Autotask arbetsplats programmet när du klickar på hello Autotask arbetsplats panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="764ed-233">When you click hello Autotask Workplace tile in hello Access Panel, you should get automatically signed-on tooyour Autotask Workplace application.</span></span>
<span data-ttu-id="764ed-234">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="764ed-234">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="764ed-235">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="764ed-235">Additional resources</span></span>

* [<span data-ttu-id="764ed-236">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="764ed-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="764ed-237">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="764ed-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_203.png

