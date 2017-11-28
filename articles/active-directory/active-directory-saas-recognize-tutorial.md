---
title: "Självstudier: Azure Active Directory-integrering med identifiera | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och identifiera."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 97d85183d0307c41a3b879d440d87a6fb0c53190
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a><span data-ttu-id="7e53a-103">Självstudier: Azure Active Directory-integrering med identifiera</span><span class="sxs-lookup"><span data-stu-id="7e53a-103">Tutorial: Azure Active Directory integration with Recognize</span></span>

<span data-ttu-id="7e53a-104">I kursen får lära du att integrera identifiera med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7e53a-104">In this tutorial, you learn how to integrate Recognize with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7e53a-105">Identifiera integrera med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7e53a-105">Integrating Recognize with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7e53a-106">Du kan styra i Azure AD som har åtkomst till identifiera</span><span class="sxs-lookup"><span data-stu-id="7e53a-106">You can control in Azure AD who has access to Recognize</span></span>
- <span data-ttu-id="7e53a-107">Du kan aktivera användarna att automatiskt hämta loggat in på identifiera (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7e53a-107">You can enable your users to automatically get signed-on to Recognize (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7e53a-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7e53a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7e53a-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7e53a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e53a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7e53a-110">Prerequisites</span></span>

<span data-ttu-id="7e53a-111">Om du vill konfigurera Azure AD-integrering med identifiera behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7e53a-111">To configure Azure AD integration with Recognize, you need the following items:</span></span>

- <span data-ttu-id="7e53a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7e53a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7e53a-113">En identifiera enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7e53a-113">A Recognize single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7e53a-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7e53a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7e53a-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7e53a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7e53a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7e53a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7e53a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7e53a-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7e53a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7e53a-118">Scenario description</span></span>
<span data-ttu-id="7e53a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7e53a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7e53a-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7e53a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7e53a-121">Att lägga till identifiera från galleriet</span><span class="sxs-lookup"><span data-stu-id="7e53a-121">Adding Recognize from the gallery</span></span>
2. <span data-ttu-id="7e53a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7e53a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-recognize-from-the-gallery"></a><span data-ttu-id="7e53a-123">Att lägga till identifiera från galleriet</span><span class="sxs-lookup"><span data-stu-id="7e53a-123">Adding Recognize from the gallery</span></span>
<span data-ttu-id="7e53a-124">Du måste lägga till identifiera från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av identifiera i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7e53a-124">To configure the integration of Recognize into Azure AD, you need to add Recognize from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7e53a-125">**Utför följande steg för att lägga till identifiera från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7e53a-125">**To add Recognize from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7e53a-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7e53a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7e53a-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7e53a-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7e53a-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e53a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7e53a-133">I sökrutan skriver **identifiera**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-133">In the search box, type **Recognize**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_search.png)

5. <span data-ttu-id="7e53a-135">Välj i resultatpanelen **identifiera**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7e53a-135">In the results panel, select **Recognize**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7e53a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7e53a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7e53a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med identifiera baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7e53a-138">In this section, you configure and test Azure AD single sign-on with Recognize based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7e53a-139">Azure AD måste du känna till användaren i identifiera motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7e53a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Recognize is to a user in Azure AD.</span></span> <span data-ttu-id="7e53a-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i identifiera upprättas.</span><span class="sxs-lookup"><span data-stu-id="7e53a-140">In other words, a link relationship between an Azure AD user and the related user in Recognize needs to be established.</span></span>

<span data-ttu-id="7e53a-141">I identifiera, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-141">In Recognize, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7e53a-142">Om du vill konfigurera och testa Azure AD enkel inloggning med identifiera, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7e53a-142">To configure and test Azure AD single sign-on with Recognize, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7e53a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7e53a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7e53a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7e53a-145">**[Skapa en testanvändare identifiera](#creating-a-recognize-test-user)**  – har en motsvarighet för Britta Simon identifiera som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7e53a-145">**[Creating a Recognize test user](#creating-a-recognize-test-user)** - to have a counterpart of Britta Simon in Recognize that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7e53a-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7e53a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7e53a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7e53a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7e53a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7e53a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7e53a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i identifiera programmet.</span><span class="sxs-lookup"><span data-stu-id="7e53a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Recognize application.</span></span>

<span data-ttu-id="7e53a-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med identifiera:**</span><span class="sxs-lookup"><span data-stu-id="7e53a-150">**To configure Azure AD single sign-on with Recognize, perform the following steps:**</span></span>

1. <span data-ttu-id="7e53a-151">I Azure-portalen på den **identifiera** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-151">In the Azure portal, on the **Recognize** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7e53a-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7e53a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_samlbase.png)

3. <span data-ttu-id="7e53a-155">På den **URL: er och identifiera domänen** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7e53a-155">On the **Recognize Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_url.png)

    <span data-ttu-id="7e53a-157">a.</span><span class="sxs-lookup"><span data-stu-id="7e53a-157">a.</span></span> <span data-ttu-id="7e53a-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://recognizeapp.com/<your-domain>/saml/sso`</span><span class="sxs-lookup"><span data-stu-id="7e53a-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://recognizeapp.com/<your-domain>/saml/sso`</span></span>

    <span data-ttu-id="7e53a-159">b.</span><span class="sxs-lookup"><span data-stu-id="7e53a-159">b.</span></span> <span data-ttu-id="7e53a-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://recognizeapp.com/<your-domain>`</span><span class="sxs-lookup"><span data-stu-id="7e53a-160">In the **Identifier** textbox, type a URL using the following pattern: `https://recognizeapp.com/<your-domain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7e53a-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7e53a-161">These values are not real.</span></span> <span data-ttu-id="7e53a-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="7e53a-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7e53a-163">Kontakta [identifiera klienten supportteamet](mailto:support@recognizeapp.com) få inloggnings-URL och du kan hämta ID-värde genom att öppna URL: en för Service Provider Metadata från avsnittet SSO-inställningarna som beskrivs senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-163">Contact [Recognize Client support team](mailto:support@recognizeapp.com) to get Sign-On URL and you can get Identifier value by opening the Service Provider Metadata URL from the SSO Settings section that is explained later in the tutorial.</span></span> <span data-ttu-id="7e53a-164">.</span><span class="sxs-lookup"><span data-stu-id="7e53a-164">.</span></span> 
 
4. <span data-ttu-id="7e53a-165">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7e53a-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_certificate.png) 

5. <span data-ttu-id="7e53a-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7e53a-169">På den **identifiera Configuration** klickar du på **konfigurera identifiera** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7e53a-169">On the **Recognize Configuration** section, click **Configure Recognize** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7e53a-170">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7e53a-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_configure.png) 

