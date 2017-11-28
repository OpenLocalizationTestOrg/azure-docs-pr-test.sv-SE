---
title: "Självstudier: Azure Active Directory-integrering med Adobe | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Adobe logga."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: b413772de1af1fbb128d29b81e5831cfd6a39ab4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a><span data-ttu-id="fc434-103">Självstudier: Azure Active Directory-integrering med Adobe</span><span class="sxs-lookup"><span data-stu-id="fc434-103">Tutorial: Azure Active Directory integration with Adobe Sign</span></span>

<span data-ttu-id="fc434-104">I kursen får lära du att integrera Adobe inloggning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="fc434-104">In this tutorial, you learn how to integrate Adobe Sign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fc434-105">Integrera Adobe inloggning med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="fc434-105">Integrating Adobe Sign with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fc434-106">Du kan styra i Azure AD som har åtkomst till Adobe inloggning</span><span class="sxs-lookup"><span data-stu-id="fc434-106">You can control in Azure AD who has access to Adobe Sign</span></span>
- <span data-ttu-id="fc434-107">Du kan aktivera användarna att automatiskt hämta loggat in på Adobe logga (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="fc434-107">You can enable your users to automatically get signed-on to Adobe Sign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fc434-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fc434-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fc434-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fc434-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc434-110">Krav</span><span class="sxs-lookup"><span data-stu-id="fc434-110">Prerequisites</span></span>

<span data-ttu-id="fc434-111">För att konfigurera Azure AD-integrering med Adobe, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="fc434-111">To configure Azure AD integration with Adobe Sign, you need the following items:</span></span>

- <span data-ttu-id="fc434-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fc434-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fc434-113">En Adobe logga enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="fc434-113">An Adobe Sign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fc434-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fc434-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fc434-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="fc434-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fc434-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="fc434-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fc434-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fc434-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fc434-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="fc434-118">Scenario description</span></span>
<span data-ttu-id="fc434-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="fc434-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fc434-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="fc434-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fc434-121">Att lägga till Adobe logga från galleriet</span><span class="sxs-lookup"><span data-stu-id="fc434-121">Adding Adobe Sign from the gallery</span></span>
2. <span data-ttu-id="fc434-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fc434-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-sign-from-the-gallery"></a><span data-ttu-id="fc434-123">Att lägga till Adobe logga från galleriet</span><span class="sxs-lookup"><span data-stu-id="fc434-123">Adding Adobe Sign from the gallery</span></span>
<span data-ttu-id="fc434-124">Du måste lägga till Adobe logga från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Adobe inloggning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc434-124">To configure the integration of Adobe Sign into Azure AD, you need to add Adobe Sign from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fc434-125">**Utför följande steg för att lägga till Adobe logga från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="fc434-125">**To add Adobe Sign from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fc434-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fc434-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fc434-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fc434-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fc434-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fc434-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="fc434-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fc434-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="fc434-133">I sökrutan skriver **Adobe logga**.</span><span class="sxs-lookup"><span data-stu-id="fc434-133">In the search box, type **Adobe Sign**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. <span data-ttu-id="fc434-135">Välj i resultatpanelen **Adobe logga**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="fc434-135">In the results panel, select **Adobe Sign**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fc434-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fc434-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fc434-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Adobe baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="fc434-138">In this section, you configure and test Azure AD single sign-on with Adobe Sign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="fc434-139">Azure AD måste du känna till motsvarande användaren i Adobe loggar till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="fc434-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Adobe Sign is to a user in Azure AD.</span></span> <span data-ttu-id="fc434-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Adobe logga upprättas.</span><span class="sxs-lookup"><span data-stu-id="fc434-140">In other words, a link relationship between an Azure AD user and the related user in Adobe Sign needs to be established.</span></span>

<span data-ttu-id="fc434-141">I Adobe signera, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="fc434-141">In Adobe Sign, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fc434-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Adobe, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="fc434-142">To configure and test Azure AD single sign-on with Adobe Sign, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fc434-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fc434-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fc434-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fc434-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fc434-145">**[Skapa en testanvändare Adobe logga](#creating-an-adobe-sign-test-user)**  – du har en motsvarighet för Britta Simon Adobe tecken som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="fc434-145">**[Creating an Adobe Sign test user](#creating-an-adobe-sign-test-user)** - to have a counterpart of Britta Simon in Adobe Sign that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fc434-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fc434-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fc434-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fc434-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fc434-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fc434-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fc434-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Adobe signera programmet.</span><span class="sxs-lookup"><span data-stu-id="fc434-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Adobe Sign application.</span></span>

<span data-ttu-id="fc434-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Adobe:**</span><span class="sxs-lookup"><span data-stu-id="fc434-150">**To configure Azure AD single sign-on with Adobe Sign, perform the following steps:**</span></span>

1. <span data-ttu-id="fc434-151">I Azure-portalen på den **Adobe logga** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="fc434-151">In the Azure portal, on the **Adobe Sign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="fc434-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fc434-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. <span data-ttu-id="fc434-155">På den **Adobe logga domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fc434-155">On the **Adobe Sign Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    <span data-ttu-id="fc434-157">a.</span><span class="sxs-lookup"><span data-stu-id="fc434-157">a.</span></span> <span data-ttu-id="fc434-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.echosign.com/`</span><span class="sxs-lookup"><span data-stu-id="fc434-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.echosign.com/`</span></span>

    <span data-ttu-id="fc434-159">b.</span><span class="sxs-lookup"><span data-stu-id="fc434-159">b.</span></span> <span data-ttu-id="fc434-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.echosign.com`</span><span class="sxs-lookup"><span data-stu-id="fc434-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.echosign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fc434-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="fc434-161">These values are not real.</span></span> <span data-ttu-id="fc434-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="fc434-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="fc434-163">Kontakta [Adobe logga klienten supportteamet](https://helpx.adobe.com/in/contact/support.html) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="fc434-163">Contact [Adobe Sign Client support team](https://helpx.adobe.com/in/contact/support.html) to get these values.</span></span> 
 
4. <span data-ttu-id="fc434-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="fc434-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. <span data-ttu-id="fc434-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fc434-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fc434-168">På den **Adobe logga Configuration** klickar du på **Konfigurera Adobe logga** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="fc434-168">On the **Adobe Sign Configuration** section, click **Configure Adobe Sign** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fc434-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="fc434-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 


7. <span data-ttu-id="fc434-171">I en annan webbläsarfönster loggar du in på webbplatsen Adobe logga företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="fc434-171">In a different web browser window, log in to your Adobe Sign company site as an administrator.</span></span>

8. <span data-ttu-id="fc434-172">Klicka på menyn högst upp **konto**, och klickar sedan på i navigeringsfönstret till vänster **SAML inställningar** under **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="fc434-172">In the menu on the top, click **Account**, and then, in the navigation pane on the left side, click **SAML Settings** under **Account Settings**.</span></span>
   
   <span data-ttu-id="fc434-173">![Kontot](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "konto")</span><span class="sxs-lookup"><span data-stu-id="fc434-173">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "Account")</span></span>

