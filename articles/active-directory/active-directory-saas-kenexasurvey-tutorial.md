---
title: "Självstudier: Azure Active Directory-integrering med IBM Kenexa undersökning Enterprise | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och IBM Kenexa undersökning Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: cf7ed886b4418ac396ca7056827ee10fd7a19ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a><span data-ttu-id="dc745-103">Självstudier: Azure Active Directory-integrering med IBM Kenexa undersökning Enterprise</span><span class="sxs-lookup"><span data-stu-id="dc745-103">Tutorial: Azure Active Directory integration with IBM Kenexa Survey Enterprise</span></span>

<span data-ttu-id="dc745-104">I kursen får du lära dig hur toointegrate IBM Kenexa undersökning Enterprise med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="dc745-104">In this tutorial, you learn how toointegrate IBM Kenexa Survey Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dc745-105">Integrera IBM Kenexa undersökning Enterprise med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="dc745-105">Integrating IBM Kenexa Survey Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="dc745-106">Du kan styra i Azure AD som har åtkomst tooIBM Kenexa undersökning Enterprise.</span><span class="sxs-lookup"><span data-stu-id="dc745-106">You can control in Azure AD who has access tooIBM Kenexa Survey Enterprise.</span></span>
- <span data-ttu-id="dc745-107">Du kan aktivera tooIBM Kenexa undersökning Enterprise användare tooautomatically inloggningen genom att använda enkel inloggning (SSO) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="dc745-107">You can enable your users tooautomatically sign in tooIBM Kenexa Survey Enterprise by using single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="dc745-108">Du kan hantera dina konton i en central plats: hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dc745-108">You can manage your accounts in one central location: hello Azure portal.</span></span>

<span data-ttu-id="dc745-109">Om du vill tooknow mer om programvara som en tjänst (SaaS) appintegrering med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dc745-109">If you want tooknow more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc745-110">Krav</span><span class="sxs-lookup"><span data-stu-id="dc745-110">Prerequisites</span></span>

<span data-ttu-id="dc745-111">tooconfigure Azure AD-integrering med IBM Kenexa undersökning Enterprise, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="dc745-111">tooconfigure Azure AD integration with IBM Kenexa Survey Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="dc745-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dc745-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dc745-113">En prenumeration för IBM Kenexa undersökning Enterprise SSO aktiverad</span><span class="sxs-lookup"><span data-stu-id="dc745-113">An IBM Kenexa Survey Enterprise SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dc745-114">När du testar hello stegen i den här självstudiekursen, rekommenderar vi att du inte använder en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="dc745-114">When you test hello steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="dc745-115">tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="dc745-115">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="dc745-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="dc745-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dc745-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dc745-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dc745-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="dc745-118">Scenario description</span></span>
<span data-ttu-id="dc745-119">I kursen får testa du Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="dc745-119">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="dc745-120">hello-scenariot som skisseras i hello kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="dc745-120">hello scenario outlined in hello tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="dc745-121">Att lägga till IBM Kenexa undersökning Enterprise från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="dc745-121">Adding IBM Kenexa Survey Enterprise from hello gallery</span></span>
* <span data-ttu-id="dc745-122">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="dc745-122">Configuring and testing Azure AD SSO</span></span>

## <a name="add-ibm-kenexa-survey-enterprise-from-hello-gallery"></a><span data-ttu-id="dc745-123">Lägg till IBM Kenexa undersökning Enterprise från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="dc745-123">Add IBM Kenexa Survey Enterprise from hello gallery</span></span>
<span data-ttu-id="dc745-124">tooconfigure hello integrering av IBM Kenexa undersökning Enterprise i Azure AD och lägga till IBM Kenexa undersökning Enterprise från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="dc745-124">tooconfigure hello integration of IBM Kenexa Survey Enterprise into Azure AD, add IBM Kenexa Survey Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="dc745-125">tooadd IBM Kenexa undersökning Enterprise från galleriet hello hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc745-125">tooadd IBM Kenexa Survey Enterprise from hello gallery, do hello following:</span></span>

