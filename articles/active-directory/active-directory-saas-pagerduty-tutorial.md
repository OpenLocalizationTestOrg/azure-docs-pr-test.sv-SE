---
title: "Självstudier: Azure Active Directory-integrering med PagerDuty | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och PagerDuty."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: bf5263ce4d8fbc231029c101f167f4b55a921e60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a><span data-ttu-id="e9f61-103">Självstudier: Azure Active Directory-integrering med PagerDuty</span><span class="sxs-lookup"><span data-stu-id="e9f61-103">Tutorial: Azure Active Directory integration with PagerDuty</span></span>

<span data-ttu-id="e9f61-104">I kursen får lära du att integrera PagerDuty med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e9f61-104">In this tutorial, you learn how to integrate PagerDuty with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e9f61-105">Integrera PagerDuty med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e9f61-105">Integrating PagerDuty with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e9f61-106">Du kan styra i Azure AD som har åtkomst till PagerDuty</span><span class="sxs-lookup"><span data-stu-id="e9f61-106">You can control in Azure AD who has access to PagerDuty</span></span>
- <span data-ttu-id="e9f61-107">Du kan aktivera användarna att automatiskt hämta loggat in på PagerDuty (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e9f61-107">You can enable your users to automatically get signed-on to PagerDuty (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e9f61-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e9f61-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e9f61-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e9f61-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9f61-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e9f61-110">Prerequisites</span></span>

<span data-ttu-id="e9f61-111">För att konfigurera Azure AD-integrering med PagerDuty, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e9f61-111">To configure Azure AD integration with PagerDuty, you need the following items:</span></span>

- <span data-ttu-id="e9f61-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e9f61-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e9f61-113">En PagerDuty enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e9f61-113">A PagerDuty single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e9f61-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e9f61-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e9f61-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e9f61-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e9f61-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e9f61-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e9f61-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e9f61-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e9f61-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e9f61-118">Scenario description</span></span>
<span data-ttu-id="e9f61-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e9f61-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e9f61-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e9f61-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e9f61-121">Att lägga till PagerDuty från galleriet</span><span class="sxs-lookup"><span data-stu-id="e9f61-121">Adding PagerDuty from the gallery</span></span>
2. <span data-ttu-id="e9f61-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9f61-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pagerduty-from-the-gallery"></a><span data-ttu-id="e9f61-123">Att lägga till PagerDuty från galleriet</span><span class="sxs-lookup"><span data-stu-id="e9f61-123">Adding PagerDuty from the gallery</span></span>
<span data-ttu-id="e9f61-124">Du måste lägga till PagerDuty från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av PagerDuty i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e9f61-124">To configure the integration of PagerDuty into Azure AD, you need to add PagerDuty from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e9f61-125">**Utför följande steg för att lägga till PagerDuty från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e9f61-125">**To add PagerDuty from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e9f61-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e9f61-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="e9f61-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e9f61-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="e9f61-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9f61-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="e9f61-133">I sökrutan skriver **PagerDuty**väljer **PagerDuty** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e9f61-133">In the search box, type **PagerDuty**, select  **PagerDuty**  from result panel then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e9f61-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9f61-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e9f61-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PagerDuty baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e9f61-136">In this section, you configure and test Azure AD single sign-on with PagerDuty based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e9f61-137">Azure AD måste du känna till användaren i PagerDuty motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e9f61-137">For single sign-on to work, Azure AD needs to know what the counterpart user in PagerDuty is to a user in Azure AD.</span></span> <span data-ttu-id="e9f61-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i PagerDuty upprättas.</span><span class="sxs-lookup"><span data-stu-id="e9f61-138">In other words, a link relationship between an Azure AD user and the related user in PagerDuty needs to be established.</span></span>

<span data-ttu-id="e9f61-139">I PagerDuty, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e9f61-139">In PagerDuty, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e9f61-140">Om du vill konfigurera och testa Azure AD enkel inloggning med PagerDuty, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e9f61-140">To configure and test Azure AD single sign-on with PagerDuty, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e9f61-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e9f61-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e9f61-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e9f61-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e9f61-143">**[Skapa en testanvändare PagerDuty](#create-a-pagerduty-test-user)**  – du har en motsvarighet för Britta Simon i PagerDuty som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e9f61-143">**[Create a PagerDuty test user](#create-a-pagerduty-test-user)** - to have a counterpart of Britta Simon in PagerDuty that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e9f61-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e9f61-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e9f61-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e9f61-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e9f61-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9f61-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e9f61-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt PagerDuty program.</span><span class="sxs-lookup"><span data-stu-id="e9f61-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PagerDuty application.</span></span>

<span data-ttu-id="e9f61-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med PagerDuty:**</span><span class="sxs-lookup"><span data-stu-id="e9f61-148">**To configure Azure AD single sign-on with PagerDuty, perform the following steps:**</span></span>

1. <span data-ttu-id="e9f61-149">I Azure-portalen på den **PagerDuty** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-149">In the Azure portal, on the **PagerDuty** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="e9f61-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e9f61-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

3. <span data-ttu-id="e9f61-153">På den **PagerDuty domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9f61-153">On the **PagerDuty Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och PagerDuty domän med enkel inloggning information](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_url.png)

    <span data-ttu-id="e9f61-155">a.</span><span class="sxs-lookup"><span data-stu-id="e9f61-155">a.</span></span> <span data-ttu-id="e9f61-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="e9f61-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    <span data-ttu-id="e9f61-157">b.</span><span class="sxs-lookup"><span data-stu-id="e9f61-157">b.</span></span> <span data-ttu-id="e9f61-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="e9f61-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e9f61-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e9f61-159">These values are not real.</span></span> <span data-ttu-id="e9f61-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="e9f61-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e9f61-161">Kontakta [PagerDuty klienten supportteamet](https://www.pagerduty.com/support/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e9f61-161">Contact [PagerDuty Client support team](https://www.pagerduty.com/support/) to get these values.</span></span> 

4. <span data-ttu-id="e9f61-162">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e9f61-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_certificate.png) 

5. <span data-ttu-id="e9f61-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e9f61-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-pagerduty-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e9f61-166">På den **PagerDuty Configuration** klickar du på **konfigurera PagerDuty** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e9f61-166">On the **PagerDuty Configuration** section, click **Configure PagerDuty** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e9f61-167">Kopiera den **Sign-Out Webbadressen och SAML enkel inloggning Service** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="e9f61-167">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![PagerDuty konfiguration](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_configure.png) 

7. <span data-ttu-id="e9f61-169">Logga in på webbplatsen Pagerduty företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="e9f61-169">In a different web browser window, log into your Pagerduty company site as an administrator.</span></span>

8. <span data-ttu-id="e9f61-170">Klicka på menyn högst upp **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-170">In the menu on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="e9f61-171">![Kontoinställningar](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="e9f61-171">![Account Settings](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "Account Settings")</span></span>

9. <span data-ttu-id="e9f61-172">Klicka på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-172">Click **Single Sign-on**.</span></span>
   
    <span data-ttu-id="e9f61-173">![Enkel inloggning](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="e9f61-173">![Single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "Single sign-on")</span></span>

10. <span data-ttu-id="e9f61-174">På den **aktivera enkel inloggning (SSO)** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9f61-174">On the **Enable Single Sign-on (SSO)** page, perform the following steps:</span></span>
   
    <span data-ttu-id="e9f61-175">![Aktivera enkel inloggning](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "aktivera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="e9f61-175">![Enable single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "Enable single sign-on")</span></span>
   
    <span data-ttu-id="e9f61-176">a.</span><span class="sxs-lookup"><span data-stu-id="e9f61-176">a.</span></span> <span data-ttu-id="e9f61-177">Öppna din Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **X.509-certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="e9f61-177">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox</span></span>
  
    <span data-ttu-id="e9f61-178">b.</span><span class="sxs-lookup"><span data-stu-id="e9f61-178">b.</span></span> <span data-ttu-id="e9f61-179">I den **inloggnings-URL** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e9f61-179">In the **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="e9f61-180">c.</span><span class="sxs-lookup"><span data-stu-id="e9f61-180">c.</span></span> <span data-ttu-id="e9f61-181">I den **logga ut URL** textruta klistra in **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e9f61-181">In the **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="e9f61-182">d.</span><span class="sxs-lookup"><span data-stu-id="e9f61-182">d.</span></span> <span data-ttu-id="e9f61-183">Välj **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-183">Select **Turn on Single Sign-on**.</span></span>
 
    <span data-ttu-id="e9f61-184">e.</span><span class="sxs-lookup"><span data-stu-id="e9f61-184">e.</span></span> <span data-ttu-id="e9f61-185">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-185">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="e9f61-186">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e9f61-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e9f61-187">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e9f61-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e9f61-188">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e9f61-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e9f61-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e9f61-189">Create an Azure AD test user</span></span>

<span data-ttu-id="e9f61-190">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e9f61-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="e9f61-192">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e9f61-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e9f61-193">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e9f61-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e9f61-195">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e9f61-197">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9f61-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e9f61-199">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9f61-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogrutan användare](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e9f61-201">a.</span><span class="sxs-lookup"><span data-stu-id="e9f61-201">a.</span></span> <span data-ttu-id="e9f61-202">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-202">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e9f61-203">b.</span><span class="sxs-lookup"><span data-stu-id="e9f61-203">b.</span></span> <span data-ttu-id="e9f61-204">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e9f61-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e9f61-205">c.</span><span class="sxs-lookup"><span data-stu-id="e9f61-205">c.</span></span> <span data-ttu-id="e9f61-206">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e9f61-207">d.</span><span class="sxs-lookup"><span data-stu-id="e9f61-207">d.</span></span> <span data-ttu-id="e9f61-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-208">Click **Create**.</span></span>
 
### <a name="create-a-pagerduty-test-user"></a><span data-ttu-id="e9f61-209">Skapa en testanvändare PagerDuty</span><span class="sxs-lookup"><span data-stu-id="e9f61-209">Create a PagerDuty test user</span></span>

<span data-ttu-id="e9f61-210">Om du vill aktivera Azure AD-användare kan logga in på PagerDuty etableras de i PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="e9f61-210">To enable Azure AD users to log in to PagerDuty, they must be provisioned into PagerDuty.</span></span>  
<span data-ttu-id="e9f61-211">När det gäller PagerDuty är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e9f61-211">In the case of PagerDuty, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="e9f61-212">Du kan använda något annat Pagerduty användarens konto skapas verktyg eller API: er som tillhandahålls av Pagerduty för att etablera Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="e9f61-212">You can use any other Pagerduty user account creation tools or APIs provided by Pagerduty to provision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="e9f61-213">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="e9f61-213">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="e9f61-214">Logga in på ditt **Pagerduty** klient.</span><span class="sxs-lookup"><span data-stu-id="e9f61-214">Log in to your **Pagerduty** tenant.</span></span>

2. <span data-ttu-id="e9f61-215">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-215">In the menu on the top, click **Users**.</span></span>

3. <span data-ttu-id="e9f61-216">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-216">Click **Add Users**.</span></span>
   
    <span data-ttu-id="e9f61-217">![Lägg till användare](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="e9f61-217">![Add Users](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "Add Users")</span></span>

4.  <span data-ttu-id="e9f61-218">På den **bjuda in ditt team** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9f61-218">On the **Invite your team** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="e9f61-219">![Bjud in ditt team](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "bjuda in ditt team")</span><span class="sxs-lookup"><span data-stu-id="e9f61-219">![Invite your team](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "Invite your team")</span></span>

    <span data-ttu-id="e9f61-220">a.</span><span class="sxs-lookup"><span data-stu-id="e9f61-220">a.</span></span> <span data-ttu-id="e9f61-221">Typ av **första och sista namnet** för användare som **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-221">Type the **First and Last Name** of user like **Britta Simon**.</span></span> 
   
    <span data-ttu-id="e9f61-222">b.</span><span class="sxs-lookup"><span data-stu-id="e9f61-222">b.</span></span> <span data-ttu-id="e9f61-223">Ange **e-post** -adressen för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="e9f61-223">Enter **Email** address of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="e9f61-224">c.</span><span class="sxs-lookup"><span data-stu-id="e9f61-224">c.</span></span> <span data-ttu-id="e9f61-225">Klicka på **Lägg till**, och klicka sedan på **skicka inbjuder**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-225">Click **Add**, and then click **Send Invites**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="e9f61-226">Alla tillagda användare får en inbjudan för att skapa ett PagerDuty-konto.</span><span class="sxs-lookup"><span data-stu-id="e9f61-226">All added users will receive an invite to create a PagerDuty account.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="e9f61-227">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e9f61-227">Assign the Azure AD test user</span></span>

<span data-ttu-id="e9f61-228">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="e9f61-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PagerDuty.</span></span>

![Tilldela rollen][200]

<span data-ttu-id="e9f61-230">**Om du vill tilldela PagerDuty Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e9f61-230">**To assign Britta Simon to PagerDuty, perform the following steps:**</span></span>

1. <span data-ttu-id="e9f61-231">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e9f61-233">Välj i listan med program **PagerDuty**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-233">In the applications list, select **PagerDuty**.</span></span>

    ![Länken PagerDuty i listan med program](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_app.png) 

3. <span data-ttu-id="e9f61-235">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e9f61-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="e9f61-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e9f61-237">Click **Add** button.</span></span> <span data-ttu-id="e9f61-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9f61-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="e9f61-240">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e9f61-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e9f61-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9f61-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e9f61-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9f61-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e9f61-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9f61-243">Test single sign-on</span></span>

<span data-ttu-id="e9f61-244">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e9f61-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e9f61-245">När du klickar på panelen PagerDuty i åtkomst Panelyou bör hämta automatiskt loggat in på ditt PagerDuty program.</span><span class="sxs-lookup"><span data-stu-id="e9f61-245">When you click the PagerDuty tile in the Access Panelyou should get automatically signed-on to your PagerDuty application.</span></span>

<span data-ttu-id="e9f61-246">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e9f61-246">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e9f61-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e9f61-247">Additional resources</span></span>

* [<span data-ttu-id="e9f61-248">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e9f61-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e9f61-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e9f61-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_203.png

