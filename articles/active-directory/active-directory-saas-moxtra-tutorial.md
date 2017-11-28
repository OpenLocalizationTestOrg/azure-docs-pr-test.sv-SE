---
title: "Självstudier: Azure Active Directory-integrering med Moxtra | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Moxtra."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: db2f041a44b6771b0a4f734e58d899298ef0847b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a><span data-ttu-id="27c79-103">Självstudier: Azure Active Directory-integrering med Moxtra</span><span class="sxs-lookup"><span data-stu-id="27c79-103">Tutorial: Azure Active Directory integration with Moxtra</span></span>

<span data-ttu-id="27c79-104">I kursen får lära du att integrera Moxtra med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="27c79-104">In this tutorial, you learn how to integrate Moxtra with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="27c79-105">Integrera Moxtra med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="27c79-105">Integrating Moxtra with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="27c79-106">Du kan styra i Azure AD som har åtkomst till Moxtra</span><span class="sxs-lookup"><span data-stu-id="27c79-106">You can control in Azure AD who has access to Moxtra</span></span>
- <span data-ttu-id="27c79-107">Du kan aktivera användarna att automatiskt hämta loggat in på Moxtra (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="27c79-107">You can enable your users to automatically get signed-on to Moxtra (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="27c79-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="27c79-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="27c79-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="27c79-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27c79-110">Krav</span><span class="sxs-lookup"><span data-stu-id="27c79-110">Prerequisites</span></span>

<span data-ttu-id="27c79-111">För att konfigurera Azure AD-integrering med Moxtra, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="27c79-111">To configure Azure AD integration with Moxtra, you need the following items:</span></span>

- <span data-ttu-id="27c79-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="27c79-112">An Azure AD subscription</span></span>
- <span data-ttu-id="27c79-113">En Moxtra enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="27c79-113">A Moxtra single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="27c79-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="27c79-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="27c79-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="27c79-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="27c79-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="27c79-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="27c79-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="27c79-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="27c79-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="27c79-118">Scenario description</span></span>
<span data-ttu-id="27c79-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="27c79-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="27c79-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="27c79-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="27c79-121">Att lägga till Moxtra från galleriet</span><span class="sxs-lookup"><span data-stu-id="27c79-121">Adding Moxtra from the gallery</span></span>
2. <span data-ttu-id="27c79-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27c79-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moxtra-from-the-gallery"></a><span data-ttu-id="27c79-123">Att lägga till Moxtra från galleriet</span><span class="sxs-lookup"><span data-stu-id="27c79-123">Adding Moxtra from the gallery</span></span>
<span data-ttu-id="27c79-124">Du måste lägga till Moxtra från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Moxtra i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="27c79-124">To configure the integration of Moxtra into Azure AD, you need to add Moxtra from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="27c79-125">**Utför följande steg för att lägga till Moxtra från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="27c79-125">**To add Moxtra from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="27c79-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="27c79-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="27c79-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="27c79-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="27c79-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="27c79-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="27c79-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27c79-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="27c79-133">I sökrutan skriver **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="27c79-133">In the search box, type **Moxtra**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. <span data-ttu-id="27c79-135">Välj i resultatpanelen **Moxtra**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="27c79-135">In the results panel, select **Moxtra**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="27c79-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27c79-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="27c79-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Moxtra baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="27c79-138">In this section, you configure and test Azure AD single sign-on with Moxtra based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="27c79-139">Azure AD måste du känna till användaren i Moxtra motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="27c79-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Moxtra is to a user in Azure AD.</span></span> <span data-ttu-id="27c79-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Moxtra upprättas.</span><span class="sxs-lookup"><span data-stu-id="27c79-140">In other words, a link relationship between an Azure AD user and the related user in Moxtra needs to be established.</span></span>

<span data-ttu-id="27c79-141">I Moxtra, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="27c79-141">In Moxtra, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="27c79-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Moxtra, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="27c79-142">To configure and test Azure AD single sign-on with Moxtra, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="27c79-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="27c79-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="27c79-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27c79-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="27c79-145">**[Skapa en testanvändare Moxtra](#creating-a-moxtra-test-user)**  – du har en motsvarighet för Britta Simon i Moxtra som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="27c79-145">**[Creating a Moxtra test user](#creating-a-moxtra-test-user)** - to have a counterpart of Britta Simon in Moxtra that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="27c79-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="27c79-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="27c79-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="27c79-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="27c79-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27c79-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="27c79-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Moxtra program.</span><span class="sxs-lookup"><span data-stu-id="27c79-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Moxtra application.</span></span>

<span data-ttu-id="27c79-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Moxtra:**</span><span class="sxs-lookup"><span data-stu-id="27c79-150">**To configure Azure AD single sign-on with Moxtra, perform the following steps:**</span></span>

1. <span data-ttu-id="27c79-151">I Azure-portalen på den **Moxtra** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="27c79-151">In the Azure portal, on the **Moxtra** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="27c79-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="27c79-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. <span data-ttu-id="27c79-155">På den **Moxtra domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="27c79-155">On the **Moxtra Domain and URLs** section, perform the following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    <span data-ttu-id="27c79-157">I den **inloggnings-URL** textruta Skriv en URL som:`https://www.moxtra.com/service/#login`</span><span class="sxs-lookup"><span data-stu-id="27c79-157">In the **Sign-on URL** textbox, type a URL as: `https://www.moxtra.com/service/#login`</span></span>

4. <span data-ttu-id="27c79-158">Moxtra program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="27c79-158">Moxtra application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="27c79-159">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="27c79-159">Configure the following claims for this application.</span></span> <span data-ttu-id="27c79-160">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="27c79-160">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="27c79-161">Följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="27c79-161">The following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. <span data-ttu-id="27c79-163">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="27c79-163">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="27c79-164">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="27c79-164">Attribute Name</span></span> | <span data-ttu-id="27c79-165">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="27c79-165">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="27c79-166">Förnamn</span><span class="sxs-lookup"><span data-stu-id="27c79-166">firstname</span></span> | <span data-ttu-id="27c79-167">User.givenName</span><span class="sxs-lookup"><span data-stu-id="27c79-167">user.givenname</span></span> |
    | <span data-ttu-id="27c79-168">Efternamn</span><span class="sxs-lookup"><span data-stu-id="27c79-168">lastname</span></span> | <span data-ttu-id="27c79-169">User.surname</span><span class="sxs-lookup"><span data-stu-id="27c79-169">user.surname</span></span> |
    | <span data-ttu-id="27c79-170">idpid</span><span class="sxs-lookup"><span data-stu-id="27c79-170">idpid</span></span>    | <span data-ttu-id="27c79-171">< SAML enhets-ID ></span><span class="sxs-lookup"><span data-stu-id="27c79-171">< SAML Entity ID ></span></span> 

    > [!Note]
    > <span data-ttu-id="27c79-172">Värdet för **idpid** attributet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="27c79-172">The value of **idpid** attribute is not real.</span></span> <span data-ttu-id="27c79-173">Du kan hämta det faktiska värdet från **Snabbreferens** avsnittet **Moxtra Configuration**.</span><span class="sxs-lookup"><span data-stu-id="27c79-173">You can get the actual value from **Quick reference** section under **Moxtra Configuration**.</span></span>
    
    <span data-ttu-id="27c79-174">a.</span><span class="sxs-lookup"><span data-stu-id="27c79-174">a.</span></span> <span data-ttu-id="27c79-175">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27c79-175">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="27c79-177">b.</span><span class="sxs-lookup"><span data-stu-id="27c79-177">b.</span></span> <span data-ttu-id="27c79-178">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="27c79-178">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="27c79-180">c.</span><span class="sxs-lookup"><span data-stu-id="27c79-180">c.</span></span> <span data-ttu-id="27c79-181">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="27c79-181">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="27c79-182">d.</span><span class="sxs-lookup"><span data-stu-id="27c79-182">d.</span></span> <span data-ttu-id="27c79-183">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="27c79-183">Click **Ok**.</span></span>
    
5. <span data-ttu-id="27c79-184">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="27c79-184">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. <span data-ttu-id="27c79-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="27c79-186">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="27c79-188">På den **Moxtra Configuration** klickar du på **konfigurera Moxtra** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="27c79-188">On the **Moxtra Configuration** section, click **Configure Moxtra** to open **Configure sign-on** window.</span></span> <span data-ttu-id="27c79-189">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="27c79-189">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. <span data-ttu-id="27c79-191">I ett nytt webbläsarfönster inloggning till webbplatsen Moxtra företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="27c79-191">In another browser window, sign on to your Moxtra company site as an administrator.</span></span>

9. <span data-ttu-id="27c79-192">Klicka på i verktygsfältet på vänster **administratörskonsolen > SAML enkel inloggning**, och klicka sedan på **ny**.</span><span class="sxs-lookup"><span data-stu-id="27c79-192">In the toolbar on the left, click **Admin Console > SAML Single Sign-on**, and then click **New**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. <span data-ttu-id="27c79-194">På den **SAML** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="27c79-194">On the **SAML** page, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    <span data-ttu-id="27c79-196">a.</span><span class="sxs-lookup"><span data-stu-id="27c79-196">a.</span></span> <span data-ttu-id="27c79-197">I den **namn** textruta, ange ett namn för din konfiguration (t.ex.: *SAML*).</span><span class="sxs-lookup"><span data-stu-id="27c79-197">In the **Name** textbox, type a name for your configuration (e.g.: *SAML*).</span></span> 
  
    <span data-ttu-id="27c79-198">b.</span><span class="sxs-lookup"><span data-stu-id="27c79-198">b.</span></span> <span data-ttu-id="27c79-199">I den **IdP enhets-ID** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="27c79-199">In the **IdP Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="27c79-200">c.</span><span class="sxs-lookup"><span data-stu-id="27c79-200">c.</span></span> <span data-ttu-id="27c79-201">I **Inloggningswebbadressen** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="27c79-201">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="27c79-202">d.</span><span class="sxs-lookup"><span data-stu-id="27c79-202">d.</span></span> <span data-ttu-id="27c79-203">I den **AuthnContextClassRef** textruta typen **urn: oasis: namn: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="27c79-203">In the **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span> 
 
    <span data-ttu-id="27c79-204">e.</span><span class="sxs-lookup"><span data-stu-id="27c79-204">e.</span></span> <span data-ttu-id="27c79-205">I den **NameID Format** textruta typen **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="27c79-205">In the **NameID Format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span> 
 
    <span data-ttu-id="27c79-206">f.</span><span class="sxs-lookup"><span data-stu-id="27c79-206">f.</span></span> <span data-ttu-id="27c79-207">Öppna certifikat som du har hämtat från Azure-portalen i anteckningar, kopiera innehållet och klistrar in det i den **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="27c79-207">Open certificate which you have downloaded from Azure portal in notepad, copy the content, and then paste it into the **Certificate** textbox.</span></span>    
 
    <span data-ttu-id="27c79-208">g.</span><span class="sxs-lookup"><span data-stu-id="27c79-208">g.</span></span> <span data-ttu-id="27c79-209">Skriv din e-postdomän SAML i textrutan SAML e-domän.</span><span class="sxs-lookup"><span data-stu-id="27c79-209">In the SAML email domain textbox, type your SAML email domain.</span></span>    
  
    >[!NOTE]
    ><span data-ttu-id="27c79-210">Klicka för att se stegen för att verifiera domänen på ”**jag**” nedan.</span><span class="sxs-lookup"><span data-stu-id="27c79-210">To see the steps to verify the domain, click the "**i**" below.</span></span>

    <span data-ttu-id="27c79-211">h.</span><span class="sxs-lookup"><span data-stu-id="27c79-211">h.</span></span> <span data-ttu-id="27c79-212">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="27c79-212">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="27c79-213">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="27c79-213">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="27c79-214">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="27c79-214">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="27c79-215">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="27c79-215">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="27c79-216">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="27c79-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="27c79-217">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27c79-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="27c79-219">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="27c79-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="27c79-220">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="27c79-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="27c79-222">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="27c79-222">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="27c79-224">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27c79-224">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="27c79-226">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="27c79-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="27c79-228">a.</span><span class="sxs-lookup"><span data-stu-id="27c79-228">a.</span></span> <span data-ttu-id="27c79-229">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="27c79-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="27c79-230">b.</span><span class="sxs-lookup"><span data-stu-id="27c79-230">b.</span></span> <span data-ttu-id="27c79-231">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="27c79-231">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="27c79-232">c.</span><span class="sxs-lookup"><span data-stu-id="27c79-232">c.</span></span> <span data-ttu-id="27c79-233">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="27c79-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="27c79-234">d.</span><span class="sxs-lookup"><span data-stu-id="27c79-234">d.</span></span> <span data-ttu-id="27c79-235">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="27c79-235">Click **Create**.</span></span>
 
### <a name="creating-a-moxtra-test-user"></a><span data-ttu-id="27c79-236">Skapa en testanvändare Moxtra</span><span class="sxs-lookup"><span data-stu-id="27c79-236">Creating a Moxtra test user</span></span>

<span data-ttu-id="27c79-237">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Moxtra.</span><span class="sxs-lookup"><span data-stu-id="27c79-237">The objective of this section is to create a user called Britta Simon in Moxtra.</span></span>

<span data-ttu-id="27c79-238">**Utför följande steg för att skapa en användare som kallas Britta Simon i Moxtra:**</span><span class="sxs-lookup"><span data-stu-id="27c79-238">**To create a user called Britta Simon in Moxtra, perform the following steps:**</span></span>

1. <span data-ttu-id="27c79-239">Logga in på webbplatsen Moxtra företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="27c79-239">Sign on to your Moxtra company site as an administrator.</span></span>

2. <span data-ttu-id="27c79-240">Klicka på i verktygsfältet på vänster **administratörskonsolen > Användarhantering**, och sedan **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="27c79-240">In the toolbar on the left, click **Admin Console > User Management**, and then **Add User**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. <span data-ttu-id="27c79-242">På den **Lägg till användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="27c79-242">On the **Add User** dialog, perform the following steps:</span></span>
  
    <span data-ttu-id="27c79-243">a.</span><span class="sxs-lookup"><span data-stu-id="27c79-243">a.</span></span> <span data-ttu-id="27c79-244">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="27c79-244">In the **First Name** textbox, type **Britta**.</span></span>
  
    <span data-ttu-id="27c79-245">b.</span><span class="sxs-lookup"><span data-stu-id="27c79-245">b.</span></span> <span data-ttu-id="27c79-246">I den **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="27c79-246">In the **Last Name** textbox, type **Simon**.</span></span>
  
    <span data-ttu-id="27c79-247">c.</span><span class="sxs-lookup"><span data-stu-id="27c79-247">c.</span></span> <span data-ttu-id="27c79-248">I den **e-post** textruta typen Brittas e-postadressen samma som på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="27c79-248">In the **Email** textbox, type Britta's email address same as on Azure portal.</span></span>
  
    <span data-ttu-id="27c79-249">d.</span><span class="sxs-lookup"><span data-stu-id="27c79-249">d.</span></span> <span data-ttu-id="27c79-250">I den **Division** textruta typen **Dev**.</span><span class="sxs-lookup"><span data-stu-id="27c79-250">In the **Division** textbox, type **Dev**.</span></span>
  
    <span data-ttu-id="27c79-251">e.</span><span class="sxs-lookup"><span data-stu-id="27c79-251">e.</span></span> <span data-ttu-id="27c79-252">I den **avdelning** textruta typen **IT**.</span><span class="sxs-lookup"><span data-stu-id="27c79-252">In the **Department** textbox, type **IT**.</span></span>
  
    <span data-ttu-id="27c79-253">f.</span><span class="sxs-lookup"><span data-stu-id="27c79-253">f.</span></span> <span data-ttu-id="27c79-254">Välj **administratör**.</span><span class="sxs-lookup"><span data-stu-id="27c79-254">Select **Administrator**.</span></span>
  
    <span data-ttu-id="27c79-255">g.</span><span class="sxs-lookup"><span data-stu-id="27c79-255">g.</span></span> <span data-ttu-id="27c79-256">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="27c79-256">Click **Add**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="27c79-257">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="27c79-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="27c79-258">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Moxtra.</span><span class="sxs-lookup"><span data-stu-id="27c79-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Moxtra.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="27c79-260">**Om du vill tilldela Moxtra Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="27c79-260">**To assign Britta Simon to Moxtra, perform the following steps:**</span></span>

1. <span data-ttu-id="27c79-261">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="27c79-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="27c79-263">Välj i listan med program **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="27c79-263">In the applications list, select **Moxtra**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. <span data-ttu-id="27c79-265">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="27c79-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="27c79-267">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="27c79-267">Click **Add** button.</span></span> <span data-ttu-id="27c79-268">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27c79-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="27c79-270">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="27c79-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="27c79-271">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27c79-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="27c79-272">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27c79-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="27c79-273">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27c79-273">Testing single sign-on</span></span>

<span data-ttu-id="27c79-274">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="27c79-274">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="27c79-275">När du klickar på panelen Moxtra på åtkomstpanelen du bör få automatiskt loggat in på ditt Moxtra program.</span><span class="sxs-lookup"><span data-stu-id="27c79-275">When you click the Moxtra tile in the Access Panel, you should get automatically signed-on to your Moxtra application.</span></span>
<span data-ttu-id="27c79-276">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="27c79-276">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27c79-277">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="27c79-277">Additional resources</span></span>

* [<span data-ttu-id="27c79-278">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="27c79-278">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="27c79-279">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="27c79-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

