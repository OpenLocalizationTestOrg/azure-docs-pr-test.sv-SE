---
title: "Självstudier: Azure Active Directory-integrering med säkra leverera | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och skydda leverera."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fccd5668-fe6f-4e6d-a9ce-ba4f321c33d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: f6e5b1e34893f6b8fe14e238e24086bb47d009a5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a><span data-ttu-id="348ae-103">Självstudier: Azure Active Directory-integrering med säkra leverera</span><span class="sxs-lookup"><span data-stu-id="348ae-103">Tutorial: Azure Active Directory integration with SECURE DELIVER</span></span>

<span data-ttu-id="348ae-104">I kursen får lära du att integrera SECURE leverera med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="348ae-104">In this tutorial, you learn how to integrate SECURE DELIVER with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="348ae-105">Integrera SECURE leverera med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="348ae-105">Integrating SECURE DELIVER with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="348ae-106">Du kan styra i Azure AD som har åtkomst till säker leverera</span><span class="sxs-lookup"><span data-stu-id="348ae-106">You can control in Azure AD who has access to SECURE DELIVER</span></span>
- <span data-ttu-id="348ae-107">Du kan aktivera användarna att automatiskt hämta loggat in på säkra leverera (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="348ae-107">You can enable your users to automatically get signed-on to SECURE DELIVER (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="348ae-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="348ae-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="348ae-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="348ae-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="348ae-110">Krav</span><span class="sxs-lookup"><span data-stu-id="348ae-110">Prerequisites</span></span>

<span data-ttu-id="348ae-111">Om du vill konfigurera Azure AD-integrering med säkra leverera behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="348ae-111">To configure Azure AD integration with SECURE DELIVER, you need the following items:</span></span>

- <span data-ttu-id="348ae-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="348ae-112">An Azure AD subscription</span></span>
- <span data-ttu-id="348ae-113">En säker tillhandahålla enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="348ae-113">A SECURE DELIVER single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="348ae-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="348ae-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="348ae-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="348ae-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="348ae-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="348ae-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="348ae-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="348ae-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="348ae-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="348ae-118">Scenario description</span></span>
<span data-ttu-id="348ae-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="348ae-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="348ae-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="348ae-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="348ae-121">Att lägga till säker leverera från galleriet</span><span class="sxs-lookup"><span data-stu-id="348ae-121">Adding SECURE DELIVER from the gallery</span></span>
2. <span data-ttu-id="348ae-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="348ae-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-secure-deliver-from-the-gallery"></a><span data-ttu-id="348ae-123">Att lägga till säker leverera från galleriet</span><span class="sxs-lookup"><span data-stu-id="348ae-123">Adding SECURE DELIVER from the gallery</span></span>
<span data-ttu-id="348ae-124">Du måste lägga till säker leverera från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av säker leverera i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="348ae-124">To configure the integration of SECURE DELIVER into Azure AD, you need to add SECURE DELIVER from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="348ae-125">**Utför följande steg för att lägga till säker leverera från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="348ae-125">**To add SECURE DELIVER from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="348ae-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="348ae-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="348ae-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="348ae-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="348ae-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="348ae-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="348ae-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="348ae-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="348ae-133">I sökrutan skriver **SECURE leverera**.</span><span class="sxs-lookup"><span data-stu-id="348ae-133">In the search box, type **SECURE DELIVER**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_search.png)

5. <span data-ttu-id="348ae-135">Välj i resultatpanelen **SECURE leverera**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="348ae-135">In the results panel, select **SECURE DELIVER**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="348ae-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="348ae-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="348ae-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med säkra leverera baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="348ae-138">In this section, you configure and test Azure AD single sign-on with SECURE DELIVER based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="348ae-139">Azure AD måste du känna till motsvarande användaren i säkra leverera till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="348ae-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SECURE DELIVER is to a user in Azure AD.</span></span> <span data-ttu-id="348ae-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i säkra leverera upprättas.</span><span class="sxs-lookup"><span data-stu-id="348ae-140">In other words, a link relationship between an Azure AD user and the related user in SECURE DELIVER needs to be established.</span></span>

<span data-ttu-id="348ae-141">I säkra leverera tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="348ae-141">In SECURE DELIVER, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="348ae-142">Om du vill konfigurera och testa Azure AD enkel inloggning med säkra leverera, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="348ae-142">To configure and test Azure AD single sign-on with SECURE DELIVER, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="348ae-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="348ae-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="348ae-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="348ae-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="348ae-145">**[Skapa en säker leverera testanvändare](#creating-a-secure-deliver-test-user)**  – du har en motsvarighet för Britta Simon i säkra leverera som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="348ae-145">**[Creating a SECURE DELIVER test user](#creating-a-secure-deliver-test-user)** - to have a counterpart of Britta Simon in SECURE DELIVER that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="348ae-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="348ae-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="348ae-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="348ae-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="348ae-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="348ae-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="348ae-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program för säker leverera.</span><span class="sxs-lookup"><span data-stu-id="348ae-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SECURE DELIVER application.</span></span>

<span data-ttu-id="348ae-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med säkra leverera:**</span><span class="sxs-lookup"><span data-stu-id="348ae-150">**To configure Azure AD single sign-on with SECURE DELIVER, perform the following steps:**</span></span>

1. <span data-ttu-id="348ae-151">I Azure-portalen på den **SECURE leverera** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="348ae-151">In the Azure portal, on the **SECURE DELIVER** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="348ae-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="348ae-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_samlbase.png)

3. <span data-ttu-id="348ae-155">På den **URL: er och skydda leverera domän** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="348ae-155">On the **SECURE DELIVER Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_url.png)

    <span data-ttu-id="348ae-157">a.</span><span class="sxs-lookup"><span data-stu-id="348ae-157">a.</span></span> <span data-ttu-id="348ae-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span><span class="sxs-lookup"><span data-stu-id="348ae-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span></span>

    <span data-ttu-id="348ae-159">b.</span><span class="sxs-lookup"><span data-stu-id="348ae-159">b.</span></span> <span data-ttu-id="348ae-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span><span class="sxs-lookup"><span data-stu-id="348ae-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="348ae-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="348ae-161">These values are not real.</span></span> <span data-ttu-id="348ae-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="348ae-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="348ae-163">Kontakta [SECURE leverera klienten supportteamet](mailto:iw-sd-support@fujifilm.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="348ae-163">Contact [SECURE DELIVER Client support team](mailto:iw-sd-support@fujifilm.com) to get these values.</span></span> 
 