7. <span data-ttu-id="7e53a-172">I en annan webbläsarfönster inloggning till identifiera klienten som en administratör.</span><span class="sxs-lookup"><span data-stu-id="7e53a-172">In a different web browser window, sign-on to your Recognize tenant as an administrator.</span></span>

8. <span data-ttu-id="7e53a-173">Klicka på det övre högra hörnet **menyn**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-173">On the upper right corner, click **Menu**.</span></span> <span data-ttu-id="7e53a-174">Gå till **företagets Admin**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-174">Go to **Company Admin**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

9. <span data-ttu-id="7e53a-176">I det vänstra navigeringsfönstret klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-176">On the left navigation pane, click **Settings**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

10. <span data-ttu-id="7e53a-178">Utför följande steg på **SSO-inställningarna** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7e53a-178">Perform the following steps on **SSO Settings** section.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
    
    <span data-ttu-id="7e53a-180">a.</span><span class="sxs-lookup"><span data-stu-id="7e53a-180">a.</span></span> <span data-ttu-id="7e53a-181">Som **aktivera enkel inloggning**väljer **på**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-181">As **Enable SSO**, select **ON**.</span></span>

    <span data-ttu-id="7e53a-182">b.</span><span class="sxs-lookup"><span data-stu-id="7e53a-182">b.</span></span> <span data-ttu-id="7e53a-183">I den **IDP enhets-ID** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-183">In the **IDP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="7e53a-184">c.</span><span class="sxs-lookup"><span data-stu-id="7e53a-184">c.</span></span> <span data-ttu-id="7e53a-185">I den **mål-url för Sso** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-185">In the **Sso target url** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="7e53a-186">d.</span><span class="sxs-lookup"><span data-stu-id="7e53a-186">d.</span></span> <span data-ttu-id="7e53a-187">I den **servicenivåmål mål-url** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-187">In the **Slo target url** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span> 
    
    <span data-ttu-id="7e53a-188">e.</span><span class="sxs-lookup"><span data-stu-id="7e53a-188">e.</span></span> <span data-ttu-id="7e53a-189">Öppna din hämtade **certifikat (Base64)** fil i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="7e53a-189">Open your downloaded **Certificate (Base64)** file in notepad, copy the content of it into your clipboard, and then paste it to the **Certificate** textbox.</span></span>
    
    <span data-ttu-id="7e53a-190">f.</span><span class="sxs-lookup"><span data-stu-id="7e53a-190">f.</span></span> <span data-ttu-id="7e53a-191">Klicka på den **Spara inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-191">Click the **Save settings** button.</span></span> 

