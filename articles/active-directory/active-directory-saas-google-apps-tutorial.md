---
title: "Självstudier: Azure Active Directory-integrering med Google Apps i Azure | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 065841d6b4fe50e953f01bba4d3f23de82b82726
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a><span data-ttu-id="bd214-103">Självstudier: Azure Active Directory-integrering med Google Apps</span><span class="sxs-lookup"><span data-stu-id="bd214-103">Tutorial: Azure Active Directory integration with Google Apps</span></span>

<span data-ttu-id="bd214-104">I kursen får lära du att integrera Google Apps med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="bd214-104">In this tutorial, you learn how to integrate Google Apps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bd214-105">Integrera Google Apps med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bd214-105">Integrating Google Apps with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bd214-106">Du kan styra i Azure AD som har åtkomst till Google Apps</span><span class="sxs-lookup"><span data-stu-id="bd214-106">You can control in Azure AD who has access to Google Apps</span></span>
- <span data-ttu-id="bd214-107">Du kan aktivera användarna att automatiskt hämta loggat in på Google Apps (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="bd214-107">You can enable your users to automatically get signed-on to Google Apps (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bd214-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bd214-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bd214-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bd214-109">If you want to know more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd214-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bd214-110">Prerequisites</span></span>

<span data-ttu-id="bd214-111">Om du vill konfigurera Azure AD-integrering med Google Apps behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="bd214-111">To configure Azure AD integration with Google Apps, you need the following items:</span></span>

- <span data-ttu-id="bd214-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd214-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bd214-113">En Google Apps enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd214-113">A Google Apps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bd214-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bd214-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bd214-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bd214-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bd214-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="bd214-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bd214-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd214-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="video-tutorial"></a><span data-ttu-id="bd214-118">Videosjälvstudie</span><span class="sxs-lookup"><span data-stu-id="bd214-118">Video tutorial</span></span>
<span data-ttu-id="bd214-119">Så här aktiverar enkel inloggning till Google Apps i två minuter:</span><span class="sxs-lookup"><span data-stu-id="bd214-119">How to Enable Single Sign-On to Google Apps in 2 Minutes:</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a><span data-ttu-id="bd214-120">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="bd214-120">Frequently Asked Questions</span></span>
1. <span data-ttu-id="bd214-121">**F: är Chromebooks och andra enheter som Chrome kompatibla med Azure AD enkel inloggning?**</span><span class="sxs-lookup"><span data-stu-id="bd214-121">**Q: Are Chromebooks and other Chrome devices compatible with Azure AD single sign-on?**</span></span>
   
    <span data-ttu-id="bd214-122">S: Ja kan användare logga in på sina Chromebook enheter med hjälp av autentiseringsuppgifterna Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd214-122">A: Yes, users are able to sign into their Chromebook devices using their Azure AD credentials.</span></span> <span data-ttu-id="bd214-123">Se den här [artikel stöd för Google Apps](https://support.google.com/chrome/a/answer/6060880) för information om varför användare kan uppmanas att autentiseringsuppgifter två gånger.</span><span class="sxs-lookup"><span data-stu-id="bd214-123">See this [Google Apps support article](https://support.google.com/chrome/a/answer/6060880) for information on why users may get prompted for credentials twice.</span></span>

2. <span data-ttu-id="bd214-124">**F: om jag aktiverar enkel inloggning, kommer användarna att kunna använda sina Azure AD-autentiseringsuppgifter för att logga in på någon Google-produkt, till exempel Google klassrum, GMail, Google Drive, YouTube och så vidare?**</span><span class="sxs-lookup"><span data-stu-id="bd214-124">**Q: If I enable single sign-on, will users be able to use their Azure AD credentials to sign into any Google product, such as Google Classroom, GMail, Google Drive, YouTube, and so on?**</span></span>
   
    <span data-ttu-id="bd214-125">S: Ja, beroende på [vilka Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) du väljer att aktivera eller inaktivera för din organisation.</span><span class="sxs-lookup"><span data-stu-id="bd214-125">A: Yes, depending on [which Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) you choose to enable or disable for your organization.</span></span>

3. <span data-ttu-id="bd214-126">**F: kan jag aktivera enkel inloggning för en delmängd av användarna Google Apps?**</span><span class="sxs-lookup"><span data-stu-id="bd214-126">**Q: Can I enable single sign-on for only a subset of my Google Apps users?**</span></span>
   
    <span data-ttu-id="bd214-127">S: inte kräver aktivera enkel inloggning omedelbart att alla Google Apps användare för autentisering med autentiseringsuppgifter för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd214-127">A: No, turning on single sign-on immediately requires all your Google Apps users to authenticate with their Azure AD credentials.</span></span> <span data-ttu-id="bd214-128">Eftersom Google Apps inte stöder att ha flera identitetsleverantörer, identitetsleverantören för Google Apps-miljö kan antingen vara Azure AD eller Google-- men inte båda samtidigt.</span><span class="sxs-lookup"><span data-stu-id="bd214-128">Because Google Apps doesn't support having multiple identity providers, the identity provider for your Google Apps environment can either be Azure AD or Google -- but not both at the same time.</span></span>

4. <span data-ttu-id="bd214-129">**F: om en användare har loggat in via Windows är de automatiskt autentisera till Google Apps utan att uppmanas att ange ett lösenord?**</span><span class="sxs-lookup"><span data-stu-id="bd214-129">**Q: If a user is signed in through Windows, are they automatically authenticate to Google Apps without getting prompted for a password?**</span></span>
   
    <span data-ttu-id="bd214-130">S: finns två alternativ för att aktivera det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="bd214-130">A: There are two options for enabling this scenario.</span></span> <span data-ttu-id="bd214-131">Först användare kan logga in på Windows 10-enheter via [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bd214-131">First, users could sign into Windows 10 devices via [Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span> <span data-ttu-id="bd214-132">Du kan också användare kan logga in på Windows-enheter som är anslutna till en domän till en lokal Active Directory som har aktiverats för enkel inloggning till Azure AD via en [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) distribution.</span><span class="sxs-lookup"><span data-stu-id="bd214-132">Alternatively, users could sign into Windows devices that are domain-joined to an on-premises Active Directory that has been enabled for single sign-on to Azure AD via an [Active Directory Federation Services (AD FS)](active-directory-aadconnect-user-signin.md) deployment.</span></span> <span data-ttu-id="bd214-133">I båda fallen måste du utföra stegen i följande guiden för att aktivera enkel inloggning mellan Azure AD och Google Apps.</span><span class="sxs-lookup"><span data-stu-id="bd214-133">Both options require you to perform the steps in the following tutorial to enable single sign-on between Azure AD and Google Apps.</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bd214-134">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="bd214-134">Scenario description</span></span>
<span data-ttu-id="bd214-135">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="bd214-135">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bd214-136">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="bd214-136">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bd214-137">Lägga till Google Apps från galleriet</span><span class="sxs-lookup"><span data-stu-id="bd214-137">Adding Google Apps from the gallery</span></span>
2. <span data-ttu-id="bd214-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd214-138">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-google-apps-from-the-gallery"></a><span data-ttu-id="bd214-139">Lägga till Google Apps från galleriet</span><span class="sxs-lookup"><span data-stu-id="bd214-139">Adding Google Apps from the gallery</span></span>
<span data-ttu-id="bd214-140">Du måste lägga till Google Apps från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Google Apps i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd214-140">To configure the integration of Google Apps into Azure AD, you need to add Google Apps from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bd214-141">**Utför följande steg för att lägga till Google Apps från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="bd214-141">**To add Google Apps from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bd214-142">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bd214-142">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bd214-144">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="bd214-144">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bd214-145">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bd214-145">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="bd214-147">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd214-147">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="bd214-149">I sökrutan skriver **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="bd214-149">In the search box, type **Google Apps**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. <span data-ttu-id="bd214-151">Välj i resultatpanelen **Google Apps**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="bd214-151">In the results panel, select **Google Apps**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bd214-153">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd214-153">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bd214-154">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Google Apps baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="bd214-154">In this section, you configure and test Azure AD single sign-on with Google Apps based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bd214-155">Azure AD måste du känna till användaren i Google Apps motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="bd214-155">For single sign-on to work, Azure AD needs to know what the counterpart user in Google Apps is to a user in Azure AD.</span></span> <span data-ttu-id="bd214-156">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Google Apps upprättas.</span><span class="sxs-lookup"><span data-stu-id="bd214-156">In other words, a link relationship between an Azure AD user and the related user in Google Apps needs to be established.</span></span>

<span data-ttu-id="bd214-157">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Google Apps.</span><span class="sxs-lookup"><span data-stu-id="bd214-157">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Google Apps.</span></span>

<span data-ttu-id="bd214-158">Om du vill konfigurera och testa Azure AD enkel inloggning med Google Apps, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="bd214-158">To configure and test Azure AD single sign-on with Google Apps, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bd214-159">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="bd214-159">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bd214-160">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd214-160">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bd214-161">**[Skapa en testanvändare för Google Apps](#creating-a-google-apps-test-user)**  – du har en motsvarighet för Britta Simon i Google-appar som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="bd214-161">**[Creating a Google Apps test user](#creating-a-google-apps-test-user)** - to have a counterpart of Britta Simon in Google Apps that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bd214-162">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bd214-162">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bd214-163">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="bd214-163">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bd214-164">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd214-164">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bd214-165">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Google Apps-program.</span><span class="sxs-lookup"><span data-stu-id="bd214-165">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Google Apps application.</span></span>

<span data-ttu-id="bd214-166">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Google Apps:**</span><span class="sxs-lookup"><span data-stu-id="bd214-166">**To configure Azure AD single sign-on with Google Apps, perform the following steps:**</span></span>

1. <span data-ttu-id="bd214-167">I Azure-portalen på den **Google Apps** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bd214-167">In the Azure portal, on the **Google Apps** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="bd214-169">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bd214-169">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. <span data-ttu-id="bd214-171">På den **Google Apps-domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd214-171">On the **Google Apps Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    <span data-ttu-id="bd214-173">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://mail.google.com/a/<yourdomain>`</span><span class="sxs-lookup"><span data-stu-id="bd214-173">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://mail.google.com/a/<yourdomain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bd214-174">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="bd214-174">This value is not real.</span></span> <span data-ttu-id="bd214-175">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="bd214-175">Update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="bd214-176">Kontakta den [Google supportteamet](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="bd214-176">contact the [Google support team](https://www.google.com/contact/).</span></span>
 
4. <span data-ttu-id="bd214-177">På den **SAML-signeringscertifikat** klickar du på **certifikat** och spara certifikatet på datorn.</span><span class="sxs-lookup"><span data-stu-id="bd214-177">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. <span data-ttu-id="bd214-179">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd214-179">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bd214-181">På den **Google Apps-konfiguration** klickar du på **konfigurera Google Apps** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="bd214-181">On the **Google Apps Configuration** section, click **Configure Google Apps** to open **Configure sign-on** window.</span></span> <span data-ttu-id="bd214-182">Kopiera den **Sign-Out URL, SAML enkel inloggning URL: en och ändra URL: en lösenord** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="bd214-182">Copy the **Sign-Out URL, SAML Single Sign-On Service URL and Change password URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. <span data-ttu-id="bd214-184">Öppna en ny flik i webbläsaren och logga in på den [Google Apps-administrationskonsolen](http://admin.google.com/) med ditt administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="bd214-184">Open a new tab in your browser, and sign into the [Google Apps Admin Console](http://admin.google.com/) using your administrator account.</span></span>

8. <span data-ttu-id="bd214-185">Klicka på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="bd214-185">Click **Security**.</span></span> <span data-ttu-id="bd214-186">Om du inte ser länken, det kan vara dolt den **fler kontroller** menyn längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="bd214-186">If you don't see the link, it may be hidden under the **More Controls** menu at the bottom of the screen.</span></span>
   
    ![Klicka på Security.][10]

9. <span data-ttu-id="bd214-188">På den **säkerhet** klickar du på **Konfigurera enkel inloggning (SSO).**</span><span class="sxs-lookup"><span data-stu-id="bd214-188">On the **Security** page, click **Set up single sign-on (SSO).**</span></span>
   
    ![Klicka på enkel inloggning.][11]

10. <span data-ttu-id="bd214-190">Utför följande konfigurationsändringar:</span><span class="sxs-lookup"><span data-stu-id="bd214-190">Perform the following configuration changes:</span></span>
   
    ![Konfigurera enkel inloggning][12]
   
    <span data-ttu-id="bd214-192">a.</span><span class="sxs-lookup"><span data-stu-id="bd214-192">a.</span></span> <span data-ttu-id="bd214-193">Välj **installationsprogrammet SSO med tredjeparts identitetsprovider**.</span><span class="sxs-lookup"><span data-stu-id="bd214-193">Select **Setup SSO with third-party identity provider**.</span></span>

    <span data-ttu-id="bd214-194">b.</span><span class="sxs-lookup"><span data-stu-id="bd214-194">b.</span></span> <span data-ttu-id="bd214-195">I den **inloggning Sidadress** fältet i Google Apps, klistra in värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bd214-195">In the **Sign-in page URL** field in Google Apps, paste the value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bd214-196">c.</span><span class="sxs-lookup"><span data-stu-id="bd214-196">c.</span></span> <span data-ttu-id="bd214-197">I den **URL för utloggning** fältet i Google Apps, klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bd214-197">In the **Sign-out page URL** field in Google Apps, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="bd214-198">d.</span><span class="sxs-lookup"><span data-stu-id="bd214-198">d.</span></span> <span data-ttu-id="bd214-199">I den **ändra lösenord URL** fältet i Google Apps, klistra in värdet för **ändra lösenord URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bd214-199">In the **Change password URL** field in Google Apps, paste the value of **Change password URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="bd214-200">e.</span><span class="sxs-lookup"><span data-stu-id="bd214-200">e.</span></span> <span data-ttu-id="bd214-201">I Google Apps för den **verifieringscertifikat**, ladda upp certifikatet som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bd214-201">In Google Apps, for the **Verification certificate**, upload the certificate that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="bd214-202">f.</span><span class="sxs-lookup"><span data-stu-id="bd214-202">f.</span></span> <span data-ttu-id="bd214-203">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="bd214-203">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="bd214-204">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="bd214-204">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bd214-205">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="bd214-205">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bd214-206">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bd214-206">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bd214-207">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd214-207">Creating an Azure AD test user</span></span>
<span data-ttu-id="bd214-208">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd214-208">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="bd214-210">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="bd214-210">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bd214-211">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bd214-211">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bd214-213">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="bd214-213">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bd214-215">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd214-215">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bd214-217">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd214-217">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bd214-219">a.</span><span class="sxs-lookup"><span data-stu-id="bd214-219">a.</span></span> <span data-ttu-id="bd214-220">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bd214-220">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bd214-221">b.</span><span class="sxs-lookup"><span data-stu-id="bd214-221">b.</span></span> <span data-ttu-id="bd214-222">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bd214-222">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bd214-223">c.</span><span class="sxs-lookup"><span data-stu-id="bd214-223">c.</span></span> <span data-ttu-id="bd214-224">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="bd214-224">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bd214-225">d.</span><span class="sxs-lookup"><span data-stu-id="bd214-225">d.</span></span> <span data-ttu-id="bd214-226">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bd214-226">Click **Create**.</span></span>
 
### <a name="creating-a-google-apps-test-user"></a><span data-ttu-id="bd214-227">Skapa en testanvändare för Google Apps</span><span class="sxs-lookup"><span data-stu-id="bd214-227">Creating a Google Apps test user</span></span>

<span data-ttu-id="bd214-228">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Google Apps programvara.</span><span class="sxs-lookup"><span data-stu-id="bd214-228">The objective of this section is to create a user called Britta Simon in Google Apps Software.</span></span> <span data-ttu-id="bd214-229">Google Apps har stöd för automatisk etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="bd214-229">Google Apps supports auto provisioning, which is by default enabled.</span></span> <span data-ttu-id="bd214-230">Det finns inga åtgärder i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="bd214-230">There is no action for you in this section.</span></span> <span data-ttu-id="bd214-231">Om en användare inte redan finns i Google Apps programvara, skapas en ny när du försöker komma åt Google Apps programvara.</span><span class="sxs-lookup"><span data-stu-id="bd214-231">If a user doesn't already exist in Google Apps Software, a new one is created when you attempt to access Google Apps Software.</span></span>

>[!NOTE] 
><span data-ttu-id="bd214-232">Om du behöver skapa en användare manuellt kan kontakta den [Google supportteamet](https://www.google.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="bd214-232">If you need to create a user manually, contact the [Google support team](https://www.google.com/contact/).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bd214-233">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="bd214-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bd214-234">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Google Apps.</span><span class="sxs-lookup"><span data-stu-id="bd214-234">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Google Apps.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="bd214-236">**Om du vill tilldela Google Apps Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bd214-236">**To assign Britta Simon to Google Apps, perform the following steps:**</span></span>

1. <span data-ttu-id="bd214-237">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bd214-237">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="bd214-239">Välj i listan med program **Google Apps**.</span><span class="sxs-lookup"><span data-stu-id="bd214-239">In the applications list, select **Google Apps**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. <span data-ttu-id="bd214-241">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="bd214-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="bd214-243">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd214-243">Click **Add** button.</span></span> <span data-ttu-id="bd214-244">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd214-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="bd214-246">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="bd214-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bd214-247">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd214-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bd214-248">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd214-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bd214-249">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd214-249">Testing single sign-on</span></span>

<span data-ttu-id="bd214-250">I det här avsnittet om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen på [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md)sedan logga in i testkonto och klicka på **Google Apps** panelen i åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="bd214-250">In this section, to test your single sign-on settings, open the Access Panel at [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), then sign into the test account, and click **Google Apps** tile in the Access Panel.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd214-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bd214-251">Additional resources</span></span>

* [<span data-ttu-id="bd214-252">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bd214-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bd214-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bd214-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="bd214-254">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="bd214-254">Configure User Provisioning</span></span>](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png