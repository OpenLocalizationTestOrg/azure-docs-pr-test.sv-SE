---
title: "Självstudier: Azure Active Directory-integrering med Concur | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Concur."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1eee0a5d-24fa-4986-9aef-3c543cfe3296
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 0b44437b3dcf69dae3587529da7d12e7809b9f55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-concur"></a><span data-ttu-id="3066d-103">Självstudier: Azure Active Directory-integrering med Concur</span><span class="sxs-lookup"><span data-stu-id="3066d-103">Tutorial: Azure Active Directory integration with Concur</span></span>

<span data-ttu-id="3066d-104">I kursen får lära du att integrera Concur med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3066d-104">In this tutorial, you learn how to integrate Concur with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3066d-105">Integrera Concur med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3066d-105">Integrating Concur with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3066d-106">Du kan styra i Azure AD som har åtkomst till Concur</span><span class="sxs-lookup"><span data-stu-id="3066d-106">You can control in Azure AD who has access to Concur</span></span>
- <span data-ttu-id="3066d-107">Du kan aktivera användarna att automatiskt hämta loggat in på Concur (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3066d-107">You can enable your users to automatically get signed-on to Concur (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3066d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3066d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3066d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3066d-109">If you want to know more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3066d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3066d-110">Prerequisites</span></span>

<span data-ttu-id="3066d-111">För att konfigurera Azure AD-integrering med Concur, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3066d-111">To configure Azure AD integration with Concur, you need the following items:</span></span>

- <span data-ttu-id="3066d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3066d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3066d-113">En Concur enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3066d-113">A Concur single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3066d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3066d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3066d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3066d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3066d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3066d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3066d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3066d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3066d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3066d-118">Scenario description</span></span>
<span data-ttu-id="3066d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3066d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3066d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3066d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3066d-121">Att lägga till Concur från galleriet</span><span class="sxs-lookup"><span data-stu-id="3066d-121">Adding Concur from the gallery</span></span>
2. <span data-ttu-id="3066d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3066d-122">Configuring and testing Azure AD single sign-on</span></span>

>[!NOTE]
><span data-ttu-id="3066d-123">Konfigurationen av prenumerationen Concur för federerad enkel inloggning via SAML har en separat åtgärd måste du kontakta [verklig klienten supportteamet](https://www.concur.co.in/contact) att utföra.</span><span class="sxs-lookup"><span data-stu-id="3066d-123">The configuration of your Concur subscription for federated SSO via SAML is a separate task, which you must contact [Concur Client support team](https://www.concur.co.in/contact) to perform.</span></span> 

## <a name="adding-concur-from-the-gallery"></a><span data-ttu-id="3066d-124">Att lägga till Concur från galleriet</span><span class="sxs-lookup"><span data-stu-id="3066d-124">Adding Concur from the gallery</span></span>
<span data-ttu-id="3066d-125">Du måste lägga till Concur från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Concur i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3066d-125">To configure the integration of Concur into Azure AD, you need to add Concur from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3066d-126">**Utför följande steg för att lägga till Concur från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3066d-126">**To add Concur from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3066d-127">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3066d-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3066d-129">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3066d-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3066d-130">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3066d-130">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3066d-132">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3066d-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3066d-134">I sökrutan skriver **Concur**.</span><span class="sxs-lookup"><span data-stu-id="3066d-134">In the search box, type **Concur**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-concur-tutorial/tutorial_concur_search.png)

5. <span data-ttu-id="3066d-136">Välj i resultatpanelen **Concur**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3066d-136">In the results panel, select **Concur**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-concur-tutorial/tutorial_concur_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3066d-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3066d-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3066d-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Concur baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3066d-139">In this section, you configure and test Azure AD single sign-on with Concur based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3066d-140">Azure AD måste du känna till användaren i Concur motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3066d-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Concur is to a user in Azure AD.</span></span> <span data-ttu-id="3066d-141">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Concur upprättas.</span><span class="sxs-lookup"><span data-stu-id="3066d-141">In other words, a link relationship between an Azure AD user and the related user in Concur needs to be established.</span></span>

<span data-ttu-id="3066d-142">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Concur.</span><span class="sxs-lookup"><span data-stu-id="3066d-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Concur.</span></span>

<span data-ttu-id="3066d-143">Om du vill konfigurera och testa Azure AD enkel inloggning med Concur, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3066d-143">To configure and test Azure AD single sign-on with Concur, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3066d-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3066d-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3066d-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3066d-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3066d-146">**[Skapa en testanvändare Concur](#creating-a-concur-test-user)**  – du har en motsvarighet för Britta Simon i Concur som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3066d-146">**[Creating a Concur test user](#creating-a-concur-test-user)** - to have a counterpart of Britta Simon in Concur that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3066d-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3066d-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3066d-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3066d-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3066d-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3066d-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3066d-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Concur program.</span><span class="sxs-lookup"><span data-stu-id="3066d-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Concur application.</span></span>

<span data-ttu-id="3066d-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Concur:**</span><span class="sxs-lookup"><span data-stu-id="3066d-151">**To configure Azure AD single sign-on with Concur, perform the following steps:**</span></span>

1. <span data-ttu-id="3066d-152">I Azure-portalen på den **Concur** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3066d-152">In the Azure portal, on the **Concur** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3066d-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3066d-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-concur-tutorial/tutorial_concur_samlbase.png)

3. <span data-ttu-id="3066d-156">På den **verklig domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3066d-156">On the **Concur Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-concur-tutorial/tutorial_concur_url.png)

    <span data-ttu-id="3066d-158">a.</span><span class="sxs-lookup"><span data-stu-id="3066d-158">a.</span></span> <span data-ttu-id="3066d-159">I den **inloggning URL** textruta Skriv det värde som använder följande mönster:`https://www.concursolutions.com/UI/SSO/<OrganizationId>`</span><span class="sxs-lookup"><span data-stu-id="3066d-159">In the **Sign on URL** textbox, type the value using the following pattern: `https://www.concursolutions.com/UI/SSO/<OrganizationId>`</span></span>

    <span data-ttu-id="3066d-160">b.</span><span class="sxs-lookup"><span data-stu-id="3066d-160">b.</span></span> <span data-ttu-id="3066d-161">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<customer-domain>.concursolutions.com`</span><span class="sxs-lookup"><span data-stu-id="3066d-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<customer-domain>.concursolutions.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3066d-162">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="3066d-162">These values are not the real.</span></span> <span data-ttu-id="3066d-163">Uppdatera dessa värden med den faktiska inloggning URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="3066d-163">Update these values with the actual Sign on URL and Identifier.</span></span> <span data-ttu-id="3066d-164">Kontakta [verklig klienten supportteamet](https://www.concur.co.in/contact) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3066d-164">Contact [Concur Client support team](https://www.concur.co.in/contact) to get these values.</span></span> 

4. <span data-ttu-id="3066d-165">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="3066d-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-concur-tutorial/tutorial_concur_certificate.png) 

5. <span data-ttu-id="3066d-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3066d-167">Click **Save** button.</span></span>

    <span data-ttu-id="3066d-168">![Konfigurera enkel inloggning](./media/active-directory-saas-concur-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="3066d-168">![Configure Single Sign-On](./media/active-directory-saas-concur-tutorial/tutorial_general_400.png)
<CS></span></span>

6. <span data-ttu-id="3066d-169">Konfigurera enkel inloggning på **Concur** sida, måste du skicka den hämtade **XML-Metadata för** Concur-stöd.</span><span class="sxs-lookup"><span data-stu-id="3066d-169">To configure single sign-on on **Concur** side, you need to send the downloaded **Metadata XML** to Concur support.</span></span> <span data-ttu-id="3066d-170">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="3066d-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

  >[!NOTE]
  ><span data-ttu-id="3066d-171">Konfigurationen av prenumerationen Concur för federerad enkel inloggning via SAML har en separat åtgärd måste du kontakta [verklig klienten supportteamet](https://www.concur.co.in/contact) att utföra.</span><span class="sxs-lookup"><span data-stu-id="3066d-171">The configuration of your Concur subscription for federated SSO via SAML is a separate task, which you must contact [Concur Client support team](https://www.concur.co.in/contact) to perform.</span></span> 
  
<CE>

> [!TIP]
> <span data-ttu-id="3066d-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="3066d-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3066d-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="3066d-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3066d-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3066d-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3066d-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3066d-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="3066d-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3066d-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3066d-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3066d-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3066d-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3066d-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3066d-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3066d-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3066d-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3066d-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3066d-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3066d-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-concur-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3066d-187">a.</span><span class="sxs-lookup"><span data-stu-id="3066d-187">a.</span></span> <span data-ttu-id="3066d-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3066d-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3066d-189">b.</span><span class="sxs-lookup"><span data-stu-id="3066d-189">b.</span></span> <span data-ttu-id="3066d-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3066d-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3066d-191">c.</span><span class="sxs-lookup"><span data-stu-id="3066d-191">c.</span></span> <span data-ttu-id="3066d-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3066d-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3066d-193">d.</span><span class="sxs-lookup"><span data-stu-id="3066d-193">d.</span></span> <span data-ttu-id="3066d-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3066d-194">Click **Create**.</span></span>
 
### <a name="creating-a-concur-test-user"></a><span data-ttu-id="3066d-195">Skapa en testanvändare Concur</span><span class="sxs-lookup"><span data-stu-id="3066d-195">Creating a Concur test user</span></span>

<span data-ttu-id="3066d-196">Programmet stöder precis i tid användaretablering och authentication-användare skapas automatiskt i programmet.</span><span class="sxs-lookup"><span data-stu-id="3066d-196">Application supports the Just in time user provisioning and after authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3066d-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3066d-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3066d-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Concur.</span><span class="sxs-lookup"><span data-stu-id="3066d-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Concur.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3066d-200">**Om du vill tilldela Concur Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3066d-200">**To assign Britta Simon to Concur, perform the following steps:**</span></span>

1. <span data-ttu-id="3066d-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3066d-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3066d-203">Välj i listan med program **Concur**.</span><span class="sxs-lookup"><span data-stu-id="3066d-203">In the applications list, select **Concur**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-concur-tutorial/tutorial_concur_app.png) 

3. <span data-ttu-id="3066d-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3066d-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3066d-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3066d-207">Click **Add** button.</span></span> <span data-ttu-id="3066d-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3066d-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3066d-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="3066d-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3066d-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3066d-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3066d-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3066d-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3066d-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3066d-213">Testing single sign-on</span></span>

<span data-ttu-id="3066d-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3066d-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3066d-215">Du bör få inloggningssidan för Concur program när du klickar på panelen Concur på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3066d-215">When you click the Concur tile in the Access Panel, you should get login page of Concur application.</span></span>
<span data-ttu-id="3066d-216">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3066d-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3066d-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3066d-217">Additional resources</span></span>

* [<span data-ttu-id="3066d-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3066d-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3066d-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3066d-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="3066d-220">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="3066d-220">Configure User Provisioning</span></span>](active-directory-saas-concur-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-concur-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-concur-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-concur-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-concur-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-concur-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-concur-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-concur-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-concur-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-concur-tutorial/tutorial_general_203.png

