---
title: "Självstudier: Azure Active Directory-integrering med Adobe kreativa molntjänster | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Adobe kreativa moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 3d13608612c77236346b0e98551d7fc427d602e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a><span data-ttu-id="a7691-103">Självstudier: Azure Active Directory-integrering med Adobe kreativa moln</span><span class="sxs-lookup"><span data-stu-id="a7691-103">Tutorial: Azure Active Directory integration with Adobe Creative Cloud</span></span>

<span data-ttu-id="a7691-104">I kursen får lära du att integrera Adobe kreativa moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a7691-104">In this tutorial, you learn how to integrate Adobe Creative Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a7691-105">Integrera Adobe kreativa moln med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a7691-105">Integrating Adobe Creative Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a7691-106">Du kan styra i Azure AD som har åtkomst till Adobe kreativa moln</span><span class="sxs-lookup"><span data-stu-id="a7691-106">You can control in Azure AD who has access to Adobe Creative Cloud</span></span>
- <span data-ttu-id="a7691-107">Du kan aktivera användarna att automatiskt hämta loggat in på Adobe kreativa molnet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a7691-107">You can enable your users to automatically get signed-on to Adobe Creative Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a7691-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="a7691-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="a7691-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a7691-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7691-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a7691-110">Prerequisites</span></span>

<span data-ttu-id="a7691-111">För att konfigurera Azure AD-integrering med Adobe kreativa molnet, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a7691-111">To configure Azure AD integration with Adobe Creative Cloud, you need the following items:</span></span>

- <span data-ttu-id="a7691-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a7691-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a7691-113">En Adobe kreativa molnet enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="a7691-113">A Adobe Creative Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a7691-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a7691-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a7691-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a7691-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a7691-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a7691-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="a7691-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a7691-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a7691-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a7691-118">Scenario description</span></span>
<span data-ttu-id="a7691-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a7691-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a7691-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a7691-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a7691-121">Att lägga till Adobe kreativa moln från galleriet</span><span class="sxs-lookup"><span data-stu-id="a7691-121">Adding Adobe Creative Cloud from the gallery</span></span>
2. <span data-ttu-id="a7691-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a7691-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a><span data-ttu-id="a7691-123">Att lägga till Adobe kreativa moln från galleriet</span><span class="sxs-lookup"><span data-stu-id="a7691-123">Adding Adobe Creative Cloud from the gallery</span></span>
<span data-ttu-id="a7691-124">Du måste lägga till Adobe kreativa moln från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Adobe kreativa moln i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a7691-124">To configure the integration of Adobe Creative Cloud into Azure AD, you need to add Adobe Creative Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a7691-125">**Utför följande steg för att lägga till Adobe kreativa moln från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a7691-125">**To add Adobe Creative Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a7691-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a7691-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a7691-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a7691-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a7691-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a7691-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a7691-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7691-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a7691-133">I sökrutan skriver **Adobe kreativa molnet**.</span><span class="sxs-lookup"><span data-stu-id="a7691-133">In the search box, type **Adobe Creative Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. <span data-ttu-id="a7691-135">Välj i resultatpanelen **Adobe kreativa molnet**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a7691-135">In the results panel, select **Adobe Creative Cloud**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a7691-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a7691-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a7691-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Adobe kreativa molnet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a7691-138">In this section, you configure and test Azure AD single sign-on with Adobe Creative Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a7691-139">Azure AD måste du känna till motsvarande användaren i Adobe kreativa molnet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a7691-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Adobe Creative Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="a7691-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Adobe kreativa molnet upprättas.</span><span class="sxs-lookup"><span data-stu-id="a7691-140">In other words, a link relationship between an Azure AD user and the related user in Adobe Creative Cloud needs to be established.</span></span>

<span data-ttu-id="a7691-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Adobe kreativa molnet.</span><span class="sxs-lookup"><span data-stu-id="a7691-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Adobe Creative Cloud.</span></span>

