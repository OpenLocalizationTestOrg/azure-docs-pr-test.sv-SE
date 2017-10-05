---
title: "Självstudier: Azure Active Directory-integrering med SkyDesk e-post | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SkyDesk e-post."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 0ffcca4161fc836192fc9c9871a905f36ea76b32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a><span data-ttu-id="0a859-103">Självstudier: Azure Active Directory-integrering med SkyDesk e-post</span><span class="sxs-lookup"><span data-stu-id="0a859-103">Tutorial: Azure Active Directory integration with SkyDesk Email</span></span>

<span data-ttu-id="0a859-104">I kursen får lära du att integrera SkyDesk e-post med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0a859-104">In this tutorial, you learn how to integrate SkyDesk Email with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0a859-105">Integrera SkyDesk e-post med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0a859-105">Integrating SkyDesk Email with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0a859-106">Du kan styra i Azure AD som har åtkomst till e-SkyDesk</span><span class="sxs-lookup"><span data-stu-id="0a859-106">You can control in Azure AD who has access to SkyDesk Email</span></span>
- <span data-ttu-id="0a859-107">Du kan aktivera användarna att automatiskt hämta inloggade till SkyDesk e-post (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="0a859-107">You can enable your users to automatically get signed-on to SkyDesk Email (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0a859-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0a859-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0a859-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0a859-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a859-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0a859-110">Prerequisites</span></span>

<span data-ttu-id="0a859-111">För att konfigurera Azure AD-integrering med SkyDesk e-post, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0a859-111">To configure Azure AD integration with SkyDesk Email, you need the following items:</span></span>

- <span data-ttu-id="0a859-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0a859-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0a859-113">En e-SkyDesk enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="0a859-113">A SkyDesk Email single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0a859-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0a859-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0a859-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0a859-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0a859-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0a859-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0a859-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0a859-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0a859-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0a859-118">Scenario description</span></span>
<span data-ttu-id="0a859-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0a859-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0a859-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0a859-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0a859-121">Lägga till SkyDesk från galleriet</span><span class="sxs-lookup"><span data-stu-id="0a859-121">Adding SkyDesk Email from the gallery</span></span>
2. <span data-ttu-id="0a859-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0a859-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skydesk-email-from-the-gallery"></a><span data-ttu-id="0a859-123">Lägga till SkyDesk från galleriet</span><span class="sxs-lookup"><span data-stu-id="0a859-123">Adding SkyDesk Email from the gallery</span></span>
<span data-ttu-id="0a859-124">Du måste lägga till SkyDesk e-post från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SkyDesk e-post i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0a859-124">To configure the integration of SkyDesk Email into Azure AD, you need to add SkyDesk Email from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0a859-125">**Utför följande steg för att lägga till SkyDesk e-post från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="0a859-125">**To add SkyDesk Email from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0a859-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0a859-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0a859-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0a859-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0a859-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0a859-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="0a859-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a859-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="0a859-133">I sökrutan skriver **SkyDesk e-**.</span><span class="sxs-lookup"><span data-stu-id="0a859-133">In the search box, type **SkyDesk Email**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_search.png)

5. <span data-ttu-id="0a859-135">Välj i resultatpanelen **SkyDesk e-**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="0a859-135">In the results panel, select **SkyDesk Email**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0a859-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0a859-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0a859-138">Du konfigurera och testa Azure AD enkel inloggning med SkyDesk e-post baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0a859-138">In this section, you configure and test Azure AD single sign-on with SkyDesk Email based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0a859-139">Azure AD måste du känna till motsvarande användaren i SkyDesk e-post till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="0a859-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SkyDesk Email is to a user in Azure AD.</span></span> <span data-ttu-id="0a859-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SkyDesk e-post upprättas.</span><span class="sxs-lookup"><span data-stu-id="0a859-140">In other words, a link relationship between an Azure AD user and the related user in SkyDesk Email needs to be established.</span></span>

<span data-ttu-id="0a859-141">I SkyDesk e-post, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="0a859-141">In SkyDesk Email, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0a859-142">Om du vill konfigurera och testa Azure AD enkel inloggning med SkyDesk e-post, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0a859-142">To configure and test Azure AD single sign-on with SkyDesk Email, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0a859-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0a859-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0a859-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0a859-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0a859-145">**[Skapa en testanvändare SkyDesk e-](#creating-a-skydesk-email-test-user)**  – du har en motsvarighet för Britta Simon i SkyDesk e-post som är länkad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0a859-145">**[Creating a SkyDesk Email test user](#creating-a-skydesk-email-test-user)** - to have a counterpart of Britta Simon in SkyDesk Email that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0a859-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0a859-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0a859-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0a859-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0a859-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0a859-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0a859-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i SkyDesk e-postprogrammet.</span><span class="sxs-lookup"><span data-stu-id="0a859-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SkyDesk Email application.</span></span>

<span data-ttu-id="0a859-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SkyDesk e-post:**</span><span class="sxs-lookup"><span data-stu-id="0a859-150">**To configure Azure AD single sign-on with SkyDesk Email, perform the following steps:**</span></span>

1. <span data-ttu-id="0a859-151">I Azure-portalen på den **SkyDesk e-** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0a859-151">In the Azure portal, on the **SkyDesk Email** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="0a859-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0a859-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

3. <span data-ttu-id="0a859-155">På den **SkyDesk e-postdomän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0a859-155">On the **SkyDesk Email Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    <span data-ttu-id="0a859-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://mail.skydesk.jp/portal/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="0a859-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://mail.skydesk.jp/portal/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0a859-158">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="0a859-158">The value is not real.</span></span> <span data-ttu-id="0a859-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="0a859-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="0a859-160">Kontakta [SkyDesk e-postklient supportteamet](https://www.skydesk.sg/support/) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="0a859-160">Contact [SkyDesk Email Client support team](https://www.skydesk.sg/support/) to get the value.</span></span> 
 
4. <span data-ttu-id="0a859-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="0a859-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

5. <span data-ttu-id="0a859-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0a859-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0a859-165">På den **SkyDesk e-postkonfigurationen** klickar du på **Konfigurera e-post från SkyDesk** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="0a859-165">On the **SkyDesk Email Configuration** section, click **Configure SkyDesk Email** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0a859-166">Kopiera den **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="0a859-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

7. <span data-ttu-id="0a859-168">Att aktivera enkel inloggning i **SkyDesk e-**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0a859-168">To enable SSO in **SkyDesk Email**, perform the following steps:</span></span>

    <span data-ttu-id="0a859-169">a.</span><span class="sxs-lookup"><span data-stu-id="0a859-169">a.</span></span> <span data-ttu-id="0a859-170">Inloggning till ditt SkyDesk e-postkonto som administratör.</span><span class="sxs-lookup"><span data-stu-id="0a859-170">Sign-on to your SkyDesk Email account as administrator.</span></span>

    <span data-ttu-id="0a859-171">b.</span><span class="sxs-lookup"><span data-stu-id="0a859-171">b.</span></span> <span data-ttu-id="0a859-172">Klicka på menyn högst upp **installationsprogrammet**, och välj **Org**.</span><span class="sxs-lookup"><span data-stu-id="0a859-172">In the menu on the top, click **Setup**, and select **Org**.</span></span> 
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    <span data-ttu-id="0a859-174">c.</span><span class="sxs-lookup"><span data-stu-id="0a859-174">c.</span></span> <span data-ttu-id="0a859-175">Klicka på **domäner** från den vänstra panelen.</span><span class="sxs-lookup"><span data-stu-id="0a859-175">Click on **Domains** from the left panel.</span></span>
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    <span data-ttu-id="0a859-177">d.</span><span class="sxs-lookup"><span data-stu-id="0a859-177">d.</span></span> <span data-ttu-id="0a859-178">Klicka på **Lägg till domän**.</span><span class="sxs-lookup"><span data-stu-id="0a859-178">Click on **Add Domain**.</span></span>
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    <span data-ttu-id="0a859-180">e.</span><span class="sxs-lookup"><span data-stu-id="0a859-180">e.</span></span> <span data-ttu-id="0a859-181">Ange ditt domännamn och verifiera domänen.</span><span class="sxs-lookup"><span data-stu-id="0a859-181">Enter your Domain name, and then verify the Domain.</span></span>
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    <span data-ttu-id="0a859-183">f.</span><span class="sxs-lookup"><span data-stu-id="0a859-183">f.</span></span> <span data-ttu-id="0a859-184">Klicka på **SAML-autentisering** från den vänstra panelen.</span><span class="sxs-lookup"><span data-stu-id="0a859-184">Click on **SAML Authentication** from the left panel.</span></span>
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

8. <span data-ttu-id="0a859-186">På den **SAML-autentisering** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0a859-186">On the **SAML Authentication** dialog page, perform the following steps:</span></span>
   
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    ><span data-ttu-id="0a859-188">Om du vill använda SAML-baserad autentisering ska du antingen har **verifierade domän** eller **portal URL** installationen.</span><span class="sxs-lookup"><span data-stu-id="0a859-188">To use SAML based authentication, you should either have **verified domain** or **portal URL** setup.</span></span> <span data-ttu-id="0a859-189">Du kan ange portalen URL: en med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="0a859-189">You can set the portal URL with the unique name.</span></span>
    > 
    > 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    <span data-ttu-id="0a859-191">a.</span><span class="sxs-lookup"><span data-stu-id="0a859-191">a.</span></span> <span data-ttu-id="0a859-192">I den **Inloggningswebbadressen** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0a859-192">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="0a859-193">b.</span><span class="sxs-lookup"><span data-stu-id="0a859-193">b.</span></span> <span data-ttu-id="0a859-194">I den **logga ut** URL: en textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0a859-194">In the **Logout** URL textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="0a859-195">c.</span><span class="sxs-lookup"><span data-stu-id="0a859-195">c.</span></span> <span data-ttu-id="0a859-196">**Ändra lösenord URL** är valfritt så lämna det tomt.</span><span class="sxs-lookup"><span data-stu-id="0a859-196">**Change Password URL** is optional so leave it blank.</span></span>

    <span data-ttu-id="0a859-197">d.</span><span class="sxs-lookup"><span data-stu-id="0a859-197">d.</span></span> <span data-ttu-id="0a859-198">Klicka på **hämta nyckeln från filen** välja hämtade certifikatet från Azure-portalen och klicka sedan på **öppna** att ladda upp certifikatet.</span><span class="sxs-lookup"><span data-stu-id="0a859-198">Click on **Get Key From File** to select your downloaded certificate from Azure portal, and then click **Open** to upload the certificate.</span></span>

    <span data-ttu-id="0a859-199">e.</span><span class="sxs-lookup"><span data-stu-id="0a859-199">e.</span></span> <span data-ttu-id="0a859-200">Som **algoritmen**väljer **RSA**.</span><span class="sxs-lookup"><span data-stu-id="0a859-200">As **Algorithm**, select **RSA**.</span></span>

    <span data-ttu-id="0a859-201">f.</span><span class="sxs-lookup"><span data-stu-id="0a859-201">f.</span></span> <span data-ttu-id="0a859-202">Klicka på **Ok** spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="0a859-202">Click **Ok** to save the changes.</span></span>

> [!TIP]
> <span data-ttu-id="0a859-203">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="0a859-203">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0a859-204">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="0a859-204">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0a859-205">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0a859-205">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0a859-206">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a859-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="0a859-207">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0a859-207">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="0a859-209">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="0a859-209">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0a859-210">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0a859-210">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0a859-212">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0a859-212">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0a859-214">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a859-214">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0a859-216">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0a859-216">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0a859-218">a.</span><span class="sxs-lookup"><span data-stu-id="0a859-218">a.</span></span> <span data-ttu-id="0a859-219">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0a859-219">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0a859-220">b.</span><span class="sxs-lookup"><span data-stu-id="0a859-220">b.</span></span> <span data-ttu-id="0a859-221">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0a859-221">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0a859-222">c.</span><span class="sxs-lookup"><span data-stu-id="0a859-222">c.</span></span> <span data-ttu-id="0a859-223">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0a859-223">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0a859-224">d.</span><span class="sxs-lookup"><span data-stu-id="0a859-224">d.</span></span> <span data-ttu-id="0a859-225">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0a859-225">Click **Create**.</span></span>
 
### <a name="creating-a-skydesk-email-test-user"></a><span data-ttu-id="0a859-226">Skapa en testanvändare SkyDesk e-post</span><span class="sxs-lookup"><span data-stu-id="0a859-226">Creating a SkyDesk Email test user</span></span>

<span data-ttu-id="0a859-227">I det här avsnittet kan du skapa en användare som kallas Britta Simon i SkyDesk e-post.</span><span class="sxs-lookup"><span data-stu-id="0a859-227">In this section, you create a user called Britta Simon in SkyDesk Email.</span></span>

1. <span data-ttu-id="0a859-228">Klicka på **användaråtkomst** från vänstra panelen i SkyDesk e-post och ange sedan ditt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="0a859-228">Click on **User Access** from the left panel in SkyDesk Email and then enter your username.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
><span data-ttu-id="0a859-230">Om du behöver skapa grupp användare, måste du kontakta den [SkyDesk e-postklient supportteamet](https://www.skydesk.sg/support/).</span><span class="sxs-lookup"><span data-stu-id="0a859-230">If you need to create bulk users, you need to contact the [SkyDesk Email Client support team](https://www.skydesk.sg/support/).</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0a859-231">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="0a859-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0a859-232">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till e-SkyDesk.</span><span class="sxs-lookup"><span data-stu-id="0a859-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SkyDesk Email.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0a859-234">**Om du vill tilldela Britta Simon SkyDesk e-post, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0a859-234">**To assign Britta Simon to SkyDesk Email, perform the following steps:**</span></span>

1. <span data-ttu-id="0a859-235">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0a859-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0a859-237">Välj i listan med program **SkyDesk e-**.</span><span class="sxs-lookup"><span data-stu-id="0a859-237">In the applications list, select **SkyDesk Email**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

3. <span data-ttu-id="0a859-239">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0a859-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="0a859-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0a859-241">Click **Add** button.</span></span> <span data-ttu-id="0a859-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a859-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="0a859-244">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="0a859-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0a859-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a859-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0a859-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a859-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0a859-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0a859-247">Testing single sign-on</span></span>

<span data-ttu-id="0a859-248">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0a859-248">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="0a859-249">När du klickar på panelen SkyDesk e-post på åtkomstpanelen du bör få automatiskt loggat in på SkyDesk e-postprogrammet.</span><span class="sxs-lookup"><span data-stu-id="0a859-249">When you click the SkyDesk Email tile in the Access Panel, you should get automatically signed-on to your SkyDesk Email application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a859-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0a859-250">Additional resources</span></span>

* [<span data-ttu-id="0a859-251">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0a859-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0a859-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0a859-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png