4. <span data-ttu-id="348ae-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="348ae-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_certificate.png) 

5. <span data-ttu-id="348ae-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="348ae-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="348ae-168">På den **SECURE leverera Configuration** klickar du på **konfigurera SECURE leverera** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="348ae-168">On the **SECURE DELIVER Configuration** section, click **Configure SECURE DELIVER** to open **Configure sign-on** window.</span></span> <span data-ttu-id="348ae-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="348ae-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_configure.png) 

7. <span data-ttu-id="348ae-171">Konfigurera enkel inloggning på **SECURE leverera** sida, måste du skicka den hämtade **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** till [SECURE leverera supportteamet](mailto:iw-sd-support@fujifilm.com).</span><span class="sxs-lookup"><span data-stu-id="348ae-171">To configure single sign-on on **SECURE DELIVER** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com).</span></span> <span data-ttu-id="348ae-172">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="348ae-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="348ae-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="348ae-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="348ae-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="348ae-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="348ae-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="348ae-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="348ae-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="348ae-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="348ae-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="348ae-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="348ae-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="348ae-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="348ae-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="348ae-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="348ae-182">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="348ae-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="348ae-184">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="348ae-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="348ae-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="348ae-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="348ae-188">a.</span><span class="sxs-lookup"><span data-stu-id="348ae-188">a.</span></span> <span data-ttu-id="348ae-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="348ae-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="348ae-190">b.</span><span class="sxs-lookup"><span data-stu-id="348ae-190">b.</span></span> <span data-ttu-id="348ae-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="348ae-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="348ae-192">c.</span><span class="sxs-lookup"><span data-stu-id="348ae-192">c.</span></span> <span data-ttu-id="348ae-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="348ae-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="348ae-194">d.</span><span class="sxs-lookup"><span data-stu-id="348ae-194">d.</span></span> <span data-ttu-id="348ae-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="348ae-195">Click **Create**.</span></span>
 
### <a name="creating-a-secure-deliver-test-user"></a><span data-ttu-id="348ae-196">Skapa en säker leverera testanvändare</span><span class="sxs-lookup"><span data-stu-id="348ae-196">Creating a SECURE DELIVER test user</span></span>

<span data-ttu-id="348ae-197">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i säkra leverera.</span><span class="sxs-lookup"><span data-stu-id="348ae-197">The objective of this section is to create a user called Britta Simon in SECURE DELIVER.</span></span> <span data-ttu-id="348ae-198">Arbeta med [SECURE leverera supportteamet](mailto:iw-sd-support@fujifilm.com) att lägga till användare i säkra leverera-konto.</span><span class="sxs-lookup"><span data-stu-id="348ae-198">Work with [SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com) to add the users in the SECURE DELIVER account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="348ae-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="348ae-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="348ae-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till säker leverera.</span><span class="sxs-lookup"><span data-stu-id="348ae-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SECURE DELIVER.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="348ae-202">**Om du vill tilldela Britta Simon SECURE leverera, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="348ae-202">**To assign Britta Simon to SECURE DELIVER, perform the following steps:**</span></span>

1. <span data-ttu-id="348ae-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="348ae-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="348ae-205">Välj i listan med program **SECURE leverera**.</span><span class="sxs-lookup"><span data-stu-id="348ae-205">In the applications list, select **SECURE DELIVER**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_app.png) 

3. <span data-ttu-id="348ae-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="348ae-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="348ae-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="348ae-209">Click **Add** button.</span></span> <span data-ttu-id="348ae-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="348ae-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="348ae-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="348ae-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="348ae-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="348ae-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="348ae-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="348ae-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="348ae-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="348ae-215">Testing single sign-on</span></span>

<span data-ttu-id="348ae-216">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="348ae-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="348ae-217">När du klickar på panelen SECURE leverera på åtkomstpanelen du bör få automatiskt loggat in på ditt program för säker leverera.</span><span class="sxs-lookup"><span data-stu-id="348ae-217">When you click the SECURE DELIVER tile in the Access Panel, you should get automatically signed-on to your SECURE DELIVER application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="348ae-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="348ae-218">Additional resources</span></span>

* [<span data-ttu-id="348ae-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="348ae-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="348ae-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="348ae-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png

