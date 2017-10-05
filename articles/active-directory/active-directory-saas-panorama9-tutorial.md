---
title: "Självstudier: Azure Active Directory-integrering med Panorama9 | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Panorama9."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 934c0743464fd32398071aa3d07f7af76fdf7e3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a><span data-ttu-id="ee73e-103">Självstudier: Azure Active Directory-integrering med Panorama9</span><span class="sxs-lookup"><span data-stu-id="ee73e-103">Tutorial: Azure Active Directory integration with Panorama9</span></span>

<span data-ttu-id="ee73e-104">I kursen får lära du att integrera Panorama9 med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ee73e-104">In this tutorial, you learn how to integrate Panorama9 with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ee73e-105">Integrera Panorama9 med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ee73e-105">Integrating Panorama9 with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ee73e-106">Du kan styra i Azure AD som har åtkomst till Panorama9</span><span class="sxs-lookup"><span data-stu-id="ee73e-106">You can control in Azure AD who has access to Panorama9</span></span>
- <span data-ttu-id="ee73e-107">Du kan aktivera användarna att automatiskt hämta loggat in på Panorama9 (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ee73e-107">You can enable your users to automatically get signed-on to Panorama9 (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ee73e-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ee73e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ee73e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ee73e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee73e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ee73e-110">Prerequisites</span></span>

<span data-ttu-id="ee73e-111">För att konfigurera Azure AD-integrering med Panorama9, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ee73e-111">To configure Azure AD integration with Panorama9, you need the following items:</span></span>

- <span data-ttu-id="ee73e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ee73e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ee73e-113">En Panorama9 enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ee73e-113">A Panorama9 single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ee73e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ee73e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ee73e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ee73e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ee73e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ee73e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ee73e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ee73e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ee73e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ee73e-118">Scenario description</span></span>
<span data-ttu-id="ee73e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ee73e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ee73e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ee73e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ee73e-121">Att lägga till Panorama9 från galleriet</span><span class="sxs-lookup"><span data-stu-id="ee73e-121">Adding Panorama9 from the gallery</span></span>
2. <span data-ttu-id="ee73e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ee73e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panorama9-from-the-gallery"></a><span data-ttu-id="ee73e-123">Att lägga till Panorama9 från galleriet</span><span class="sxs-lookup"><span data-stu-id="ee73e-123">Adding Panorama9 from the gallery</span></span>
<span data-ttu-id="ee73e-124">Du måste lägga till Panorama9 från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Panorama9 i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ee73e-124">To configure the integration of Panorama9 into Azure AD, you need to add Panorama9 from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ee73e-125">**Utför följande steg för att lägga till Panorama9 från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ee73e-125">**To add Panorama9 from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ee73e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ee73e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ee73e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ee73e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ee73e-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ee73e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ee73e-133">I sökrutan skriver **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-133">In the search box, type **Panorama9**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_search.png)

5. <span data-ttu-id="ee73e-135">Välj i resultatpanelen **Panorama9**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ee73e-135">In the results panel, select **Panorama9**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ee73e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ee73e-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="ee73e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Panorama9 baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ee73e-138">In this section, you configure and test Azure AD single sign-on with Panorama9 based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ee73e-139">Azure AD måste du känna till användaren i Panorama9 motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ee73e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Panorama9 is to a user in Azure AD.</span></span> <span data-ttu-id="ee73e-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Panorama9 upprättas.</span><span class="sxs-lookup"><span data-stu-id="ee73e-140">In other words, a link relationship between an Azure AD user and the related user in Panorama9 needs to be established.</span></span>

<span data-ttu-id="ee73e-141">I Panorama9, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ee73e-141">In Panorama9, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ee73e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Panorama9, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ee73e-142">To configure and test Azure AD single sign-on with Panorama9, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ee73e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ee73e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ee73e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ee73e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ee73e-145">**[Skapa en testanvändare Panorama9](#creating-a-panorama9-test-user)**  – du har en motsvarighet för Britta Simon i Panorama9 som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ee73e-145">**[Creating a Panorama9 test user](#creating-a-panorama9-test-user)** - to have a counterpart of Britta Simon in Panorama9 that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ee73e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ee73e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ee73e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ee73e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ee73e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ee73e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ee73e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Panorama9 program.</span><span class="sxs-lookup"><span data-stu-id="ee73e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Panorama9 application.</span></span>

<span data-ttu-id="ee73e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Panorama9:**</span><span class="sxs-lookup"><span data-stu-id="ee73e-150">**To configure Azure AD single sign-on with Panorama9, perform the following steps:**</span></span>

1. <span data-ttu-id="ee73e-151">I Azure-portalen på den **Panorama9** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-151">In the Azure portal, on the **Panorama9** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ee73e-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ee73e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_samlbase.png)

3. <span data-ttu-id="ee73e-155">På den **Panorama9 domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ee73e-155">On the **Panorama9 Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_url.png)

    <span data-ttu-id="ee73e-157">a.</span><span class="sxs-lookup"><span data-stu-id="ee73e-157">a.</span></span> <span data-ttu-id="ee73e-158">I den **inloggnings-URL** textruta Skriv en URL som:`https://dashboard.panorama9.com/saml/access/3262`</span><span class="sxs-lookup"><span data-stu-id="ee73e-158">In the **Sign-on URL** textbox, type a URL as: `https://dashboard.panorama9.com/saml/access/3262`</span></span>

    <span data-ttu-id="ee73e-159">b.</span><span class="sxs-lookup"><span data-stu-id="ee73e-159">b.</span></span> <span data-ttu-id="ee73e-160">I den **identifierare** textruta Skriv en URL med följande mönster:`http://www.panorama9.com/saml20/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="ee73e-160">In the **Identifier** textbox, type a URL using the following pattern: `http://www.panorama9.com/saml20/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ee73e-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ee73e-161">These values are not real.</span></span> <span data-ttu-id="ee73e-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="ee73e-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ee73e-163">Kontakta [Panorama9 klienten supportteamet](https://support.panorama9.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ee73e-163">Contact [Panorama9 Client support team](https://support.panorama9.com) to get these values.</span></span> 
 
4. <span data-ttu-id="ee73e-164">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="ee73e-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_certificate.png) 

5. <span data-ttu-id="ee73e-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ee73e-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ee73e-168">På den **Panorama9 Configuration** klickar du på **konfigurera Panorama9** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ee73e-168">On the **Panorama9 Configuration** section, click **Configure Panorama9** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ee73e-169">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ee73e-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_configure.png) 

5. <span data-ttu-id="ee73e-171">Logga in på webbplatsen Panorama9 företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="ee73e-171">In a different web browser window, log into your Panorama9 company site as an administrator.</span></span>

6. <span data-ttu-id="ee73e-172">Klicka på i verktygsfältet högst upp **hantera**, och klicka sedan på **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-172">In the toolbar on the top, click **Manage**, and then click **Extensions**.</span></span>
   
   <span data-ttu-id="ee73e-173">![Tillägg](./media/active-directory-saas-panorama9-tutorial/ic790023.png "tillägg")</span><span class="sxs-lookup"><span data-stu-id="ee73e-173">![Extensions](./media/active-directory-saas-panorama9-tutorial/ic790023.png "Extensions")</span></span>
7. <span data-ttu-id="ee73e-174">På den **tillägg** dialogrutan klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-174">On the **Extensions** dialog, click **Single Sign-On**.</span></span>
   
   <span data-ttu-id="ee73e-175">![Enkel inloggning](./media/active-directory-saas-panorama9-tutorial/ic790024.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="ee73e-175">![Single Sign-On](./media/active-directory-saas-panorama9-tutorial/ic790024.png "Single Sign-On")</span></span>
8. <span data-ttu-id="ee73e-176">I den **inställningar** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ee73e-176">In the **Settings** section, perform the following steps:</span></span>
   
   <span data-ttu-id="ee73e-177">![Inställningar för](./media/active-directory-saas-panorama9-tutorial/ic790025.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="ee73e-177">![Settings](./media/active-directory-saas-panorama9-tutorial/ic790025.png "Settings")</span></span>
   
    <span data-ttu-id="ee73e-178">a.</span><span class="sxs-lookup"><span data-stu-id="ee73e-178">a.</span></span> <span data-ttu-id="ee73e-179">I **identitet providern URL** textruta klistra in värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ee73e-179">In **Identity provider URL** textbox, paste the value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="ee73e-180">b.</span><span class="sxs-lookup"><span data-stu-id="ee73e-180">b.</span></span> <span data-ttu-id="ee73e-181">I **certifikat fingeravtryck** textruta klistra in den **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ee73e-181">In **Certificate fingerprint** textbox, paste the **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>    
         
9. <span data-ttu-id="ee73e-182">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-182">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="ee73e-183">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ee73e-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ee73e-184">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ee73e-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ee73e-185">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ee73e-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ee73e-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ee73e-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="ee73e-187">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ee73e-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ee73e-189">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ee73e-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ee73e-190">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ee73e-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ee73e-192">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ee73e-194">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ee73e-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ee73e-196">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ee73e-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ee73e-198">a.</span><span class="sxs-lookup"><span data-stu-id="ee73e-198">a.</span></span> <span data-ttu-id="ee73e-199">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ee73e-200">b.</span><span class="sxs-lookup"><span data-stu-id="ee73e-200">b.</span></span> <span data-ttu-id="ee73e-201">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ee73e-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ee73e-202">c.</span><span class="sxs-lookup"><span data-stu-id="ee73e-202">c.</span></span> <span data-ttu-id="ee73e-203">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ee73e-204">d.</span><span class="sxs-lookup"><span data-stu-id="ee73e-204">d.</span></span> <span data-ttu-id="ee73e-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-205">Click **Create**.</span></span>
 
### <a name="creating-a-panorama9-test-user"></a><span data-ttu-id="ee73e-206">Skapa en testanvändare Panorama9</span><span class="sxs-lookup"><span data-stu-id="ee73e-206">Creating a Panorama9 test user</span></span>

<span data-ttu-id="ee73e-207">För att aktivera Azure AD-användare att logga in på Panorama9 etableras de i Panorama9.</span><span class="sxs-lookup"><span data-stu-id="ee73e-207">In order to enable Azure AD users to log into Panorama9, they must be provisioned into Panorama9.</span></span>  

<span data-ttu-id="ee73e-208">När det gäller Panorama9 är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ee73e-208">In the case of Panorama9, provisioning is a manual task.</span></span>

<span data-ttu-id="ee73e-209">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="ee73e-209">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="ee73e-210">Logga in på ditt **Panorama9** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="ee73e-210">Log in to your **Panorama9** company site as an administrator.</span></span>

2. <span data-ttu-id="ee73e-211">Klicka på menyn högst upp **hantera**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-211">In the menu on the top, click **Manage**, and then click **Users**.</span></span>
   
  <span data-ttu-id="ee73e-212">![Användare](./media/active-directory-saas-panorama9-tutorial/ic790027.png "användare")</span><span class="sxs-lookup"><span data-stu-id="ee73e-212">![Users](./media/active-directory-saas-panorama9-tutorial/ic790027.png "Users")</span></span>

3. <span data-ttu-id="ee73e-213">I avsnittet användare klickar du på  **+**  att lägga till nya användare.</span><span class="sxs-lookup"><span data-stu-id="ee73e-213">In the Users section, Click **+** to add new user.</span></span>

 <span data-ttu-id="ee73e-214">![Användare](./media/active-directory-saas-panorama9-tutorial/ic790028.png "användare")</span><span class="sxs-lookup"><span data-stu-id="ee73e-214">![Users](./media/active-directory-saas-panorama9-tutorial/ic790028.png "Users")</span></span>

4. <span data-ttu-id="ee73e-215">Gå till avsnittet användaren data, ange en giltig Azure Active Directory-användare som du vill etablera i e-postadress i **e-post** textruta.</span><span class="sxs-lookup"><span data-stu-id="ee73e-215">Go to the User data section, type the email address of a valid Azure Active Directory user you want to provision into the **Email** textbox.</span></span>

5. <span data-ttu-id="ee73e-216">Gå till avsnittet för användare klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-216">Come to the Users section, Click **Save**.</span></span>
   
> [!NOTE]
    > <span data-ttu-id="ee73e-217">Azure Active Directory kontoinnehavaren får ett e-postmeddelande och följer en länk för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="ee73e-217">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ee73e-218">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ee73e-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ee73e-219">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Panorama9.</span><span class="sxs-lookup"><span data-stu-id="ee73e-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Panorama9.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ee73e-221">**Om du vill tilldela Panorama9 Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ee73e-221">**To assign Britta Simon to Panorama9, perform the following steps:**</span></span>

1. <span data-ttu-id="ee73e-222">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ee73e-224">Välj i listan med program **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-224">In the applications list, select **Panorama9**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_app.png) 

3. <span data-ttu-id="ee73e-226">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ee73e-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ee73e-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ee73e-228">Click **Add** button.</span></span> <span data-ttu-id="ee73e-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ee73e-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ee73e-231">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ee73e-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ee73e-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ee73e-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ee73e-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ee73e-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ee73e-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ee73e-234">Testing single sign-on</span></span>

<span data-ttu-id="ee73e-235">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ee73e-235">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ee73e-236">När du klickar på panelen Panorama9 på åtkomstpanelen du bör få automatiskt loggat in på Panorama9 program.</span><span class="sxs-lookup"><span data-stu-id="ee73e-236">When you click the Panorama9 tile in the Access Panel, you should get automatically signed-on to Panorama9 application.</span></span>
<span data-ttu-id="ee73e-237">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ee73e-237">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee73e-238">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ee73e-238">Additional resources</span></span>

* [<span data-ttu-id="ee73e-239">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ee73e-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ee73e-240">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ee73e-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_203.png

