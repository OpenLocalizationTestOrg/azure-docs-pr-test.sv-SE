---
title: "Självstudier: Azure Active Directory-integrering med AppDynamics | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och AppDynamics."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 25fd1df0-411c-4f55-8be3-4273b543100f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 634e68bdb937eba68b27b824dc62fe2677e24ffe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appdynamics"></a><span data-ttu-id="1661c-103">Självstudier: Azure Active Directory-integrering med AppDynamics</span><span class="sxs-lookup"><span data-stu-id="1661c-103">Tutorial: Azure Active Directory integration with AppDynamics</span></span>

<span data-ttu-id="1661c-104">I kursen får lära du att integrera AppDynamics med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1661c-104">In this tutorial, you learn how to integrate AppDynamics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1661c-105">Integrera AppDynamics med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1661c-105">Integrating AppDynamics with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1661c-106">Du kan styra i Azure AD som har åtkomst till AppDynamics</span><span class="sxs-lookup"><span data-stu-id="1661c-106">You can control in Azure AD who has access to AppDynamics</span></span>
- <span data-ttu-id="1661c-107">Du kan aktivera användarna att automatiskt hämta loggat in på AppDynamics (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1661c-107">You can enable your users to automatically get signed-on to AppDynamics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1661c-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1661c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1661c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1661c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1661c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1661c-110">Prerequisites</span></span>

<span data-ttu-id="1661c-111">För att konfigurera Azure AD-integrering med AppDynamics, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="1661c-111">To configure Azure AD integration with AppDynamics, you need the following items:</span></span>

- <span data-ttu-id="1661c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1661c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1661c-113">En AppDynamics enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="1661c-113">An AppDynamics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1661c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1661c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1661c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1661c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1661c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1661c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1661c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1661c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1661c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1661c-118">Scenario description</span></span>
<span data-ttu-id="1661c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1661c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1661c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1661c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1661c-121">Att lägga till AppDynamics från galleriet</span><span class="sxs-lookup"><span data-stu-id="1661c-121">Adding AppDynamics from the gallery</span></span>
2. <span data-ttu-id="1661c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1661c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appdynamics-from-the-gallery"></a><span data-ttu-id="1661c-123">Att lägga till AppDynamics från galleriet</span><span class="sxs-lookup"><span data-stu-id="1661c-123">Adding AppDynamics from the gallery</span></span>
<span data-ttu-id="1661c-124">Du måste lägga till AppDynamics från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av AppDynamics i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1661c-124">To configure the integration of AppDynamics into Azure AD, you need to add AppDynamics from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1661c-125">**Utför följande steg för att lägga till AppDynamics från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="1661c-125">**To add AppDynamics from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1661c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1661c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1661c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1661c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1661c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1661c-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="1661c-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1661c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="1661c-133">I sökrutan skriver **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="1661c-133">In the search box, type **AppDynamics**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_search.png)

5. <span data-ttu-id="1661c-135">Välj i resultatpanelen **AppDynamics**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="1661c-135">In the results panel, select **AppDynamics**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1661c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1661c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1661c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AppDynamics baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1661c-138">In this section, you configure and test Azure AD single sign-on with AppDynamics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1661c-139">Azure AD måste du känna till användaren i AppDynamics motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="1661c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AppDynamics is to a user in Azure AD.</span></span> <span data-ttu-id="1661c-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i AppDynamics upprättas.</span><span class="sxs-lookup"><span data-stu-id="1661c-140">In other words, a link relationship between an Azure AD user and the related user in AppDynamics needs to be established.</span></span>

<span data-ttu-id="1661c-141">I AppDynamics, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="1661c-141">In AppDynamics, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1661c-142">Om du vill konfigurera och testa Azure AD enkel inloggning med AppDynamics, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1661c-142">To configure and test Azure AD single sign-on with AppDynamics, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1661c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1661c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1661c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1661c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1661c-145">**[Skapa en testanvändare AppDynamics](#creating-an-appdynamics-test-user)**  – du har en motsvarighet för Britta Simon i AppDynamics som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1661c-145">**[Creating an AppDynamics test user](#creating-an-appdynamics-test-user)** - to have a counterpart of Britta Simon in AppDynamics that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1661c-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1661c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1661c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1661c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1661c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1661c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1661c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt AppDynamics program.</span><span class="sxs-lookup"><span data-stu-id="1661c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AppDynamics application.</span></span>

<span data-ttu-id="1661c-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med AppDynamics:**</span><span class="sxs-lookup"><span data-stu-id="1661c-150">**To configure Azure AD single sign-on with AppDynamics, perform the following steps:**</span></span>

1. <span data-ttu-id="1661c-151">I Azure-portalen på den **AppDynamics** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1661c-151">In the Azure portal, on the **AppDynamics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="1661c-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1661c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_samlbase.png)

3. <span data-ttu-id="1661c-155">På den **AppDynamics domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1661c-155">On the **AppDynamics Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_url.png)

    <span data-ttu-id="1661c-157">a.</span><span class="sxs-lookup"><span data-stu-id="1661c-157">a.</span></span> <span data-ttu-id="1661c-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.saas.appdynamics.com`</span><span class="sxs-lookup"><span data-stu-id="1661c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.saas.appdynamics.com`</span></span>

    <span data-ttu-id="1661c-159">b.</span><span class="sxs-lookup"><span data-stu-id="1661c-159">b.</span></span> <span data-ttu-id="1661c-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.saas.appdynamics.com/controller`</span><span class="sxs-lookup"><span data-stu-id="1661c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.saas.appdynamics.com/controller`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1661c-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="1661c-161">These values are not real.</span></span> <span data-ttu-id="1661c-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="1661c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1661c-163">Kontakta [AppDynamics klienten supportteamet](https://www.appdynamics.com/support/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="1661c-163">Contact [AppDynamics Client support team](https://www.appdynamics.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="1661c-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="1661c-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_certificate.png) 

5. <span data-ttu-id="1661c-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1661c-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1661c-168">På den **AppDynamics Configuration** klickar du på **konfigurera AppDynamics** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="1661c-168">On the **AppDynamics Configuration** section, click **Configure AppDynamics** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1661c-169">Kopiera den **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="1661c-169">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_configure.png) 

7. <span data-ttu-id="1661c-171">I en annan webbläsarfönster loggar du in på webbplatsen AppDynamics företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="1661c-171">In a different web browser window, log in to your AppDynamics company site as an administrator.</span></span>

8. <span data-ttu-id="1661c-172">Klicka på i verktygsfältet högst upp **inställningar**, och klicka sedan på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="1661c-172">In the toolbar on the top, click **Settings**, and then click **Administration**.</span></span>
   
    <span data-ttu-id="1661c-173">![Administration](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="1661c-173">![Administration](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "Administration")</span></span>

9. <span data-ttu-id="1661c-174">Klicka på den **autentiseringsprovider** fliken.</span><span class="sxs-lookup"><span data-stu-id="1661c-174">Click the **Authentication Provider** tab.</span></span>
   
    <span data-ttu-id="1661c-175">![Autentiseringsprovider](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "autentiseringsprovider")</span><span class="sxs-lookup"><span data-stu-id="1661c-175">![Authentication Provider](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "Authentication Provider")</span></span>

10. <span data-ttu-id="1661c-176">I den **autentiseringsprovider** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1661c-176">In the **Authentication Provider** section, perform the following steps:</span></span>
   
    <span data-ttu-id="1661c-177">![SAML-konfiguration](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="1661c-177">![SAML Configuration](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML Configuration")</span></span>   

    <span data-ttu-id="1661c-178">a.</span><span class="sxs-lookup"><span data-stu-id="1661c-178">a.</span></span> <span data-ttu-id="1661c-179">Som **autentiseringsprovider**väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="1661c-179">As **Authentication Provider**, select **SAML**.</span></span>

    <span data-ttu-id="1661c-180">b.</span><span class="sxs-lookup"><span data-stu-id="1661c-180">b.</span></span> <span data-ttu-id="1661c-181">I den **Inloggningswebbadressen** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1661c-181">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="1661c-182">c.</span><span class="sxs-lookup"><span data-stu-id="1661c-182">c.</span></span> <span data-ttu-id="1661c-183">I den **logga ut URL** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1661c-183">In the **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="1661c-184">d.</span><span class="sxs-lookup"><span data-stu-id="1661c-184">d.</span></span> <span data-ttu-id="1661c-185">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="1661c-185">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Certificate** textbox</span></span>

    <span data-ttu-id="1661c-186">e.</span><span class="sxs-lookup"><span data-stu-id="1661c-186">e.</span></span> <span data-ttu-id="1661c-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1661c-187">Click **Save**.</span></span>

     <span data-ttu-id="1661c-188">![Spara](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "spara")</span><span class="sxs-lookup"><span data-stu-id="1661c-188">![Save](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "Save")</span></span>

> [!TIP]
> <span data-ttu-id="1661c-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="1661c-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1661c-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="1661c-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1661c-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1661c-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1661c-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1661c-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="1661c-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1661c-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="1661c-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="1661c-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1661c-196">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1661c-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1661c-198">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1661c-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1661c-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1661c-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1661c-202">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1661c-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1661c-204">a.</span><span class="sxs-lookup"><span data-stu-id="1661c-204">a.</span></span> <span data-ttu-id="1661c-205">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1661c-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1661c-206">b.</span><span class="sxs-lookup"><span data-stu-id="1661c-206">b.</span></span> <span data-ttu-id="1661c-207">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1661c-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1661c-208">c.</span><span class="sxs-lookup"><span data-stu-id="1661c-208">c.</span></span> <span data-ttu-id="1661c-209">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1661c-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1661c-210">d.</span><span class="sxs-lookup"><span data-stu-id="1661c-210">d.</span></span> <span data-ttu-id="1661c-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1661c-211">Click **Create**.</span></span>
 
### <a name="creating-an-appdynamics-test-user"></a><span data-ttu-id="1661c-212">Skapa en testanvändare AppDynamics</span><span class="sxs-lookup"><span data-stu-id="1661c-212">Creating an AppDynamics test user</span></span>

<span data-ttu-id="1661c-213">Om du vill aktivera Azure AD-användare kan logga in på AppDynamics etableras de i AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="1661c-213">To enable Azure AD users to log in to AppDynamics, they must be provisioned into AppDynamics.</span></span> <span data-ttu-id="1661c-214">När det gäller AppDynamics är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1661c-214">In the case of AppDynamics, provisioning is a manual task.</span></span>

<span data-ttu-id="1661c-215">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="1661c-215">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="1661c-216">Logga in på webbplatsen AppDynamics företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="1661c-216">Log in to your AppDynamics company site as an administrator.</span></span>

2. <span data-ttu-id="1661c-217">Gå till **användare**, och klicka sedan på  **+**  att öppna den **skapa användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1661c-217">Go to **Users**, and then click **+** to open the **Create User** dialog.</span></span>
   
    <span data-ttu-id="1661c-218">![Användare](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "användare")</span><span class="sxs-lookup"><span data-stu-id="1661c-218">![Users](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "Users")</span></span>

3. <span data-ttu-id="1661c-219">I den **skapa användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1661c-219">In the **Create User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="1661c-220">![Skapa användare](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="1661c-220">![Create User](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "Create User")</span></span>
   
    <span data-ttu-id="1661c-221">a.</span><span class="sxs-lookup"><span data-stu-id="1661c-221">a.</span></span> <span data-ttu-id="1661c-222">Typ av **användarnamn**, **namn**, **e-post**, **nytt lösenord**, **Upprepa nytt lösenord** av en giltig AAD-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="1661c-222">Type the **Username**, **Name**, **Email**, **New Password**, **Repeat New Password** of a valid AAD account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="1661c-223">b.</span><span class="sxs-lookup"><span data-stu-id="1661c-223">b.</span></span> <span data-ttu-id="1661c-224">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1661c-224">Click **Save**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="1661c-225">Du kan använda något annat AppDynamics användarens konto skapas verktyg eller API: er som tillhandahålls av AppDynamics att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="1661c-225">You can use any other AppDynamics user account creation tools or APIs provided by AppDynamics to provision Azure AD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1661c-226">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="1661c-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1661c-227">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="1661c-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AppDynamics.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="1661c-229">**Om du vill tilldela AppDynamics Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1661c-229">**To assign Britta Simon to AppDynamics, perform the following steps:**</span></span>

1. <span data-ttu-id="1661c-230">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1661c-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1661c-232">Välj i listan med program **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="1661c-232">In the applications list, select **AppDynamics**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_app.png) 

3. <span data-ttu-id="1661c-234">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1661c-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="1661c-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1661c-236">Click **Add** button.</span></span> <span data-ttu-id="1661c-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1661c-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="1661c-239">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="1661c-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1661c-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1661c-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1661c-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1661c-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1661c-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1661c-242">Testing single sign-on</span></span>

<span data-ttu-id="1661c-243">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="1661c-243">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="1661c-244">När du klickar på panelen AppDynamics på åtkomstpanelen du bör få automatiskt loggat in på ditt AppDynamics program.</span><span class="sxs-lookup"><span data-stu-id="1661c-244">When you click the AppDynamics tile in the Access Panel, you should get automatically signed-on to your AppDynamics application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1661c-245">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1661c-245">Additional resources</span></span>

* [<span data-ttu-id="1661c-246">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1661c-246">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1661c-247">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1661c-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_203.png

