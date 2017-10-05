---
title: "Självstudier: Azure Active Directory-integrering med Sprinklr | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Sprinklr."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 6e1622cd55e3b0e8063604ac9dc0cb0673fa9753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a><span data-ttu-id="d8482-103">Självstudier: Azure Active Directory-integrering med Sprinklr</span><span class="sxs-lookup"><span data-stu-id="d8482-103">Tutorial: Azure Active Directory integration with Sprinklr</span></span>

<span data-ttu-id="d8482-104">I kursen får lära du att integrera Sprinklr med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d8482-104">In this tutorial, you learn how to integrate Sprinklr with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d8482-105">Integrera Sprinklr med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d8482-105">Integrating Sprinklr with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d8482-106">Du kan styra i Azure AD som har åtkomst till Sprinklr</span><span class="sxs-lookup"><span data-stu-id="d8482-106">You can control in Azure AD who has access to Sprinklr</span></span>
- <span data-ttu-id="d8482-107">Du kan aktivera användarna att automatiskt hämta loggat in på Sprinklr (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d8482-107">You can enable your users to automatically get signed-on to Sprinklr (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d8482-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d8482-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d8482-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d8482-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8482-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d8482-110">Prerequisites</span></span>

<span data-ttu-id="d8482-111">För att konfigurera Azure AD-integrering med Sprinklr, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d8482-111">To configure Azure AD integration with Sprinklr, you need the following items:</span></span>

- <span data-ttu-id="d8482-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d8482-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d8482-113">En Sprinklr enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="d8482-113">A Sprinklr single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d8482-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d8482-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d8482-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d8482-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d8482-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d8482-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d8482-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d8482-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d8482-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d8482-118">Scenario description</span></span>
<span data-ttu-id="d8482-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d8482-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d8482-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d8482-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d8482-121">Att lägga till Sprinklr från galleriet</span><span class="sxs-lookup"><span data-stu-id="d8482-121">Adding Sprinklr from the gallery</span></span>
2. <span data-ttu-id="d8482-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8482-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sprinklr-from-the-gallery"></a><span data-ttu-id="d8482-123">Att lägga till Sprinklr från galleriet</span><span class="sxs-lookup"><span data-stu-id="d8482-123">Adding Sprinklr from the gallery</span></span>
<span data-ttu-id="d8482-124">Du måste lägga till Sprinklr från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Sprinklr i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8482-124">To configure the integration of Sprinklr into Azure AD, you need to add Sprinklr from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d8482-125">**Utför följande steg för att lägga till Sprinklr från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d8482-125">**To add Sprinklr from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d8482-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d8482-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d8482-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d8482-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d8482-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d8482-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d8482-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8482-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d8482-133">I sökrutan skriver **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="d8482-133">In the search box, type **Sprinklr**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. <span data-ttu-id="d8482-135">Välj i resultatpanelen **Sprinklr**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d8482-135">In the results panel, select **Sprinklr**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d8482-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8482-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d8482-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Sprinklr baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d8482-138">In this section, you configure and test Azure AD single sign-on with Sprinklr based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d8482-139">Azure AD måste du känna till användaren i Sprinklr motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d8482-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Sprinklr is to a user in Azure AD.</span></span> <span data-ttu-id="d8482-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Sprinklr upprättas.</span><span class="sxs-lookup"><span data-stu-id="d8482-140">In other words, a link relationship between an Azure AD user and the related user in Sprinklr needs to be established.</span></span>

<span data-ttu-id="d8482-141">I Sprinklr, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d8482-141">In Sprinklr, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d8482-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Sprinklr, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d8482-142">To configure and test Azure AD single sign-on with Sprinklr, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d8482-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d8482-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d8482-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8482-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d8482-145">**[Skapa en testanvändare Sprinklr](#creating-a-sprinklr-test-user)**  – du har en motsvarighet för Britta Simon i Sprinklr som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d8482-145">**[Creating a Sprinklr test user](#creating-a-sprinklr-test-user)** - to have a counterpart of Britta Simon in Sprinklr that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d8482-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d8482-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d8482-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d8482-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d8482-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8482-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d8482-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Sprinklr program.</span><span class="sxs-lookup"><span data-stu-id="d8482-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sprinklr application.</span></span>

<span data-ttu-id="d8482-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Sprinklr:**</span><span class="sxs-lookup"><span data-stu-id="d8482-150">**To configure Azure AD single sign-on with Sprinklr, perform the following steps:**</span></span>

1. <span data-ttu-id="d8482-151">I Azure-portalen på den **Sprinklr** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d8482-151">In the Azure portal, on the **Sprinklr** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d8482-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d8482-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. <span data-ttu-id="d8482-155">På den **Sprinklr domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8482-155">On the **Sprinklr Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    <span data-ttu-id="d8482-157">a.</span><span class="sxs-lookup"><span data-stu-id="d8482-157">a.</span></span> <span data-ttu-id="d8482-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="d8482-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    <span data-ttu-id="d8482-159">b.</span><span class="sxs-lookup"><span data-stu-id="d8482-159">b.</span></span> <span data-ttu-id="d8482-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="d8482-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d8482-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d8482-161">These values are not real.</span></span> <span data-ttu-id="d8482-162">Uppdatera värdet med det faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="d8482-162">Update the value with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d8482-163">Kontakta [Sprinklr klienten supportteamet](https://www.sprinklr.com/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d8482-163">Contact [Sprinklr Client support team](https://www.sprinklr.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="d8482-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d8482-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. <span data-ttu-id="d8482-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d8482-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d8482-168">På den **Sprinklr Configuration** klickar du på **konfigurera Sprinklr** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d8482-168">On the **Sprinklr Configuration** section, click **Configure Sprinklr** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d8482-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d8482-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

7. <span data-ttu-id="d8482-170">I en annan webbläsarfönster loggar du in på webbplatsen Sprinklr företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="d8482-170">In a different web browser window, log in to your Sprinklr company site as an administrator.</span></span>

8. <span data-ttu-id="d8482-171">Gå till **Administration \> inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d8482-171">Go to **Administration \> Settings**.</span></span>
   
    <span data-ttu-id="d8482-172">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="d8482-172">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

9. <span data-ttu-id="d8482-173">Gå till **hantera Partner \> för enkel inloggning** på i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="d8482-173">Go to **Manage Partner \> Single Sign** on from the left pane.</span></span>
   
    <span data-ttu-id="d8482-174">![Hantera Partner](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "hantera Partner")</span><span class="sxs-lookup"><span data-stu-id="d8482-174">![Manage Partner](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Manage Partner")</span></span>

10. <span data-ttu-id="d8482-175">Klicka på **+ Lägg till en enda inloggningar**.</span><span class="sxs-lookup"><span data-stu-id="d8482-175">Click **+Add Single Sign Ons**.</span></span>
   
    <span data-ttu-id="d8482-176">![Enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="d8482-176">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "Single Sign-Ons")</span></span>

11. <span data-ttu-id="d8482-177">På den **för enkelinloggning** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8482-177">On the **Single Sign on** page, perform the following steps:</span></span>
   
    <span data-ttu-id="d8482-178">![Enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="d8482-178">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "Single Sign-Ons")</span></span>

    <span data-ttu-id="d8482-179">a.</span><span class="sxs-lookup"><span data-stu-id="d8482-179">a.</span></span> <span data-ttu-id="d8482-180">I den **namn** textruta, ange ett namn för konfigurationen (till exempel: *WAADSSOTest*).</span><span class="sxs-lookup"><span data-stu-id="d8482-180">In the **Name** textbox, type a name for your configuration (for example: *WAADSSOTest*).</span></span>

    <span data-ttu-id="d8482-181">b.</span><span class="sxs-lookup"><span data-stu-id="d8482-181">b.</span></span> <span data-ttu-id="d8482-182">Välj **aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="d8482-182">Select **Enabled**.</span></span>

    <span data-ttu-id="d8482-183">c.</span><span class="sxs-lookup"><span data-stu-id="d8482-183">c.</span></span> <span data-ttu-id="d8482-184">Välj **nya SSO certifikatet**.</span><span class="sxs-lookup"><span data-stu-id="d8482-184">Select **Use new SSO Certificate**.</span></span>
             
    <span data-ttu-id="d8482-185">e.</span><span class="sxs-lookup"><span data-stu-id="d8482-185">e.</span></span> <span data-ttu-id="d8482-186">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **providern identitetscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="d8482-186">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="d8482-187">f.</span><span class="sxs-lookup"><span data-stu-id="d8482-187">f.</span></span> <span data-ttu-id="d8482-188">Klistra in den **SAML enhets-ID** värde som du har kopierat från Azure Portal till den **enhets-Id** textruta.</span><span class="sxs-lookup"><span data-stu-id="d8482-188">Paste the **SAML Entity ID** value which you have copied from Azure Portal into the **Entity Id** textbox.</span></span>

    <span data-ttu-id="d8482-189">g.</span><span class="sxs-lookup"><span data-stu-id="d8482-189">g.</span></span> <span data-ttu-id="d8482-190">Klistra in den **SAML enkel inloggning Tjänstwebbadress** värde som du har kopierat från Azure Portal till den **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="d8482-190">Paste the **SAML Single Sign-On Service URL** value which you have copied from Azure Portal into the **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="d8482-191">h.</span><span class="sxs-lookup"><span data-stu-id="d8482-191">h.</span></span> <span data-ttu-id="d8482-192">Klistra in den **Sign-Out URL** värde som du har kopierat från Azure Portal till den **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="d8482-192">Paste the **Sign-Out URL** value which you have copied from Azure Portal into the **Identity Provider Logout URL** textbox.</span></span>
     
    <span data-ttu-id="d8482-193">Jag.</span><span class="sxs-lookup"><span data-stu-id="d8482-193">i.</span></span> <span data-ttu-id="d8482-194">Som **SAML-ID typ**väljer **Assertion innehåller användare ”s sprinklr.com användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="d8482-194">As **SAML User ID Type**, select **Assertion contains User”s sprinklr.com username**.</span></span>

    <span data-ttu-id="d8482-195">j.</span><span class="sxs-lookup"><span data-stu-id="d8482-195">j.</span></span> <span data-ttu-id="d8482-196">Som **SAML Användarplats ID**väljer **användar-ID är i elementet namnidentifierare i instruktionen ämne**.</span><span class="sxs-lookup"><span data-stu-id="d8482-196">As **SAML User ID Location**, select **User ID is in the Name Identifier element of the Subject statement**.</span></span>

    <span data-ttu-id="d8482-197">k.</span><span class="sxs-lookup"><span data-stu-id="d8482-197">k.</span></span> <span data-ttu-id="d8482-198">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d8482-198">Click **Save**.</span></span>
       
    <span data-ttu-id="d8482-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="d8482-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span></span>

> [!TIP]
> <span data-ttu-id="d8482-200">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d8482-200">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d8482-201">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d8482-201">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d8482-202">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d8482-202">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d8482-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8482-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="d8482-204">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8482-204">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d8482-206">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d8482-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d8482-207">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d8482-207">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d8482-209">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d8482-209">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d8482-211">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8482-211">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d8482-213">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8482-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d8482-215">a.</span><span class="sxs-lookup"><span data-stu-id="d8482-215">a.</span></span> <span data-ttu-id="d8482-216">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d8482-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d8482-217">b.</span><span class="sxs-lookup"><span data-stu-id="d8482-217">b.</span></span> <span data-ttu-id="d8482-218">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d8482-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d8482-219">c.</span><span class="sxs-lookup"><span data-stu-id="d8482-219">c.</span></span> <span data-ttu-id="d8482-220">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d8482-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d8482-221">d.</span><span class="sxs-lookup"><span data-stu-id="d8482-221">d.</span></span> <span data-ttu-id="d8482-222">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d8482-222">Click **Create**.</span></span>
 
### <a name="creating-a-sprinklr-test-user"></a><span data-ttu-id="d8482-223">Skapa en testanvändare Sprinklr</span><span class="sxs-lookup"><span data-stu-id="d8482-223">Creating a Sprinklr test user</span></span>

1. <span data-ttu-id="d8482-224">Logga in på webbplatsen Sprinklr företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="d8482-224">Log in to your Sprinklr company site as an administrator.</span></span>

2. <span data-ttu-id="d8482-225">Gå till **Administration \> inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d8482-225">Go to **Administration \> Settings**.</span></span>
   
    <span data-ttu-id="d8482-226">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="d8482-226">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

3. <span data-ttu-id="d8482-227">Gå till **hantera klienten \> användare** i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="d8482-227">Go to **Manage Client \> Users** from the left pane.</span></span>
   
    <span data-ttu-id="d8482-228">![Inställningar för](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="d8482-228">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "Settings")</span></span>

4. <span data-ttu-id="d8482-229">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="d8482-229">Click **Add User**.</span></span>
   
    <span data-ttu-id="d8482-230">![Inställningar för](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="d8482-230">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "Settings")</span></span>

5. <span data-ttu-id="d8482-231">På den **Redigera användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8482-231">On the **Edit user** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="d8482-232">![Redigera användare](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Redigera användare")</span><span class="sxs-lookup"><span data-stu-id="d8482-232">![Edit user](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Edit user")</span></span> 

    <span data-ttu-id="d8482-233">a.</span><span class="sxs-lookup"><span data-stu-id="d8482-233">a.</span></span> <span data-ttu-id="d8482-234">I den **e-post**, **Förnamn** och **efternamn** textrutor, ange information för en Azure AD-användarkonto som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="d8482-234">In the **Email**, **First Name** and **Last Name** textboxes, type the information of an Azure AD user account you want to provision.</span></span>

    <span data-ttu-id="d8482-235">b.</span><span class="sxs-lookup"><span data-stu-id="d8482-235">b.</span></span> <span data-ttu-id="d8482-236">Välj **lösenord inaktiveras**.</span><span class="sxs-lookup"><span data-stu-id="d8482-236">Select **Password Disabled**.</span></span>

    <span data-ttu-id="d8482-237">c.</span><span class="sxs-lookup"><span data-stu-id="d8482-237">c.</span></span> <span data-ttu-id="d8482-238">Välj **språk**.</span><span class="sxs-lookup"><span data-stu-id="d8482-238">Select **Language**.</span></span>

    <span data-ttu-id="d8482-239">d.</span><span class="sxs-lookup"><span data-stu-id="d8482-239">d.</span></span> <span data-ttu-id="d8482-240">Välj **användartyp**.</span><span class="sxs-lookup"><span data-stu-id="d8482-240">Select **User Type**.</span></span>

    <span data-ttu-id="d8482-241">e.</span><span class="sxs-lookup"><span data-stu-id="d8482-241">e.</span></span> <span data-ttu-id="d8482-242">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="d8482-242">Click **Update**.</span></span>
   
     >[!IMPORTANT]
     ><span data-ttu-id="d8482-243">**Lösenord inaktiveras** måste väljas för att aktivera en användare loggar in via en identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="d8482-243">**Password Disabled** must be selected to enable a user to log in via an Identity provider.</span></span> 
     
6. <span data-ttu-id="d8482-244">Gå till **rollen**, och utför sedan följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8482-244">Go to **Role**, and then perform the following steps:</span></span>
   
    <span data-ttu-id="d8482-245">![Samarbeta roller](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner roller")</span><span class="sxs-lookup"><span data-stu-id="d8482-245">![Partner Roles](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner Roles")</span></span>

    <span data-ttu-id="d8482-246">a.</span><span class="sxs-lookup"><span data-stu-id="d8482-246">a.</span></span> <span data-ttu-id="d8482-247">Från den **Global** väljer **alla\_behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="d8482-247">From the **Global** list, select **ALL\_Permissions**.</span></span>  

    <span data-ttu-id="d8482-248">b.</span><span class="sxs-lookup"><span data-stu-id="d8482-248">b.</span></span> <span data-ttu-id="d8482-249">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="d8482-249">Click **Update**.</span></span>

>[!NOTE]
><span data-ttu-id="d8482-250">Du kan använda något annat Sprinklr användarens konto skapas verktyg eller API: er som tillhandahålls av Sprinklr att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="d8482-250">You can use any other Sprinklr user account creation tools or APIs provided by Sprinklr to provision Azure AD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d8482-251">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d8482-251">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d8482-252">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Sprinklr.</span><span class="sxs-lookup"><span data-stu-id="d8482-252">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sprinklr.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d8482-254">**Om du vill tilldela Sprinklr Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8482-254">**To assign Britta Simon to Sprinklr, perform the following steps:**</span></span>

1. <span data-ttu-id="d8482-255">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d8482-255">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d8482-257">Välj i listan med program **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="d8482-257">In the applications list, select **Sprinklr**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. <span data-ttu-id="d8482-259">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d8482-259">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d8482-261">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d8482-261">Click **Add** button.</span></span> <span data-ttu-id="d8482-262">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8482-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d8482-264">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d8482-264">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d8482-265">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8482-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d8482-266">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8482-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d8482-267">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8482-267">Testing single sign-on</span></span>

<span data-ttu-id="d8482-268">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d8482-268">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d8482-269">När du klickar på panelen Sprinklr på åtkomstpanelen du bör få automatiskt loggat in på ditt program Sprinklr för mer information om panelen åtkomst finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d8482-269">When you click the Sprinklr tile in the Access Panel, you should get automatically signed-on to your Sprinklr application For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d8482-270">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d8482-270">Additional resources</span></span>

* [<span data-ttu-id="d8482-271">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d8482-271">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d8482-272">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d8482-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

