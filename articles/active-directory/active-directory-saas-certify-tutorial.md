---
title: "Självstudier: Azure Active Directory-integrering med certifiera | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och certifiera."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b36e020-175a-4534-b341-85260739f889
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bbb2357d17535de438555a0b1f8256b134c8a40e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-certify"></a><span data-ttu-id="721a0-103">Självstudier: Azure Active Directory-integrering med Certify</span><span class="sxs-lookup"><span data-stu-id="721a0-103">Tutorial: Azure Active Directory integration with Certify</span></span>

<span data-ttu-id="721a0-104">I kursen får lära du att integrera certifiera med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="721a0-104">In this tutorial, you learn how to integrate Certify with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="721a0-105">Integrera certifiera med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="721a0-105">Integrating Certify with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="721a0-106">Du kan styra i Azure AD som har åtkomst till Certify</span><span class="sxs-lookup"><span data-stu-id="721a0-106">You can control in Azure AD who has access to Certify</span></span>
- <span data-ttu-id="721a0-107">Du kan aktivera användarna att automatiskt hämta loggat in på certifiera (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="721a0-107">You can enable your users to automatically get signed-on to Certify (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="721a0-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="721a0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="721a0-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="721a0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="721a0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="721a0-110">Prerequisites</span></span>

<span data-ttu-id="721a0-111">Om du vill konfigurera Azure AD-integrering med certifiera behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="721a0-111">To configure Azure AD integration with Certify, you need the following items:</span></span>

- <span data-ttu-id="721a0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="721a0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="721a0-113">En certifiera enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="721a0-113">A Certify single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="721a0-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="721a0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="721a0-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="721a0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="721a0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="721a0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="721a0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="721a0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="721a0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="721a0-118">Scenario description</span></span>
<span data-ttu-id="721a0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="721a0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="721a0-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="721a0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="721a0-121">Att lägga till Certify från galleriet</span><span class="sxs-lookup"><span data-stu-id="721a0-121">Adding Certify from the gallery</span></span>
2. <span data-ttu-id="721a0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="721a0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-certify-from-the-gallery"></a><span data-ttu-id="721a0-123">Att lägga till Certify från galleriet</span><span class="sxs-lookup"><span data-stu-id="721a0-123">Adding Certify from the gallery</span></span>
<span data-ttu-id="721a0-124">Du måste lägga till certifiera från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av certifiera i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="721a0-124">To configure the integration of Certify into Azure AD, you need to add Certify from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="721a0-125">**Utför följande steg för att lägga till Certify från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="721a0-125">**To add Certify from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="721a0-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="721a0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="721a0-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="721a0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="721a0-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="721a0-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="721a0-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="721a0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="721a0-133">I sökrutan skriver **Certify**.</span><span class="sxs-lookup"><span data-stu-id="721a0-133">In the search box, type **Certify**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-certify-tutorial/tutorial_certify_search.png)

5. <span data-ttu-id="721a0-135">Välj i resultatpanelen **Certify**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="721a0-135">In the results panel, select **Certify**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-certify-tutorial/tutorial_certify_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="721a0-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="721a0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="721a0-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med certifiera baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="721a0-138">In this section, you configure and test Azure AD single sign-on with Certify based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="721a0-139">Azure AD måste du känna till användaren i certifiera motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="721a0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Certify is to a user in Azure AD.</span></span> <span data-ttu-id="721a0-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i certifiera upprättas.</span><span class="sxs-lookup"><span data-stu-id="721a0-140">In other words, a link relationship between an Azure AD user and the related user in Certify needs to be established.</span></span>

<span data-ttu-id="721a0-141">I Certify tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="721a0-141">In Certify, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="721a0-142">Om du vill konfigurera och testa Azure AD enkel inloggning med certifiera, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="721a0-142">To configure and test Azure AD single sign-on with Certify, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="721a0-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="721a0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="721a0-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="721a0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="721a0-145">**[Skapa en testanvändare certifiera](#creating-a-certify-test-user)**  – har en motsvarighet för Britta Simon certifiera som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="721a0-145">**[Creating a Certify test user](#creating-a-certify-test-user)** - to have a counterpart of Britta Simon in Certify that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="721a0-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="721a0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="721a0-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="721a0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="721a0-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="721a0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="721a0-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i certifiera programmet.</span><span class="sxs-lookup"><span data-stu-id="721a0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Certify application.</span></span>

<span data-ttu-id="721a0-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med certifiera:**</span><span class="sxs-lookup"><span data-stu-id="721a0-150">**To configure Azure AD single sign-on with Certify, perform the following steps:**</span></span>

1. <span data-ttu-id="721a0-151">I Azure-portalen på den **Certify** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="721a0-151">In the Azure portal, on the **Certify** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="721a0-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="721a0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-certify-tutorial/tutorial_certify_samlbase.png)

3. <span data-ttu-id="721a0-155">På den **certifiera domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="721a0-155">On the **Certify Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-certify-tutorial/tutorial_certify_url.png)

    <span data-ttu-id="721a0-157">I den **identifierare** textruta anger du URL:`https://www.certify.com`</span><span class="sxs-lookup"><span data-stu-id="721a0-157">In the **Identifier** textbox, type the URL: `https://www.certify.com`</span></span>

