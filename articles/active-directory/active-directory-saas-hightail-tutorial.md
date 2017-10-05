---
title: "Självstudier: Azure Active Directory-integrering med Hightail | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Hightail."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: ba55f9b62d274aa3eb91723c62b53f54de0891b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a><span data-ttu-id="74819-103">Självstudier: Azure Active Directory-integrering med Hightail</span><span class="sxs-lookup"><span data-stu-id="74819-103">Tutorial: Azure Active Directory integration with Hightail</span></span>

<span data-ttu-id="74819-104">I kursen får lära du att integrera Hightail med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="74819-104">In this tutorial, you learn how to integrate Hightail with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="74819-105">Integrera Hightail med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="74819-105">Integrating Hightail with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="74819-106">Du kan styra i Azure AD som har åtkomst till Hightail</span><span class="sxs-lookup"><span data-stu-id="74819-106">You can control in Azure AD who has access to Hightail</span></span>
- <span data-ttu-id="74819-107">Du kan aktivera användarna att automatiskt hämta loggat in på Hightail (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="74819-107">You can enable your users to automatically get signed-on to Hightail (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="74819-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="74819-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="74819-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="74819-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74819-110">Krav</span><span class="sxs-lookup"><span data-stu-id="74819-110">Prerequisites</span></span>

<span data-ttu-id="74819-111">För att konfigurera Azure AD-integrering med Hightail, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="74819-111">To configure Azure AD integration with Hightail, you need the following items:</span></span>

- <span data-ttu-id="74819-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="74819-112">An Azure AD subscription</span></span>
- <span data-ttu-id="74819-113">En Hightail enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="74819-113">A Hightail single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="74819-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="74819-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="74819-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="74819-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="74819-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="74819-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="74819-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="74819-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="74819-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="74819-118">Scenario description</span></span>
<span data-ttu-id="74819-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="74819-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="74819-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="74819-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="74819-121">Att lägga till Hightail från galleriet</span><span class="sxs-lookup"><span data-stu-id="74819-121">Adding Hightail from the gallery</span></span>
2. <span data-ttu-id="74819-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74819-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hightail-from-the-gallery"></a><span data-ttu-id="74819-123">Att lägga till Hightail från galleriet</span><span class="sxs-lookup"><span data-stu-id="74819-123">Adding Hightail from the gallery</span></span>
<span data-ttu-id="74819-124">Du måste lägga till Hightail från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Hightail i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="74819-124">To configure the integration of Hightail into Azure AD, you need to add Hightail from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="74819-125">**Utför följande steg för att lägga till Hightail från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="74819-125">**To add Hightail from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="74819-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="74819-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="74819-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="74819-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="74819-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="74819-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="74819-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74819-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="74819-133">I sökrutan skriver **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="74819-133">In the search box, type **Hightail**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. <span data-ttu-id="74819-135">Välj i resultatpanelen **Hightail**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="74819-135">In the results panel, select **Hightail**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="74819-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74819-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="74819-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Hightail baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="74819-138">In this section, you configure and test Azure AD single sign-on with Hightail based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="74819-139">Azure AD måste du känna till användaren i Hightail motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="74819-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Hightail is to a user in Azure AD.</span></span> <span data-ttu-id="74819-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Hightail upprättas.</span><span class="sxs-lookup"><span data-stu-id="74819-140">In other words, a link relationship between an Azure AD user and the related user in Hightail needs to be established.</span></span>

<span data-ttu-id="74819-141">I Hightail, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="74819-141">In Hightail, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="74819-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Hightail, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="74819-142">To configure and test Azure AD single sign-on with Hightail, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="74819-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="74819-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="74819-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74819-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="74819-145">**[Skapa en testanvändare Hightail](#creating-a-hightail-test-user)**  – du har en motsvarighet för Britta Simon i Hightail som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="74819-145">**[Creating a Hightail test user](#creating-a-hightail-test-user)** - to have a counterpart of Britta Simon in Hightail that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="74819-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="74819-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="74819-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="74819-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="74819-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74819-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="74819-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Hightail program.</span><span class="sxs-lookup"><span data-stu-id="74819-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Hightail application.</span></span>

<span data-ttu-id="74819-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Hightail:**</span><span class="sxs-lookup"><span data-stu-id="74819-150">**To configure Azure AD single sign-on with Hightail, perform the following steps:**</span></span>

1. <span data-ttu-id="74819-151">I Azure-portalen på den **Hightail** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="74819-151">In the Azure portal, on the **Hightail** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="74819-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="74819-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. <span data-ttu-id="74819-155">På den **Hightail domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="74819-155">On the **Hightail Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     <span data-ttu-id="74819-157">I den **Reply URL** textruta ange Webbadressen som:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span><span class="sxs-lookup"><span data-stu-id="74819-157">In the **Reply URL** textbox, type the URL as: `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="74819-158">Föregående värde är inte verkliga värde.</span><span class="sxs-lookup"><span data-stu-id="74819-158">The preceding value is not real value.</span></span> <span data-ttu-id="74819-159">Du uppdaterar värdet med faktiska Reply-URL, vilket beskrivs senare i självstudierna.</span><span class="sxs-lookup"><span data-stu-id="74819-159">You will update the value with the actual Reply URL, which is explained later in the tutorial.</span></span>
 
4. <span data-ttu-id="74819-160">På den **Hightail domän och URL: er** om du vill konfigurera programmet i **SP initierade läge**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="74819-160">On the **Hightail Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    <span data-ttu-id="74819-162">a.</span><span class="sxs-lookup"><span data-stu-id="74819-162">a.</span></span> <span data-ttu-id="74819-163">Klicka på den **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="74819-163">Click the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="74819-164">b.</span><span class="sxs-lookup"><span data-stu-id="74819-164">b.</span></span> <span data-ttu-id="74819-165">I den **logga URL** textruta ange Webbadressen som:`https://www.hightail.com/loginSSO`</span><span class="sxs-lookup"><span data-stu-id="74819-165">In the **Sign On URL** textbox, type the URL as: `https://www.hightail.com/loginSSO`</span></span>

4. <span data-ttu-id="74819-166">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="74819-166">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. <span data-ttu-id="74819-168">Hightail program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="74819-168">Hightail application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="74819-169">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="74819-169">Please configure the following claims for this application.</span></span> <span data-ttu-id="74819-170">Du kan hantera värden för attributen från den **”Atrribute”** för programmet.</span><span class="sxs-lookup"><span data-stu-id="74819-170">You can manage the values of these attributes from the **"Atrribute"** tab of the application.</span></span> <span data-ttu-id="74819-171">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="74819-171">The following screenshot shows an example for this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. <span data-ttu-id="74819-173">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="74819-173">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="74819-174">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="74819-174">Attribute Name</span></span> | <span data-ttu-id="74819-175">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="74819-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="74819-176">Förnamn</span><span class="sxs-lookup"><span data-stu-id="74819-176">FirstName</span></span> | <span data-ttu-id="74819-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="74819-177">user.givenname</span></span> |
    | <span data-ttu-id="74819-178">Efternamn</span><span class="sxs-lookup"><span data-stu-id="74819-178">LastName</span></span> | <span data-ttu-id="74819-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="74819-179">user.surname</span></span> |
    | <span data-ttu-id="74819-180">E-post</span><span class="sxs-lookup"><span data-stu-id="74819-180">Email</span></span> | <span data-ttu-id="74819-181">User.Mail</span><span class="sxs-lookup"><span data-stu-id="74819-181">user.mail</span></span> |    
    | <span data-ttu-id="74819-182">Användaridentiteten</span><span class="sxs-lookup"><span data-stu-id="74819-182">UserIdentity</span></span> | <span data-ttu-id="74819-183">User.Mail</span><span class="sxs-lookup"><span data-stu-id="74819-183">user.mail</span></span> |
    
    <span data-ttu-id="74819-184">a.</span><span class="sxs-lookup"><span data-stu-id="74819-184">a.</span></span> <span data-ttu-id="74819-185">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74819-185">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="74819-188">b.</span><span class="sxs-lookup"><span data-stu-id="74819-188">b.</span></span> <span data-ttu-id="74819-189">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="74819-189">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="74819-190">c.</span><span class="sxs-lookup"><span data-stu-id="74819-190">c.</span></span> <span data-ttu-id="74819-191">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="74819-191">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="74819-192">d.</span><span class="sxs-lookup"><span data-stu-id="74819-192">d.</span></span> <span data-ttu-id="74819-193">Lämna den **Namespace** tomt.</span><span class="sxs-lookup"><span data-stu-id="74819-193">Leave the **Namespace** blank.</span></span>
    
    <span data-ttu-id="74819-194">e.</span><span class="sxs-lookup"><span data-stu-id="74819-194">e.</span></span> <span data-ttu-id="74819-195">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="74819-195">Click **Ok**.</span></span>

7. <span data-ttu-id="74819-196">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="74819-196">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="74819-198">På den **Hightail Configuration** klickar du på **konfigurera Hightail** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="74819-198">On the **Hightail Configuration** section, click **Configure Hightail** to open **Configure sign-on** window.</span></span> <span data-ttu-id="74819-199">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="74819-199">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    ><span data-ttu-id="74819-201">Innan du konfigurerar den enkel inloggning på Hightail app kan du lista för tillåten din e-postdomän med Hightail team så att alla användare som använder den här domänen kan använda enkel inloggning funktioner.</span><span class="sxs-lookup"><span data-stu-id="74819-201">Before configuring the Single Sign On at Hightail app, please white list your email domain with Hightail team so that all the users who are using this domain can use Single Sign On functionality.</span></span>


9. <span data-ttu-id="74819-202">För att få SSO konfigurerats för ditt program måste logga in till din Hightail-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="74819-202">To get SSO configured for your application, you need to sign-on to your Hightail tenant as an administrator.</span></span>
   
    <span data-ttu-id="74819-203">a.</span><span class="sxs-lookup"><span data-stu-id="74819-203">a.</span></span> <span data-ttu-id="74819-204">Klicka på menyn högst upp i **konto** fliken och markera **konfigurera SAML**.</span><span class="sxs-lookup"><span data-stu-id="74819-204">In the menu on the top, click the **Account** tab and select **Configure SAML**.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    <span data-ttu-id="74819-206">b.</span><span class="sxs-lookup"><span data-stu-id="74819-206">b.</span></span> <span data-ttu-id="74819-207">Markera kryssrutan för **aktivera SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="74819-207">Select the checkbox of **Enable SAML Authentication**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    <span data-ttu-id="74819-209">c.</span><span class="sxs-lookup"><span data-stu-id="74819-209">c.</span></span> <span data-ttu-id="74819-210">Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen, kopiera innehållet i den till Urklipp och klistra in den till den **SAML Tokensigneringscertifikatet** textruta.</span><span class="sxs-lookup"><span data-stu-id="74819-210">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **SAML Token Signing Certificate** textbox.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    <span data-ttu-id="74819-212">d.</span><span class="sxs-lookup"><span data-stu-id="74819-212">d.</span></span> <span data-ttu-id="74819-213">I den **SAML Authority (identitetsleverantör)** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** kopieras från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="74819-213">In the **SAML Authority (Identity Provider)** textbox, paste the value of **SAML Single Sign-On Service URL** copied from Azure portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    <span data-ttu-id="74819-215">e.</span><span class="sxs-lookup"><span data-stu-id="74819-215">e.</span></span> <span data-ttu-id="74819-216">Om du vill konfigurera programmet i **IDP initierade läge** Välj **”identitetsprovider (IdP) initierade logga in”**.</span><span class="sxs-lookup"><span data-stu-id="74819-216">If you wish to configure the application in **IDP initiated mode** select **"Identity Provider (IdP) initiated log in"**.</span></span> <span data-ttu-id="74819-217">Om **SP initierade läge** Välj **”ServiceProvider (SP) initierade logga in”**.</span><span class="sxs-lookup"><span data-stu-id="74819-217">If **SP initiated mode** select **"Service Provider (SP) initiated log in"**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    <span data-ttu-id="74819-219">f.</span><span class="sxs-lookup"><span data-stu-id="74819-219">f.</span></span> <span data-ttu-id="74819-220">Kopiera URL för SAML-konsumenten för din instans och klistra in den i **Reply URL** TextBox-kontroll i **Hightail domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="74819-220">Copy the SAML consumer URL for your instance and paste it in **Reply URL** textbox in **Hightail Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="74819-221">g.</span><span class="sxs-lookup"><span data-stu-id="74819-221">g.</span></span> <span data-ttu-id="74819-222">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="74819-222">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="74819-223">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="74819-223">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="74819-224">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="74819-224">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="74819-225">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="74819-225">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="74819-226">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="74819-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="74819-227">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74819-227">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="74819-229">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="74819-229">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="74819-230">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="74819-230">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="74819-232">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="74819-232">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="74819-234">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74819-234">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="74819-236">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="74819-236">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="74819-238">a.</span><span class="sxs-lookup"><span data-stu-id="74819-238">a.</span></span> <span data-ttu-id="74819-239">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="74819-239">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="74819-240">b.</span><span class="sxs-lookup"><span data-stu-id="74819-240">b.</span></span> <span data-ttu-id="74819-241">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="74819-241">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="74819-242">c.</span><span class="sxs-lookup"><span data-stu-id="74819-242">c.</span></span> <span data-ttu-id="74819-243">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="74819-243">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="74819-244">d.</span><span class="sxs-lookup"><span data-stu-id="74819-244">d.</span></span> <span data-ttu-id="74819-245">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="74819-245">Click **Create**.</span></span>
 
### <a name="creating-a-hightail-test-user"></a><span data-ttu-id="74819-246">Skapa en testanvändare Hightail</span><span class="sxs-lookup"><span data-stu-id="74819-246">Creating a Hightail test user</span></span>

<span data-ttu-id="74819-247">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Hightail.</span><span class="sxs-lookup"><span data-stu-id="74819-247">The objective of this section is to create a user called Britta Simon in Hightail.</span></span> 

<span data-ttu-id="74819-248">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="74819-248">There is no action item for you in this section.</span></span> <span data-ttu-id="74819-249">Hightail stöder just-in-time-användaretablering baserat på de anpassa anspråk.</span><span class="sxs-lookup"><span data-stu-id="74819-249">Hightail supports just-in-time user provisioning based on the custom claims.</span></span> <span data-ttu-id="74819-250">Om du har konfigurerat anpassade anspråk som visas i avsnittet  **[konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  ovan, skapas automatiskt en användare i program som det inte finns.</span><span class="sxs-lookup"><span data-stu-id="74819-250">If you have configured the custom claims as shown in the section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** above, a user is automatically created in the application it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="74819-251">Om du behöver skapa en användare manuellt, måste du kontakta den [Hightail supportteamet](mailto:support@hightail.com).</span><span class="sxs-lookup"><span data-stu-id="74819-251">If you need to create a user manually, you need to contact the [Hightail support team](mailto:support@hightail.com).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="74819-252">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="74819-252">Assigning the Azure AD test user</span></span>

<span data-ttu-id="74819-253">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Hightail.</span><span class="sxs-lookup"><span data-stu-id="74819-253">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Hightail.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="74819-255">**Om du vill tilldela Hightail Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="74819-255">**To assign Britta Simon to Hightail, perform the following steps:**</span></span>

1. <span data-ttu-id="74819-256">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="74819-256">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="74819-258">Välj i listan med program **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="74819-258">In the applications list, select **Hightail**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. <span data-ttu-id="74819-260">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="74819-260">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="74819-262">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="74819-262">Click **Add** button.</span></span> <span data-ttu-id="74819-263">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74819-263">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="74819-265">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="74819-265">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="74819-266">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74819-266">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="74819-267">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74819-267">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="74819-268">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74819-268">Testing single sign-on</span></span>

<span data-ttu-id="74819-269">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="74819-269">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="74819-270">När du klickar på panelen Hightail på åtkomstpanelen du bör få automatiskt loggat in på ditt Hightail program.</span><span class="sxs-lookup"><span data-stu-id="74819-270">When you click the Hightail tile in the Access Panel, you should get automatically signed-on to your Hightail application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="74819-271">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="74819-271">Additional resources</span></span>

* [<span data-ttu-id="74819-272">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="74819-272">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="74819-273">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="74819-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