9. <span data-ttu-id="fc434-174">Utför följande steg i avsnittet SAML-inställningar:</span><span class="sxs-lookup"><span data-stu-id="fc434-174">In the SAML Settings section, perform the following steps:</span></span>
   
   <span data-ttu-id="fc434-175">![Inställningar för SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="fc434-175">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML Settings")</span></span>
   
   <span data-ttu-id="fc434-176">a.</span><span class="sxs-lookup"><span data-stu-id="fc434-176">a.</span></span> <span data-ttu-id="fc434-177">Som **SAML läge**väljer **SAML obligatoriska**.</span><span class="sxs-lookup"><span data-stu-id="fc434-177">As **SAML Mode**, select **SAML Mandatory**.</span></span>
   
   <span data-ttu-id="fc434-178">b.</span><span class="sxs-lookup"><span data-stu-id="fc434-178">b.</span></span> <span data-ttu-id="fc434-179">Välj **Tillåt EchoSign Kontoadministratörer kan logga in med hjälp av autentiseringsuppgifterna EchoSign**.</span><span class="sxs-lookup"><span data-stu-id="fc434-179">Select **Allow EchoSign Account Administrators to log in using their EchoSign Credentials**.</span></span>
   
   <span data-ttu-id="fc434-180">c.</span><span class="sxs-lookup"><span data-stu-id="fc434-180">c.</span></span> <span data-ttu-id="fc434-181">Som **Användarskapelse**väljer **Lägg automatiskt till användare som autentiseras via SAML**.</span><span class="sxs-lookup"><span data-stu-id="fc434-181">As **User Creation**, select **Automatically add users authenticated through SAML**.</span></span>

10. <span data-ttu-id="fc434-182">Flytta, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fc434-182">Move on, performing the following steps:</span></span>

       <span data-ttu-id="fc434-183">![Inställningar för SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="fc434-183">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML Settings")</span></span>

    <span data-ttu-id="fc434-184">a.</span><span class="sxs-lookup"><span data-stu-id="fc434-184">a.</span></span> <span data-ttu-id="fc434-185">Klistra in **SAML enhets-ID**, som du har kopierat från Azure-portalen i den **IdP enhets-ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="fc434-185">Paste **SAML Entity ID**, which you have copied from Azure portal into the **IdP Entity ID** textbox.</span></span>
    
    <span data-ttu-id="fc434-186">b.</span><span class="sxs-lookup"><span data-stu-id="fc434-186">b.</span></span> <span data-ttu-id="fc434-187">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från Azure-portalen i den **IdP inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="fc434-187">Paste **SAML Single Sign-On Service URL**, which you have copied from Azure portal into the **IdP Login URL** textbox.</span></span>
   
    <span data-ttu-id="fc434-188">c.</span><span class="sxs-lookup"><span data-stu-id="fc434-188">c.</span></span> <span data-ttu-id="fc434-189">Klistra in **Sign-Out URL**, som du har kopierat från Azure-portalen i den **IdP logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="fc434-189">Paste **Sign-Out URL**, which you have copied from Azure portal into the **IdP Logout URL** textbox.</span></span>

    <span data-ttu-id="fc434-190">d.</span><span class="sxs-lookup"><span data-stu-id="fc434-190">d.</span></span> <span data-ttu-id="fc434-191">Öppna din hämtade **Certificate(Base64)** fil i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **IdP certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="fc434-191">Open your downloaded **Certificate(Base64)** file in notepad, copy the content of it into your clipboard, and then paste it to the **IdP Certificate** textbox</span></span>

    <span data-ttu-id="fc434-192">e.</span><span class="sxs-lookup"><span data-stu-id="fc434-192">e.</span></span> <span data-ttu-id="fc434-193">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="fc434-193">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="fc434-194">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="fc434-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fc434-195">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="fc434-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fc434-196">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fc434-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fc434-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="fc434-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="fc434-198">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fc434-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="fc434-200">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="fc434-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fc434-201">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fc434-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fc434-203">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="fc434-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fc434-205">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fc434-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fc434-207">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fc434-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fc434-209">a.</span><span class="sxs-lookup"><span data-stu-id="fc434-209">a.</span></span> <span data-ttu-id="fc434-210">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fc434-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fc434-211">b.</span><span class="sxs-lookup"><span data-stu-id="fc434-211">b.</span></span> <span data-ttu-id="fc434-212">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fc434-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fc434-213">c.</span><span class="sxs-lookup"><span data-stu-id="fc434-213">c.</span></span> <span data-ttu-id="fc434-214">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="fc434-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fc434-215">d.</span><span class="sxs-lookup"><span data-stu-id="fc434-215">d.</span></span> <span data-ttu-id="fc434-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fc434-216">Click **Create**.</span></span>
 
### <a name="creating-an-adobe-sign-test-user"></a><span data-ttu-id="fc434-217">Skapa en Adobe logga testanvändare</span><span class="sxs-lookup"><span data-stu-id="fc434-217">Creating an Adobe Sign test user</span></span>

<span data-ttu-id="fc434-218">Om du vill aktivera Azure AD-användare kan logga in till Adobe inloggning, måste de etableras i Adobe logga.</span><span class="sxs-lookup"><span data-stu-id="fc434-218">To enable Azure AD users to log in to Adobe Sign, they must be provisioned into Adobe Sign.</span></span> <span data-ttu-id="fc434-219">Adobe logga är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="fc434-219">In the case of Adobe Sign, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="fc434-220">Du kan använda något annat Adobe logga användarens konto skapas verktyg eller API: er som tillhandahålls av Adobe logga etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="fc434-220">You can use any other Adobe Sign user account creation tools or APIs provided by Adobe Sign to provision AAD user accounts.</span></span> 

<span data-ttu-id="fc434-221">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="fc434-221">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="fc434-222">Logga in på ditt **Adobe logga** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="fc434-222">Log in to your **Adobe Sign** company site as administrator.</span></span>

2. <span data-ttu-id="fc434-223">Klicka på menyn högst upp **konto**, och klickar sedan på i navigeringsfönstret till vänster **användare och grupper**, och klicka sedan på **skapa en ny användare**.</span><span class="sxs-lookup"><span data-stu-id="fc434-223">In the menu on the top, click **Account**, and then, in the navigation pane on the left side, click **Users & Groups**, and then, click **Create a new user**.</span></span>
   
   <span data-ttu-id="fc434-224">![Kontot](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "konto")</span><span class="sxs-lookup"><span data-stu-id="fc434-224">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "Account")</span></span>
   
3. <span data-ttu-id="fc434-225">I den **skapa nya användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="fc434-225">In the **Create New User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="fc434-226">![Skapa användare](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="fc434-226">![Create User](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "Create User")</span></span>
   
   <span data-ttu-id="fc434-227">a.</span><span class="sxs-lookup"><span data-stu-id="fc434-227">a.</span></span> <span data-ttu-id="fc434-228">Typ av **e-postadress**, **Förnamn**, och **efternamn** av en giltig AAD-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="fc434-228">Type the **Email Address**, **First Name**, and **Last Name** of a valid AAD account you want to provision into the related textboxes.</span></span>
   
   <span data-ttu-id="fc434-229">b.</span><span class="sxs-lookup"><span data-stu-id="fc434-229">b.</span></span> <span data-ttu-id="fc434-230">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="fc434-230">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="fc434-231">Azure Active Directory kontoinnehavaren får ett e-postmeddelande som innehåller en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="fc434-231">The Azure Active Directory account holder receives an email that includes a link to confirm the account before it becomes active.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fc434-232">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="fc434-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fc434-233">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Adobe logga.</span><span class="sxs-lookup"><span data-stu-id="fc434-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Adobe Sign.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="fc434-235">**Om du vill tilldela Adobe logga Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fc434-235">**To assign Britta Simon to Adobe Sign, perform the following steps:**</span></span>

1. <span data-ttu-id="fc434-236">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fc434-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="fc434-238">Välj i listan med program **Adobe logga**.</span><span class="sxs-lookup"><span data-stu-id="fc434-238">In the applications list, select **Adobe Sign**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. <span data-ttu-id="fc434-240">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="fc434-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="fc434-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="fc434-242">Click **Add** button.</span></span> <span data-ttu-id="fc434-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fc434-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="fc434-245">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="fc434-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fc434-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fc434-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fc434-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fc434-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fc434-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fc434-248">Testing single sign-on</span></span>

<span data-ttu-id="fc434-249">När du klickar på panelen Adobe logga på åtkomstpanelen du bör få automatiskt loggat in på Adobe signera programmet.</span><span class="sxs-lookup"><span data-stu-id="fc434-249">When you click the Adobe Sign tile in the Access Panel, you should get automatically signed-on to your Adobe Sign application.</span></span>
<span data-ttu-id="fc434-250">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fc434-250">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fc434-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="fc434-251">Additional resources</span></span>

* [<span data-ttu-id="fc434-252">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fc434-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fc434-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fc434-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

