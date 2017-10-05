---
title: "Självstudier: Azure Active Directory-integrering med Boomi | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Boomi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 1121d22beddf73fd2109a4b410422f76dd37478e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a><span data-ttu-id="951fe-103">Självstudier: Azure Active Directory-integrering med Boomi</span><span class="sxs-lookup"><span data-stu-id="951fe-103">Tutorial: Azure Active Directory integration with Boomi</span></span>

<span data-ttu-id="951fe-104">I kursen får lära du att integrera Boomi med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="951fe-104">In this tutorial, you learn how to integrate Boomi with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="951fe-105">Integrera Boomi med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="951fe-105">Integrating Boomi with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="951fe-106">Du kan styra i Azure AD som har åtkomst till Boomi</span><span class="sxs-lookup"><span data-stu-id="951fe-106">You can control in Azure AD who has access to Boomi</span></span>
- <span data-ttu-id="951fe-107">Du kan aktivera användarna att automatiskt hämta loggat in på Boomi (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="951fe-107">You can enable your users to automatically get signed-on to Boomi (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="951fe-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="951fe-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="951fe-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="951fe-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="951fe-110">Krav</span><span class="sxs-lookup"><span data-stu-id="951fe-110">Prerequisites</span></span>

<span data-ttu-id="951fe-111">För att konfigurera Azure AD-integrering med Boomi, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="951fe-111">To configure Azure AD integration with Boomi, you need the following items:</span></span>

- <span data-ttu-id="951fe-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="951fe-112">An Azure AD subscription</span></span>
- <span data-ttu-id="951fe-113">En Boomi enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="951fe-113">A Boomi single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="951fe-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="951fe-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="951fe-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="951fe-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="951fe-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="951fe-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="951fe-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="951fe-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="951fe-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="951fe-118">Scenario description</span></span>
<span data-ttu-id="951fe-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="951fe-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="951fe-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="951fe-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="951fe-121">Att lägga till Boomi från galleriet</span><span class="sxs-lookup"><span data-stu-id="951fe-121">Adding Boomi from the gallery</span></span>
2. <span data-ttu-id="951fe-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="951fe-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-boomi-from-the-gallery"></a><span data-ttu-id="951fe-123">Att lägga till Boomi från galleriet</span><span class="sxs-lookup"><span data-stu-id="951fe-123">Adding Boomi from the gallery</span></span>
<span data-ttu-id="951fe-124">Du måste lägga till Boomi från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Boomi i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="951fe-124">To configure the integration of Boomi into Azure AD, you need to add Boomi from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="951fe-125">**Utför följande steg för att lägga till Boomi från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="951fe-125">**To add Boomi from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="951fe-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="951fe-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="951fe-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="951fe-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="951fe-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="951fe-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="951fe-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="951fe-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="951fe-133">I sökrutan skriver **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="951fe-133">In the search box, type **Boomi**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. <span data-ttu-id="951fe-135">Välj i resultatpanelen **Boomi**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="951fe-135">In the results panel, select **Boomi**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="951fe-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="951fe-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="951fe-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Boomi baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="951fe-138">In this section, you configure and test Azure AD single sign-on with Boomi based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="951fe-139">Azure AD måste du känna till användaren i Boomi motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="951fe-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Boomi is to a user in Azure AD.</span></span> <span data-ttu-id="951fe-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Boomi upprättas.</span><span class="sxs-lookup"><span data-stu-id="951fe-140">In other words, a link relationship between an Azure AD user and the related user in Boomi needs to be established.</span></span>

<span data-ttu-id="951fe-141">I Boomi, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="951fe-141">In Boomi, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="951fe-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Boomi, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="951fe-142">To configure and test Azure AD single sign-on with Boomi, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="951fe-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="951fe-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="951fe-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="951fe-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="951fe-145">**[Skapa en testanvändare Boomi](#creating-a-boomi-test-user)**  – du har en motsvarighet för Britta Simon i Boomi som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="951fe-145">**[Creating a Boomi test user](#creating-a-boomi-test-user)** - to have a counterpart of Britta Simon in Boomi that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="951fe-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="951fe-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="951fe-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="951fe-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="951fe-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="951fe-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="951fe-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Boomi program.</span><span class="sxs-lookup"><span data-stu-id="951fe-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Boomi application.</span></span>

<span data-ttu-id="951fe-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Boomi:**</span><span class="sxs-lookup"><span data-stu-id="951fe-150">**To configure Azure AD single sign-on with Boomi, perform the following steps:**</span></span>

1. <span data-ttu-id="951fe-151">I Azure-portalen på den **Boomi** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="951fe-151">In the Azure portal, on the **Boomi** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="951fe-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="951fe-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. <span data-ttu-id="951fe-155">På den **Boomi domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="951fe-155">On the **Boomi Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    <span data-ttu-id="951fe-157">a.</span><span class="sxs-lookup"><span data-stu-id="951fe-157">a.</span></span> <span data-ttu-id="951fe-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="951fe-158">In the **Identifier** textbox, type a URL using the following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    <span data-ttu-id="951fe-159">b.</span><span class="sxs-lookup"><span data-stu-id="951fe-159">b.</span></span> <span data-ttu-id="951fe-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="951fe-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="951fe-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="951fe-161">These values are not real.</span></span> <span data-ttu-id="951fe-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="951fe-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="951fe-163">Kontakta [Boomi supportteamet](https://boomi.com/company/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="951fe-163">Contact [Boomi support team](https://boomi.com/company/contact/) to get these values.</span></span>

4. <span data-ttu-id="951fe-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="951fe-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. <span data-ttu-id="951fe-166">Boomi program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="951fe-166">Boomi application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="951fe-167">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="951fe-167">Please configure the following claims for this application.</span></span> <span data-ttu-id="951fe-168">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="951fe-168">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="951fe-169">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="951fe-169">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="951fe-171">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan för varje rad som visas i tabellen nedan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="951fe-171">In the **User Attributes** section on the **Single sign-on** dialog, for each row shown in the table below, perform the following steps:</span></span>

    | <span data-ttu-id="951fe-172">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="951fe-172">Attribute Name</span></span> | <span data-ttu-id="951fe-173">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="951fe-173">Attribute Value</span></span> |
    | -------------- | --------------- |
    | <span data-ttu-id="951fe-174">FEDERATION_ID</span><span class="sxs-lookup"><span data-stu-id="951fe-174">FEDERATION_ID</span></span> | <span data-ttu-id="951fe-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="951fe-175">user.mail</span></span> |
    
    <span data-ttu-id="951fe-176">a.</span><span class="sxs-lookup"><span data-stu-id="951fe-176">a.</span></span> <span data-ttu-id="951fe-177">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="951fe-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="951fe-180">b.</span><span class="sxs-lookup"><span data-stu-id="951fe-180">b.</span></span> <span data-ttu-id="951fe-181">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="951fe-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="951fe-182">c.</span><span class="sxs-lookup"><span data-stu-id="951fe-182">c.</span></span> <span data-ttu-id="951fe-183">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="951fe-183">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="951fe-184">d.</span><span class="sxs-lookup"><span data-stu-id="951fe-184">d.</span></span> <span data-ttu-id="951fe-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="951fe-185">Click **Ok**.</span></span>

6. <span data-ttu-id="951fe-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="951fe-186">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="951fe-188">På den **Boomi Configuration** klickar du på **konfigurera Boomi** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="951fe-188">On the **Boomi Configuration** section, click **Configure Boomi** to open **Configure sign-on** window.</span></span> <span data-ttu-id="951fe-189">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="951fe-189">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. <span data-ttu-id="951fe-191">Logga in på webbplatsen Boomi företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="951fe-191">In a different web browser window, log into your Boomi company site as an administrator.</span></span> 

9. <span data-ttu-id="951fe-192">Gå till **företagsnamn** och gå till **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="951fe-192">Navigate to **Company Name** and go to **Set up**.</span></span>

10. <span data-ttu-id="951fe-193">Klicka på den **SSO-alternativ** fliken och utföra nedanstående steg.</span><span class="sxs-lookup"><span data-stu-id="951fe-193">Click the **SSO Options** tab and perform below steps.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    <span data-ttu-id="951fe-195">a.</span><span class="sxs-lookup"><span data-stu-id="951fe-195">a.</span></span> <span data-ttu-id="951fe-196">Kontrollera **aktivera SAML enkel inloggning** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="951fe-196">Check **Enable SAML Single Sign-On** checkbox.</span></span>

    <span data-ttu-id="951fe-197">b.</span><span class="sxs-lookup"><span data-stu-id="951fe-197">b.</span></span> <span data-ttu-id="951fe-198">Klicka på **importera** hämtade certifikatet från Azure AD för att överföra **providern identitetscertifikat**.</span><span class="sxs-lookup"><span data-stu-id="951fe-198">Click **Import** to upload the downloaded certificate from Azure AD to **Identity Provider Certificate**.</span></span>
    
    <span data-ttu-id="951fe-199">c.</span><span class="sxs-lookup"><span data-stu-id="951fe-199">c.</span></span> <span data-ttu-id="951fe-200">I den **identitet providern inloggnings-URL** textruta, ange värdet för **SAML inloggning tjänst-URL för enkel** från Azure AD-konfigurationsfönstret.</span><span class="sxs-lookup"><span data-stu-id="951fe-200">In the **Identity Provider Login URL** textbox, put the value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="951fe-201">d.</span><span class="sxs-lookup"><span data-stu-id="951fe-201">d.</span></span> <span data-ttu-id="951fe-202">Som **Federation Id plats**väljer **Federation-Id är i FEDERATION_ID attributelementet** knappen.</span><span class="sxs-lookup"><span data-stu-id="951fe-202">As **Federation Id Location**, select **Federation Id is in FEDERATION_ID Attribute element** radio button.</span></span> 

    <span data-ttu-id="951fe-203">e.</span><span class="sxs-lookup"><span data-stu-id="951fe-203">e.</span></span> <span data-ttu-id="951fe-204">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="951fe-204">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="951fe-205">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="951fe-205">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="951fe-206">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="951fe-206">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="951fe-207">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="951fe-207">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="951fe-208">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="951fe-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="951fe-209">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="951fe-209">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="951fe-211">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="951fe-211">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="951fe-212">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="951fe-212">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="951fe-214">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="951fe-214">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="951fe-216">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="951fe-216">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="951fe-218">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="951fe-218">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="951fe-220">a.</span><span class="sxs-lookup"><span data-stu-id="951fe-220">a.</span></span> <span data-ttu-id="951fe-221">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="951fe-221">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="951fe-222">b.</span><span class="sxs-lookup"><span data-stu-id="951fe-222">b.</span></span> <span data-ttu-id="951fe-223">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="951fe-223">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="951fe-224">c.</span><span class="sxs-lookup"><span data-stu-id="951fe-224">c.</span></span> <span data-ttu-id="951fe-225">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="951fe-225">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="951fe-226">d.</span><span class="sxs-lookup"><span data-stu-id="951fe-226">d.</span></span> <span data-ttu-id="951fe-227">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="951fe-227">Click **Create**.</span></span>
 
### <a name="creating-a-boomi-test-user"></a><span data-ttu-id="951fe-228">Skapa en testanvändare Boomi</span><span class="sxs-lookup"><span data-stu-id="951fe-228">Creating a Boomi test user</span></span>

<span data-ttu-id="951fe-229">För att aktivera Azure AD-användare kan logga in på Boomi etableras de i Boomi.</span><span class="sxs-lookup"><span data-stu-id="951fe-229">In order to enable Azure AD users to log in to Boomi, they must be provisioned into Boomi.</span></span> <span data-ttu-id="951fe-230">När det gäller Boomi är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="951fe-230">In the case of Boomi, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="951fe-231">Utför följande steg om du vill konfigurera ett användarkonto:</span><span class="sxs-lookup"><span data-stu-id="951fe-231">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="951fe-232">Logga in på webbplatsen Boomi företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="951fe-232">Log in to your Boomi company site as an administrator.</span></span>

2. <span data-ttu-id="951fe-233">När du loggar in, gå till **Användarhantering** och gå till **användare**.</span><span class="sxs-lookup"><span data-stu-id="951fe-233">After logging in, navigate to **User Management** and go to **Users**.</span></span>

    <span data-ttu-id="951fe-234">![Användare](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "användare")</span><span class="sxs-lookup"><span data-stu-id="951fe-234">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "Users")</span></span>

3. <span data-ttu-id="951fe-235">Klicka på  **+**  ikon och **Lägg till/Underhåll användarroller** öppnas.</span><span class="sxs-lookup"><span data-stu-id="951fe-235">Click **+**  icon and the **Add/Maintain User Roles** dialog opens.</span></span>

    <span data-ttu-id="951fe-236">![Användare](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "användare")</span><span class="sxs-lookup"><span data-stu-id="951fe-236">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "Users")</span></span>

    <span data-ttu-id="951fe-237">![Användare](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "användare")</span><span class="sxs-lookup"><span data-stu-id="951fe-237">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "Users")</span></span>

    <span data-ttu-id="951fe-238">a.</span><span class="sxs-lookup"><span data-stu-id="951fe-238">a.</span></span> <span data-ttu-id="951fe-239">I den **användarens e-postadress** textruta, ange den e-posten för användare som BrittaSimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="951fe-239">In the **User e-mail address** textbox, type the email of user like BrittaSimon@contoso.com.</span></span>
    
    <span data-ttu-id="951fe-240">b.</span><span class="sxs-lookup"><span data-stu-id="951fe-240">b.</span></span> <span data-ttu-id="951fe-241">I den **Förnamn** textruta, ange först namnet på användaren som Britta.</span><span class="sxs-lookup"><span data-stu-id="951fe-241">In the **First name** textbox, type the First name of user like Britta.</span></span>

    <span data-ttu-id="951fe-242">c.</span><span class="sxs-lookup"><span data-stu-id="951fe-242">c.</span></span> <span data-ttu-id="951fe-243">I den **efternamn** textruta anger efternamn för användaren som Simon.</span><span class="sxs-lookup"><span data-stu-id="951fe-243">In the **Last name** textbox, type the Last name of user like Simon.</span></span>
    
    <span data-ttu-id="951fe-244">d.</span><span class="sxs-lookup"><span data-stu-id="951fe-244">d.</span></span> <span data-ttu-id="951fe-245">Ange användarens **Federation ID**.</span><span class="sxs-lookup"><span data-stu-id="951fe-245">Enter the user's **Federation ID**.</span></span> <span data-ttu-id="951fe-246">Varje användare måste ha ett ID för Federation som unikt identifierar användaren i kontot.</span><span class="sxs-lookup"><span data-stu-id="951fe-246">Each user must have a Federation ID that uniquely identifies the user within the account.</span></span>
    
    <span data-ttu-id="951fe-247">e.</span><span class="sxs-lookup"><span data-stu-id="951fe-247">e.</span></span> <span data-ttu-id="951fe-248">Tilldela den **standardanvändare** du användaren rollen.</span><span class="sxs-lookup"><span data-stu-id="951fe-248">Assign the **Standard User** role to the user.</span></span> <span data-ttu-id="951fe-249">Tilldela inte en administratörsroll eftersom som skulle ge honom normal luften åtkomst samt åtkomst för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="951fe-249">Do not assign the Administrator role because that would give him normal Atmosphere access as well as single sign-on access.</span></span>
    
    <span data-ttu-id="951fe-250">f.</span><span class="sxs-lookup"><span data-stu-id="951fe-250">f.</span></span> <span data-ttu-id="951fe-251">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="951fe-251">Click **OK**.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="951fe-252">Användaren får inte en Välkommen e-postmeddelandet som innehåller ett lösenord som kan användas för att logga in på kontot AtomSphere eftersom sitt lösenord hanteras via identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="951fe-252">The user will not receive a welcome notification email containing a password that can be used to log in to the AtomSphere account because his password is managed through the identity provider.</span></span> <span data-ttu-id="951fe-253">Du kan använda andra Boomi användarens konto skapas verktyg eller API: er som tillhandahålls av Boomi att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="951fe-253">You may use any other Boomi user account creation tools or APIs provided by Boomi to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="951fe-254">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="951fe-254">Assigning the Azure AD test user</span></span>