11. <span data-ttu-id="7e53a-192">Bredvid den **SSO inställningar** och kopiera Webbadressen under **url för tjänstmetadata providern**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-192">Beside the **SSO Settings** section, copy the URL under **Service Provider Metadata url**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

12. <span data-ttu-id="7e53a-194">Öppna den **Metadata URL-länk** under en tom webbläsare för att hämta Metadatadokumentet.</span><span class="sxs-lookup"><span data-stu-id="7e53a-194">Open the **Metadata URL link** under a blank browser to download the metadata document.</span></span> <span data-ttu-id="7e53a-195">Kopiera EntityDescriptor value(entityID) från filen och klistra in den i **identifierare** TextBox-kontroll i **URL: er och identifiera domänen avsnittet** på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-195">Then copy the EntityDescriptor value(entityID) from the file and paste it in **Identifier** textbox in **Recognize Domain and URLs section** on Azure portal.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> <span data-ttu-id="7e53a-197">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="7e53a-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7e53a-198">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="7e53a-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7e53a-199">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7e53a-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7e53a-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7e53a-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="7e53a-201">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7e53a-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7e53a-203">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7e53a-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7e53a-204">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7e53a-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7e53a-206">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7e53a-208">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e53a-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7e53a-210">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7e53a-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7e53a-212">a.</span><span class="sxs-lookup"><span data-stu-id="7e53a-212">a.</span></span> <span data-ttu-id="7e53a-213">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7e53a-214">b.</span><span class="sxs-lookup"><span data-stu-id="7e53a-214">b.</span></span> <span data-ttu-id="7e53a-215">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7e53a-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7e53a-216">c.</span><span class="sxs-lookup"><span data-stu-id="7e53a-216">c.</span></span> <span data-ttu-id="7e53a-217">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7e53a-218">d.</span><span class="sxs-lookup"><span data-stu-id="7e53a-218">d.</span></span> <span data-ttu-id="7e53a-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-219">Click **Create**.</span></span>
 
### <a name="creating-a-recognize-test-user"></a><span data-ttu-id="7e53a-220">Skapa en testanvändare identifiera</span><span class="sxs-lookup"><span data-stu-id="7e53a-220">Creating a Recognize test user</span></span>

