---
title: "Självstudier: Azure Active Directory-integrering med Citrix ShareFile | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Citrix ShareFile."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: b85680104fe4f33638c559b2a12483a2312a4476
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a><span data-ttu-id="7df86-103">Självstudier: Azure Active Directory-integrering med Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="7df86-103">Tutorial: Azure Active Directory integration with Citrix ShareFile</span></span>

<span data-ttu-id="7df86-104">I kursen får lära du att integrera Citrix ShareFile med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7df86-104">In this tutorial, you learn how to integrate Citrix ShareFile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7df86-105">Integrera Citrix ShareFile med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7df86-105">Integrating Citrix ShareFile with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7df86-106">Du kan styra i Azure AD som har åtkomst till Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="7df86-106">You can control in Azure AD who has access to Citrix ShareFile.</span></span>
- <span data-ttu-id="7df86-107">Du kan aktivera användarna att automatiskt hämta loggat in på Citrix ShareFile (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="7df86-107">You can enable your users to automatically get signed-on to Citrix ShareFile (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="7df86-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7df86-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="7df86-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7df86-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7df86-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7df86-110">Prerequisites</span></span>

<span data-ttu-id="7df86-111">Om du vill konfigurera Azure AD-integrering med Citrix ShareFile behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7df86-111">To configure Azure AD integration with Citrix ShareFile, you need the following items:</span></span>

- <span data-ttu-id="7df86-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7df86-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7df86-113">En Citrix ShareFile enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7df86-113">A Citrix ShareFile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7df86-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7df86-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7df86-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7df86-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7df86-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7df86-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7df86-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7df86-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7df86-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7df86-118">Scenario description</span></span>
<span data-ttu-id="7df86-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7df86-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7df86-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7df86-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7df86-121">Lägg till Citrix ShareFile från galleriet</span><span class="sxs-lookup"><span data-stu-id="7df86-121">Add Citrix ShareFile from the gallery</span></span>
2. <span data-ttu-id="7df86-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7df86-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-citrix-sharefile-from-the-gallery"></a><span data-ttu-id="7df86-123">Lägg till Citrix ShareFile från galleriet</span><span class="sxs-lookup"><span data-stu-id="7df86-123">Add Citrix ShareFile from the gallery</span></span>
<span data-ttu-id="7df86-124">Du måste lägga till Citrix ShareFile från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Citrix ShareFile i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7df86-124">To configure the integration of Citrix ShareFile into Azure AD, you need to add Citrix ShareFile from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7df86-125">**Utför följande steg för att lägga till Citrix ShareFile från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7df86-125">**To add Citrix ShareFile from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7df86-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7df86-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="7df86-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7df86-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7df86-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7df86-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="7df86-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7df86-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="7df86-133">I sökrutan skriver **Citrix ShareFile**väljer **Citrix ShareFile** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7df86-133">In the search box, type **Citrix ShareFile**, select **Citrix ShareFile** from result panel then click **Add** button to add the application.</span></span>

    ![Citrix ShareFile i resultatlistan](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="7df86-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7df86-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="7df86-136">Du konfigurera och testa Azure AD enkel inloggning med Citrix ShareFile baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="7df86-136">In this section, you configure and test Azure AD single sign-on with Citrix ShareFile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7df86-137">Azure AD måste du känna till användaren i Citrix ShareFile motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7df86-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Citrix ShareFile is to a user in Azure AD.</span></span> <span data-ttu-id="7df86-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Citrix ShareFile upprättas.</span><span class="sxs-lookup"><span data-stu-id="7df86-138">In other words, a link relationship between an Azure AD user and the related user in Citrix ShareFile needs to be established.</span></span>

<span data-ttu-id="7df86-139">I Citrix ShareFile tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7df86-139">In Citrix ShareFile, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7df86-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Citrix ShareFile, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7df86-140">To configure and test Azure AD single sign-on with Citrix ShareFile, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7df86-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7df86-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7df86-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7df86-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7df86-143">**[Skapa en testanvändare Citrix ShareFile](#create-a-citrix-sharefile-test-user)**  – du har en motsvarighet för Britta Simon i Citrix ShareFile som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7df86-143">**[Create a Citrix ShareFile test user](#create-a-citrix-sharefile-test-user)** - to have a counterpart of Britta Simon in Citrix ShareFile that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7df86-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7df86-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7df86-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7df86-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="7df86-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7df86-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="7df86-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Citrix ShareFile-program.</span><span class="sxs-lookup"><span data-stu-id="7df86-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Citrix ShareFile application.</span></span>

<span data-ttu-id="7df86-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Citrix ShareFile:**</span><span class="sxs-lookup"><span data-stu-id="7df86-148">**To configure Azure AD single sign-on with Citrix ShareFile, perform the following steps:**</span></span>

1. <span data-ttu-id="7df86-149">I Azure-portalen på den **Citrix ShareFile** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7df86-149">In the Azure portal, on the **Citrix ShareFile** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="7df86-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7df86-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. <span data-ttu-id="7df86-153">På den **Citrix ShareFile domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7df86-153">On the **Citrix ShareFile Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Citrix ShareFile domän med enkel inloggning information](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_url.png)
    
    <span data-ttu-id="7df86-155">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant-name>.sharefile.com/saml/login`</span><span class="sxs-lookup"><span data-stu-id="7df86-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.sharefile.com/saml/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7df86-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7df86-156">This value is not real.</span></span> <span data-ttu-id="7df86-157">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="7df86-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="7df86-158">Kontakta [Citrix ShareFile klienten supportteamet](https://www.citrix.co.in/products/sharefile/support.html) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="7df86-158">Contact [Citrix ShareFile Client support team](https://www.citrix.co.in/products/sharefile/support.html) to get this value.</span></span> 

4. <span data-ttu-id="7df86-159">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7df86-159">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_certificate.png) 

5. <span data-ttu-id="7df86-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7df86-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-sharefile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7df86-163">På den **Citrix-konfiguration för ShareFile** klickar du på **konfigurera Citrix ShareFile** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7df86-163">On the **Citrix ShareFile Configuration** section, click **Configure Citrix ShareFile** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7df86-164">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7df86-164">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Citrix ShareFile konfiguration](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_configure.png) 

7. <span data-ttu-id="7df86-166">Logga in i ett annat webbläsarfönster din **Citrix ShareFile** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="7df86-166">In a different web browser window, log into your **Citrix ShareFile** company site as an administrator.</span></span>

8. <span data-ttu-id="7df86-167">Klicka på i verktygsfältet högst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="7df86-167">In the toolbar on the top, click **Admin**.</span></span>

9. <span data-ttu-id="7df86-168">I det vänstra navigeringsfönstret väljer **Konfigurera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7df86-168">In the left navigation pane, select **Configure Single Sign-On**.</span></span>
   
    <span data-ttu-id="7df86-169">![Kontot Administration](./media/active-directory-saas-sharefile-tutorial/ic773627.png "konto Administration")</span><span class="sxs-lookup"><span data-stu-id="7df86-169">![Account Administration](./media/active-directory-saas-sharefile-tutorial/ic773627.png "Account Administration")</span></span>

10. <span data-ttu-id="7df86-170">På den **enkel inloggning / SAML 2.0-konfiguration** dialogrutan sidan under **grundläggande inställningar**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7df86-170">On the **Single Sign-On/ SAML 2.0 Configuration** dialog page under **Basic Settings**, perform the following steps:</span></span>
   
    <span data-ttu-id="7df86-171">![Enkel inloggning](./media/active-directory-saas-sharefile-tutorial/ic773628.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7df86-171">![Single sign-on](./media/active-directory-saas-sharefile-tutorial/ic773628.png "Single sign-on")</span></span>
   
    <span data-ttu-id="7df86-172">a.</span><span class="sxs-lookup"><span data-stu-id="7df86-172">a.</span></span> <span data-ttu-id="7df86-173">Klicka på **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="7df86-173">Click **Enable SAML**.</span></span>
    
    <span data-ttu-id="7df86-174">b.</span><span class="sxs-lookup"><span data-stu-id="7df86-174">b.</span></span> <span data-ttu-id="7df86-175">I **din IDP utfärdaren / enhets-ID** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7df86-175">In **Your IDP Issuer/ Entity ID** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="7df86-176">c.</span><span class="sxs-lookup"><span data-stu-id="7df86-176">c.</span></span> <span data-ttu-id="7df86-177">Klicka på **ändra** bredvid den **X.509-certifikat** fältet och sedan ladda upp det certifikat du laddade ned från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7df86-177">Click **Change** next to the **X.509 Certificate** field and then upload the certificate you downloaded from the Azure portal.</span></span>
    
    <span data-ttu-id="7df86-178">d.</span><span class="sxs-lookup"><span data-stu-id="7df86-178">d.</span></span> <span data-ttu-id="7df86-179">I **Inloggningswebbadressen** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7df86-179">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="7df86-180">e.</span><span class="sxs-lookup"><span data-stu-id="7df86-180">e.</span></span> <span data-ttu-id="7df86-181">I **logga ut URL** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7df86-181">In **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

11. <span data-ttu-id="7df86-182">Klicka på **spara** i Citrix ShareFile management portal.</span><span class="sxs-lookup"><span data-stu-id="7df86-182">Click **Save** on the Citrix ShareFile management portal.</span></span>

> [!TIP]
> <span data-ttu-id="7df86-183">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="7df86-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7df86-184">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="7df86-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7df86-185">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7df86-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="7df86-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7df86-186">Create an Azure AD test user</span></span>

<span data-ttu-id="7df86-187">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7df86-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="7df86-189">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7df86-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7df86-190">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="7df86-190">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="7df86-192">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7df86-192">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="7df86-194">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7df86-194">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="7df86-196">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7df86-196">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png)

    <span data-ttu-id="7df86-198">a.</span><span class="sxs-lookup"><span data-stu-id="7df86-198">a.</span></span> <span data-ttu-id="7df86-199">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7df86-199">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7df86-200">b.</span><span class="sxs-lookup"><span data-stu-id="7df86-200">b.</span></span> <span data-ttu-id="7df86-201">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="7df86-201">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="7df86-202">c.</span><span class="sxs-lookup"><span data-stu-id="7df86-202">c.</span></span> <span data-ttu-id="7df86-203">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="7df86-203">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="7df86-204">d.</span><span class="sxs-lookup"><span data-stu-id="7df86-204">d.</span></span> <span data-ttu-id="7df86-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7df86-205">Click **Create**.</span></span>
 
### <a name="create-a-citrix-sharefile-test-user"></a><span data-ttu-id="7df86-206">Skapa en testanvändare Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="7df86-206">Create a Citrix ShareFile test user</span></span>

<span data-ttu-id="7df86-207">För att aktivera Azure AD-användare kan logga in Citrix ShareFile etableras de i Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="7df86-207">In order to enable Azure AD users to log into Citrix ShareFile, they must be provisioned into Citrix ShareFile.</span></span> <span data-ttu-id="7df86-208">Citrix ShareFile är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7df86-208">In the case of Citrix ShareFile, provisioning is a manual task.</span></span>

<span data-ttu-id="7df86-209">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="7df86-209">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="7df86-210">Logga in på ditt **Citrix ShareFile** klient.</span><span class="sxs-lookup"><span data-stu-id="7df86-210">Log in to your **Citrix ShareFile** tenant.</span></span>

2. <span data-ttu-id="7df86-211">Klicka på **hantera användare \> hantera användare Home \> + skapa medarbetare**.</span><span class="sxs-lookup"><span data-stu-id="7df86-211">Click **Manage Users \> Manage Users Home \> + Create Employee**.</span></span>
   
   <span data-ttu-id="7df86-212">![Skapa medarbetare](./media/active-directory-saas-sharefile-tutorial/IC781050.png "skapa medarbetare")</span><span class="sxs-lookup"><span data-stu-id="7df86-212">![Create Employee](./media/active-directory-saas-sharefile-tutorial/IC781050.png "Create Employee")</span></span>

3. <span data-ttu-id="7df86-213">På den **grundläggande Information** avsnittet, utföra nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="7df86-213">On the **Basic Information** section, perform below steps:</span></span>
   
   <span data-ttu-id="7df86-214">![Grundläggande Information](./media/active-directory-saas-sharefile-tutorial/IC799951.png "grundläggande Information")</span><span class="sxs-lookup"><span data-stu-id="7df86-214">![Basic Information](./media/active-directory-saas-sharefile-tutorial/IC799951.png "Basic Information")</span></span>
   
   <span data-ttu-id="7df86-215">a.</span><span class="sxs-lookup"><span data-stu-id="7df86-215">a.</span></span> <span data-ttu-id="7df86-216">I den **e-postadress** textruta skriver Britta Simon som e-postadress  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="7df86-216">In the **Email Address** textbox, type the email address of Britta Simon as **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="7df86-217">b.</span><span class="sxs-lookup"><span data-stu-id="7df86-217">b.</span></span> <span data-ttu-id="7df86-218">I den **Förnamn** textruta typen **Förnamn** för användare som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="7df86-218">In the **First Name** textbox, type **first name** of user as **Britta**.</span></span>
   
   <span data-ttu-id="7df86-219">c.</span><span class="sxs-lookup"><span data-stu-id="7df86-219">c.</span></span> <span data-ttu-id="7df86-220">I den **efternamn** textruta typen **efternamn** för användare som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="7df86-220">In the **Last Name** textbox, type **last name** of user as **Simon**.</span></span>

4. <span data-ttu-id="7df86-221">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="7df86-221">Click **Add User**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="7df86-222">Azure AD-kontoinnehavaren kommer ett e-postmeddelande och länken för att bekräfta sina konton innan den aktiveras. Du kan använda andra Citrix ShareFile användare skapa verktyg eller API: er som tillhandahålls av Citrix ShareFile för att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="7df86-222">The Azure AD account holder will receive an email and follow a link to confirm their account before it becomes active.You can use any other Citrix ShareFile user account creation tools or APIs provided by Citrix ShareFile to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="7df86-223">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7df86-223">Assign the Azure AD test user</span></span>

<span data-ttu-id="7df86-224">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="7df86-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Citrix ShareFile.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="7df86-226">**Om du vill tilldela Citrix ShareFile Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7df86-226">**To assign Britta Simon to Citrix ShareFile, perform the following steps:**</span></span>

1. <span data-ttu-id="7df86-227">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7df86-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7df86-229">Välj i listan med program **Citrix ShareFile**.</span><span class="sxs-lookup"><span data-stu-id="7df86-229">In the applications list, select **Citrix ShareFile**.</span></span>

    ![Länken Citrix ShareFile i listan med program](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_app.png)  

3. <span data-ttu-id="7df86-231">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7df86-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="7df86-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7df86-233">Click **Add** button.</span></span> <span data-ttu-id="7df86-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7df86-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="7df86-236">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7df86-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7df86-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7df86-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7df86-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7df86-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="7df86-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7df86-239">Test single sign-on</span></span>

<span data-ttu-id="7df86-240">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7df86-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7df86-241">När du klickar på panelen Citrix ShareFile på åtkomstpanelen du bör få automatiskt loggat in på ditt Citrix ShareFile-program.</span><span class="sxs-lookup"><span data-stu-id="7df86-241">When you click the Citrix ShareFile tile in the Access Panel, you should get automatically signed-on to your Citrix ShareFile application.</span></span>
<span data-ttu-id="7df86-242">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7df86-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7df86-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7df86-243">Additional resources</span></span>

* [<span data-ttu-id="7df86-244">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7df86-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7df86-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7df86-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png

