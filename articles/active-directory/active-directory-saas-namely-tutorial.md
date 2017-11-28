---
title: "Självstudier: Azure Active Directory-integrering med nämligen | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och nämligen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 1d7e8fbcfc757853ab909bbb05522f3dc387715d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a><span data-ttu-id="30230-103">Självstudier: Azure Active Directory-integrering med nämligen</span><span class="sxs-lookup"><span data-stu-id="30230-103">Tutorial: Azure Active Directory integration with Namely</span></span>

<span data-ttu-id="30230-104">I kursen får lära du att integrera nämligen med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="30230-104">In this tutorial, you learn how to integrate Namely with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="30230-105">Integrera nämligen med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="30230-105">Integrating Namely with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="30230-106">Du kan styra i Azure AD som har åtkomst till nämligen</span><span class="sxs-lookup"><span data-stu-id="30230-106">You can control in Azure AD who has access to Namely</span></span>
- <span data-ttu-id="30230-107">Du kan ge användarna automatiskt får loggat in på nämligen (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="30230-107">You can enable your users to automatically get signed-on to Namely (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="30230-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="30230-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="30230-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="30230-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30230-110">Krav</span><span class="sxs-lookup"><span data-stu-id="30230-110">Prerequisites</span></span>

<span data-ttu-id="30230-111">Integrering med nämligen du behöver följande för att konfigurera Azure AD:</span><span class="sxs-lookup"><span data-stu-id="30230-111">To configure Azure AD integration with Namely, you need the following items:</span></span>

- <span data-ttu-id="30230-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="30230-112">An Azure AD subscription</span></span>
- <span data-ttu-id="30230-113">En nämligen enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="30230-113">A Namely single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="30230-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="30230-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="30230-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="30230-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="30230-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="30230-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="30230-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="30230-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="30230-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="30230-118">Scenario description</span></span>
<span data-ttu-id="30230-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="30230-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="30230-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="30230-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="30230-121">Att lägga till nämligen från galleriet</span><span class="sxs-lookup"><span data-stu-id="30230-121">Adding Namely from the gallery</span></span>
2. <span data-ttu-id="30230-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="30230-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-namely-from-the-gallery"></a><span data-ttu-id="30230-123">Att lägga till nämligen från galleriet</span><span class="sxs-lookup"><span data-stu-id="30230-123">Adding Namely from the gallery</span></span>
<span data-ttu-id="30230-124">Du måste lägga till nämligen från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av nämligen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="30230-124">To configure the integration of Namely into Azure AD, you need to add Namely from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="30230-125">**Utför följande steg för att lägga till nämligen från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="30230-125">**To add Namely from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="30230-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="30230-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="30230-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="30230-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="30230-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="30230-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="30230-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="30230-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="30230-133">I sökrutan skriver **nämligen**.</span><span class="sxs-lookup"><span data-stu-id="30230-133">In the search box, type **Namely**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_search.png)

5. <span data-ttu-id="30230-135">Välj i resultatpanelen **nämligen**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="30230-135">In the results panel, select **Namely**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="30230-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="30230-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="30230-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med nämligen baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="30230-138">In this section, you configure and test Azure AD single sign-on with Namely based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="30230-139">Azure AD måste du känna till användaren i motsvarande nämligen till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="30230-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Namely is to a user in Azure AD.</span></span> <span data-ttu-id="30230-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i nämligen upprättas.</span><span class="sxs-lookup"><span data-stu-id="30230-140">In other words, a link relationship between an Azure AD user and the related user in Namely needs to be established.</span></span>

<span data-ttu-id="30230-141">I det kan tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="30230-141">In Namely, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="30230-142">Om du vill konfigurera och testa Azure AD måste enkel inloggning med nämligen dig du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="30230-142">To configure and test Azure AD single sign-on with Namely, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="30230-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="30230-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="30230-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30230-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="30230-145">**[Skapa en testanvändare nämligen](#creating-a-namely-test-user)**  – du har en motsvarighet för Britta Simon i nämligen som är länkade till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="30230-145">**[Creating a Namely test user](#creating-a-namely-test-user)** - to have a counterpart of Britta Simon in Namely that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="30230-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="30230-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="30230-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="30230-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="30230-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="30230-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="30230-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din nämligen program.</span><span class="sxs-lookup"><span data-stu-id="30230-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Namely application.</span></span>

<span data-ttu-id="30230-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med nämligen:**</span><span class="sxs-lookup"><span data-stu-id="30230-150">**To configure Azure AD single sign-on with Namely, perform the following steps:**</span></span>

1. <span data-ttu-id="30230-151">I Azure-portalen på den **nämligen** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="30230-151">In the Azure portal, on the **Namely** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="30230-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="30230-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_samlbase.png)