4. <span data-ttu-id="721a0-158">På den **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="721a0-158">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-certify-tutorial/tutorial_certify_certificate.png) 

5. <span data-ttu-id="721a0-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="721a0-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-certify-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="721a0-162">På den **certifiera Configuration** klickar du på **konfigurera certifiera** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="721a0-162">On the **Certify Configuration** section, click **Configure Certify** to open **Configure sign-on** window.</span></span> <span data-ttu-id="721a0-163">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="721a0-163">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-certify-tutorial/tutorial_certify_configure.png) 

7. <span data-ttu-id="721a0-165">Konfigurera enkel inloggning på **Certify** sida, måste du skicka den hämtade **Certificate(Raw)** och **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** att [certifiera supportteamet](mailto:support@certify.com).</span><span class="sxs-lookup"><span data-stu-id="721a0-165">To configure single sign-on on **Certify** side, you need to send the downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Certify support team](mailto:support@certify.com).</span></span> <span data-ttu-id="721a0-166">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="721a0-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="721a0-167">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="721a0-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="721a0-168">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="721a0-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="721a0-169">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="721a0-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="721a0-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="721a0-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="721a0-171">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="721a0-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="721a0-173">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="721a0-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="721a0-174">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="721a0-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-certify-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="721a0-176">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="721a0-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-certify-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="721a0-178">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="721a0-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-certify-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="721a0-180">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="721a0-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-certify-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="721a0-182">a.</span><span class="sxs-lookup"><span data-stu-id="721a0-182">a.</span></span> <span data-ttu-id="721a0-183">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="721a0-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="721a0-184">b.</span><span class="sxs-lookup"><span data-stu-id="721a0-184">b.</span></span> <span data-ttu-id="721a0-185">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="721a0-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="721a0-186">c.</span><span class="sxs-lookup"><span data-stu-id="721a0-186">c.</span></span> <span data-ttu-id="721a0-187">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="721a0-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="721a0-188">d.</span><span class="sxs-lookup"><span data-stu-id="721a0-188">d.</span></span> <span data-ttu-id="721a0-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="721a0-189">Click **Create**.</span></span>
 
### <a name="creating-a-certify-test-user"></a><span data-ttu-id="721a0-190">Skapa en testanvändare certifiera</span><span class="sxs-lookup"><span data-stu-id="721a0-190">Creating a Certify test user</span></span>

<span data-ttu-id="721a0-191">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i certifiera.</span><span class="sxs-lookup"><span data-stu-id="721a0-191">The objective of this section is to create a user called Britta Simon in Certify.</span></span> <span data-ttu-id="721a0-192">Certifiera stöder just-in-time-etablering, som är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="721a0-192">Certify supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="721a0-193">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="721a0-193">There is no action item for you in this section.</span></span> <span data-ttu-id="721a0-194">En ny användare skapas vid ett försök att komma åt Certify om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="721a0-194">A new user will be created during an attempt to access Certify if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="721a0-195">Om du behöver skapa en användare manuellt måste du kontakta den [certifiera supportteamet](mailto:support@certify.com).</span><span class="sxs-lookup"><span data-stu-id="721a0-195">If you need to create an user manually, you need to contact the [Certify support team](mailto:support@certify.com).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="721a0-196">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="721a0-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="721a0-197">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Certify.</span><span class="sxs-lookup"><span data-stu-id="721a0-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Certify.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="721a0-199">**Om du vill tilldela certifiera Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="721a0-199">**To assign Britta Simon to Certify, perform the following steps:**</span></span>

1. <span data-ttu-id="721a0-200">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="721a0-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="721a0-202">Välj i listan med program **Certify**.</span><span class="sxs-lookup"><span data-stu-id="721a0-202">In the applications list, select **Certify**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-certify-tutorial/tutorial_certify_app.png) 

3. <span data-ttu-id="721a0-204">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="721a0-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="721a0-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="721a0-206">Click **Add** button.</span></span> <span data-ttu-id="721a0-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="721a0-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="721a0-209">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="721a0-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="721a0-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="721a0-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="721a0-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="721a0-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="721a0-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="721a0-212">Testing single sign-on</span></span>

<span data-ttu-id="721a0-213">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="721a0-213">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="721a0-214">När du klickar på panelen certifiera på åtkomstpanelen du bör få automatiskt loggat in på certifiera programmet.</span><span class="sxs-lookup"><span data-stu-id="721a0-214">When you click the Certify tile in the Access Panel, you should get automatically signed-on to your Certify application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="721a0-215">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="721a0-215">Additional resources</span></span>

* [<span data-ttu-id="721a0-216">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="721a0-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="721a0-217">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="721a0-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-certify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-certify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-certify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-certify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-certify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-certify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-certify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-certify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-certify-tutorial/tutorial_general_203.png

