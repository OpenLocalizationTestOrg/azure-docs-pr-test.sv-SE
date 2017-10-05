---
title: "Självstudier: Azure Active Directory-integrering med GitHub | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och GitHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 9dc12bc2e313bcb2000724d035156c5054d14e1c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a><span data-ttu-id="88cc0-103">Självstudier: Azure Active Directory-integrering med GitHub</span><span class="sxs-lookup"><span data-stu-id="88cc0-103">Tutorial: Azure Active Directory integration with GitHub</span></span>

<span data-ttu-id="88cc0-104">I kursen får lära du att integrera GitHub med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="88cc0-104">In this tutorial, you learn how to integrate GitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="88cc0-105">Integrera GitHub med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="88cc0-105">Integrating GitHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="88cc0-106">Du kan styra i Azure AD som har åtkomst till GitHub</span><span class="sxs-lookup"><span data-stu-id="88cc0-106">You can control in Azure AD who has access to GitHub</span></span>
- <span data-ttu-id="88cc0-107">Du kan aktivera användarna att automatiskt hämta loggat in på GitHub (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="88cc0-107">You can enable your users to automatically get signed-on to GitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="88cc0-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="88cc0-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="88cc0-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="88cc0-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88cc0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="88cc0-110">Prerequisites</span></span>

<span data-ttu-id="88cc0-111">För att konfigurera Azure AD-integrering med GitHub, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="88cc0-111">To configure Azure AD integration with GitHub, you need the following items:</span></span>

- <span data-ttu-id="88cc0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="88cc0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="88cc0-113">En GitHub enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="88cc0-113">A GitHub single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="88cc0-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="88cc0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="88cc0-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="88cc0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="88cc0-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="88cc0-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="88cc0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="88cc0-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="88cc0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="88cc0-118">Scenario description</span></span>
<span data-ttu-id="88cc0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="88cc0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="88cc0-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="88cc0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="88cc0-121">Att lägga till GitHub från galleriet</span><span class="sxs-lookup"><span data-stu-id="88cc0-121">Adding GitHub from the gallery</span></span>
2. <span data-ttu-id="88cc0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88cc0-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-github-from-the-gallery"></a><span data-ttu-id="88cc0-123">Att lägga till GitHub från galleriet</span><span class="sxs-lookup"><span data-stu-id="88cc0-123">Adding GitHub from the gallery</span></span>
<span data-ttu-id="88cc0-124">Du måste lägga till GitHub från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av GitHub i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="88cc0-124">To configure the integration of GitHub into Azure AD, you need to add GitHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="88cc0-125">**Utför följande steg för att lägga till GitHub från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="88cc0-125">**To add GitHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="88cc0-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="88cc0-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="88cc0-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="88cc0-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="88cc0-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="88cc0-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="88cc0-133">I sökrutan skriver **GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-133">In the search box, type **GitHub.com**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. <span data-ttu-id="88cc0-135">Välj i resultatpanelen **GitHub**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="88cc0-135">In the results panel, select **GitHub**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="88cc0-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88cc0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="88cc0-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med GitHub baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="88cc0-138">In this section, you configure and test Azure AD single sign-on with GitHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="88cc0-139">Azure AD måste du känna till användaren i GitHub motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="88cc0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in GitHub is to a user in Azure AD.</span></span> <span data-ttu-id="88cc0-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i GitHub upprättas.</span><span class="sxs-lookup"><span data-stu-id="88cc0-140">In other words, a link relationship between an Azure AD user and the related user in GitHub needs to be established.</span></span>

<span data-ttu-id="88cc0-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i GitHub.</span><span class="sxs-lookup"><span data-stu-id="88cc0-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in GitHub.</span></span>

<span data-ttu-id="88cc0-142">Om du vill konfigurera och testa Azure AD enkel inloggning med GitHub, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="88cc0-142">To configure and test Azure AD single sign-on with GitHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="88cc0-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="88cc0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="88cc0-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="88cc0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="88cc0-145">**[Skapa en testanvändare i GitHub](#creating-a-GitHub-test-user)**  – du har en motsvarighet för Britta Simon i GitHub som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="88cc0-145">**[Creating a GitHub test user](#creating-a-GitHub-test-user)** - to have a counterpart of Britta Simon in GitHub that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="88cc0-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88cc0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="88cc0-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="88cc0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="88cc0-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88cc0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="88cc0-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt GitHub-program.</span><span class="sxs-lookup"><span data-stu-id="88cc0-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your GitHub application.</span></span>

<span data-ttu-id="88cc0-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med GitHub:**</span><span class="sxs-lookup"><span data-stu-id="88cc0-150">**To configure Azure AD single sign-on with GitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="88cc0-151">I Azure-hanteringsportalen på den **GitHub** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-151">In the Azure Management portal, on the **GitHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="88cc0-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="88cc0-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. <span data-ttu-id="88cc0-155">På den **GitHub domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="88cc0-155">On the **GitHub Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    <span data-ttu-id="88cc0-157">a.</span><span class="sxs-lookup"><span data-stu-id="88cc0-157">a.</span></span> <span data-ttu-id="88cc0-158">I den **inloggnings-URL** textruta Skriv värdet som:`https://github.com/orgs/<entity-id>/sso`</span><span class="sxs-lookup"><span data-stu-id="88cc0-158">In the **Sign-on URL** textbox, type the value as: `https://github.com/orgs/<entity-id>/sso`</span></span>

    <span data-ttu-id="88cc0-159">b.</span><span class="sxs-lookup"><span data-stu-id="88cc0-159">b.</span></span> <span data-ttu-id="88cc0-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://github.com/orgs/<entity-id>`</span><span class="sxs-lookup"><span data-stu-id="88cc0-160">In the **Identifier** textbox, type a URL using the following pattern: `https://github.com/orgs/<entity-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="88cc0-161">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="88cc0-161">Please note that these are not the real values.</span></span> <span data-ttu-id="88cc0-162">Du måste uppdatera dessa värden med de faktiska rör-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="88cc0-162">You have to update these values with the actual Sing-on URL and Identifier.</span></span> <span data-ttu-id="88cc0-163">Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren.</span><span class="sxs-lookup"><span data-stu-id="88cc0-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="88cc0-164">Gå till GitHub-administratör att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="88cc0-164">Go to GitHub Admin section to retrieve these values.</span></span> 

4. <span data-ttu-id="88cc0-165">På den **användarattribut** väljer **användar-ID** som user.mail.</span><span class="sxs-lookup"><span data-stu-id="88cc0-165">On the **User Attributes** section, select **User Identifier** as user.mail.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. <span data-ttu-id="88cc0-167">På den **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-167">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. <span data-ttu-id="88cc0-169">På den **skapa nya certifikat** dialogrutan, klicka på kalenderikonen och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-169">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="88cc0-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="88cc0-170">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. <span data-ttu-id="88cc0-172">På den **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="88cc0-172">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. <span data-ttu-id="88cc0-174">På popup-fönstret **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-174">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="88cc0-176">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="88cc0-176">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. <span data-ttu-id="88cc0-178">På den **GitHub Configuration** klickar du på **konfigurera GitHub** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="88cc0-178">On the **GitHub Configuration** section, click **Configure GitHub** to open **Configure sign-on** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. <span data-ttu-id="88cc0-181">Logga in på webbplatsen GitHub organisation som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="88cc0-181">In a different web browser window, log into your GitHub organization site as an administrator.</span></span>

12. <span data-ttu-id="88cc0-182">Gå till **inställningar** och på **säkerhet**</span><span class="sxs-lookup"><span data-stu-id="88cc0-182">Navigate to **Settings** and click **Security**</span></span>

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. <span data-ttu-id="88cc0-184">Kontrollera den **aktivera SAML-autentisering** rutan avslöja konfigurationsfält enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88cc0-184">Check the **Enable SAML authentication** box, revealing the Single Sign-on configuration fields.</span></span> <span data-ttu-id="88cc0-185">Använd sedan på enkel inloggning URL-adressen för att uppdatera den enda inloggnings-URL på Azure AD-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="88cc0-185">Then, use the single sign-on URL value to update the Single sign-on URL on Azure AD configuration.</span></span>

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. <span data-ttu-id="88cc0-187">Konfigurera följande fält:</span><span class="sxs-lookup"><span data-stu-id="88cc0-187">Configure the following fields:</span></span>

    <span data-ttu-id="88cc0-188">a.</span><span class="sxs-lookup"><span data-stu-id="88cc0-188">a.</span></span> <span data-ttu-id="88cc0-189">**Logga in URL: en**: Ange **SAML enkel inloggning Tjänstwebbadress** från den **konfigurera GitHub** avsnittet om Azure AD</span><span class="sxs-lookup"><span data-stu-id="88cc0-189">**Sign on URL**: Enter **SAML Single sign-on Service URL** from the **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="88cc0-190">b.</span><span class="sxs-lookup"><span data-stu-id="88cc0-190">b.</span></span> <span data-ttu-id="88cc0-191">**Utfärdaren**: Ange **SAML enhets-ID** från den **konfigurera GitHub** avsnittet om Azure AD</span><span class="sxs-lookup"><span data-stu-id="88cc0-191">**Issuer**: Enter **SAML Entity ID** from the **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="88cc0-192">c.</span><span class="sxs-lookup"><span data-stu-id="88cc0-192">c.</span></span> <span data-ttu-id="88cc0-193">**Certifikatets offentliga**: öppna hämtat certifikat från Azure AD i en anteckningar och kopiera innehållet inklusive ”BEGIN CERTIFICATE” och ”END CERTIFICATE”</span><span class="sxs-lookup"><span data-stu-id="88cc0-193">**Public Certificate**: Open the downloaded certificate from Azure AD in a notepad and copy the content including "BEGIN CERTIFICATE" and "END CERTIFICATE"</span></span>

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. <span data-ttu-id="88cc0-195">Klicka på **Test SAML-konfiguration** Kontrollera att inga verifieringsfel eller fel under enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="88cc0-195">Click on **Test SAML configuration** to confirm that no validation failures or errors during SSO.</span></span>

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. <span data-ttu-id="88cc0-197">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="88cc0-197">Click **Save**</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="88cc0-198">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="88cc0-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="88cc0-199">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="88cc0-199">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="88cc0-201">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="88cc0-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="88cc0-202">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="88cc0-202">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="88cc0-204">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="88cc0-204">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="88cc0-206">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="88cc0-206">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="88cc0-208">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="88cc0-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="88cc0-210">a.</span><span class="sxs-lookup"><span data-stu-id="88cc0-210">a.</span></span> <span data-ttu-id="88cc0-211">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="88cc0-212">b.</span><span class="sxs-lookup"><span data-stu-id="88cc0-212">b.</span></span> <span data-ttu-id="88cc0-213">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="88cc0-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="88cc0-214">c.</span><span class="sxs-lookup"><span data-stu-id="88cc0-214">c.</span></span> <span data-ttu-id="88cc0-215">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="88cc0-216">d.</span><span class="sxs-lookup"><span data-stu-id="88cc0-216">d.</span></span> <span data-ttu-id="88cc0-217">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-217">Click **Create**.</span></span> 


### <a name="creating-a-github-test-user"></a><span data-ttu-id="88cc0-218">Skapa en testanvändare i GitHub</span><span class="sxs-lookup"><span data-stu-id="88cc0-218">Creating a GitHub test user</span></span>

<span data-ttu-id="88cc0-219">För att aktivera Azure AD-användare att logga in på GitHub, måste de etableras i GitHub.</span><span class="sxs-lookup"><span data-stu-id="88cc0-219">In order to enable Azure AD users to log into GitHub, they must be provisioned into GitHub.</span></span>  
<span data-ttu-id="88cc0-220">När det gäller GitHub är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="88cc0-220">In the case of GitHub, provisioning is a manual task.</span></span>

<span data-ttu-id="88cc0-221">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="88cc0-221">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="88cc0-222">Logga in på webbplatsen GitHub företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="88cc0-222">Log in to your GitHub company site as an administrator.</span></span>

2. <span data-ttu-id="88cc0-223">Klicka på **personer**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-223">Click **People**.</span></span>

    <span data-ttu-id="88cc0-224">![Personer](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "personer")</span><span class="sxs-lookup"><span data-stu-id="88cc0-224">![People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "People")</span></span>

3. <span data-ttu-id="88cc0-225">Klicka på **inbjudan medlem**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-225">Click **Invite member**.</span></span>

    <span data-ttu-id="88cc0-226">![Bjuda in användare](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "bjuda in användare")</span><span class="sxs-lookup"><span data-stu-id="88cc0-226">![Invite Users](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "Invite Users")</span></span>

4. <span data-ttu-id="88cc0-227">På den **inbjudan medlem** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="88cc0-227">On the **Invite member** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="88cc0-228">a.</span><span class="sxs-lookup"><span data-stu-id="88cc0-228">a.</span></span> <span data-ttu-id="88cc0-229">I den **e-post** textruta skriver Britta Simon konto e-postadress.</span><span class="sxs-lookup"><span data-stu-id="88cc0-229">In the **Email** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="88cc0-230">![Bjuda in](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="88cc0-230">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "Invite People")</span></span>
    
    <span data-ttu-id="88cc0-231">b.</span><span class="sxs-lookup"><span data-stu-id="88cc0-231">b.</span></span> <span data-ttu-id="88cc0-232">Klicka på **skicka inbjudan**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-232">Click **Send Invitation**.</span></span>

    <span data-ttu-id="88cc0-233">![Bjuda in](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="88cc0-233">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "Invite People")</span></span>

    > [!NOTE]
    > <span data-ttu-id="88cc0-234">Innehavaren för Azure Active Directory-konto ett e-postmeddelande och länken för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="88cc0-234">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="88cc0-235">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="88cc0-235">Assigning the Azure AD test user</span></span>

<span data-ttu-id="88cc0-236">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till GitHub.</span><span class="sxs-lookup"><span data-stu-id="88cc0-236">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to GitHub.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="88cc0-238">**Om du vill tilldela GitHub Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="88cc0-238">**To assign Britta Simon to GitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="88cc0-239">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-239">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="88cc0-241">Välj i listan med program **GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-241">In the applications list, select **GitHub.com**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. <span data-ttu-id="88cc0-243">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="88cc0-243">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="88cc0-245">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="88cc0-245">Click **Add** button.</span></span> <span data-ttu-id="88cc0-246">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="88cc0-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="88cc0-248">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="88cc0-248">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="88cc0-249">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="88cc0-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="88cc0-250">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="88cc0-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="88cc0-251">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="88cc0-251">Testing single sign-on</span></span>

<span data-ttu-id="88cc0-252">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="88cc0-252">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="88cc0-253">När du klickar på GitHub-panelen på åtkomstpanelen du ska hämta loggat in på ditt GitHub-program.</span><span class="sxs-lookup"><span data-stu-id="88cc0-253">When you click the GitHub tile in the Access Panel, you should get signed-on to your GitHub application.</span></span> <span data-ttu-id="88cc0-254">Du måste logga in som en organisation konto men sedan måste du logga in med ditt eget konto.</span><span class="sxs-lookup"><span data-stu-id="88cc0-254">You'll be logging in as an Organization account but then need to log in with your personal account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="88cc0-255">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="88cc0-255">Additional resources</span></span>

* [<span data-ttu-id="88cc0-256">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="88cc0-256">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="88cc0-257">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="88cc0-257">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