<span data-ttu-id="951fe-255">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Boomi.</span><span class="sxs-lookup"><span data-stu-id="951fe-255">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Boomi.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="951fe-257">**Om du vill tilldela Boomi Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="951fe-257">**To assign Britta Simon to Boomi, perform the following steps:**</span></span>

1. <span data-ttu-id="951fe-258">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="951fe-258">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="951fe-260">Välj i listan med program **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="951fe-260">In the applications list, select **Boomi**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. <span data-ttu-id="951fe-262">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="951fe-262">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="951fe-264">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="951fe-264">Click **Add** button.</span></span> <span data-ttu-id="951fe-265">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="951fe-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="951fe-267">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="951fe-267">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="951fe-268">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="951fe-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="951fe-269">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="951fe-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="951fe-270">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="951fe-270">Testing single sign-on</span></span>

<span data-ttu-id="951fe-271">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="951fe-271">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="951fe-272">När du klickar på panelen Boomi på åtkomstpanelen du bör få automatiskt loggat in på ditt Boomi program.</span><span class="sxs-lookup"><span data-stu-id="951fe-272">When you click the Boomi tile in the Access Panel, you should get automatically signed-on to your Boomi application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="951fe-273">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="951fe-273">Additional resources</span></span>

* [<span data-ttu-id="951fe-274">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="951fe-274">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="951fe-275">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="951fe-275">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