<span data-ttu-id="7e53a-221">För att aktivera Azure AD-användare att logga in på identifiera etableras de i identifiera.</span><span class="sxs-lookup"><span data-stu-id="7e53a-221">In order to enable Azure AD users to log into Recognize, they must be provisioned into Recognize.</span></span> <span data-ttu-id="7e53a-222">Identifiera är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7e53a-222">In the case of Recognize, provisioning is a manual task.</span></span>

<span data-ttu-id="7e53a-223">Den här appen stöder inte SCIM etablering men har en annan användare sync som etablerar användare.</span><span class="sxs-lookup"><span data-stu-id="7e53a-223">This app doesn't support SCIM provisioning but has an alternate user sync that provisions users.</span></span> 

<span data-ttu-id="7e53a-224">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="7e53a-224">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="7e53a-225">Logga in på webbplatsen identifiera företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="7e53a-225">Log into your Recognize company site as an administrator.</span></span>

2. <span data-ttu-id="7e53a-226">Klicka på det övre högra hörnet **menyn**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-226">On the upper right corner, click **Menu**.</span></span> <span data-ttu-id="7e53a-227">Gå till **företagets Admin**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-227">Go to **Company Admin**.</span></span>

3. <span data-ttu-id="7e53a-228">I det vänstra navigeringsfönstret klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-228">On the left navigation pane, click **Settings**.</span></span>

4. <span data-ttu-id="7e53a-229">Utför följande steg på **användaren Sync** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7e53a-229">Perform the following steps on **User Sync** section.</span></span>
   
   <span data-ttu-id="7e53a-230">![Ny användare](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="7e53a-230">![New User](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "New User")</span></span>
   
   <span data-ttu-id="7e53a-231">a.</span><span class="sxs-lookup"><span data-stu-id="7e53a-231">a.</span></span> <span data-ttu-id="7e53a-232">Som **synkronisering aktiverat**väljer **på**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-232">As **Sync Enabled**, select **ON**.</span></span>
   
   <span data-ttu-id="7e53a-233">b.</span><span class="sxs-lookup"><span data-stu-id="7e53a-233">b.</span></span> <span data-ttu-id="7e53a-234">Som **Välj sync providern**väljer **Microsoft / Office 365**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-234">As **Choose sync provider**, select **Microsoft / Office 365**.</span></span>
   
   <span data-ttu-id="7e53a-235">c.</span><span class="sxs-lookup"><span data-stu-id="7e53a-235">c.</span></span> <span data-ttu-id="7e53a-236">Klicka på **köra användaren synkronisering**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-236">Click **Run User Sync**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7e53a-237">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7e53a-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7e53a-238">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till identifiera.</span><span class="sxs-lookup"><span data-stu-id="7e53a-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Recognize.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7e53a-240">**Om du vill tilldela identifiera Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7e53a-240">**To assign Britta Simon to Recognize, perform the following steps:**</span></span>

1. <span data-ttu-id="7e53a-241">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7e53a-243">Välj i listan med program **identifiera**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-243">In the applications list, select **Recognize**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_app.png) 

3. <span data-ttu-id="7e53a-245">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7e53a-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7e53a-247">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7e53a-247">Click **Add** button.</span></span> <span data-ttu-id="7e53a-248">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e53a-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7e53a-250">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7e53a-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7e53a-251">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e53a-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7e53a-252">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e53a-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7e53a-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7e53a-253">Testing single sign-on</span></span>

<span data-ttu-id="7e53a-254">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7e53a-254">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7e53a-255">När du klickar på panelen identifiera på åtkomstpanelen du bör få automatiskt inloggade att identifiera programmet.</span><span class="sxs-lookup"><span data-stu-id="7e53a-255">When you click the Recognize tile in the Access Panel, you should get automatically signed-on to your Recognize application.</span></span> <span data-ttu-id="7e53a-256">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7e53a-256">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e53a-257">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7e53a-257">Additional resources</span></span>

* [<span data-ttu-id="7e53a-258">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7e53a-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7e53a-259">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7e53a-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png