3. <span data-ttu-id="30230-155">På den **nämligen domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="30230-155">On the **Namely Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_url.png)

    <span data-ttu-id="30230-157">a.</span><span class="sxs-lookup"><span data-stu-id="30230-157">a.</span></span> <span data-ttu-id="30230-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.namely.com`</span><span class="sxs-lookup"><span data-stu-id="30230-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.namely.com`</span></span>

    <span data-ttu-id="30230-159">b.</span><span class="sxs-lookup"><span data-stu-id="30230-159">b.</span></span> <span data-ttu-id="30230-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.namely.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="30230-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.namely.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="30230-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="30230-161">These values are not real.</span></span> <span data-ttu-id="30230-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="30230-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="30230-163">Kontakta [nämligen klienten supportteamet](https://www.namely.com/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="30230-163">Contact [Namely Client support team](https://www.namely.com/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="30230-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="30230-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_certificate.png) 

5. <span data-ttu-id="30230-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="30230-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="30230-168">På den **nämligen Configuration** klickar du på **konfigurera nämligen** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="30230-168">On the **Namely Configuration** section, click **Configure Namely** to open **Configure sign-on** window.</span></span> <span data-ttu-id="30230-169">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="30230-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_configure.png) 

7. <span data-ttu-id="30230-171">I ett nytt webbläsarfönster, logga in på ditt nämligen företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="30230-171">In another browser window, sign on to your Namely company site as an administrator.</span></span>

8. <span data-ttu-id="30230-172">Klicka på i verktygsfältet högst upp **företagets**.</span><span class="sxs-lookup"><span data-stu-id="30230-172">In the toolbar on the top, click **Company**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

9. <span data-ttu-id="30230-174">Klicka på fliken **Settings** (Inställningar).</span><span class="sxs-lookup"><span data-stu-id="30230-174">Click the **Settings** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 

10. <span data-ttu-id="30230-176">Klicka på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="30230-176">Click **SAML**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 

11. <span data-ttu-id="30230-178">På den **SAML inställningar** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="30230-178">On the **SAML Settings** page, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png)
 
    <span data-ttu-id="30230-180">a.</span><span class="sxs-lookup"><span data-stu-id="30230-180">a.</span></span> <span data-ttu-id="30230-181">Klicka på **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="30230-181">Click **Enable SAML**.</span></span> 

    <span data-ttu-id="30230-182">b.</span><span class="sxs-lookup"><span data-stu-id="30230-182">b.</span></span> <span data-ttu-id="30230-183">I den **identitet providern SSO url** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="30230-183">In the **Identity provider SSO url** textbox,  paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="30230-184">c.</span><span class="sxs-lookup"><span data-stu-id="30230-184">c.</span></span> <span data-ttu-id="30230-185">Öppna din hämtat certifikat i anteckningar, kopiera innehållet och klistrar in det i den **providern identitetscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="30230-185">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **Identity provider certificate** textbox.</span></span>
     
    <span data-ttu-id="30230-186">d.</span><span class="sxs-lookup"><span data-stu-id="30230-186">d.</span></span> <span data-ttu-id="30230-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="30230-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="30230-188">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="30230-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="30230-189">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="30230-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="30230-190">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="30230-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="30230-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="30230-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="30230-192">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30230-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="30230-194">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="30230-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="30230-195">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="30230-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="30230-197">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="30230-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="30230-199">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="30230-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="30230-201">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="30230-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="30230-203">a.</span><span class="sxs-lookup"><span data-stu-id="30230-203">a.</span></span> <span data-ttu-id="30230-204">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="30230-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="30230-205">b.</span><span class="sxs-lookup"><span data-stu-id="30230-205">b.</span></span> <span data-ttu-id="30230-206">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="30230-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="30230-207">c.</span><span class="sxs-lookup"><span data-stu-id="30230-207">c.</span></span> <span data-ttu-id="30230-208">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="30230-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="30230-209">d.</span><span class="sxs-lookup"><span data-stu-id="30230-209">d.</span></span> <span data-ttu-id="30230-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="30230-210">Click **Create**.</span></span>
 
### <a name="creating-a-namely-test-user"></a><span data-ttu-id="30230-211">Skapa en nämligen testanvändare</span><span class="sxs-lookup"><span data-stu-id="30230-211">Creating a Namely test user</span></span>

<span data-ttu-id="30230-212">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i nämligen.</span><span class="sxs-lookup"><span data-stu-id="30230-212">The objective of this section is to create a user called Britta Simon in Namely.</span></span>

<span data-ttu-id="30230-213">**Utför följande steg för att skapa en användare som kallas Britta Simon i nämligen:**</span><span class="sxs-lookup"><span data-stu-id="30230-213">**To create a user called Britta Simon in Namely, perform the following steps:**</span></span>

1. <span data-ttu-id="30230-214">Logga in på ditt nämligen företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="30230-214">Sign-on to your Namely company site as an administrator.</span></span>

2. <span data-ttu-id="30230-215">Klicka på i verktygsfältet högst upp **personer**.</span><span class="sxs-lookup"><span data-stu-id="30230-215">In the toolbar on the top, click **People**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

3. <span data-ttu-id="30230-217">Klicka på den **Directory** fliken.</span><span class="sxs-lookup"><span data-stu-id="30230-217">Click the **Directory** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

4. <span data-ttu-id="30230-219">Klicka på **Lägg till ny Person**.</span><span class="sxs-lookup"><span data-stu-id="30230-219">Click **Add New Person**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_12.png)

5. <span data-ttu-id="30230-221">På den **Lägg till ny Person** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="30230-221">On the **Add New Person** dialog, perform the following steps:</span></span>

    <span data-ttu-id="30230-222">a.</span><span class="sxs-lookup"><span data-stu-id="30230-222">a.</span></span> <span data-ttu-id="30230-223">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="30230-223">In the **First name** textbox, type **Britta**.</span></span>

    <span data-ttu-id="30230-224">b.</span><span class="sxs-lookup"><span data-stu-id="30230-224">b.</span></span> <span data-ttu-id="30230-225">I den **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="30230-225">In the **Last name** textbox, type **Simon**.</span></span>

    <span data-ttu-id="30230-226">c.</span><span class="sxs-lookup"><span data-stu-id="30230-226">c.</span></span> <span data-ttu-id="30230-227">I den **e-post** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="30230-227">In the **Email** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="30230-228">d.</span><span class="sxs-lookup"><span data-stu-id="30230-228">d.</span></span> <span data-ttu-id="30230-229">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="30230-229">Click **Save**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="30230-230">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="30230-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="30230-231">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till nämligen.</span><span class="sxs-lookup"><span data-stu-id="30230-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Namely.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="30230-233">**Så här tilldelar Britta Simon nämligen utföra följande steg:**</span><span class="sxs-lookup"><span data-stu-id="30230-233">**To assign Britta Simon to Namely, perform the following steps:**</span></span>

1. <span data-ttu-id="30230-234">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="30230-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="30230-236">Välj i listan med program **nämligen**.</span><span class="sxs-lookup"><span data-stu-id="30230-236">In the applications list, select **Namely**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_app.png) 

3. <span data-ttu-id="30230-238">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="30230-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="30230-240">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="30230-240">Click **Add** button.</span></span> <span data-ttu-id="30230-241">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="30230-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="30230-243">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="30230-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="30230-244">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="30230-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="30230-245">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="30230-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="30230-246">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="30230-246">Testing single sign-on</span></span>

<span data-ttu-id="30230-247">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="30230-247">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="30230-248">När du klickar på den nämligen panelen i åtkomstpanelen, du bör få automatiskt loggat in på ditt nämligen program</span><span class="sxs-lookup"><span data-stu-id="30230-248">When you click the Namely tile in the Access Panel, you should get automatically signed-on to your Namely application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30230-249">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="30230-249">Additional resources</span></span>

* [<span data-ttu-id="30230-250">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="30230-250">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="30230-251">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="30230-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-namely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png