<span data-ttu-id="a7691-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Adobe kreativa molnet, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a7691-142">To configure and test Azure AD single sign-on with Adobe Creative Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a7691-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a7691-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a7691-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a7691-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a7691-145">**[Skapa en testanvändare Adobe kreativa molnet](#creating-an-adobe-creative-cloud-test-user)**  – du har en motsvarighet för Britta Simon i Adobe kreativa moln som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="a7691-145">**[Creating an Adobe Creative Cloud test user](#creating-an-adobe-creative-cloud-test-user)** - to have a counterpart of Britta Simon in Adobe Creative Cloud that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="a7691-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a7691-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a7691-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a7691-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a7691-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a7691-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a7691-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i tillämpningsprogrammet Adobe kreativa molnet.</span><span class="sxs-lookup"><span data-stu-id="a7691-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Adobe Creative Cloud application.</span></span>

<span data-ttu-id="a7691-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Adobe kreativa molnet:**</span><span class="sxs-lookup"><span data-stu-id="a7691-150">**To configure Azure AD single sign-on with Adobe Creative Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="a7691-151">I Azure-hanteringsportalen på den **Adobe kreativa molnet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a7691-151">In the Azure Management portal, on the **Adobe Creative Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a7691-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="a7691-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. <span data-ttu-id="a7691-155">På den **Adobe kreativa molnet domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="a7691-155">On the **Adobe Creative Cloud Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    <span data-ttu-id="a7691-157">a.</span><span class="sxs-lookup"><span data-stu-id="a7691-157">a.</span></span> <span data-ttu-id="a7691-158">I den **identifierare** textruta Skriv värdet som:`https://www.okta.com/saml2/service-provider/<token>`</span><span class="sxs-lookup"><span data-stu-id="a7691-158">In the **Identifier** textbox, type the value as: `https://www.okta.com/saml2/service-provider/<token>`</span></span>

    <span data-ttu-id="a7691-159">b.</span><span class="sxs-lookup"><span data-stu-id="a7691-159">b.</span></span> <span data-ttu-id="a7691-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.okta.com/auth/saml20/accauthlinktest`</span><span class="sxs-lookup"><span data-stu-id="a7691-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.okta.com/auth/saml20/accauthlinktest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a7691-161">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="a7691-161">Please note that these are not the real values.</span></span> <span data-ttu-id="a7691-162">Du måste uppdatera dessa värden med de faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="a7691-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="a7691-163">Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren.</span><span class="sxs-lookup"><span data-stu-id="a7691-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="a7691-164">Om du behöver skapa en användare manuellt måste du kontakta supportteamet Adobe kreativa molnet.</span><span class="sxs-lookup"><span data-stu-id="a7691-164">If you need to create an user manually, you need to contact the Adobe Creative Cloud support team.</span></span>

4. <span data-ttu-id="a7691-165">På den **Adobe kreativa molnet domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="a7691-165">On the **Adobe Creative Cloud Domain and URLs** section, perform the following steps if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    <span data-ttu-id="a7691-167">a.</span><span class="sxs-lookup"><span data-stu-id="a7691-167">a.</span></span> <span data-ttu-id="a7691-168">Klicka på den **visa avancerade inställningar för URL: en** alternativet</span><span class="sxs-lookup"><span data-stu-id="a7691-168">Click on the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="a7691-169">b.</span><span class="sxs-lookup"><span data-stu-id="a7691-169">b.</span></span> <span data-ttu-id="a7691-170">I den **inloggnings-URL** textruta Skriv värdet som:`https://adobe.com`</span><span class="sxs-lookup"><span data-stu-id="a7691-170">In the **Sign-on URL** textbox, type the value as: `https://adobe.com`</span></span>

5. <span data-ttu-id="a7691-171">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a7691-171">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. <span data-ttu-id="a7691-173">På den **Adobe kreativa Molnkonfigurationen** klickar du på **Konfigurera Adobe kreativa molnet** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a7691-173">On the **Adobe Creative Cloud Configuration** section, click **Configure Adobe Creative Cloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a7691-174">Kopiera den **SAML enhets-Id** och **URL för SAML SSO-Service** från Snabbreferens avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a7691-174">Please copy the **SAML Entity Id** and **SAML SSO Service URL** from Quick Reference section.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. <span data-ttu-id="a7691-176">I en annan webbläsarfönster inloggning till Adobe kreativa moln-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="a7691-176">In a different web browser window, sign-on to your Adobe Creative Cloud tenant as an administrator.</span></span>

