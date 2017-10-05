---
title: "Självstudier: Azure Active Directory-integrering med BlueJeans | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och BlueJeans."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 03bf65852b8d3cf14aebf155891a028db86e78d0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a><span data-ttu-id="83dc3-103">Självstudier: Azure Active Directory-integrering med BlueJeans</span><span class="sxs-lookup"><span data-stu-id="83dc3-103">Tutorial: Azure Active Directory integration with BlueJeans</span></span>

<span data-ttu-id="83dc3-104">I kursen får lära du att integrera BlueJeans med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="83dc3-104">In this tutorial, you learn how to integrate BlueJeans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="83dc3-105">Integrera BlueJeans med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="83dc3-105">Integrating BlueJeans with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="83dc3-106">Du kan styra i Azure AD som har åtkomst till BlueJeans</span><span class="sxs-lookup"><span data-stu-id="83dc3-106">You can control in Azure AD who has access to BlueJeans</span></span>
- <span data-ttu-id="83dc3-107">Du kan aktivera användarna att automatiskt hämta loggat in på BlueJeans (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="83dc3-107">You can enable your users to automatically get signed-on to BlueJeans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="83dc3-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="83dc3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="83dc3-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="83dc3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83dc3-110">Krav</span><span class="sxs-lookup"><span data-stu-id="83dc3-110">Prerequisites</span></span>

<span data-ttu-id="83dc3-111">För att konfigurera Azure AD-integrering med BlueJeans, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="83dc3-111">To configure Azure AD integration with BlueJeans, you need the following items:</span></span>

- <span data-ttu-id="83dc3-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="83dc3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="83dc3-113">En BlueJeans enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="83dc3-113">A BlueJeans single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="83dc3-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="83dc3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="83dc3-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="83dc3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="83dc3-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="83dc3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="83dc3-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="83dc3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="83dc3-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="83dc3-118">Scenario description</span></span>
<span data-ttu-id="83dc3-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="83dc3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="83dc3-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="83dc3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="83dc3-121">Att lägga till BlueJeans från galleriet</span><span class="sxs-lookup"><span data-stu-id="83dc3-121">Adding BlueJeans from the gallery</span></span>
2. <span data-ttu-id="83dc3-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="83dc3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bluejeans-from-the-gallery"></a><span data-ttu-id="83dc3-123">Att lägga till BlueJeans från galleriet</span><span class="sxs-lookup"><span data-stu-id="83dc3-123">Adding BlueJeans from the gallery</span></span>
<span data-ttu-id="83dc3-124">Du måste lägga till BlueJeans från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av BlueJeans i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83dc3-124">To configure the integration of BlueJeans into Azure AD, you need to add BlueJeans from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="83dc3-125">**Utför följande steg för att lägga till BlueJeans från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="83dc3-125">**To add BlueJeans from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="83dc3-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="83dc3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="83dc3-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="83dc3-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="83dc3-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="83dc3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="83dc3-133">I sökrutan skriver **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-133">In the search box, type **BlueJeans**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. <span data-ttu-id="83dc3-135">Välj i resultatpanelen **BlueJeans**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="83dc3-135">In the results panel, select **BlueJeans**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="83dc3-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="83dc3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="83dc3-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BlueJeans baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="83dc3-138">In this section, you configure and test Azure AD single sign-on with BlueJeans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="83dc3-139">Azure AD måste du känna till användaren i BlueJeans motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="83dc3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BlueJeans is to a user in Azure AD.</span></span> <span data-ttu-id="83dc3-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i BlueJeans upprättas.</span><span class="sxs-lookup"><span data-stu-id="83dc3-140">In other words, a link relationship between an Azure AD user and the related user in BlueJeans needs to be established.</span></span>

<span data-ttu-id="83dc3-141">I BlueJeans, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="83dc3-141">In BlueJeans, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="83dc3-142">Om du vill konfigurera och testa Azure AD enkel inloggning med BlueJeans, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="83dc3-142">To configure and test Azure AD single sign-on with BlueJeans, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="83dc3-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="83dc3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="83dc3-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="83dc3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="83dc3-145">**[Skapa en testanvändare BlueJeans](#creating-a-bluejeans-test-user)**  – du har en motsvarighet för Britta Simon i BlueJeans som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="83dc3-145">**[Creating a BlueJeans test user](#creating-a-bluejeans-test-user)** - to have a counterpart of Britta Simon in BlueJeans that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="83dc3-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="83dc3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="83dc3-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="83dc3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="83dc3-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="83dc3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="83dc3-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt BlueJeans program.</span><span class="sxs-lookup"><span data-stu-id="83dc3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BlueJeans application.</span></span>

<span data-ttu-id="83dc3-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med BlueJeans:**</span><span class="sxs-lookup"><span data-stu-id="83dc3-150">**To configure Azure AD single sign-on with BlueJeans, perform the following steps:**</span></span>

1. <span data-ttu-id="83dc3-151">I Azure-portalen på den **BlueJeans** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-151">In the Azure portal, on the **BlueJeans** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="83dc3-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="83dc3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. <span data-ttu-id="83dc3-155">På den **BlueJeans domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="83dc3-155">On the **BlueJeans Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    <span data-ttu-id="83dc3-157">a.</span><span class="sxs-lookup"><span data-stu-id="83dc3-157">a.</span></span> <span data-ttu-id="83dc3-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="83dc3-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    <span data-ttu-id="83dc3-159">b.</span><span class="sxs-lookup"><span data-stu-id="83dc3-159">b.</span></span> <span data-ttu-id="83dc3-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="83dc3-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="83dc3-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="83dc3-161">These values are not real.</span></span> <span data-ttu-id="83dc3-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="83dc3-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="83dc3-163">Kontakta [BlueJeans klienten supportteamet](https://support.bluejeans.com/contact) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="83dc3-163">Contact [BlueJeans Client support team](https://support.bluejeans.com/contact) to get these values.</span></span> 
 
4. <span data-ttu-id="83dc3-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="83dc3-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. <span data-ttu-id="83dc3-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="83dc3-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="83dc3-168">På den **BlueJeans Configuration** klickar du på **konfigurera BlueJeans** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="83dc3-168">On the **BlueJeans Configuration** section, click **Configure BlueJeans** to open **Configure sign-on** window.</span></span> <span data-ttu-id="83dc3-169">Kopiera den **Sign-Out URL, ändra lösenord URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="83dc3-169">Copy the **Sign-Out URL, Change Password URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. <span data-ttu-id="83dc3-171">I en annan webbläsarfönstret, logga in på ditt **BlueJeans** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="83dc3-171">In a different web browser window, log in to your **BlueJeans** company site as an administrator.</span></span>

8. <span data-ttu-id="83dc3-172">Gå till **ADMIN \> gruppinställningarna \> säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-172">Go to **ADMIN \> Group Settings \> Security**.</span></span>
   
   <span data-ttu-id="83dc3-173">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="83dc3-173">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")</span></span>

9. <span data-ttu-id="83dc3-174">I den **säkerhet** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="83dc3-174">In the **Security** section, perform the following steps:</span></span>
   
   <span data-ttu-id="83dc3-175">![SAML för enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML för enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="83dc3-175">![SAML Single Sign On](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML Single Sign On")</span></span>   
   
   <span data-ttu-id="83dc3-176">a.</span><span class="sxs-lookup"><span data-stu-id="83dc3-176">a.</span></span> <span data-ttu-id="83dc3-177">Välj **SAML för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-177">Select **SAML Single Sign On**.</span></span>
  
   <span data-ttu-id="83dc3-178">b.</span><span class="sxs-lookup"><span data-stu-id="83dc3-178">b.</span></span> <span data-ttu-id="83dc3-179">Välj **aktivera automatisk etablering**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-179">Select **Enable automatic provisioning**.</span></span>

10. <span data-ttu-id="83dc3-180">Gå vidare med följande steg:</span><span class="sxs-lookup"><span data-stu-id="83dc3-180">Move on with the following steps:</span></span>

    <span data-ttu-id="83dc3-181">![Certifikat sökvägen](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "certifikat sökväg")</span><span class="sxs-lookup"><span data-stu-id="83dc3-181">![Certificate Path](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Certificate Path")</span></span>
    
    <span data-ttu-id="83dc3-182">a.</span><span class="sxs-lookup"><span data-stu-id="83dc3-182">a.</span></span> <span data-ttu-id="83dc3-183">Klicka på **Välj fil**, och sedan ladda upp hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="83dc3-183">Click **Choose File**, and then upload the downloaded certificate.</span></span>
   
    <span data-ttu-id="83dc3-184">b.</span><span class="sxs-lookup"><span data-stu-id="83dc3-184">b.</span></span> <span data-ttu-id="83dc3-185">Klistra in **SAML enkel inloggning Tjänstwebbadress** till den **inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="83dc3-185">Paste **SAML Single Sign-On Service URL** into the **Login URL** textbox.</span></span>
   
    <span data-ttu-id="83dc3-186">c.</span><span class="sxs-lookup"><span data-stu-id="83dc3-186">c.</span></span> <span data-ttu-id="83dc3-187">Klistra in **ändra lösenord URL** till den **lösenord ändra URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="83dc3-187">Paste **Change Password URL** into the **Password Change URL** textbox.</span></span>
   
    <span data-ttu-id="83dc3-188">d.</span><span class="sxs-lookup"><span data-stu-id="83dc3-188">d.</span></span> <span data-ttu-id="83dc3-189">Klistra in **Sign-Out URL** till den **logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="83dc3-189">Paste **Sign-Out URL** into the **Logout URL** textbox.</span></span>

11. <span data-ttu-id="83dc3-190">Gå vidare med följande steg:</span><span class="sxs-lookup"><span data-stu-id="83dc3-190">Move on with the following steps:</span></span>
    
    <span data-ttu-id="83dc3-191">![Spara ändringarna](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "spara ändringar")</span><span class="sxs-lookup"><span data-stu-id="83dc3-191">![Save Changes](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Save Changes")</span></span>
    
    <span data-ttu-id="83dc3-192">a.</span><span class="sxs-lookup"><span data-stu-id="83dc3-192">a.</span></span> <span data-ttu-id="83dc3-193">I den **användar-id** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="83dc3-193">In the **User id** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="83dc3-194">b.</span><span class="sxs-lookup"><span data-stu-id="83dc3-194">b.</span></span> <span data-ttu-id="83dc3-195">I den **e-post** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="83dc3-195">In the **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="83dc3-196">c.</span><span class="sxs-lookup"><span data-stu-id="83dc3-196">c.</span></span> <span data-ttu-id="83dc3-197">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-197">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="83dc3-198">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="83dc3-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="83dc3-199">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="83dc3-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="83dc3-200">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="83dc3-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="83dc3-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="83dc3-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="83dc3-202">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="83dc3-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="83dc3-204">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="83dc3-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="83dc3-205">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="83dc3-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="83dc3-207">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="83dc3-209">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="83dc3-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="83dc3-211">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="83dc3-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="83dc3-213">a.</span><span class="sxs-lookup"><span data-stu-id="83dc3-213">a.</span></span> <span data-ttu-id="83dc3-214">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="83dc3-215">b.</span><span class="sxs-lookup"><span data-stu-id="83dc3-215">b.</span></span> <span data-ttu-id="83dc3-216">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="83dc3-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="83dc3-217">c.</span><span class="sxs-lookup"><span data-stu-id="83dc3-217">c.</span></span> <span data-ttu-id="83dc3-218">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="83dc3-219">d.</span><span class="sxs-lookup"><span data-stu-id="83dc3-219">d.</span></span> <span data-ttu-id="83dc3-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-220">Click **Create**.</span></span>
 
### <a name="creating-a-bluejeans-test-user"></a><span data-ttu-id="83dc3-221">Skapa en testanvändare BlueJeans</span><span class="sxs-lookup"><span data-stu-id="83dc3-221">Creating a BlueJeans test user</span></span>

<span data-ttu-id="83dc3-222">Om du vill aktivera Azure AD-användare kan logga in på BlueJeans etableras de i BlueJeans.</span><span class="sxs-lookup"><span data-stu-id="83dc3-222">To enable Azure AD users to log in to BlueJeans, they must be provisioned into BlueJeans.</span></span>  

<span data-ttu-id="83dc3-223">Vid BlueJeans är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="83dc3-223">In case of BlueJeans, provisioning is a manual task.</span></span>

<span data-ttu-id="83dc3-224">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="83dc3-224">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="83dc3-225">Logga in på ditt **BlueJeans** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="83dc3-225">Log in to your **BlueJeans** company site as an administrator.</span></span>

2. <span data-ttu-id="83dc3-226">Gå till **ADMIN \> hantera användare \> lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-226">Go to **ADMIN \> Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="83dc3-227">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="83dc3-227">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")</span></span>
   
   >[!IMPORTANT]
   ><span data-ttu-id="83dc3-228">Den **Lägg till användare** fliken är endast tillgänglig om i den **säkerhetsfliken**, **aktivera automatisk etablering** är avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="83dc3-228">The **Add User** tab is only available if, in the **Security tab**, **Enable automatic provisioning** is unchecked.</span></span> 
   
3. <span data-ttu-id="83dc3-229">I den **Lägg till användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="83dc3-229">In the **Add User** section, perform the following steps:</span></span>

    <span data-ttu-id="83dc3-230">![Lägg till användare](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="83dc3-230">![Add User](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Add User")</span></span>
    
    <span data-ttu-id="83dc3-231">a.</span><span class="sxs-lookup"><span data-stu-id="83dc3-231">a.</span></span> <span data-ttu-id="83dc3-232">Ange en **BlueJeans användarnamn**, en **e-postadress**, **BlueJeans möte ID**, **kontrollant lösenord**, en **fullständigt namn** , **företagets** av en giltig AAD-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="83dc3-232">Type a **BlueJeans Username**, an **Email address**, a **BlueJeans Meeting ID**, a **Moderator Passcode**, a **Full Name**, the **Company** of a valid AAD account you want to provision into the related textboxes.</span></span>
    
    <span data-ttu-id="83dc3-233">b.</span><span class="sxs-lookup"><span data-stu-id="83dc3-233">b.</span></span> <span data-ttu-id="83dc3-234">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-234">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="83dc3-235">Du kan använda något annat BlueJeans användarens konto skapas verktyg eller API: er som tillhandahålls av BlueJeans att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="83dc3-235">You can use any other BlueJeans user account creation tools or APIs provided by BlueJeans to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="83dc3-236">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="83dc3-236">Assigning the Azure AD test user</span></span>

<span data-ttu-id="83dc3-237">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till BlueJeans.</span><span class="sxs-lookup"><span data-stu-id="83dc3-237">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BlueJeans.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="83dc3-239">**Om du vill tilldela BlueJeans Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="83dc3-239">**To assign Britta Simon to BlueJeans, perform the following steps:**</span></span>

1. <span data-ttu-id="83dc3-240">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-240">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="83dc3-242">Välj i listan med program **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-242">In the applications list, select **BlueJeans**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. <span data-ttu-id="83dc3-244">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="83dc3-244">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="83dc3-246">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="83dc3-246">Click **Add** button.</span></span> <span data-ttu-id="83dc3-247">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="83dc3-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="83dc3-249">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="83dc3-249">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="83dc3-250">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="83dc3-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="83dc3-251">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="83dc3-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="83dc3-252">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="83dc3-252">Testing single sign-on</span></span>

<span data-ttu-id="83dc3-253">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="83dc3-253">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="83dc3-254">Du bör få inloggningssidan för BlueJeans program när du klickar på panelen BlueJeans på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="83dc3-254">When you click the BlueJeans tile in the Access Panel, you should get login page of BlueJeans application.</span></span>
<span data-ttu-id="83dc3-255">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="83dc3-255">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="83dc3-256">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="83dc3-256">Additional resources</span></span>

* [<span data-ttu-id="83dc3-257">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="83dc3-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="83dc3-258">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="83dc3-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

