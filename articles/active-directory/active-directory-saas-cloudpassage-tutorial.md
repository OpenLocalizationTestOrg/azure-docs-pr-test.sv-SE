---
title: "Självstudier: Azure Active Directory-integrering med CloudPassage | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och CloudPassage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 094740e20570665e975dec1a591989e411f90c16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a><span data-ttu-id="bd644-103">Självstudier: Azure Active Directory-integrering med CloudPassage</span><span class="sxs-lookup"><span data-stu-id="bd644-103">Tutorial: Azure Active Directory integration with CloudPassage</span></span>

<span data-ttu-id="bd644-104">I kursen får lära du att integrera CloudPassage med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="bd644-104">In this tutorial, you learn how to integrate CloudPassage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bd644-105">Integrera CloudPassage med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bd644-105">Integrating CloudPassage with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bd644-106">Du kan styra i Azure AD som har åtkomst till CloudPassage</span><span class="sxs-lookup"><span data-stu-id="bd644-106">You can control in Azure AD who has access to CloudPassage</span></span>
- <span data-ttu-id="bd644-107">Du kan aktivera användarna att automatiskt hämta loggat in på CloudPassage (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="bd644-107">You can enable your users to automatically get signed-on to CloudPassage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bd644-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bd644-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bd644-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bd644-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd644-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bd644-110">Prerequisites</span></span>

<span data-ttu-id="bd644-111">För att konfigurera Azure AD-integrering med CloudPassage, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="bd644-111">To configure Azure AD integration with CloudPassage, you need the following items:</span></span>

- <span data-ttu-id="bd644-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd644-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bd644-113">En CloudPassage enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd644-113">A CloudPassage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bd644-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bd644-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bd644-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bd644-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bd644-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="bd644-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bd644-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd644-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bd644-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="bd644-118">Scenario description</span></span>
<span data-ttu-id="bd644-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="bd644-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bd644-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="bd644-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bd644-121">Att lägga till CloudPassage från galleriet</span><span class="sxs-lookup"><span data-stu-id="bd644-121">Adding CloudPassage from the gallery</span></span>
2. <span data-ttu-id="bd644-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd644-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloudpassage-from-the-gallery"></a><span data-ttu-id="bd644-123">Att lägga till CloudPassage från galleriet</span><span class="sxs-lookup"><span data-stu-id="bd644-123">Adding CloudPassage from the gallery</span></span>
<span data-ttu-id="bd644-124">Du måste lägga till CloudPassage från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av CloudPassage i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd644-124">To configure the integration of CloudPassage into Azure AD, you need to add CloudPassage from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bd644-125">**Utför följande steg för att lägga till CloudPassage från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="bd644-125">**To add CloudPassage from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bd644-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bd644-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bd644-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="bd644-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bd644-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bd644-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="bd644-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd644-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="bd644-133">I sökrutan skriver **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="bd644-133">In the search box, type **CloudPassage**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_search.png)

5. <span data-ttu-id="bd644-135">Välj i resultatpanelen **CloudPassage**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="bd644-135">In the results panel, select **CloudPassage**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bd644-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd644-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bd644-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med CloudPassage baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="bd644-138">In this section, you configure and test Azure AD single sign-on with CloudPassage based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bd644-139">Azure AD måste du känna till användaren i CloudPassage motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="bd644-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CloudPassage is to a user in Azure AD.</span></span> <span data-ttu-id="bd644-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i CloudPassage upprättas.</span><span class="sxs-lookup"><span data-stu-id="bd644-140">In other words, a link relationship between an Azure AD user and the related user in CloudPassage needs to be established.</span></span>