8.  <span data-ttu-id="a7691-177">Gå till **identitet** i det vänstra navigeringsfönstret och klicka på din domän.</span><span class="sxs-lookup"><span data-stu-id="a7691-177">Go to **Identity** on the left navigation pane and click your domain.</span></span> <span data-ttu-id="a7691-178">Utför följande steg på **enkel inloggning på konfiguration krävs** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a7691-178">Then perform the following steps on **Single Sign On Configuration Required** section.</span></span>

    <span data-ttu-id="a7691-179">![Inställningar för](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="a7691-179">![Settings](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "Settings")</span></span>

9. <span data-ttu-id="a7691-180">Klicka på **Bläddra** hämtade certifikatet från Azure AD för att överföra **IDP certifikat**.</span><span class="sxs-lookup"><span data-stu-id="a7691-180">Click **Browse** to upload the downloaded certificate from Azure AD to **IDP Certificate**.</span></span>

10. <span data-ttu-id="a7691-181">I den **IDP utfärdaren** textruta, ange värdet för **SAML enhets-Id** som du kopierade från **konfigurera inloggning** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a7691-181">In the **IDP issuer** textbox, put the value of **SAML Entity Id** which you copied from **Configure sign-on** section in Azure portal.</span></span>

11. <span data-ttu-id="a7691-182">I den **IDP inloggnings-URL** textruta, ange värdet för **URL för SAML SSO-Service** som du kopierade från **konfigurera inloggning** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a7691-182">In the **IDP Login URL** textbox, put the value of **SAML SSO Service URL** which you copied from **Configure sign-on** section in Azure portal.</span></span>

12. <span data-ttu-id="a7691-183">Välj **HTTP - omdirigering** som **IDP bindning**.</span><span class="sxs-lookup"><span data-stu-id="a7691-183">Select **HTTP - Redirect** as **IDP Binding**.</span></span>

13. <span data-ttu-id="a7691-184">Välj **e-postadress** som **inloggningen Användarinställning**.</span><span class="sxs-lookup"><span data-stu-id="a7691-184">Select **Email Address** as **User Login Setting**.</span></span>
 
14. <span data-ttu-id="a7691-185">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a7691-185">Click **Save** button.</span></span>

15. <span data-ttu-id="a7691-186">Instrumentpanelen visas nu XML **”hämta Metadata”** filen.</span><span class="sxs-lookup"><span data-stu-id="a7691-186">The dashboard will now present the XML **"Download Metadata"** file.</span></span> <span data-ttu-id="a7691-187">Den innehåller Adobe EntityDescriptor Webbadressen och AssertionConsumerService.</span><span class="sxs-lookup"><span data-stu-id="a7691-187">It contains Adobe’s EntityDescriptor URL and AssertionConsumerService URL.</span></span> <span data-ttu-id="a7691-188">Öppna filen och konfigurera dem i Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="a7691-188">Please open the file and configure them in the Azure AD application.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    <span data-ttu-id="a7691-191">a.</span><span class="sxs-lookup"><span data-stu-id="a7691-191">a.</span></span> <span data-ttu-id="a7691-192">Använd EntityDescriptor värdet Adobe för **identifierare** på den **konfigurera Appinställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7691-192">Use the EntityDescriptor value Adobe provided you for **Identifier** on the **Configure App Settings** dialog.</span></span>

    <span data-ttu-id="a7691-193">b.</span><span class="sxs-lookup"><span data-stu-id="a7691-193">b.</span></span> <span data-ttu-id="a7691-194">Använd AssertionConsumerService värdet Adobe för **Reply URL** på den **konfigurera Appinställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7691-194">Use the AssertionConsumerService value Adobe provided you for **Reply URL** on the **Configure App Settings** dialog.</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a7691-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a7691-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="a7691-196">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a7691-196">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a7691-198">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a7691-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a7691-199">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a7691-199">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a7691-201">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="a7691-201">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a7691-203">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7691-203">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a7691-205">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a7691-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a7691-207">a.</span><span class="sxs-lookup"><span data-stu-id="a7691-207">a.</span></span> <span data-ttu-id="a7691-208">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a7691-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a7691-209">b.</span><span class="sxs-lookup"><span data-stu-id="a7691-209">b.</span></span> <span data-ttu-id="a7691-210">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a7691-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a7691-211">c.</span><span class="sxs-lookup"><span data-stu-id="a7691-211">c.</span></span> <span data-ttu-id="a7691-212">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a7691-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a7691-213">d.</span><span class="sxs-lookup"><span data-stu-id="a7691-213">d.</span></span> <span data-ttu-id="a7691-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a7691-214">Click **Create**.</span></span> 

### <a name="creating-an-adobe-creative-cloud-test-user"></a><span data-ttu-id="a7691-215">Skapa en testanvändare Adobe kreativa moln</span><span class="sxs-lookup"><span data-stu-id="a7691-215">Creating an Adobe Creative Cloud test user</span></span>

<span data-ttu-id="a7691-216">För att aktivera Azure AD-användare att logga in på Adobe kreativa molnet, måste de etableras i Adobe kreativa moln.</span><span class="sxs-lookup"><span data-stu-id="a7691-216">In order to enable Azure AD users to log into Adobe Creative Cloud, they must be provisioned into Adobe Creative Cloud.</span></span>  
<span data-ttu-id="a7691-217">Adobe kreativa moln är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a7691-217">In the case of Adobe Creative Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="a7691-218">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="a7691-218">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="a7691-219">Logga in på webbplatsen Adobe kreativa molnet företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="a7691-219">Log in to your Adobe Creative Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="a7691-220">Klicka på **personer**.</span><span class="sxs-lookup"><span data-stu-id="a7691-220">Click **People**.</span></span>

    <span data-ttu-id="a7691-221">![Personer](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "personer")</span><span class="sxs-lookup"><span data-stu-id="a7691-221">![People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="a7691-222">Klicka på **bjuda in användare**.</span><span class="sxs-lookup"><span data-stu-id="a7691-222">Click **Invite User**.</span></span>

    <span data-ttu-id="a7691-223">![Bjuda in användare](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "bjuda in användare")</span><span class="sxs-lookup"><span data-stu-id="a7691-223">![Invite Users](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="a7691-224">På den **bjuda in personer** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a7691-224">On the **Invite People** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="a7691-225">![Bjuda in](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="a7691-225">![Invite People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="a7691-226">a.</span><span class="sxs-lookup"><span data-stu-id="a7691-226">a.</span></span> <span data-ttu-id="a7691-227">I den **e-post** textruta skriver Britta Simon konto e-postadress.</span><span class="sxs-lookup"><span data-stu-id="a7691-227">In the **Email** textbox, type the email address of Britta Simon account.</span></span>
    
    <span data-ttu-id="a7691-228">b.</span><span class="sxs-lookup"><span data-stu-id="a7691-228">b.</span></span> <span data-ttu-id="a7691-229">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="a7691-229">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a7691-230">Innehavaren för Azure Active Directory-konto ett e-postmeddelande och länken för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="a7691-230">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a7691-231">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a7691-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a7691-232">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning med sin åtkomst beviljas till Adobe kreativa moln.</span><span class="sxs-lookup"><span data-stu-id="a7691-232">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Adobe Creative Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a7691-234">**Om du vill tilldela Adobe kreativa molnet Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a7691-234">**To assign Britta Simon to Adobe Creative Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="a7691-235">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a7691-235">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a7691-237">Välj i listan med program **Adobe kreativa molnet**.</span><span class="sxs-lookup"><span data-stu-id="a7691-237">In the applications list, select **Adobe Creative Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. <span data-ttu-id="a7691-239">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a7691-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a7691-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a7691-241">Click **Add** button.</span></span> <span data-ttu-id="a7691-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7691-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a7691-244">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a7691-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a7691-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7691-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a7691-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7691-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a7691-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a7691-247">Testing single sign-on</span></span>

<span data-ttu-id="a7691-248">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a7691-248">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a7691-249">När du klickar på panelen Adobe kreativa molnet på åtkomstpanelen du bör få automatiskt inloggade i tillämpningsprogrammet Adobe kreativa molnet.</span><span class="sxs-lookup"><span data-stu-id="a7691-249">When you click the Adobe Creative Cloud tile in the Access Panel, you should get automatically signed-on to your Adobe Creative Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="a7691-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a7691-250">Additional resources</span></span>

* [<span data-ttu-id="a7691-251">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a7691-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a7691-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a7691-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png