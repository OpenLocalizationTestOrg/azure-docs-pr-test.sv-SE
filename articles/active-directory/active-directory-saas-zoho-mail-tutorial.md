---
title: "Självstudier: Azure Active Directory-integrering med Zoho | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Zoho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: f0688cb75584ada805b944d2ef5409d66ab37339
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a><span data-ttu-id="c1c2c-103">Självstudier: Azure Active Directory-integrering med Zoho</span><span class="sxs-lookup"><span data-stu-id="c1c2c-103">Tutorial: Azure Active Directory integration with Zoho</span></span>

<span data-ttu-id="c1c2c-104">I kursen får lära du att integrera Zoho med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c1c2c-104">In this tutorial, you learn how to integrate Zoho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c1c2c-105">Integrera Zoho med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-105">Integrating Zoho with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c1c2c-106">Du kan styra i Azure AD som har åtkomst till Zoho.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-106">You can control in Azure AD who has access to Zoho.</span></span>
- <span data-ttu-id="c1c2c-107">Du kan aktivera användarna att automatiskt hämta loggat in på Zoho (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-107">You can enable your users to automatically get signed-on to Zoho (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c1c2c-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="c1c2c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c1c2c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1c2c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c1c2c-110">Prerequisites</span></span>

<span data-ttu-id="c1c2c-111">För att konfigurera Azure AD-integrering med Zoho, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-111">To configure Azure AD integration with Zoho, you need the following items:</span></span>

- <span data-ttu-id="c1c2c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c1c2c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c1c2c-113">En Zoho enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c1c2c-113">A Zoho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c1c2c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c1c2c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c1c2c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c1c2c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c1c2c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c1c2c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c1c2c-118">Scenario description</span></span>
<span data-ttu-id="c1c2c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c1c2c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c1c2c-121">Att lägga till Zoho från galleriet</span><span class="sxs-lookup"><span data-stu-id="c1c2c-121">Adding Zoho from the gallery</span></span>
2. <span data-ttu-id="c1c2c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c1c2c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoho-from-the-gallery"></a><span data-ttu-id="c1c2c-123">Att lägga till Zoho från galleriet</span><span class="sxs-lookup"><span data-stu-id="c1c2c-123">Adding Zoho from the gallery</span></span>
<span data-ttu-id="c1c2c-124">Du måste lägga till Zoho från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Zoho i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-124">To configure the integration of Zoho into Azure AD, you need to add Zoho from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c1c2c-125">**Utför följande steg för att lägga till Zoho från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c1c2c-125">**To add Zoho from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c1c2c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="c1c2c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c1c2c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="c1c2c-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="c1c2c-133">I sökrutan skriver **Zoho**väljer **Zoho** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-133">In the search box, type **Zoho**, select **Zoho** from result panel then click **Add** button to add the application.</span></span>

    ![Zoho i resultatlistan](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c1c2c-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c1c2c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c1c2c-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zoho baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-136">In this section, you configure and test Azure AD single sign-on with Zoho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c1c2c-137">Azure AD måste du känna till användaren i Zoho motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Zoho is to a user in Azure AD.</span></span> <span data-ttu-id="c1c2c-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Zoho upprättas.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-138">In other words, a link relationship between an Azure AD user and the related user in Zoho needs to be established.</span></span>

<span data-ttu-id="c1c2c-139">I Zoho, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-139">In Zoho, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c1c2c-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Zoho, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-140">To configure and test Azure AD single sign-on with Zoho, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c1c2c-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c1c2c-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c1c2c-143">**[Skapa en testanvändare Zoho](#create-a-zoho-test-user)**  – du har en motsvarighet för Britta Simon i Zoho som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-143">**[Create a Zoho test user](#create-a-zoho-test-user)** - to have a counterpart of Britta Simon in Zoho that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c1c2c-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c1c2c-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c1c2c-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c1c2c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c1c2c-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Zoho program.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zoho application.</span></span>

<span data-ttu-id="c1c2c-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Zoho:**</span><span class="sxs-lookup"><span data-stu-id="c1c2c-148">**To configure Azure AD single sign-on with Zoho, perform the following steps:**</span></span>

1. <span data-ttu-id="c1c2c-149">I Azure-portalen på den **Zoho** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-149">In the Azure portal, on the **Zoho** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="c1c2c-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. <span data-ttu-id="c1c2c-153">På den **Zoho domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-153">On the **Zoho Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Zoho domän med enkel inloggning information](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_url.png)

    <span data-ttu-id="c1c2c-155">a.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-155">a.</span></span> <span data-ttu-id="c1c2c-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.zohomail.com`</span><span class="sxs-lookup"><span data-stu-id="c1c2c-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.zohomail.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c1c2c-157">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-157">This value is not real.</span></span> <span data-ttu-id="c1c2c-158">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-158">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="c1c2c-159">Kontakta [Zoho klienten supportteamet](https://www.zoho.com/mail/contact.html) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-159">Contact [Zoho Client support team](https://www.zoho.com/mail/contact.html) to get this value.</span></span> 
 
4. <span data-ttu-id="c1c2c-160">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-160">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. <span data-ttu-id="c1c2c-162">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-162">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c1c2c-164">På den **Zoho Configuration** klickar du på **konfigurera Zoho** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-164">On the **Zoho Configuration** section, click **Configure Zoho** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c1c2c-165">Kopiera den **Sign-Out URL, ändra lösenord URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c1c2c-165">Copy the **Sign-Out URL, Change Password URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Zoho konfiguration](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. <span data-ttu-id="c1c2c-167">Logga in på webbplatsen för företagets Zoho e-post som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-167">In a different web browser window, log into your Zoho Mail company site as an administrator.</span></span>

8. <span data-ttu-id="c1c2c-168">Gå till den **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-168">Go to the **Control panel**.</span></span>
   
    <span data-ttu-id="c1c2c-169">![Kontrollpanelen](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Kontrollpanelen")</span><span class="sxs-lookup"><span data-stu-id="c1c2c-169">![Control Panel](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Control Panel")</span></span>

9. <span data-ttu-id="c1c2c-170">Klicka på den **SAML-autentisering** fliken.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-170">Click the **SAML Authentication** tab.</span></span>
   
    <span data-ttu-id="c1c2c-171">![SAML-autentisering](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="c1c2c-171">![SAML Authentication](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML Authentication")</span></span>

10. <span data-ttu-id="c1c2c-172">I den **information om autentisering av SAML** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-172">In the **SAML Authentication Details** section, perform the following steps:</span></span>
   
    <span data-ttu-id="c1c2c-173">![Information om autentisering av SAML](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML-autentisering-information")</span><span class="sxs-lookup"><span data-stu-id="c1c2c-173">![SAML Authentication Details](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML Authentication Details")</span></span>
   
    <span data-ttu-id="c1c2c-174">a.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-174">a.</span></span> <span data-ttu-id="c1c2c-175">I den **inloggnings-URL** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-175">In the **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="c1c2c-176">b.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-176">b.</span></span> <span data-ttu-id="c1c2c-177">I den **logga ut URL** textruta klistra in **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-177">In the **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="c1c2c-178">c.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-178">c.</span></span> <span data-ttu-id="c1c2c-179">I den **ändra lösenord URL** textruta klistra in **ändra lösenord URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-179">In the **Change Password URL** textbox, paste **Change Password URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="c1c2c-180">d.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-180">d.</span></span> <span data-ttu-id="c1c2c-181">Öppna din Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **PublicKey** textruta.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-181">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **PublicKey** textbox.</span></span>
   
    <span data-ttu-id="c1c2c-182">e.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-182">e.</span></span> <span data-ttu-id="c1c2c-183">Som **algoritmen**väljer **RSA**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-183">As **Algorithm**, select **RSA**.</span></span>
   
    <span data-ttu-id="c1c2c-184">f.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-184">f.</span></span> <span data-ttu-id="c1c2c-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-185">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="c1c2c-186">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c1c2c-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c1c2c-187">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c1c2c-188">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c1c2c-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c1c2c-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1c2c-189">Create an Azure AD test user</span></span>

<span data-ttu-id="c1c2c-190">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="c1c2c-192">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c1c2c-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c1c2c-193">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-193">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c1c2c-195">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-195">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c1c2c-197">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-197">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c1c2c-199">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-199">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c1c2c-201">a.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-201">a.</span></span> <span data-ttu-id="c1c2c-202">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-202">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c1c2c-203">b.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-203">b.</span></span> <span data-ttu-id="c1c2c-204">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-204">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="c1c2c-205">c.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-205">c.</span></span> <span data-ttu-id="c1c2c-206">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-206">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="c1c2c-207">d.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-207">d.</span></span> <span data-ttu-id="c1c2c-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-208">Click **Create**.</span></span>
 
### <a name="create-a-zoho-test-user"></a><span data-ttu-id="c1c2c-209">Skapa en testanvändare Zoho</span><span class="sxs-lookup"><span data-stu-id="c1c2c-209">Create a Zoho test user</span></span>

<span data-ttu-id="c1c2c-210">För att aktivera Azure AD-användare att logga in på Zoho e-post, måste de etableras i Zoho e-post.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-210">In order to enable Azure AD users to log into Zoho Mail, they must be provisioned into Zoho Mail.</span></span> <span data-ttu-id="c1c2c-211">När det gäller Zoho e-post är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-211">In the case of Zoho Mail, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="c1c2c-212">Du kan använda något annat Zoho e användarens konto skapas verktyg eller API: er som tillhandahålls av Zoho e-post till etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-212">You can use any other Zoho Mail user account creation tools or APIs provided by Zoho Mail to provision AAD user accounts.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="c1c2c-213">Utför följande steg om du vill konfigurera ett användarkonto:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-213">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="c1c2c-214">Logga in på ditt **Zoho e** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-214">Log in to your **Zoho Mail** company site as an administrator.</span></span>

2. <span data-ttu-id="c1c2c-215">Gå till **Kontrollpanelen \> e-post och dokument**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-215">Go to **Control Panel \> Mail & Docs**.</span></span>

3. <span data-ttu-id="c1c2c-216">Gå till **användarinformation \> lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-216">Go to **User Details \> Add User**.</span></span>
   
    <span data-ttu-id="c1c2c-217">![Lägg till användare](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="c1c2c-217">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "Add User")</span></span>

4. <span data-ttu-id="c1c2c-218">På den **lägga till användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c1c2c-218">On the **Add users** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="c1c2c-219">![Lägg till användare](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="c1c2c-219">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "Add User")</span></span>
   
    <span data-ttu-id="c1c2c-220">a.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-220">a.</span></span> <span data-ttu-id="c1c2c-221">I den **Förnamn** textruta Ange först namnet på användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-221">In the **First Name** textbox, type the first name of user like **Britta**.</span></span>

    <span data-ttu-id="c1c2c-222">b.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-222">b.</span></span> <span data-ttu-id="c1c2c-223">I den **efternamn** textruta typen efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-223">In the **Last Name** textbox, type the last name of user like **Simon**.</span></span>

    <span data-ttu-id="c1c2c-224">c.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-224">c.</span></span> <span data-ttu-id="c1c2c-225">I den **e-post-ID** textruta e-post-id typ av användare som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="c1c2c-225">In the **Email ID** textbox, type the email id of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="c1c2c-226">d.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-226">d.</span></span> <span data-ttu-id="c1c2c-227">I den **lösenord** textruta ange lösenord för användaren.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-227">In the **Password** textbox, enter password of user.</span></span>
   
    <span data-ttu-id="c1c2c-228">e.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-228">e.</span></span> <span data-ttu-id="c1c2c-229">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-229">Click **OK**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="c1c2c-230">Azure Active Directory kontoinnehavaren får ett e-postmeddelande med en länk till bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-230">The Azure Active Directory account holder will receive an email with a link to confirm the account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c1c2c-231">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c1c2c-231">Assign the Azure AD test user</span></span>

<span data-ttu-id="c1c2c-232">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Zoho.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zoho.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="c1c2c-234">**Om du vill tilldela Zoho Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c1c2c-234">**To assign Britta Simon to Zoho, perform the following steps:**</span></span>

1. <span data-ttu-id="c1c2c-235">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c1c2c-237">Välj i listan med program **Zoho**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-237">In the applications list, select **Zoho**.</span></span>

    ![Länken Zoho i listan med program](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_app.png)  

3. <span data-ttu-id="c1c2c-239">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="c1c2c-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-241">Click **Add** button.</span></span> <span data-ttu-id="c1c2c-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="c1c2c-244">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c1c2c-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c1c2c-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c1c2c-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c1c2c-247">Test single sign-on</span></span>

<span data-ttu-id="c1c2c-248">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-248">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c1c2c-249">När du klickar på panelen Zoho på åtkomstpanelen du bör få automatiskt loggat in på ditt Zoho program.</span><span class="sxs-lookup"><span data-stu-id="c1c2c-249">When you click the Zoho tile in the Access Panel, you should get automatically signed-on to your Zoho application.</span></span>
<span data-ttu-id="c1c2c-250">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c1c2c-250">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c1c2c-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c1c2c-251">Additional resources</span></span>

* [<span data-ttu-id="c1c2c-252">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c1c2c-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c1c2c-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c1c2c-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_203.png

