---
title: "Självstudier: Azure Active Directory-integrering med Clarizen | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Clarizen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: f24ccda3b90e5df9a203a444dfda905043b30276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a><span data-ttu-id="9a975-103">Självstudier: Azure Active Directory-integrering med Clarizen</span><span class="sxs-lookup"><span data-stu-id="9a975-103">Tutorial: Azure Active Directory integration with Clarizen</span></span>

<span data-ttu-id="9a975-104">I kursen får du lära dig hur toointegrate Azure Active Directory (AD Azure) med Clarizen.</span><span class="sxs-lookup"><span data-stu-id="9a975-104">In this tutorial, you learn how toointegrate Azure Active Directory (Azure AD) with Clarizen.</span></span> <span data-ttu-id="9a975-105">Den här integreringen ger du hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9a975-105">This integration gives you hello following benefits:</span></span>

- <span data-ttu-id="9a975-106">Du kan styra, i Azure AD, som har åtkomst till tooClarizen.</span><span class="sxs-lookup"><span data-stu-id="9a975-106">You can control, in Azure AD, who has access tooClarizen.</span></span>
- <span data-ttu-id="9a975-107">Du kan låta dina användare toobe loggas automatiskt in tooClarizen (enkel inloggning) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="9a975-107">You can enable your users toobe automatically signed in tooClarizen (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="9a975-108">Du kan hantera dina konton i en central plats, hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9a975-108">You can manage your accounts in one central location, hello Azure portal.</span></span>

<span data-ttu-id="9a975-109">hello-scenario i den här kursen består av två huvudsakliga aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="9a975-109">hello scenario in this tutorial consists of two main tasks:</span></span>

1. <span data-ttu-id="9a975-110">Lägg till Clarizen från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="9a975-110">Add Clarizen from hello gallery.</span></span>
2. <span data-ttu-id="9a975-111">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9a975-111">Configure and test Azure AD single sign-on.</span></span>

<span data-ttu-id="9a975-112">Om du vill ha mer information om programvara som en tjänst (SaaS) appintegrering med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9a975-112">If you want more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a975-113">Krav</span><span class="sxs-lookup"><span data-stu-id="9a975-113">Prerequisites</span></span>
<span data-ttu-id="9a975-114">tooconfigure Azure AD-integrering med Clarizen, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9a975-114">tooconfigure Azure AD integration with Clarizen, you need hello following items:</span></span>

- <span data-ttu-id="9a975-115">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9a975-115">An Azure AD subscription</span></span>
- <span data-ttu-id="9a975-116">En Clarizen-prenumeration som är aktiverad för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a975-116">A Clarizen subscription that's enabled for single sign-on</span></span>

<span data-ttu-id="9a975-117">tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9a975-117">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="9a975-118">Testa Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9a975-118">Test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9a975-119">Använd inte i produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9a975-119">Don't use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9a975-120">Om du inte har en Azure AD-testmiljö, kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9a975-120">If you don't have an Azure AD test environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="add-clarizen-from-hello-gallery"></a><span data-ttu-id="9a975-121">Lägg till Clarizen från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9a975-121">Add Clarizen from hello gallery</span></span>
<span data-ttu-id="9a975-122">tooconfigure hello integrering av Clarizen i Azure AD och lägga till Clarizen från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9a975-122">tooconfigure hello integration of Clarizen into Azure AD, add Clarizen from hello gallery tooyour list of managed SaaS apps.</span></span>

1. <span data-ttu-id="9a975-123">I hello [Azure-portalen](https://portal.azure.com), i hello vänster, klicka på hello **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9a975-123">In hello [Azure portal](https://portal.azure.com), in hello left pane, click hello **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory-ikonen][1]

2. <span data-ttu-id="9a975-125">Klicka på **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9a975-125">Click **Enterprise applications**.</span></span> <span data-ttu-id="9a975-126">Klicka på **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9a975-126">Then click **All applications**.</span></span>

    ![Klicka på ”företagsprogram” och ”alla program”][2]

3. <span data-ttu-id="9a975-128">Klicka på hello **Lägg till** knappen hello överst i dialogrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="9a975-128">Click hello **Add** button at hello top of hello dialog box.</span></span>

    ![Hej ”Lägg till”-knappen][3]

4. <span data-ttu-id="9a975-130">Skriv i sökrutan hello **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="9a975-130">In hello search box, type **Clarizen**.</span></span>

    ![Att skriva ”Clarizen” i sökrutan för hello](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. <span data-ttu-id="9a975-132">I resultatfönstret hello väljer **Clarizen**, och klicka sedan på **Lägg till** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9a975-132">In hello results pane, select **Clarizen**, and then click **Add** tooadd hello application.</span></span>

    ![Att välja Clarizen i resultatfönstret hello](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9a975-134">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a975-134">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="9a975-135">I följande avsnitt hello, konfigurera och testa Azure AD enkel inloggning med Clarizen baserat på hello testanvändare Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9a975-135">In hello following sections, you configure and test Azure AD single sign-on with Clarizen based on hello test user Britta Simon.</span></span>

<span data-ttu-id="9a975-136">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Clarizen är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9a975-136">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Clarizen is tooa user in Azure AD.</span></span> <span data-ttu-id="9a975-137">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Clarizen toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9a975-137">In other words, a link relationship between an Azure AD user and hello related user in Clarizen needs toobe established.</span></span> <span data-ttu-id="9a975-138">Du etablerar den här länken relationen genom att tilldela värdet hello **användarnamn** i Azure AD som hello värde för **användarnamn** i Clarizen.</span><span class="sxs-lookup"><span data-stu-id="9a975-138">You establish this link relationship by assigning hello value of **user name** in Azure AD as hello value of **Username** in Clarizen.</span></span>

<span data-ttu-id="9a975-139">tooconfigure och testa Azure AD enkel inloggning med Clarizen fullständig hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9a975-139">tooconfigure and test Azure AD single sign-on with Clarizen, complete hello following building blocks:</span></span>

1. <span data-ttu-id="9a975-140">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9a975-140">**[Configure Azure AD single sign-on](#configure-azure-ad-single-sign-on)** tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9a975-141">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9a975-141">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9a975-142">**[Skapa en testanvändare Clarizen](#create-a-clarizen-test-user)**  toohave en motsvarighet för Britta Simon i Clarizen som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="9a975-142">**[Create a Clarizen test user](#create-a-clarizen-test-user)** toohave a counterpart of Britta Simon in Clarizen that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="9a975-143">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9a975-143">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9a975-144">**[Testa enkel inloggning](#test-single-sign-on)**  tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9a975-144">**[Test single sign-on](#test-single-sign-on)** tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9a975-145">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a975-145">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="9a975-146">Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Clarizen program.</span><span class="sxs-lookup"><span data-stu-id="9a975-146">Enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Clarizen application.</span></span>

1. <span data-ttu-id="9a975-147">I hello Azure-portalen på hello **Clarizen** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9a975-147">In hello Azure portal, on hello **Clarizen** application integration page, click **Single sign-on**.</span></span>

    ![Klicka på ”enkel inloggning”][4]

2. <span data-ttu-id="9a975-149">I hello **enkel inloggning** i dialogrutan för **läge**väljer **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9a975-149">In hello **Single sign-on** dialog box, for **Mode**, select **SAML-based Sign-on** tooenable single sign-on.</span></span>

    ![Att välja ”SAML-baserade inloggning”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. <span data-ttu-id="9a975-151">I hello **Clarizen domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a975-151">In hello **Clarizen Domain and URLs** section, perform hello following steps:</span></span>

    ![Kryssrutor för identifierare och svars-URL](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    <span data-ttu-id="9a975-153">a.</span><span class="sxs-lookup"><span data-stu-id="9a975-153">a.</span></span> <span data-ttu-id="9a975-154">I hello **identifierare** rutan hello TYPVÄRDE som: **Clarizen**</span><span class="sxs-lookup"><span data-stu-id="9a975-154">In hello **Identifier** box, type hello value as: **Clarizen**</span></span>

    <span data-ttu-id="9a975-155">b.</span><span class="sxs-lookup"><span data-stu-id="9a975-155">b.</span></span> <span data-ttu-id="9a975-156">I hello **Reply URL** skriver en URL med hjälp av hello följer mönstret: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span><span class="sxs-lookup"><span data-stu-id="9a975-156">In hello **Reply URL** box, type a URL by using hello following pattern: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span></span>

    > [!NOTE]
    > <span data-ttu-id="9a975-157">Detta är inte hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="9a975-157">These are not hello real values.</span></span> <span data-ttu-id="9a975-158">Du har toouse hello faktiska identifierare och reply URL.</span><span class="sxs-lookup"><span data-stu-id="9a975-158">You have toouse hello actual identifier and reply URL.</span></span> <span data-ttu-id="9a975-159">Här föreslår vi att du använder hello unikt värde i en sträng som hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="9a975-159">Here we suggest that you use hello unique value of a string as hello identifier.</span></span> <span data-ttu-id="9a975-160">tooget hello faktiska värden, kontakta hello [Clarizen supportteamet](https://success.clarizen.com/hc/en-us/requests/new).</span><span class="sxs-lookup"><span data-stu-id="9a975-160">tooget hello actual values, contact hello [Clarizen support team](https://success.clarizen.com/hc/en-us/requests/new).</span></span>

4. <span data-ttu-id="9a975-161">På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="9a975-161">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Klicka på ”Skapa nytt certifikat”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. <span data-ttu-id="9a975-163">I hello **skapa nya certifikat** dialogrutan rutan, klicka hello kalendern och välj ett förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="9a975-163">In hello **Create New Certificate** dialog box, click hello calendar icon and select an expiry date.</span></span> <span data-ttu-id="9a975-164">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9a975-164">Then click **Save**.</span></span>

    ![Att välja och spara upphör att gälla](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="9a975-166">I hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="9a975-166">In hello **SAML Signing Certificate** section, select **Make new certificate active**, and then click **Save**.</span></span>

    ![Om du markerar kryssrutan för hello för att göra hello nytt certifikat active](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. <span data-ttu-id="9a975-168">I hello **förnyelsecertifikat** dialogrutan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a975-168">In hello **Rollover certificate** dialog box, click **OK**.</span></span>

    ![Klicka på ”OK” tooconfirm som du vill toomake hello certifikat active](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9a975-170">I hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="9a975-170">In hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Klicka på ”(Base64)” toostart hello hämtning](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. <span data-ttu-id="9a975-172">I hello **Clarizen Configuration** klickar du på **konfigurera Clarizen** tooopen hello **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="9a975-172">In hello **Clarizen Configuration** section, click **Configure Clarizen** tooopen hello **Configure sign-on** window.</span></span>

    ![Klicka på ”Konfigurera Clarizen”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    ![”Konfigurera inloggning” fönster, inklusive filer och URL: er](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. <span data-ttu-id="9a975-175">Logga in tooyour Clarizen företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="9a975-175">In a different web browser window, sign in tooyour Clarizen company site as an administrator.</span></span>

11. <span data-ttu-id="9a975-176">Klicka på ditt användarnamn och klicka sedan på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="9a975-176">Click your username, and then click **Settings**.</span></span>

    <span data-ttu-id="9a975-177">![Klicka på ”inställningar” under ditt användarnamn](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="9a975-177">![Clicking "Settings" under your username](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "Settings")</span></span>

12. <span data-ttu-id="9a975-178">Klicka på hello **globala inställningar** fliken. Sedan Nästa för**federerad autentisering**, klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="9a975-178">Click hello **Global Settings** tab. Then, next too**Federated Authentication**, click **edit**.</span></span>

    <span data-ttu-id="9a975-179">![”Globala inställningar”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globala inställningar")</span><span class="sxs-lookup"><span data-stu-id="9a975-179">!["Global Settings" tab](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "Global Settings")</span></span>

13. <span data-ttu-id="9a975-180">I hello **federerad autentisering** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a975-180">In hello **Federated Authentication** dialog box, perform hello following steps:</span></span>

    <span data-ttu-id="9a975-181">![”Extern autentisering” dialogrutan](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "federerad autentisering")</span><span class="sxs-lookup"><span data-stu-id="9a975-181">!["Federated Authentication" dialog box](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication")</span></span>

    <span data-ttu-id="9a975-182">a.</span><span class="sxs-lookup"><span data-stu-id="9a975-182">a.</span></span> <span data-ttu-id="9a975-183">Välj **aktivera federerad autentisering**.</span><span class="sxs-lookup"><span data-stu-id="9a975-183">Select **Enable Federated Authentication**.</span></span>

    <span data-ttu-id="9a975-184">b.</span><span class="sxs-lookup"><span data-stu-id="9a975-184">b.</span></span> <span data-ttu-id="9a975-185">Klicka på **överför** tooupload hämtade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="9a975-185">Click **Upload** tooupload your downloaded certificate.</span></span>

    <span data-ttu-id="9a975-186">c.</span><span class="sxs-lookup"><span data-stu-id="9a975-186">c.</span></span> <span data-ttu-id="9a975-187">I hello **inloggning URL** ange hello värdet för **SAML inloggning tjänst-URL för enkel** från hello Azure AD-konfigurationsfönstret.</span><span class="sxs-lookup"><span data-stu-id="9a975-187">In hello **Sign-in URL** box, enter hello value of **SAML Single Sign-On Service URL** from hello Azure AD application configuration window.</span></span>

    <span data-ttu-id="9a975-188">d.</span><span class="sxs-lookup"><span data-stu-id="9a975-188">d.</span></span> <span data-ttu-id="9a975-189">I hello **Sign-out URL** ange hello värdet för **Sign-Out URL** från hello Azure AD-konfigurationsfönstret.</span><span class="sxs-lookup"><span data-stu-id="9a975-189">In hello **Sign-out URL** box, enter hello value of **Sign-Out URL** from hello Azure AD application configuration window.</span></span>

    <span data-ttu-id="9a975-190">e.</span><span class="sxs-lookup"><span data-stu-id="9a975-190">e.</span></span> <span data-ttu-id="9a975-191">Välj **använder POST**.</span><span class="sxs-lookup"><span data-stu-id="9a975-191">Select **Use POST**.</span></span>

    <span data-ttu-id="9a975-192">f.</span><span class="sxs-lookup"><span data-stu-id="9a975-192">f.</span></span> <span data-ttu-id="9a975-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9a975-193">Click **Save**.</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9a975-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a975-194">Create an Azure AD test user</span></span>
<span data-ttu-id="9a975-195">Skapa en testanvändare kallas Britta Simon i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9a975-195">In hello Azure portal, create a test user called Britta Simon.</span></span>

![Namn och e-postadress för användare hello Azure AD][100]

1. <span data-ttu-id="9a975-197">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9a975-197">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory-ikonen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="9a975-199">Klicka på **användare och grupper**, och klicka sedan på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="9a975-199">Click **Users and groups**, and then click **All users** toodisplay hello list of users.</span></span>

    ![Klicka på ”användare och grupper” och ”alla användare”](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="9a975-201">Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9a975-201">At hello top of hello dialog box, click **Add** tooopen hello **User** dialog box.</span></span>

    ![Hej ”Lägg till”-knappen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="9a975-203">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a975-203">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogrutan ”användare” med namn, e-postadress och lösenord som har fyllt i](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    <span data-ttu-id="9a975-205">a.</span><span class="sxs-lookup"><span data-stu-id="9a975-205">a.</span></span> <span data-ttu-id="9a975-206">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9a975-206">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9a975-207">b.</span><span class="sxs-lookup"><span data-stu-id="9a975-207">b.</span></span> <span data-ttu-id="9a975-208">I hello **användarnamn** rutan typen hello e-postadress hello Britta Simon konto.</span><span class="sxs-lookup"><span data-stu-id="9a975-208">In hello **User name** box, type hello email address of hello Britta Simon account.</span></span>

    <span data-ttu-id="9a975-209">c.</span><span class="sxs-lookup"><span data-stu-id="9a975-209">c.</span></span> <span data-ttu-id="9a975-210">Välj **visa lösenordet** och anteckna värdet hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9a975-210">Select **Show Password** and write down hello value of **Password**.</span></span>

    <span data-ttu-id="9a975-211">d.</span><span class="sxs-lookup"><span data-stu-id="9a975-211">d.</span></span> <span data-ttu-id="9a975-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9a975-212">Click **Create**.</span></span>

### <a name="create-a-clarizen-test-user"></a><span data-ttu-id="9a975-213">Skapa en testanvändare Clarizen</span><span class="sxs-lookup"><span data-stu-id="9a975-213">Create a Clarizen test user</span></span>
<span data-ttu-id="9a975-214">tooenable Azure AD-användare toosign i tooClarizen, måste du etablera användarkonton.</span><span class="sxs-lookup"><span data-stu-id="9a975-214">tooenable Azure AD users toosign in tooClarizen, you must provision user accounts.</span></span> <span data-ttu-id="9a975-215">Hello gäller Clarizen är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9a975-215">In hello case of Clarizen, provisioning is a manual task.</span></span>

1. <span data-ttu-id="9a975-216">Logga in tooyour Clarizen företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="9a975-216">Sign in tooyour Clarizen company site as an administrator.</span></span>

2. <span data-ttu-id="9a975-217">Klicka på **personer**.</span><span class="sxs-lookup"><span data-stu-id="9a975-217">Click **People**.</span></span>

    <span data-ttu-id="9a975-218">![Klicka på ”personer”](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "personer")</span><span class="sxs-lookup"><span data-stu-id="9a975-218">![Clicking "People"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="9a975-219">Klicka på **bjuda in användare**.</span><span class="sxs-lookup"><span data-stu-id="9a975-219">Click **Invite User**.</span></span>

    <span data-ttu-id="9a975-220">![”Bjuda in användare” knappen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "bjuda in användare")</span><span class="sxs-lookup"><span data-stu-id="9a975-220">!["Invite User" button](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="9a975-221">I hello **bjuda in personer** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9a975-221">In hello **Invite People** dialog box, perform hello following steps:</span></span>

    <span data-ttu-id="9a975-222">![”” Dialogrutan](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "bjuda in användare")</span><span class="sxs-lookup"><span data-stu-id="9a975-222">!["Invite People" dialog box](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="9a975-223">a.</span><span class="sxs-lookup"><span data-stu-id="9a975-223">a.</span></span> <span data-ttu-id="9a975-224">I hello **e-post** rutan typen hello e-postadress hello Britta Simon konto.</span><span class="sxs-lookup"><span data-stu-id="9a975-224">In hello **Email** box, type hello email address of hello Britta Simon account.</span></span>

    <span data-ttu-id="9a975-225">b.</span><span class="sxs-lookup"><span data-stu-id="9a975-225">b.</span></span> <span data-ttu-id="9a975-226">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="9a975-226">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9a975-227">hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="9a975-227">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9a975-228">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9a975-228">Assign hello Azure AD test user</span></span>
<span data-ttu-id="9a975-229">Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooClarizen sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9a975-229">Enable Britta Simon toouse Azure single sign-on by granting her access tooClarizen.</span></span>

![Tilldelad användare][200]

1. <span data-ttu-id="9a975-231">I hello Azure-portalen, öppna hello program, toohello directory bläddringsläge, klicka på **företagsprogram**, och klicka sedan på **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9a975-231">In hello Azure portal, open hello applications view, browse toohello directory view, click **Enterprise applications**, and then click **All applications**.</span></span>

    ![Klicka på ”företagsprogram” och ”alla program”][201]

2. <span data-ttu-id="9a975-233">Välj i listan med program hello **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="9a975-233">In hello applications list, select **Clarizen**.</span></span>

    ![Välj Clarizen hello listan](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. <span data-ttu-id="9a975-235">Hello vänster klickar du på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9a975-235">In hello left pane, click **Users and groups**.</span></span>

    ![Klicka på ”användare och grupper”][202]

4. <span data-ttu-id="9a975-237">Klicka på hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9a975-237">Click hello **Add** button.</span></span> <span data-ttu-id="9a975-238">Sedan hello **Lägg uppdrag** dialogrutan **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9a975-238">Then, in hello **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![Hej ”Lägg till”-knappen och dialogrutan för hello ”Lägg till tilldelning”][203]

5. <span data-ttu-id="9a975-240">I hello **användare och grupper** dialogrutan **Britta Simon** i hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="9a975-240">In hello **Users and groups** dialog box, select **Britta Simon** in hello list of users.</span></span>

6. <span data-ttu-id="9a975-241">I hello **användare och grupper** dialogrutan klickar du på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="9a975-241">In hello **Users and groups** dialog box, click hello **Select** button.</span></span>

7. <span data-ttu-id="9a975-242">I hello **Lägg uppdrag** dialogrutan klickar du på hello **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="9a975-242">In hello **Add Assignment** dialog box, click hello **Assign** button.</span></span>

### <a name="test-single-sign-on"></a><span data-ttu-id="9a975-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9a975-243">Test single sign-on</span></span>
<span data-ttu-id="9a975-244">Testa Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9a975-244">Test your Azure AD single sign-on configuration by using hello Access Panel.</span></span>

<span data-ttu-id="9a975-245">När du klickar på hello Clarizen panelen i hello åtkomstpanelen bör vara loggas du automatiskt tooyour Clarizen program.</span><span class="sxs-lookup"><span data-stu-id="9a975-245">When you click hello Clarizen tile in hello Access Panel, you should be automatically signed in tooyour Clarizen application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a975-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9a975-246">Additional resources</span></span>

* [<span data-ttu-id="9a975-247">Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9a975-247">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9a975-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9a975-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