<span data-ttu-id="bd644-141">I CloudPassage, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="bd644-141">In CloudPassage, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="bd644-142">Om du vill konfigurera och testa Azure AD enkel inloggning med CloudPassage, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="bd644-142">To configure and test Azure AD single sign-on with CloudPassage, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bd644-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="bd644-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bd644-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd644-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bd644-145">**[Skapa en testanvändare CloudPassage](#creating-a-cloudpassage-test-user)**  – du har en motsvarighet för Britta Simon i CloudPassage som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="bd644-145">**[Creating a CloudPassage test user](#creating-a-cloudpassage-test-user)** - to have a counterpart of Britta Simon in CloudPassage that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bd644-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bd644-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bd644-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="bd644-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bd644-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd644-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bd644-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt CloudPassage program.</span><span class="sxs-lookup"><span data-stu-id="bd644-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CloudPassage application.</span></span>

<span data-ttu-id="bd644-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med CloudPassage:**</span><span class="sxs-lookup"><span data-stu-id="bd644-150">**To configure Azure AD single sign-on with CloudPassage, perform the following steps:**</span></span>

1. <span data-ttu-id="bd644-151">I Azure-portalen på den **CloudPassage** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bd644-151">In the Azure portal, on the **CloudPassage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="bd644-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bd644-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

3. <span data-ttu-id="bd644-155">På den **CloudPassage domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd644-155">On the **CloudPassage Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    <span data-ttu-id="bd644-157">a.</span><span class="sxs-lookup"><span data-stu-id="bd644-157">a.</span></span> <span data-ttu-id="bd644-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://portal.cloudpassage.com/saml/init/accountid`</span><span class="sxs-lookup"><span data-stu-id="bd644-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://portal.cloudpassage.com/saml/init/accountid`</span></span>

    <span data-ttu-id="bd644-159">b.</span><span class="sxs-lookup"><span data-stu-id="bd644-159">b.</span></span> <span data-ttu-id="bd644-160">I den **Reply URL** textruta Skriv en URL med följande mönster: `https://portal.cloudpassage.com/saml/consume/accountid`.</span><span class="sxs-lookup"><span data-stu-id="bd644-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://portal.cloudpassage.com/saml/consume/accountid`.</span></span> <span data-ttu-id="bd644-161">Du kan hämta värdet för det här attributet genom att klicka på **SSO installationsprogrammet dokumentationen** i den **inställningar för enkel inloggning** på CloudPassage-portal.</span><span class="sxs-lookup"><span data-stu-id="bd644-161">You can get your value for this attribute by clicking **SSO Setup documentation** in the **Single Sign-on Settings** section of your CloudPassage portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > <span data-ttu-id="bd644-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="bd644-163">These values are not real.</span></span> <span data-ttu-id="bd644-164">Uppdatera dessa värden med den faktiska Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="bd644-164">Update these values with the actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="bd644-165">Kontakta [CloudPassage klienten supportteamet](https://www.cloudpassage.com/company/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="bd644-165">Contact [CloudPassage Client support team](https://www.cloudpassage.com/company/contact/) to get these values.</span></span> 

4. <span data-ttu-id="bd644-166">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="bd644-166">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

5. <span data-ttu-id="bd644-168">Tillämpningsprogrammet CloudPassage förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="bd644-168">Your CloudPassage application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="bd644-169">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="bd644-169">The following screenshot shows an example for this.</span></span>
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

6. <span data-ttu-id="bd644-171">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden ovan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd644-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>

    | <span data-ttu-id="bd644-172">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="bd644-172">Attribute Name</span></span> | <span data-ttu-id="bd644-173">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="bd644-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="bd644-174">Förnamn</span><span class="sxs-lookup"><span data-stu-id="bd644-174">firstname</span></span> |<span data-ttu-id="bd644-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="bd644-175">user.givenname</span></span> |
    | <span data-ttu-id="bd644-176">Efternamn</span><span class="sxs-lookup"><span data-stu-id="bd644-176">lastname</span></span> |<span data-ttu-id="bd644-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="bd644-177">user.surname</span></span> |
    | <span data-ttu-id="bd644-178">E-post</span><span class="sxs-lookup"><span data-stu-id="bd644-178">email</span></span> |<span data-ttu-id="bd644-179">User.Mail</span><span class="sxs-lookup"><span data-stu-id="bd644-179">user.mail</span></span> |
    
    <span data-ttu-id="bd644-180">a.</span><span class="sxs-lookup"><span data-stu-id="bd644-180">a.</span></span> <span data-ttu-id="bd644-181">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd644-181">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="bd644-184">b.</span><span class="sxs-lookup"><span data-stu-id="bd644-184">b.</span></span> <span data-ttu-id="bd644-185">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="bd644-185">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="bd644-186">c.</span><span class="sxs-lookup"><span data-stu-id="bd644-186">c.</span></span> <span data-ttu-id="bd644-187">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="bd644-187">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="bd644-188">d.</span><span class="sxs-lookup"><span data-stu-id="bd644-188">d.</span></span> <span data-ttu-id="bd644-189">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd644-189">Click **Ok**.</span></span>

7. <span data-ttu-id="bd644-190">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd644-190">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="bd644-192">På den **CloudPassage Configuration** klickar du på **konfigurera CloudPassage** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="bd644-192">On the **CloudPassage Configuration** section, click **Configure CloudPassage** to open **Configure sign-on** window.</span></span> <span data-ttu-id="bd644-193">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="bd644-193">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

9. <span data-ttu-id="bd644-195">I ett annat webbläsarfönster inloggning till webbplatsen CloudPassage företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="bd644-195">In a different browser window, sign-on to your CloudPassage company site as administrator.</span></span>

10. <span data-ttu-id="bd644-196">Klicka på menyn högst upp **inställningar**, och klicka sedan på **Platsadministration**.</span><span class="sxs-lookup"><span data-stu-id="bd644-196">In the menu on the top, click **Settings**, and then click **Site Administration**.</span></span> 
   
    ![Konfigurera enkel inloggning][12]

11. <span data-ttu-id="bd644-198">Klicka på den **autentiseringsinställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="bd644-198">Click the **Authentication Settings** tab.</span></span> 
   
    ![Konfigurera enkel inloggning][13]

12. <span data-ttu-id="bd644-200">I den **inställningar för enkel inloggning** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd644-200">In the **Single Sign-on Settings** section, perform the following steps:</span></span> 
   
    ![Konfigurera enkel inloggning][14]

    <span data-ttu-id="bd644-202">a.</span><span class="sxs-lookup"><span data-stu-id="bd644-202">a.</span></span> <span data-ttu-id="bd644-203">Välj **aktivera enkel sign-on(SSO) (SSO installationsprogrammet dokumentation)** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="bd644-203">Select **Enable Single sign-on(SSO)(SSO Setup Documentation)** checkbox.</span></span>
    
    <span data-ttu-id="bd644-204">b.</span><span class="sxs-lookup"><span data-stu-id="bd644-204">b.</span></span> <span data-ttu-id="bd644-205">Klistra in **SAML enhets-ID** till den **utfärdar-URL för SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="bd644-205">Paste **SAML Entity ID** into the **SAML issuer URL** textbox.</span></span>
  
    <span data-ttu-id="bd644-206">c.</span><span class="sxs-lookup"><span data-stu-id="bd644-206">c.</span></span> <span data-ttu-id="bd644-207">Klistra in **SAML enkel inloggning Tjänstwebbadress** till den **slutpunkts-URL för SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="bd644-207">Paste **SAML Single Sign-On Service URL** into the **SAML endpoint URL** textbox.</span></span>
  
    <span data-ttu-id="bd644-208">d.</span><span class="sxs-lookup"><span data-stu-id="bd644-208">d.</span></span> <span data-ttu-id="bd644-209">Klistra in **Sign-Out URL** till den **logga ut landningssida** textruta.</span><span class="sxs-lookup"><span data-stu-id="bd644-209">Paste **Sign-Out URL** into the **Logout landing page** textbox.</span></span>
  
    <span data-ttu-id="bd644-210">e.</span><span class="sxs-lookup"><span data-stu-id="bd644-210">e.</span></span> <span data-ttu-id="bd644-211">Öppna din hämtat certifikat i anteckningar, kopiera innehållet för hämtat certifikat till Urklipp och klistrar in det i den **x 509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="bd644-211">Open your downloaded certificate in notepad, copy the content of downloaded certificate into your clipboard, and then paste it into the **x 509 certificate** textbox.</span></span>
  
    <span data-ttu-id="bd644-212">f.</span><span class="sxs-lookup"><span data-stu-id="bd644-212">f.</span></span> <span data-ttu-id="bd644-213">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bd644-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="bd644-214">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="bd644-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bd644-215">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="bd644-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bd644-216">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bd644-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bd644-217">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd644-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="bd644-218">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd644-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="bd644-220">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="bd644-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bd644-221">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bd644-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bd644-223">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="bd644-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bd644-225">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd644-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bd644-227">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd644-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bd644-229">a.</span><span class="sxs-lookup"><span data-stu-id="bd644-229">a.</span></span> <span data-ttu-id="bd644-230">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bd644-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bd644-231">b.</span><span class="sxs-lookup"><span data-stu-id="bd644-231">b.</span></span> <span data-ttu-id="bd644-232">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bd644-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bd644-233">c.</span><span class="sxs-lookup"><span data-stu-id="bd644-233">c.</span></span> <span data-ttu-id="bd644-234">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="bd644-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bd644-235">d.</span><span class="sxs-lookup"><span data-stu-id="bd644-235">d.</span></span> <span data-ttu-id="bd644-236">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bd644-236">Click **Create**.</span></span>
 
### <a name="creating-a-cloudpassage-test-user"></a><span data-ttu-id="bd644-237">Skapa en testanvändare CloudPassage</span><span class="sxs-lookup"><span data-stu-id="bd644-237">Creating a CloudPassage test user</span></span>

<span data-ttu-id="bd644-238">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i CloudPassage.</span><span class="sxs-lookup"><span data-stu-id="bd644-238">The objective of this section is to create a user called Britta Simon in CloudPassage.</span></span>

<span data-ttu-id="bd644-239">**Utför följande steg för att skapa en användare som kallas Britta Simon i CloudPassage:**</span><span class="sxs-lookup"><span data-stu-id="bd644-239">**To create a user called Britta Simon in CloudPassage, perform the following steps:**</span></span>

1. <span data-ttu-id="bd644-240">Logga in på ditt **CloudPassage** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="bd644-240">Sign-on to your **CloudPassage** company site as an administrator.</span></span> 

2. <span data-ttu-id="bd644-241">Klicka på i verktygsfältet högst upp **inställningar**, och klicka sedan på **Platsadministration**.</span><span class="sxs-lookup"><span data-stu-id="bd644-241">In the toolbar on the top, click **Settings**, and then click **Site Administration**.</span></span> 
   
   ![Skapa en testanvändare CloudPassage][22] 

3. <span data-ttu-id="bd644-243">Klicka på den **användare** fliken och klicka sedan på **Lägg till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="bd644-243">Click the **Users** tab, and then click **Add New User**.</span></span> 
   
   ![Skapa en testanvändare CloudPassage][23]

4. <span data-ttu-id="bd644-245">I den **Lägg till nya användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd644-245">In the **Add New User** section, perform the following steps:</span></span> 
   
   ![Skapa en testanvändare CloudPassage][24]
    
    <span data-ttu-id="bd644-247">a.</span><span class="sxs-lookup"><span data-stu-id="bd644-247">a.</span></span> <span data-ttu-id="bd644-248">I den **Förnamn** textruta skriver Britta.</span><span class="sxs-lookup"><span data-stu-id="bd644-248">In the **First Name** textbox, type Britta.</span></span> 
  
    <span data-ttu-id="bd644-249">b.</span><span class="sxs-lookup"><span data-stu-id="bd644-249">b.</span></span> <span data-ttu-id="bd644-250">I den **efternamn** textruta skriver Simon.</span><span class="sxs-lookup"><span data-stu-id="bd644-250">In the **Last Name** textbox, type Simon.</span></span>
  
    <span data-ttu-id="bd644-251">c.</span><span class="sxs-lookup"><span data-stu-id="bd644-251">c.</span></span> <span data-ttu-id="bd644-252">I den **användarnamn** textruta den **e-post** textruta och **Skriv e-** textruta anger Brittas användarnamn i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd644-252">In the **Username** textbox, the **Email** textbox and the **Retype Email** textbox, type Britta's user name in Azure AD.</span></span>
  
    <span data-ttu-id="bd644-253">d.</span><span class="sxs-lookup"><span data-stu-id="bd644-253">d.</span></span> <span data-ttu-id="bd644-254">Som **behörighet**väljer **aktivera Halo Portal åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="bd644-254">As **Access Type**, select **Enable Halo Portal Access**.</span></span>
  
    <span data-ttu-id="bd644-255">e.</span><span class="sxs-lookup"><span data-stu-id="bd644-255">e.</span></span> <span data-ttu-id="bd644-256">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="bd644-256">Click **Add**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bd644-257">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="bd644-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bd644-258">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till CloudPassage.</span><span class="sxs-lookup"><span data-stu-id="bd644-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CloudPassage.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="bd644-260">**Om du vill tilldela CloudPassage Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bd644-260">**To assign Britta Simon to CloudPassage, perform the following steps:**</span></span>

1. <span data-ttu-id="bd644-261">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bd644-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="bd644-263">Välj i listan med program **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="bd644-263">In the applications list, select **CloudPassage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

3. <span data-ttu-id="bd644-265">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="bd644-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="bd644-267">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd644-267">Click **Add** button.</span></span> <span data-ttu-id="bd644-268">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd644-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="bd644-270">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="bd644-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bd644-271">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd644-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bd644-272">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd644-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bd644-273">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd644-273">Testing single sign-on</span></span>

<span data-ttu-id="bd644-274">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="bd644-274">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="bd644-275">När du klickar på panelen CloudPassage på åtkomstpanelen du bör få automatiskt loggat in på ditt CloudPassage program.</span><span class="sxs-lookup"><span data-stu-id="bd644-275">When you click the CloudPassage tile in the Access Panel, you should get automatically signed-on to your CloudPassage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd644-276">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bd644-276">Additional resources</span></span>

* [<span data-ttu-id="bd644-277">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bd644-277">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bd644-278">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bd644-278">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_203.png

