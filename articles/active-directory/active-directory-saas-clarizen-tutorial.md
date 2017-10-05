---
title: "Självstudier: Azure Active Directory-integrering med Clarizen | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Clarizen."
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
ms.openlocfilehash: 574c6877bddac8be7d6d541bfabbdc10f6be3101
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a><span data-ttu-id="84b7a-103">Självstudier: Azure Active Directory-integrering med Clarizen</span><span class="sxs-lookup"><span data-stu-id="84b7a-103">Tutorial: Azure Active Directory integration with Clarizen</span></span>

<span data-ttu-id="84b7a-104">I kursen får lära du att integrera Azure Active Directory (AD Azure) med Clarizen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-104">In this tutorial, you learn how to integrate Azure Active Directory (Azure AD) with Clarizen.</span></span> <span data-ttu-id="84b7a-105">Den här integreringen ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="84b7a-105">This integration gives you the following benefits:</span></span>

- <span data-ttu-id="84b7a-106">Du kan styra, i Azure AD, som har åtkomst till Clarizen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-106">You can control, in Azure AD, who has access to Clarizen.</span></span>
- <span data-ttu-id="84b7a-107">Du kan aktivera dina användare loggas in automatiskt till Clarizen (enkel inloggning) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="84b7a-107">You can enable your users to be automatically signed in to Clarizen (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="84b7a-108">Du kan hantera dina konton i en central plats, Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-108">You can manage your accounts in one central location, the Azure portal.</span></span>

<span data-ttu-id="84b7a-109">Ett scenario för den här kursen består av två huvudsakliga aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="84b7a-109">The scenario in this tutorial consists of two main tasks:</span></span>

1. <span data-ttu-id="84b7a-110">Lägg till Clarizen från galleriet.</span><span class="sxs-lookup"><span data-stu-id="84b7a-110">Add Clarizen from the gallery.</span></span>
2. <span data-ttu-id="84b7a-111">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84b7a-111">Configure and test Azure AD single sign-on.</span></span>

<span data-ttu-id="84b7a-112">Om du vill ha mer information om programvara som en tjänst (SaaS) appintegrering med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84b7a-112">If you want more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84b7a-113">Krav</span><span class="sxs-lookup"><span data-stu-id="84b7a-113">Prerequisites</span></span>
<span data-ttu-id="84b7a-114">För att konfigurera Azure AD-integrering med Clarizen, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="84b7a-114">To configure Azure AD integration with Clarizen, you need the following items:</span></span>

- <span data-ttu-id="84b7a-115">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="84b7a-115">An Azure AD subscription</span></span>
- <span data-ttu-id="84b7a-116">En Clarizen-prenumeration som är aktiverad för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84b7a-116">A Clarizen subscription that's enabled for single sign-on</span></span>

<span data-ttu-id="84b7a-117">Följ dessa rekommendationer för att testa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="84b7a-117">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="84b7a-118">Testa Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="84b7a-118">Test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84b7a-119">Använd inte i produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="84b7a-119">Don't use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="84b7a-120">Om du inte har en Azure AD-testmiljö, kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84b7a-120">If you don't have an Azure AD test environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="add-clarizen-from-the-gallery"></a><span data-ttu-id="84b7a-121">Lägg till Clarizen från galleriet</span><span class="sxs-lookup"><span data-stu-id="84b7a-121">Add Clarizen from the gallery</span></span>
<span data-ttu-id="84b7a-122">För att konfigurera integrering av Clarizen i Azure AD, lägga till Clarizen från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="84b7a-122">To configure the integration of Clarizen into Azure AD, add Clarizen from the gallery to your list of managed SaaS apps.</span></span>

1. <span data-ttu-id="84b7a-123">I den [Azure-portalen](https://portal.azure.com), i den vänstra rutan klickar du på den **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="84b7a-123">In the [Azure portal](https://portal.azure.com), in the left pane, click the **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory-ikonen][1]

2. <span data-ttu-id="84b7a-125">Klicka på **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-125">Click **Enterprise applications**.</span></span> <span data-ttu-id="84b7a-126">Klicka på **alla program**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-126">Then click **All applications**.</span></span>

    ![Klicka på ”företagsprogram” och ”alla program”][2]

3. <span data-ttu-id="84b7a-128">Klicka på den **Lägg till** längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84b7a-128">Click the **Add** button at the top of the dialog box.</span></span>

    ![Knappen ”Lägg till”][3]

4. <span data-ttu-id="84b7a-130">I sökrutan skriver **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-130">In the search box, type **Clarizen**.</span></span>

    ![Att skriva ”Clarizen” i sökrutan](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. <span data-ttu-id="84b7a-132">I resultatfönstret, Välj **Clarizen**, och klicka sedan på **Lägg till** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="84b7a-132">In the results pane, select **Clarizen**, and then click **Add** to add the application.</span></span>

    ![Att välja Clarizen i resultatfönstret](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="84b7a-134">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84b7a-134">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="84b7a-135">I följande avsnitt för att konfigurera och testa Azure AD enkel inloggning med Clarizen baserat på testanvändare Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84b7a-135">In the following sections, you configure and test Azure AD single sign-on with Clarizen based on the test user Britta Simon.</span></span>

<span data-ttu-id="84b7a-136">Azure AD måste du känna till användaren i Clarizen motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="84b7a-136">For single sign-on to work, Azure AD needs to know what the counterpart user in Clarizen is to a user in Azure AD.</span></span> <span data-ttu-id="84b7a-137">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Clarizen upprättas.</span><span class="sxs-lookup"><span data-stu-id="84b7a-137">In other words, a link relationship between an Azure AD user and the related user in Clarizen needs to be established.</span></span> <span data-ttu-id="84b7a-138">Du skapar den här länken relation genom att tilldela värdet för **användarnamn** i Azure AD som värde för **användarnamn** i Clarizen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-138">You establish this link relationship by assigning the value of **user name** in Azure AD as the value of **Username** in Clarizen.</span></span>

<span data-ttu-id="84b7a-139">Slutför följande byggblock för att konfigurera och testa Azure AD enkel inloggning med Clarizen:</span><span class="sxs-lookup"><span data-stu-id="84b7a-139">To configure and test Azure AD single sign-on with Clarizen, complete the following building blocks:</span></span>

1. <span data-ttu-id="84b7a-140">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  att användarna ska kunna använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-140">**[Configure Azure AD single sign-on](#configure-azure-ad-single-sign-on)** to enable your users to use this feature.</span></span>
2. <span data-ttu-id="84b7a-141">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  att testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84b7a-141">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84b7a-142">**[Skapa en testanvändare Clarizen](#create-a-clarizen-test-user)**  har en motsvarighet för Britta Simon Clarizen som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="84b7a-142">**[Create a Clarizen test user](#create-a-clarizen-test-user)** to have a counterpart of Britta Simon in Clarizen that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="84b7a-143">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  att aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84b7a-143">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84b7a-144">**[Testa enkel inloggning](#test-single-sign-on)**  att kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="84b7a-144">**[Test single sign-on](#test-single-sign-on)** to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="84b7a-145">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84b7a-145">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="84b7a-146">Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Clarizen program.</span><span class="sxs-lookup"><span data-stu-id="84b7a-146">Enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Clarizen application.</span></span>

1. <span data-ttu-id="84b7a-147">I Azure-portalen på den **Clarizen** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-147">In the Azure portal, on the **Clarizen** application integration page, click **Single sign-on**.</span></span>

    ![Klicka på ”enkel inloggning”][4]

2. <span data-ttu-id="84b7a-149">I den **enkel inloggning** i dialogrutan för **läge**väljer **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84b7a-149">In the **Single sign-on** dialog box, for **Mode**, select **SAML-based Sign-on** to enable single sign-on.</span></span>

    ![Att välja ”SAML-baserade inloggning”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. <span data-ttu-id="84b7a-151">I den **Clarizen domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="84b7a-151">In the **Clarizen Domain and URLs** section, perform the following steps:</span></span>

    ![Kryssrutor för identifierare och svars-URL](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    <span data-ttu-id="84b7a-153">a.</span><span class="sxs-lookup"><span data-stu-id="84b7a-153">a.</span></span> <span data-ttu-id="84b7a-154">I den **identifierare** Skriv värdet som: **Clarizen**</span><span class="sxs-lookup"><span data-stu-id="84b7a-154">In the **Identifier** box, type the value as: **Clarizen**</span></span>

    <span data-ttu-id="84b7a-155">b.</span><span class="sxs-lookup"><span data-stu-id="84b7a-155">b.</span></span> <span data-ttu-id="84b7a-156">I den **Reply URL** skriver en URL med hjälp av följande mönster: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span><span class="sxs-lookup"><span data-stu-id="84b7a-156">In the **Reply URL** box, type a URL by using the following pattern: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**</span></span>

    > [!NOTE]
    > <span data-ttu-id="84b7a-157">Detta är inte de verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="84b7a-157">These are not the real values.</span></span> <span data-ttu-id="84b7a-158">Du måste använda faktiska identifierare och reply URL.</span><span class="sxs-lookup"><span data-stu-id="84b7a-158">You have to use the actual identifier and reply URL.</span></span> <span data-ttu-id="84b7a-159">Här föreslår vi att du använder ett unikt värde i en sträng som identifierare.</span><span class="sxs-lookup"><span data-stu-id="84b7a-159">Here we suggest that you use the unique value of a string as the identifier.</span></span> <span data-ttu-id="84b7a-160">För att få de faktiska värdena kan kontakta den [Clarizen supportteamet](https://success.clarizen.com/hc/en-us/requests/new).</span><span class="sxs-lookup"><span data-stu-id="84b7a-160">To get the actual values, contact the [Clarizen support team](https://success.clarizen.com/hc/en-us/requests/new).</span></span>

4. <span data-ttu-id="84b7a-161">På den **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-161">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Klicka på ”Skapa nytt certifikat”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. <span data-ttu-id="84b7a-163">I den **skapa nya certifikat** dialogrutan rutan, klicka på kalenderikonen och väljer ett förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="84b7a-163">In the **Create New Certificate** dialog box, click the calendar icon and select an expiry date.</span></span> <span data-ttu-id="84b7a-164">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-164">Then click **Save**.</span></span>

    ![Att välja och spara upphör att gälla](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="84b7a-166">I den **SAML-signeringscertifikat** väljer **aktivera nya certifikatet**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-166">In the **SAML Signing Certificate** section, select **Make new certificate active**, and then click **Save**.</span></span>

    ![Om du markerar kryssrutan för att göra det nya certifikatet active](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. <span data-ttu-id="84b7a-168">I den **förnyelsecertifikat** dialogrutan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-168">In the **Rollover certificate** dialog box, click **OK**.</span></span>

    ![Klicka på ”OK” för att bekräfta att du vill aktivera certifikatet](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="84b7a-170">I den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="84b7a-170">In the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Klicka på ”intyg (Base64)” för att starta nedladdningen](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. <span data-ttu-id="84b7a-172">I den **Clarizen Configuration** klickar du på **konfigurera Clarizen** att öppna den **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="84b7a-172">In the **Clarizen Configuration** section, click **Configure Clarizen** to open the **Configure sign-on** window.</span></span>

    ![Klicka på ”Konfigurera Clarizen”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    ![”Konfigurera inloggning” fönster, inklusive filer och URL: er](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. <span data-ttu-id="84b7a-175">I en annan webbläsarfönster loggar du in på ditt Clarizen företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="84b7a-175">In a different web browser window, sign in to your Clarizen company site as an administrator.</span></span>

11. <span data-ttu-id="84b7a-176">Klicka på ditt användarnamn och klicka sedan på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-176">Click your username, and then click **Settings**.</span></span>

    <span data-ttu-id="84b7a-177">![Klicka på ”inställningar” under ditt användarnamn](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="84b7a-177">![Clicking "Settings" under your username](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "Settings")</span></span>

12. <span data-ttu-id="84b7a-178">Klicka på den **globala inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="84b7a-178">Click the **Global Settings** tab.</span></span> <span data-ttu-id="84b7a-179">Sedan bredvid **federerad autentisering**, klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-179">Then, next to **Federated Authentication**, click **edit**.</span></span>

    <span data-ttu-id="84b7a-180">![”Globala inställningar”](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globala inställningar")</span><span class="sxs-lookup"><span data-stu-id="84b7a-180">!["Global Settings" tab](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "Global Settings")</span></span>

13. <span data-ttu-id="84b7a-181">I den **federerad autentisering** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="84b7a-181">In the **Federated Authentication** dialog box, perform the following steps:</span></span>

    <span data-ttu-id="84b7a-182">![”Extern autentisering” dialogrutan](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "federerad autentisering")</span><span class="sxs-lookup"><span data-stu-id="84b7a-182">!["Federated Authentication" dialog box](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication")</span></span>

    <span data-ttu-id="84b7a-183">a.</span><span class="sxs-lookup"><span data-stu-id="84b7a-183">a.</span></span> <span data-ttu-id="84b7a-184">Välj **aktivera federerad autentisering**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-184">Select **Enable Federated Authentication**.</span></span>

    <span data-ttu-id="84b7a-185">b.</span><span class="sxs-lookup"><span data-stu-id="84b7a-185">b.</span></span> <span data-ttu-id="84b7a-186">Klicka på **överför** att överföra din hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="84b7a-186">Click **Upload** to upload your downloaded certificate.</span></span>

    <span data-ttu-id="84b7a-187">c.</span><span class="sxs-lookup"><span data-stu-id="84b7a-187">c.</span></span> <span data-ttu-id="84b7a-188">I den **inloggning URL** anger du värdet för **SAML inloggning tjänst-URL för enkel** från Azure AD-konfigurationsfönstret.</span><span class="sxs-lookup"><span data-stu-id="84b7a-188">In the **Sign-in URL** box, enter the value of **SAML Single Sign-On Service URL** from the Azure AD application configuration window.</span></span>

    <span data-ttu-id="84b7a-189">d.</span><span class="sxs-lookup"><span data-stu-id="84b7a-189">d.</span></span> <span data-ttu-id="84b7a-190">I den **Sign-out URL** anger du värdet för **Sign-Out URL** från Azure AD-konfigurationsfönstret.</span><span class="sxs-lookup"><span data-stu-id="84b7a-190">In the **Sign-out URL** box, enter the value of **Sign-Out URL** from the Azure AD application configuration window.</span></span>

    <span data-ttu-id="84b7a-191">e.</span><span class="sxs-lookup"><span data-stu-id="84b7a-191">e.</span></span> <span data-ttu-id="84b7a-192">Välj **använder POST**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-192">Select **Use POST**.</span></span>

    <span data-ttu-id="84b7a-193">f.</span><span class="sxs-lookup"><span data-stu-id="84b7a-193">f.</span></span> <span data-ttu-id="84b7a-194">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-194">Click **Save**.</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="84b7a-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="84b7a-195">Create an Azure AD test user</span></span>
<span data-ttu-id="84b7a-196">Skapa en testanvändare kallas Britta Simon i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-196">In the Azure portal, create a test user called Britta Simon.</span></span>

![Namn och e-postadress för användare i Azure AD][100]

1. <span data-ttu-id="84b7a-198">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="84b7a-198">In the Azure portal, in the left pane, click the **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory-ikonen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="84b7a-200">Klicka på **användare och grupper**, och klicka sedan på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="84b7a-200">Click **Users and groups**, and then click **All users** to display the list of users.</span></span>

    ![Klicka på ”användare och grupper” och ”alla användare”](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="84b7a-202">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84b7a-202">At the top of the dialog box, click **Add** to open the **User** dialog box.</span></span>

    ![Knappen ”Lägg till”](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="84b7a-204">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="84b7a-204">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan ”användare” med namn, e-postadress och lösenord som har fyllt i](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    <span data-ttu-id="84b7a-206">a.</span><span class="sxs-lookup"><span data-stu-id="84b7a-206">a.</span></span> <span data-ttu-id="84b7a-207">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-207">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84b7a-208">b.</span><span class="sxs-lookup"><span data-stu-id="84b7a-208">b.</span></span> <span data-ttu-id="84b7a-209">I den **användarnamn** Skriv e-postadressen för kontot som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84b7a-209">In the **User name** box, type the email address of the Britta Simon account.</span></span>

    <span data-ttu-id="84b7a-210">c.</span><span class="sxs-lookup"><span data-stu-id="84b7a-210">c.</span></span> <span data-ttu-id="84b7a-211">Välj **visa lösenordet** och anteckna värdet för **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-211">Select **Show Password** and write down the value of **Password**.</span></span>

    <span data-ttu-id="84b7a-212">d.</span><span class="sxs-lookup"><span data-stu-id="84b7a-212">d.</span></span> <span data-ttu-id="84b7a-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-213">Click **Create**.</span></span>

### <a name="create-a-clarizen-test-user"></a><span data-ttu-id="84b7a-214">Skapa en testanvändare Clarizen</span><span class="sxs-lookup"><span data-stu-id="84b7a-214">Create a Clarizen test user</span></span>
<span data-ttu-id="84b7a-215">Om du vill aktivera Azure AD-användare att logga in på Clarizen, måste du etablera användarkonton.</span><span class="sxs-lookup"><span data-stu-id="84b7a-215">To enable Azure AD users to sign in to Clarizen, you must provision user accounts.</span></span> <span data-ttu-id="84b7a-216">När det gäller Clarizen är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="84b7a-216">In the case of Clarizen, provisioning is a manual task.</span></span>

1. <span data-ttu-id="84b7a-217">Logga in på webbplatsen Clarizen företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="84b7a-217">Sign in to your Clarizen company site as an administrator.</span></span>

2. <span data-ttu-id="84b7a-218">Klicka på **personer**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-218">Click **People**.</span></span>

    <span data-ttu-id="84b7a-219">![Klicka på ”personer”](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "personer")</span><span class="sxs-lookup"><span data-stu-id="84b7a-219">![Clicking "People"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="84b7a-220">Klicka på **bjuda in användare**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-220">Click **Invite User**.</span></span>

    <span data-ttu-id="84b7a-221">![”Bjuda in användare” knappen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "bjuda in användare")</span><span class="sxs-lookup"><span data-stu-id="84b7a-221">!["Invite User" button](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="84b7a-222">I den **bjuda in personer** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="84b7a-222">In the **Invite People** dialog box, perform the following steps:</span></span>

    <span data-ttu-id="84b7a-223">![”” Dialogrutan](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "bjuda in användare")</span><span class="sxs-lookup"><span data-stu-id="84b7a-223">!["Invite People" dialog box](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="84b7a-224">a.</span><span class="sxs-lookup"><span data-stu-id="84b7a-224">a.</span></span> <span data-ttu-id="84b7a-225">I den **e-post** Skriv e-postadressen för kontot som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84b7a-225">In the **Email** box, type the email address of the Britta Simon account.</span></span>

    <span data-ttu-id="84b7a-226">b.</span><span class="sxs-lookup"><span data-stu-id="84b7a-226">b.</span></span> <span data-ttu-id="84b7a-227">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-227">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="84b7a-228">Innehavaren för Azure Active Directory-konto ett e-postmeddelande och länken för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="84b7a-228">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="84b7a-229">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="84b7a-229">Assign the Azure AD test user</span></span>
<span data-ttu-id="84b7a-230">Aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Clarizen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-230">Enable Britta Simon to use Azure single sign-on by granting her access to Clarizen.</span></span>

![Tilldelad användare][200]

1. <span data-ttu-id="84b7a-232">I Azure portal, öppna program visas bläddrar du till vyn directory klickar du på **företagsprogram**, och klicka sedan på **alla program**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-232">In the Azure portal, open the applications view, browse to the directory view, click **Enterprise applications**, and then click **All applications**.</span></span>

    ![Klicka på ”företagsprogram” och ”alla program”][201]

2. <span data-ttu-id="84b7a-234">Välj i listan med program **Clarizen**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-234">In the applications list, select **Clarizen**.</span></span>

    ![Att välja Clarizen i listan](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. <span data-ttu-id="84b7a-236">I den vänstra rutan klickar du på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-236">In the left pane, click **Users and groups**.</span></span>

    ![Klicka på ”användare och grupper”][202]

4. <span data-ttu-id="84b7a-238">Klicka på den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-238">Click the **Add** button.</span></span> <span data-ttu-id="84b7a-239">I den **Lägg uppdrag** dialogrutan **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="84b7a-239">Then, in the **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![Knappen ”Lägg till” och dialogrutan ”Lägg till tilldelning”][203]

5. <span data-ttu-id="84b7a-241">I den **användare och grupper** dialogrutan **Britta Simon** i listan över användare.</span><span class="sxs-lookup"><span data-stu-id="84b7a-241">In the **Users and groups** dialog box, select **Britta Simon** in the list of users.</span></span>

6. <span data-ttu-id="84b7a-242">I den **användare och grupper** dialogrutan klickar du på den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-242">In the **Users and groups** dialog box, click the **Select** button.</span></span>

7. <span data-ttu-id="84b7a-243">I den **Lägg uppdrag** dialogrutan klickar du på den **tilldela** knappen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-243">In the **Add Assignment** dialog box, click the **Assign** button.</span></span>

### <a name="test-single-sign-on"></a><span data-ttu-id="84b7a-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84b7a-244">Test single sign-on</span></span>
<span data-ttu-id="84b7a-245">Testa Azure AD enkel inloggning konfigurationen med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-245">Test your Azure AD single sign-on configuration by using the Access Panel.</span></span>

<span data-ttu-id="84b7a-246">När du klickar på panelen Clarizen på åtkomstpanelen bör du vara automatiskt inloggad i tillämpningsprogrammet Clarizen.</span><span class="sxs-lookup"><span data-stu-id="84b7a-246">When you click the Clarizen tile in the Access Panel, you should be automatically signed in to your Clarizen application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84b7a-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="84b7a-247">Additional resources</span></span>

* [<span data-ttu-id="84b7a-248">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="84b7a-248">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84b7a-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="84b7a-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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