1. <span data-ttu-id="dc745-126">I hello [Azure-portalen](https://portal.azure.com), i hello vänster, klicka på hello **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="dc745-126">In hello [Azure portal](https://portal.azure.com), in hello left pane, click hello **Azure Active Directory** button.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="dc745-128">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dc745-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="dc745-130">tooadd ett program, klickar du på hello **nytt program** knappen.</span><span class="sxs-lookup"><span data-stu-id="dc745-130">tooadd an application, click hello **New application** button.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="dc745-132">Skriv i sökrutan hello **IBM Kenexa undersökning Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="dc745-132">In hello search box, type **IBM Kenexa Survey Enterprise**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. <span data-ttu-id="dc745-134">Markera i hello resultat **IBM Kenexa undersökning Enterprise**, och klicka sedan på hello **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="dc745-134">In hello results list, select **IBM Kenexa Survey Enterprise**, and then click hello **Add** button tooadd hello application.</span></span>

    ![IBM Kenexa undersökning Enterprise i hello resultatlistan](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="dc745-136">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dc745-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="dc745-137">I det här avsnittet kan du konfigurera och testa Azure AD SSO med IBM Kenexa undersökning Enterprise baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="dc745-137">In this section, you configure and test Azure AD SSO with IBM Kenexa Survey Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="dc745-138">För enkel inloggning toowork måste Azure AD tooidentify hello IBM Kenexa undersökning Enterprise användaren motsvarighet i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dc745-138">For SSO toowork, Azure AD needs tooidentify hello IBM Kenexa Survey Enterprise user counterpart in Azure AD.</span></span> <span data-ttu-id="dc745-139">Med andra ord upprätta Azure AD en länk förhållandet mellan en Azure AD-användare och en relaterad användare i IBM Kenexa undersökning företag.</span><span class="sxs-lookup"><span data-stu-id="dc745-139">In other words, Azure AD must establish a link relationship between an Azure AD user and a related user in IBM Kenexa Survey Enterprise.</span></span>

<span data-ttu-id="dc745-140">tooestablish hello länken relationen, tilldela hello värdet för hello **användarnamn** i IBM Kenexa undersökning företag som hello värde för hello **användarnamn** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dc745-140">tooestablish hello link relationship, assign hello value of hello **user name** in IBM Kenexa Survey Enterprise as hello value of hello **Username** in Azure AD.</span></span>

<span data-ttu-id="dc745-141">tooconfigure och testa Azure AD SSO med IBM Kenexa undersökning Enterprise fullständig hello byggblock i hello följande två avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dc745-141">tooconfigure and test Azure AD SSO with IBM Kenexa Survey Enterprise, complete hello building blocks in hello next two sections.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="dc745-142">Konfigurera Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dc745-142">Configure Azure AD SSO</span></span>

<span data-ttu-id="dc745-143">I det här avsnittet Aktivera Azure AD SSO i hello Azure-portalen och konfigurera enkel inloggning i IBM Kenexa undersökning Enterprise-programmet hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc745-143">In this section, you enable Azure AD SSO in hello Azure portal and configure SSO in your IBM Kenexa Survey Enterprise application by doing hello following:</span></span>

1. <span data-ttu-id="dc745-144">I hello Azure-portalen på hello **IBM Kenexa undersökning Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="dc745-144">In hello Azure portal, on hello **IBM Kenexa Survey Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![IBM Kenexa undersökning Enterprise Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="dc745-146">I hello **enkel inloggning** i dialogrutan hello **läge** väljer **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dc745-146">In hello **Single sign-on** dialog box, in hello **Mode** box, select **SAML-based Sign-on** tooenable SSO.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. <span data-ttu-id="dc745-148">I hello **IBM Kenexa undersökning företagsdomänen och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="dc745-148">In hello **IBM Kenexa Survey Enterprise Domain and URLs** section, perform hello following steps:</span></span>

    ![IBM Kenexa undersökning företagsdomänen och URL: er med enkel inloggning information](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    <span data-ttu-id="dc745-150">a.</span><span class="sxs-lookup"><span data-stu-id="dc745-150">a.</span></span> <span data-ttu-id="dc745-151">I hello **identifierare** textruta typ en URL med hello följande mönster:`https://surveys.kenexa.com/<companycode>`</span><span class="sxs-lookup"><span data-stu-id="dc745-151">In hello **Identifier** textbox, type a URL with hello following pattern: `https://surveys.kenexa.com/<companycode>`</span></span>

    <span data-ttu-id="dc745-152">b.</span><span class="sxs-lookup"><span data-stu-id="dc745-152">b.</span></span> <span data-ttu-id="dc745-153">I hello **Reply URL** textruta typ en URL med hello följande mönster:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span><span class="sxs-lookup"><span data-stu-id="dc745-153">In hello **Reply URL** textbox, type a URL with hello following pattern: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dc745-154">hello föregående värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="dc745-154">hello preceding values are not real.</span></span> <span data-ttu-id="dc745-155">Uppdatera dem med hello faktiska identifierare och reply URL.</span><span class="sxs-lookup"><span data-stu-id="dc745-155">Update them with hello actual identifier and reply URL.</span></span> <span data-ttu-id="dc745-156">tooobtain hello faktiska värden, kontakta hello [IBM Kenexa undersökning Enterprise supportteamet](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="dc745-156">tooobtain hello actual values, contact hello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

4. <span data-ttu-id="dc745-157">Under **SAML-signeringscertifikat**, klickar du på **certifikat (Base64)**, och sedan spara hello certifikat filen tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="dc745-157">Under **SAML Signing Certificate**, click **Certificate (Base64)**, and then save hello certificate file tooyour computer.</span></span>

    ![länk för hämtning av hello-certifikat (Base64)](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    <span data-ttu-id="dc745-159">hello IBM Kenexa undersökning företagsprogram förväntar tooreceive hello Security intyg Markup Language (SAML) intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar toohello konfigurationen av din token SAML-attribut.</span><span class="sxs-lookup"><span data-stu-id="dc745-159">hello IBM Kenexa Survey Enterprise application expects tooreceive hello Security Assertions Markup Language (SAML) assertions in a specific format, which requires you tooadd custom attribute mappings toohello configuration of your SAML token attributes.</span></span> <span data-ttu-id="dc745-160">hello måste värdet för hello användar-ID-anspråk i hello svar matcha hello SSO-ID som har konfigurerats i hello Kenexa system.</span><span class="sxs-lookup"><span data-stu-id="dc745-160">hello value of hello user-identifier claim in hello response must match hello SSO ID that's configured in hello Kenexa system.</span></span> <span data-ttu-id="dc745-161">toomap hello lämpliga användar-ID i din organisation som SSO Internet Datagram Protocol (IDP), arbeta med hello [IBM Kenexa undersökning Enterprise supportteamet](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="dc745-161">toomap hello appropriate user identifier in your organization as SSO Internet Datagram Protocol (IDP), work with hello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> 

    <span data-ttu-id="dc745-162">Som standard anger hello användar-ID som värde för hello användarens huvudnamn (UPN) i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dc745-162">By default, Azure AD sets hello user identifier as hello user principal name (UPN) value.</span></span> <span data-ttu-id="dc745-163">Du kan ändra värdet på hello **attributet** fliken som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="dc745-163">You can change this value on hello **Attribute** tab, as shown in hello following screenshot.</span></span> <span data-ttu-id="dc745-164">hello integration fungerar endast när du har slutfört hello mappning av på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="dc745-164">hello integration works only after you've completed hello mapping correctly.</span></span>
    
    ![dialogrutan för hello användarattribut](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png) 

5. <span data-ttu-id="dc745-166">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dc745-166">Click **Save**.</span></span>

    ![hello Konfigurera enkel inloggning spara knappen](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dc745-168">tooopen hello **konfigurera inloggning** fönstret under **företagskonfiguration för IBM Kenexa undersökning**, klickar du på **konfigurera IBM Kenexa undersökning Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="dc745-168">tooopen hello **Configure sign-on** window, under **IBM Kenexa Survey Enterprise Configuration**, click **Configure IBM Kenexa Survey Enterprise**.</span></span> 
 
    ![hello konfigurera IBM Kenexa undersökning Enterprise länk](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. <span data-ttu-id="dc745-170">Kopiera hello **Sign-Out URL**, **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** värden från hello **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dc745-170">Copy hello **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values from hello **Quick Reference** section.</span></span>

8. <span data-ttu-id="dc745-171">I hello **konfigurera inloggning** fönstret under **Snabbreferens**, kopiera hello **Sign-Out URL**, **SAML enhets-ID**, och  **SAML inloggning tjänst-URL för enkel** värden.</span><span class="sxs-lookup"><span data-stu-id="dc745-171">In hello **Configure sign-on** window, under **Quick Reference**, copy hello **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values.</span></span>

9. <span data-ttu-id="dc745-172">tooconfigure SSO på hello **IBM Kenexa undersökning Enterprise** sida, skicka hello hämtas **certifikat (Base64)**, **Sign-Out URL**, **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** värden toohello [IBM Kenexa undersökning Enterprise supportteamet](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="dc745-172">tooconfigure SSO on hello **IBM Kenexa Survey Enterprise** side, send hello downloaded **Certificate (Base64)**, **Sign-Out URL**, **SAML Entity ID**, and **SAML single sign-on Service URL** values toohello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span>

> [!TIP]
> <span data-ttu-id="dc745-173">Du kan se tooa kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com) när du ställer in hello app.</span><span class="sxs-lookup"><span data-stu-id="dc745-173">You can refer tooa concise version of these instructions in hello [Azure portal](https://portal.azure.com) while you are setting up hello app.</span></span> <span data-ttu-id="dc745-174">När du har lagt till hello appen från hello **Active Directory** > **företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** fliken och sedan ansluta till hello inbäddade dokumentationen via hello **Configuration** avsnittet hello slutet.</span><span class="sxs-lookup"><span data-stu-id="dc745-174">After you add hello app from hello **Active Directory** > **Enterprise Applications** section, simply click hello **single sign-on** tab, and then access hello embedded documentation through hello **Configuration** section at hello end.</span></span> <span data-ttu-id="dc745-175">toolearn mer om hello inbäddade dokumentationen funktionen finns [Azure AD inbäddade dokumentationen](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="dc745-175">toolearn more about hello embedded documentation feature, see [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="dc745-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="dc745-176">Create an Azure AD test user</span></span>
<span data-ttu-id="dc745-177">I det här avsnittet skapar du testanvändare Britta Simon i hello Azure-portalen hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc745-177">In this section, you create test user Britta Simon in hello Azure portal by doing hello following:</span></span>

![Skapa en testanvändare i Azure AD][100]

1. <span data-ttu-id="dc745-179">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="dc745-179">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dc745-181">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="dc745-181">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dc745-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dc745-183">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dc745-185">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="dc745-185">In hello **User** dialog box, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dc745-187">a.</span><span class="sxs-lookup"><span data-stu-id="dc745-187">a.</span></span> <span data-ttu-id="dc745-188">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dc745-188">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dc745-189">b.</span><span class="sxs-lookup"><span data-stu-id="dc745-189">b.</span></span> <span data-ttu-id="dc745-190">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dc745-190">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="dc745-191">c.</span><span class="sxs-lookup"><span data-stu-id="dc745-191">c.</span></span> <span data-ttu-id="dc745-192">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="dc745-192">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="dc745-193">d.</span><span class="sxs-lookup"><span data-stu-id="dc745-193">d.</span></span> <span data-ttu-id="dc745-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="dc745-194">Click **Create**.</span></span>
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a><span data-ttu-id="dc745-195">Skapa en testanvändare IBM Kenexa undersökning Enterprise</span><span class="sxs-lookup"><span data-stu-id="dc745-195">Create an IBM Kenexa Survey Enterprise test user</span></span>

<span data-ttu-id="dc745-196">I det här avsnittet kan du skapa en användare som kallas Britta Simon i IBM Kenexa undersökning Enterprise.</span><span class="sxs-lookup"><span data-stu-id="dc745-196">In this section, you create a user called Britta Simon in IBM Kenexa Survey Enterprise.</span></span> 

<span data-ttu-id="dc745-197">toocreate användare i hello IBM Kenexa undersökning Enterprise system och mappa hello SSO-ID för dem, kan du arbeta med hello [IBM Kenexa undersökning Enterprise supportteamet](https://www.ibm.com/support/home/?lnk=fcw).</span><span class="sxs-lookup"><span data-stu-id="dc745-197">toocreate users in hello IBM Kenexa Survey Enterprise system and map hello SSO ID for them, you can work with hello [IBM Kenexa Survey Enterprise support team](https://www.ibm.com/support/home/?lnk=fcw).</span></span> <span data-ttu-id="dc745-198">Detta SSO-ID-värde ska också vara mappad toohello användaren identifierarvärde från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dc745-198">This SSO ID value should also be mapped toohello user identifier value from Azure AD.</span></span> <span data-ttu-id="dc745-199">Du kan ändra denna standardinställning på hello **attributet** fliken.</span><span class="sxs-lookup"><span data-stu-id="dc745-199">You can change this default setting on hello **Attribute** tab.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="dc745-200">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="dc745-200">Assign hello Azure AD test user</span></span>

<span data-ttu-id="dc745-201">I det här avsnittet kan låta du användare Britta Simon toouse Azure SSO genom att bevilja åtkomst tooIBM Kenexa undersökning Enterprise.</span><span class="sxs-lookup"><span data-stu-id="dc745-201">In this section, you enable user Britta Simon toouse Azure SSO by granting access tooIBM Kenexa Survey Enterprise.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="dc745-203">tooassign användaren Britta Simon tooIBM Kenexa undersökning Enterprise hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc745-203">tooassign user Britta Simon tooIBM Kenexa Survey Enterprise, do hello following:</span></span>

1. <span data-ttu-id="dc745-204">Öppna hello i hello Azure-portalen, **program** visa, gå toohello **Directory** väljer **företagsprogram**, och klicka sedan på **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dc745-204">In hello Azure portal, open hello **Applications** view, go toohello **Directory** view, select **Enterprise applications**, and then click **All applications**.</span></span>

    ![Hej ”företagsprogram” och ”alla program” länkar][201] 

2. <span data-ttu-id="dc745-206">I hello **program** väljer **IBM Kenexa undersökning Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="dc745-206">In hello **Applications** list, select **IBM Kenexa Survey Enterprise**.</span></span>

    ![hello IBM Kenexa undersökning Enterprise länken i listan med program hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. <span data-ttu-id="dc745-208">Hello vänster klickar du på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="dc745-208">In hello left pane, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="dc745-210">Klicka på hello **Lägg till** knappen och sedan, i hello **Lägg uppdrag** väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="dc745-210">Click hello **Add** button and then, in hello **Add Assignment** pane, select **Users and groups**.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="dc745-212">I hello **användare och grupper** i dialogrutan hello **användare** väljer **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="dc745-212">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="dc745-213">I hello **användare och grupper** dialogrutan klickar du på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="dc745-213">In hello **Users and groups** dialog box, click hello **Select** button.</span></span>

7. <span data-ttu-id="dc745-214">I hello **Lägg uppdrag** dialogrutan klickar du på hello **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="dc745-214">In hello **Add Assignment** dialog box, click hello **Assign** button.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="dc745-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dc745-215">Test single sign-on</span></span>

<span data-ttu-id="dc745-216">I det här avsnittet testa du konfigurationen av Azure AD SSO med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="dc745-216">In this section, you test your Azure AD SSO configuration by using hello Access Panel.</span></span>

<span data-ttu-id="dc745-217">När du klickar på hello **IBM Kenexa undersökning Enterprise** panelen i Hej åtkomstpanelen, bör vara loggas du automatiskt tooyour IBM Kenexa undersökning företagsprogram.</span><span class="sxs-lookup"><span data-stu-id="dc745-217">When you click hello **IBM Kenexa Survey Enterprise** tile in hello Access Panel, you should be automatically signed in tooyour IBM Kenexa Survey Enterprise application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc745-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="dc745-218">Additional resources</span></span>

* [<span data-ttu-id="dc745-219">Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc745-219">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dc745-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dc745-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
